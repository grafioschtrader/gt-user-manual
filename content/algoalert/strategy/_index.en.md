---
title: "Strategy"
date: 2026-02-12T12:00:00+01:00
draft: false
weight: 20
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
GT supports various strategy types, from simple price alerts to complex trading strategies. Each strategy is assigned to a node in the hierarchical AlgoTop tree structure and monitors the associated securities based on the configured conditions.

## Simple Strategies
Simple strategies use flat key-value parameters configured via dynamically generated form fields. When creating a strategy via **Create Strategy**, the strategy type is selected, and the system automatically displays the appropriate input fields. Simple strategies include price alerts, indicator alerts and portfolio rebalancing.

## Complex Strategies
Complex strategies have a nested configuration stored as JSON. A YAML text field is available for editing. When selecting a complex strategy, instead of dynamic form fields, a text area labeled **Strategy Configuration (YAML)** appears where the configuration can be written or pasted as YAML. When saving, the YAML is automatically converted to JSON.

## Strategy Types Overview

{{< mermaid >}}
graph TD
    S["Strategy Types"] --> R["Rebalancing"]
    S --> P["Price Alerts"]
    S --> I["Indicator Alerts"]
    S --> K["Complex Strategies"]
    R --> R1["Portfolio Rebalancing"]
    P --> P1["Absolut price gain/lose alert"]
    P --> P2["Holdings percentage gain/lose price Alert"]
    P --> P3["Gain/loss warning in a period"]
    I --> I1["Moving average crossing alert"]
    I --> I2["RSI threshold alert"]
    I --> I3["Custom expression alert"]
    K --> K1["Mean Reversion Dip"]
{{< /mermaid >}}

The following table lists all available strategy types with their assignment levels. The level indicates whether the strategy can be assigned at the top level (portfolio), asset class level or security level.

| Strategy Type | Levels | Category | Details |
|---|---|---|---|
| [Portfolio Rebalancing](./rebalancing/) | Portfolio, Asset Class, Security | Rebalancing | Periodic rebalancing of portfolio allocation |
| [Absolut price gain/lose alert](./pricealerts/) | Security | Price Alert | Alert on absolute price thresholds |
| [Holdings percentage gain/lose price Alert](./pricealerts/) | Portfolio, Asset Class, Security | Price Alert | Alert on percentage position change |
| [Gain/loss warning in a period](./pricealerts/) | Portfolio, Asset Class, Security | Price Alert | Alert on price change over a time period |
| [Moving average crossing alert](./indicatoralerts/) | Security | Indicator | Alert on moving average crossing |
| [RSI threshold alert](./indicatoralerts/) | Security | Indicator | Alert on RSI threshold breach |
| [Custom expression alert](./indicatoralerts/) | Security | Indicator | Alert with custom expression |
| [Mean Reversion Dip](./meanreversiondip/) | Security | Complex | Dip-buying with profit/loss management |

{{% notice style="info" title="Note" %}}
For complex strategies, an enhanced editor with syntax highlighting and visual configuration will be provided in future versions.
{{% /notice %}}
