---
title: "Rule-based trading and alerts"
date: 2026-09-26T12:00:00+02:00
draft: false
weight: 22
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress - Target version V0.38.0" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
GT supports rule-based trading and a comprehensive alert system. The strategies belong to three kinds: the core allocation for the long-term rebalancing of the portfolio, trading strategies for individual securities, and alerts that merely observe prices and holdings. Which kind takes effect where, in your own portfolio or in a simulation, is explained in the section [Where each strategy takes effect](#where-each-strategy-takes-effect).

## Hierarchical Structure
The algo system in GT is built hierarchically. At the top sits the **Portfolio based strategy**, usually linked to a watchlist. Below it are the asset classes or custom categories with their percentage weightings, and at the lowest level the individual securities. Strategies can be assigned to each of these levels.

{{< mermaid >}}
graph TD
    A["Portfolio based strategy"] --> B["Asset Class 1 (e.g. Equities 60%)"]
    A --> C["Asset Class 2 (e.g. Bonds 40%)"]
    B --> D["Security A"]
    B --> E["Security B"]
    C --> F["Security C"]
    D --> G["Strategy 1"]
    D --> H["Strategy 2"]
    E --> I["Strategy 3"]
{{< /mermaid >}}

## Three Kinds of Strategies
The **core allocation** defines the long-term distribution of the portfolio across asset classes. [Portfolio rebalancing](./strategy/rebalancing/) compares the holdings with the target weightings of the tree at fixed intervals and determines the purchases and sales that restore the desired distribution. This kind is suitable for a strategic, passive investment approach.

A **trading strategy** is assigned to an individual security and decides by itself when it is bought and when it is sold. At present this is the [Mean Reversion Dip](./strategy/meanreversiondip/) with its profit and loss management. A trading strategy defines everything an order needs: the direction, the quantity on entry and on exit, the budget within the weighting of the tree and the cooldowns between two trades.

An **alert** observes a price, a holding or a technical indicator and reports as soon as a condition is met. This includes the [price alerts](./strategy/pricealerts/) and the [indicator alerts](./strategy/indicatoralerts/). An alert is an observation and not an order: it tells you that something has happened, but neither whether to buy or to sell nor how much.

## Where each strategy takes effect
GT evaluates strategies in two places. [Portfolio monitoring](./algo/#portfolio-monitoring) continuously observes your own portfolio in the background. A **simulation environment** is a separated copy of your portfolios and accounts as of an opening date in the past; how you create one is described under [Rule based trading](./algo/#create-a-simulation-environment). The [Historical replay](./historicalrun/) evaluates the strategies in it against past price data. Evaluating future, simulated price paths follows in a later version. Not every kind of strategy takes effect in both places:

| Kind | Strategy type | Portfolio monitoring | Simulation |
|---|---|---|---|
| Core allocation | Portfolio rebalancing | Recommendation as a message in the inbox | Purchases and sales are booked |
| Trading strategy | Mean Reversion Dip | Recommendation and notification | Purchases and sales are booked |
| Alert | Price alerts and indicator alerts | Alert in the inbox | Not evaluated |

In your own portfolio GT never books anything by itself. Rebalancing and the Mean Reversion Dip also only deliver recommendations there; you decide whether and when to act, and you record the trade as an ordinary transaction. In a simulation, however, there is nobody who could react to a message. The replay therefore executes by itself what a strategy proposes: an order that arises at the close of a trading day is booked at the closing price of the next eligible trading day.

This only works with strategies that describe a complete order. Rebalancing calculates direction and quantity from the deviation from the target weightings, the Mean Reversion Dip from its configuration. An alert lacks this information: a price falling below a limit may be a buying opportunity or a reason to sell, and how much should be traded cannot be derived from it either. In addition, alerts react to running intraday prices, whereas the simulation calculates with the past daily closing prices. Alerts are therefore not evaluated in a simulation; they belong exclusively to your own portfolio.

If you want to check how a buy or sell rule would have worked in the past, describe it with the Mean Reversion Dip. Its entry after a price movement, its profit target, its stop-loss and its indicator rules capture conditions similar to many price and indicator alerts and additionally define the quantity.

### Rebalancing and Mean Reversion Dip together
Both can be used in the same portfolio based strategy: rebalancing on the top level, the Mean Reversion Dip on individual securities. Portfolio monitoring and the simulation treat this combination in the same way. So that the two do not cancel each other out, the asset class of a security with a Mean Reversion Dip is considered tactical. Its weighting is then only an upper limit. Rebalancing reduces the asset class when it exceeds this limit, but never buys into it, not even for the other securities of the same asset class. When to buy is decided by the Mean Reversion Dip alone, and it may not exceed the upper limit either.

A dip position opened shortly before is therefore not sold again at the next checkpoint, as long as it has not grown above its upper limit through price gains. Likewise, rebalancing does not rebuild a position that the Mean Reversion Dip has closed. Preferably put securities with a Mean Reversion Dip in an asset class of their own, so that the other asset classes continue to be fully rebalanced. An alert does not make an asset class tactical. Details are described under [Tactical Groups Are Not Filled Up](./strategy/rebalancing/#tactical-groups-are-not-filled-up).

{{< mermaid >}}
graph TD
    T["Portfolio based strategy"] --> A["Asset class equities"]
    T --> D["Asset class dip candidates<br/>tactical: weighting is an upper limit"]
    A --> A1["World ETF"]
    A --> A2["US ETF"]
    D --> D1["Share X"]
    R["Portfolio rebalancing"] -.->|"buys and sells"| A
    R -.->|"sells only above the upper limit"| D
    M["Mean Reversion Dip"] -.->|"buys and sells"| D1
{{< /mermaid >}}

## Navigation
In GT you can reach the rule-based trading area via the navigation panel on the left side. Click on **Rule-based Trading** to open the tree view of your portfolio strategies. An overview of all alerts of your tenant is found on the client in the tab **Alert**.
