---
title: "Strategy"
date: 2026-09-26T12:00:00+02:00
draft: false
weight: 20
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress - Target version V0.38.0" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
GT supports various strategy types, from simple price alerts to complex trading strategies. Each strategy is assigned to a node in the hierarchical tree structure of the portfolio based strategy and monitors the associated securities based on the configured conditions.

## Simple Strategies
Simple strategies use flat key-value parameters configured via dynamically generated form fields. When creating a strategy via **Create Strategy definition**, you select the strategy type under **Strategy name** in the dialog **Strategy definition**, and the system automatically displays the appropriate input fields. Only the types permitted on the selected level, and not already present there in a unique form, are offered. When editing, only the parameters can be changed, not the strategy type. Simple strategies include price alerts, indicator alerts and portfolio rebalancing.

## Complex Strategies
Complex strategies have a nested configuration. When a complex strategy is selected, a YAML editor with syntax highlighting, autocompletion, continuous syntax checking and explanations when hovering over a key appears instead of dynamic form fields. A new strategy is pre-filled with a template; **Load Template** restores it at any time. The checkbox **Active** appears for complex strategies only: when it is switched off, even an incomplete draft can be saved, which is not evaluated. **Apply** saves the strategy.

See [Editing and validating YAML]({{% ref "/intro/userinterface#yaml-editor" %}}) for the shared controls. **Validate** takes the **Active** setting into account: an inactive draft may remain incomplete, but any YAML text entered must be syntactically valid. An active strategy must also meet the requirements for an executable configuration. **Apply** validates again; errors leave the text in the dialog for correction.

## Strategy Types Overview

{{< mermaid >}}
graph TD
    S["Strategy Types"] --> R["Rebalancing"]
    S --> P["Price Alerts"]
    S --> I["Indicator Alerts"]
    S --> K["Complex Strategies"]
    R --> R1["Portfolio Rebalancing"]
    P --> P1["Absolute price gain/lose"]
    P --> P2["Holdings gain/lose"]
    P --> P3["Gain/loss in a period"]
    I --> I1["Moving average crossing"]
    I --> I2["RSI threshold"]
    I --> I3["Custom expression"]
    K --> K1["Mean Reversion Dip"]
{{< /mermaid >}}

The following table lists all available strategy types with their assignment levels. The level indicates whether the strategy can be assigned to the portfolio based strategy, to an asset class or to a security. The column Effect states what the strategy brings about in the portfolio monitoring of your own portfolio and in a simulation. Alerts are not evaluated in a simulation because they know neither the direction nor the quantity of an order; the reasoning is given under [Where each strategy takes effect](../#where-each-strategy-takes-effect). Portfolio rebalancing and the Mean Reversion Dip can be entered only once per node, all other types several times.

| Strategy Type | Levels | Category | Details | Effect |
|---|---|---|---|---|
| [Portfolio Rebalancing](./rebalancing/) | Portfolio based strategy | Rebalancing | Comparison of the allocation with the target weightings of the tree | Monitoring: recommendation · Simulation: purchases and sales |
| [Absolute price gain/lose](./pricealerts/) | Security | Price Alert | Alert on absolute price thresholds | Monitoring only: alert |
| [Holdings gain/lose](./pricealerts/) | Portfolio based strategy, Asset class, Security | Price Alert | Alert on percentage position change or price limits of a held position | Monitoring only: alert |
| [Gain/loss in a period](./pricealerts/) | Security | Price Alert | Alert on price change over a time period | Monitoring only: alert |
| [Moving average crossing](./indicatoralerts/) | Security | Indicator | Alert on moving average crossing | Monitoring only: alert |
| [RSI threshold](./indicatoralerts/) | Security | Indicator | Alert on RSI threshold breach | Monitoring only: alert |
| [Custom expression](./indicatoralerts/) | Security | Indicator | Alert with custom expression | Monitoring only: alert |
| [Mean Reversion Dip](./meanreversiondip/) | Security | Complex | Dip-buying with profit/loss management | Monitoring: recommendation · Simulation: purchases and sales |

{{% notice style="info" title="Note" %}}
Scale-out, averaging down and stop-loss are not strategy types of their own but sections of the YAML configuration of the [Mean Reversion Dip](./meanreversiondip/).
{{% /notice %}}
