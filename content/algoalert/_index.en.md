---
title: "Rule-based trading and alerts"
date: 2026-02-12T12:00:00+01:00
draft: false
weight: 22
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
GT supports rule-based trading and a comprehensive alert system. This functionality is organized into two layers: the strategic core allocation for long-term portfolio rebalancing and the tactical trading layer for speculative strategies with alerts.

## Hierarchical Structure
The algo system in GT is built hierarchically. At the top sits the portfolio strategy (**AlgoTop**), linked to a watchlist. Below it are the asset classes with their percentage weightings, and at the lowest level the individual securities with their assigned strategies.

{{< mermaid >}}
graph TD
    A["Portfolio Strategy (AlgoTop)"] --> B["Asset Class 1 (e.g. Equities 60%)"]
    A --> C["Asset Class 2 (e.g. Bonds 40%)"]
    B --> D["Security A"]
    B --> E["Security B"]
    C --> F["Security C"]
    D --> G["Strategy 1"]
    D --> H["Strategy 2"]
    E --> I["Strategy 3"]
{{< /mermaid >}}

## Two Operating Layers
GT distinguishes between two operating layers for rule-based trading.

The **core allocation** defines the long-term distribution of the portfolio across different asset classes. Through periodic rebalancing, deviations from target weightings are detected and action recommendations are generated. This layer is suitable for a strategic, passive investment approach.

The **tactical trading layer** enables assigning speculative strategies to individual securities. This encompasses simple price alerts, technical indicators and complex trading strategies such as dip-buying with profit and loss management.

## Two Execution Modes
Strategies in GT can operate in two modes. In **alarm mode**, the defined strategies are evaluated against current market data. When the configured conditions are met, the system generates alert messages that inform the user via the internal messaging system. The user then independently decides on execution.

{{% notice style="info" title="Simulation mode" %}}
A **simulation mode** for backtesting strategies against historical data is planned but not yet implemented. In a future version it will be possible to test strategies with a copy of the portfolio against historical price data.
{{% /notice %}}

## Navigation
In GT you can reach the rule-based trading area via the navigation panel on the left side. Click on **Rule-based Trading** to open the tree view of your portfolio strategies. Additionally, you will find an overview of all active alerts for your tenant under **Tenant Alerts**.
