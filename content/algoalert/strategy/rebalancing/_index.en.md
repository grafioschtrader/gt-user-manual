---
title: "Portfolio Rebalancing"
date: 2026-09-29T12:00:00+02:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress - Target version V0.38.0" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
Portfolio rebalancing compares the actual distribution of your assets with the target weightings you have entered in the tree of the rule-based strategy. When the configured interval has elapsed and an asset class has drifted further from its target than you allow, you receive a recommendation on which securities you would have to buy or sell, and how much, to restore the intended distribution.

GT never carries out these recommendations itself. No transaction is booked and no order is placed; you decide whether and when to act.

## Step by Step
1. Create a portfolio based strategy under **Rule-based Trading**, either by hand, with **Create strategy from portfolio** from your current holdings or with **Create strategy from watchlist**, see [Rule based trading](../../algo/).
2. Add asset classes and securities and set their **Weighting**. Check the column **Sum of weightings**: the asset classes and, separately, the securities of each asset class must add up to 100%; **Normalize all percentages** establishes this.
3. On the portfolio based strategy choose **Create Strategy definition**, select **Portfolio rebalance** under **Strategy name** and enter its parameters.
4. Make sure the portfolio based strategy is active.
5. Open the report [Asset Classes with Cash](../../../reportportfolio/securitycashaccountreport/) and select your strategy under **Compare with strategy** to see targets, deviations and proposed trades.
6. When a checkpoint is reached, read the messages in your inbox and book the trades yourself as ordinary transactions.

## Where the Target Weightings Live
The target weightings are not a setting of the strategy but the **Weighting** column in the tree of the rule-based strategy. Each level divides up the amount of the level above it, and the weightings of siblings must add up to 100%. The menu entries for normalising the weightings establish this automatically when needed.

At the very top sits the **Maximum investment**: the share of your net equity the strategy may invest at all. Everything below divides exactly that amount, and not the total assets a second time.

An example with net equity of 100,000:

| Level | Weighting | Amount | Share of total assets |
|---|---:|---:|---:|
| Maximum investment | 48.2% | 48,200 | 48.2% |
| Asset class group within that budget | 50% | 24,100 | 24.1% |
| Security within that group | 60% | 14,460 | 14.46% |

The report always shows the right-hand column, the share of total assets. Only that makes a group and a single security comparable with each other.

{{< mermaid >}}
graph TD
    T["Portfolio based strategy<br/>Maximum investment 48.2% = 48,200"] --> A["Asset class group Equities<br/>50% = 24,100 = 24.1% of assets"]
    T --> B["Asset class group Bonds<br/>50% = 24,100 = 24.1% of assets"]
    A --> A1["World ETF<br/>60% = 14,460 = 14.46% of assets"]
    A --> A2["US ETF<br/>40% = 9,640 = 9.64% of assets"]
{{< /mermaid >}}

## Parameters of the Strategy
Rebalancing is entered exclusively at the top level, that is on the portfolio based strategy itself. There is no second rebalancing on an asset class group or on a security, because the weighting of the tree already states the target there, and two targets for the same position would contradict each other.

| Parameter | Description |
|---|---|
| **Number of redeployments per year (R)** | Specifies how often per year the allocation is compared with its targets. The interval between two checkpoints is 365 days divided by this number. Allowed values: 1 to 53. |
| **Class deviation (percentage points of investment budget)** | The tolerance of an asset class. Only when its amount deviates from its target by more than this share of the **Maximum investment** amount is it adjusted. Allowed values: 1 to 49. |
| **Security allocation band (percentage points)** | The band of a security around its weighting, in percentage points of the target amount of its asset class. Within this band an adjustment may move a security above or below its exact target. Allowed values: 0 to 100, the default is 5. |
| **Maximum traded securities per asset class** | How many distinct securities of an asset class are traded at most at one checkpoint. Any whole number from 1 is allowed, the default is 3. |

