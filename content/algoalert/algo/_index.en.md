---
title: "Rule based trading"
date: 2026-02-12T12:00:00+01:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
Rule-based trading in GT is based on a hierarchical tree structure. At the root sits the portfolio strategy (**AlgoTop**), which establishes a link to an existing watchlist. Below this root, asset classes with percentage weightings are created, and within the asset classes, securities from the linked watchlist are assigned.

## Creating and Editing a Portfolio Strategy
You create a new portfolio strategy via the context menu in the tree view of **Rule-based Trading**. When creating a portfolio strategy, the following properties are configured:

| Property | Description |
|---|---|
| **Name** | Name of the portfolio strategy |
| **Watchlist** | The linked watchlist from which securities can be selected |
| **Maximum investment** | Maximum percentage of the portfolio that may be invested |
| **Primary securities account** | The primary securities account for transactions |
| **Secondary securities account** | An optional secondary securities account |

After the portfolio strategy has been created, asset classes and securities can be added as child nodes.

## Adding Asset Classes
Via the context menu of the portfolio strategy you can create a new asset class as a child node using **Add asset class**. Each asset class receives a percentage **Weighting** that defines the target share of this asset class in the overall portfolio. The sum of all weightings should ideally equal 100%.

## Adding Securities
Within an asset class, you can add individual securities via **Add strategy security**. The available securities come from the watchlist linked to the parent portfolio strategy.

## Assigning Strategies
Strategies can be assigned both at the asset class level and at the security level. Via **Create Strategy** in the context menu, a dialog opens where the desired strategy type is selected. Depending on the strategy type, different configuration fields appear. Detailed information about the individual strategy types can be found under [Strategy](../strategy/).

{{< mermaid >}}
graph TD
    A["Portfolio Strategy<br/>(Name, Watchlist, Max. Investment)"] --> B["Asset Class<br/>(Weighting %)"]
    A --> C["Asset Class<br/>(Weighting %)"]
    B --> D["Security<br/>(from Watchlist)"]
    B --> E["Security<br/>(from Watchlist)"]
    D --> F["Strategy"]
    E --> G["Strategy"]
    C --> H["Security<br/>(from Watchlist)"]
    H --> I["Strategy"]
{{< /mermaid >}}

{{% notice style="info" title="Note" %}}
The asset class at the tree level does not directly correspond to an asset class in GT. It serves for grouping and weighting securities within the portfolio strategy.
{{% /notice %}}