The last two parameters can be overridden for an individual asset class in the dialog **Strategy asset class**, see [Rule based trading](../../algo/#adding-and-editing-an-asset-class). If the fields stay empty there, the setting of the portfolio rebalance applies.

## When a Recommendation Arises
The parameters answer different questions. The number of redeployments per year sets how often the allocation is compared at all: a checkpoint recurs every 365 days divided by that number, rounded to whole days, so four redeployments per year give a checkpoint every 91 days. The class deviation decides at such a checkpoint which asset classes are adjusted: only those whose amount deviates from the target by more than the tolerance. The adjustment of such an asset class then covers its whole deviation. How a purchase this requires is paid for is described in [How Purchases Are Funded](#how-purchases-are-funded).

The security band and the trade limit then determine through which securities this adjustment is made. GT ranks the securities of the asset class: if the asset class has to grow, the most underweight securities come first, if it has to shrink, the most overweight ones; on a tie the higher trading volume decides. In turn, each security takes up as much of the adjustment as its band allows, until the adjustment is fully placed or the trade limit is reached. An asset class is thus brought back with few trades instead of setting every security exactly to its target. If a remainder is left because the trade limit is reached or the bands do not suffice, the report shows **Maximum number of traded securities reached** or **Remaining adjustment cannot be placed within eligible security bands**. Securities not used carry the reason **Not selected for this class adjustment**.

A recommendation therefore arises only when a checkpoint is reached and at least one asset class lies outside its tolerance. A drift between two checkpoints is visible in the report and triggers no recommendation; GT does report its beginning, see [Allocation outside its tolerance](#allocation-outside-its-tolerance). A checkpoint at which every asset class is within its tolerance passes without a message; the next interval counts from it all the same. The very first evaluation of a portfolio based strategy is always a checkpoint.

{{< mermaid >}}
graph TD
    A["Daily evaluation"] --> B{"Checkpoint reached?"}
    B -->|No| C["Report updated,<br/>a new drift is only reported"]
    B -->|Yes| D{"Asset class outside<br/>its tolerance?"}
    D -->|No| E["No message,<br/>next interval starts"]
    D -->|Yes| H{"Is the cash enough<br/>for the purchases?"}
    H -->|No| I["Reduce overweight asset classes<br/>in proportion"]
    H -->|Yes| G["Select securities within their band<br/>and the trade limit"]
    I --> G
    G --> F["Message per selected security,<br/>next interval starts"]
{{< /mermaid >}}

If a recommendation arises, you receive a message in your inbox for each selected security, provided the delivery of alerts is switched on. The messages arise in the order of the deviation, beginning with the positions furthest above their target, so that you can reduce positions before you build up others. The comparison itself can be viewed at any time in the [Asset Classes with Cash](../../../reportportfolio/securitycashaccountreport/) report, even when nothing is due.

## Allocation Outside Its Tolerance
Between two checkpoints GT proposes no trades, but it reports when the allocation leaves its tolerance. On the daily evaluation of the strategy assigned to [portfolio monitoring](../../algo/#portfolio-monitoring), a message **Allocation outside its tolerance** arises as soon as an asset class deviates from its target by more than its **Class deviation**, a security leaves its **Security allocation band**, or the gross exposure exceeds the **Maximum investment**. The message names the asset class, the security or the portfolio based strategy concerned with target, actual and deviation.

What is reported is the beginning of such a situation. If an asset class stays outside its tolerance for weeks, you receive a single message; another one follows only after it has been inside in between. A situation that already exists at the first evaluation is reported at once. The message is a notice only: it makes no checkpoint due, moves none and proposes no trade. Tactical groups report nothing, because their target is a ceiling. The messages follow the switch **Alerts enabled** of the rebalancing, appear in the **Alert diagnostics** under **Notifications** and are counted on the dashboard card **Rebalancing monitoring** as **Outside the tolerance**.

## How Purchases Are Funded
A rebalancing redistributes the assets already invested and brings no new money into the portfolio. If an underweight asset class has to be increased, the purchase is therefore paid out of the available cash first. If that is not enough, the overweight asset classes are reduced in proportion to their overweight until the missing amount is covered. This also applies when these asset classes are themselves still within their tolerance. In a fully invested portfolio this is the normal case: one large deviation below target is usually matched by several smaller deviations above target, none of which exceeds the tolerance on its own. Without these sales the purchase could not be paid for at all.

An asset class is never reduced below its own target. In the report it carries the reason **Inside the tolerance, reduced to pay for the purchase of an underweight class**. The securities are selected by the same rules as for any other adjustment, that is by the security band and the trade limit of the asset class concerned.

An example with a net worth of 100'000 and a class deviation of 8 percentage points:

| Asset class | Target | Actual | Deviation | Adjustment |
|---|---:|---:|---:|---:|
| Bonds | 30'000 | 20'000 | −10 | Buy 10'000 |
| Equities | 30'000 | 36'000 | +6 | Sell 6'000 |
| Commodities | 20'000 | 24'000 | +4 | Sell 4'000 |
| Gold | 20'000 | 20'000 | 0 | none |

Only the bonds lie outside the tolerance. Since there is no cash, equities and commodities pay for the purchase in the ratio 6 to 4.

As the proceeds of a sale are only available once it has settled, it is advisable to execute the sales first and the purchases afterwards. The [historical simulation](../../historicalrun/) does this automatically: if a purchase could not be executed in full at the checkpoint for lack of available money, the simulation buys the rest on the following trading days until the target is reached, but for no more than five trading days. In the log such a day appears with the reason **Completion of the purchases of the preceding checkpoint**. The completion does not move the next checkpoint. The first follow-up day may pass without a purchase, because the proceeds of a sale made at the checkpoint are often only available on the second day. If a later day passes without a purchase, the completion ends, because no more money is to be expected then.

## Evaluation and Activation
GT evaluates the portfolio rebalance in the background once per day, against the closing prices of the last completed day. It is evaluated as long as the portfolio based strategy is assigned to portfolio monitoring, see [Rule based trading](../../algo/#portfolio-monitoring); the portfolio rebalance itself is active once created. If the strategy is not ready, for instance because the weightings of a level do not add up to 100%, its daily evaluation is skipped. It then cannot be chosen under **Compare with strategy**; the reason appears when you move the mouse over the entry. Which findings cause this is described under [Readiness of the strategy](../../algo/#readiness-of-the-strategy).

To evaluate at once, for example after changing weightings, open **Alert diagnostics** on the client's tab **Alert** and choose **Evaluate now**. The plan is then recalculated even if it was already evaluated that day. A checkpoint is not brought forward by this.

## The Comparison Uses Gross Exposure
The actual value of a position is its gross exposure, that is the amount with which it participates in the market, without offsetting long against short positions. For an ordinary long position this equals the position value. For a short position and for margin products it is more than the amount attributed to your assets: a short sale creates no additional investment capital, even though it brings money into the account.

The recommended action is always the trade and not the direction of the exposure. A short position that has grown too large is therefore reduced with **Buy** and not with **Sell**.

The recommended number of units is the amount divided by the price of one unit and is not rounded. Round it to a tradable quantity yourself.

## Tactical Groups Are Not Filled Up
If an asset class group or one of its securities carries a trading strategy of its own, its weighting counts only as an upper limit. Rebalancing reduces such a position when it has grown too large, but never opens or increases it: when to enter is decided by the strategy sitting there. The unused budget of such groups is reported separately, so it is apparent why the report is not filled up to the **Maximum investment**.

The upper limit applies to the whole group: if only one of its securities carries a Mean Reversion Dip, rebalancing no longer fills up its other securities either. Put such securities in a group of their own; how the two work together is described under [Rebalancing and Mean Reversion Dip together](../../#rebalancing-and-mean-reversion-dip-together). A price or indicator alert is not a trading strategy in this sense and does not make a group tactical.

## When the Investment Ceiling Is Exceeded
If gross exposure rises above the **Maximum investment**, for instance because prices have risen, the report states **Investment ceiling exceeded**. In this state only recommendations that reduce the exposure are issued; all others appear as **Blocked** with the corresponding reason. The same applies when net equity is not positive, because no target amount can be calculated then.

A portfolio whose gross exposure is larger than its net equity is still evaluated and displayed. It is merely not built up any further.

{{% notice style="info" title="Note" %}}
The valuation is always based on the closing prices of a completed trading day. The report states this **Valuation date**, so it is apparent which state the comparison refers to.
{{% /notice %}}
