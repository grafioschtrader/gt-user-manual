---
title: "Portfolio Rebalancing"
date: 2026-02-12T12:00:00+01:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
Portfolio rebalancing is a strategy that periodically compares the actual allocation of the portfolio with the target weightings. When the deviation exceeds a configured threshold, action recommendations for buy or sell transactions are generated to restore the original target allocation.

## How It Works
Rebalancing compares the current share of the overall portfolio for each asset class with the configured target share. If the actual share deviates from the target share by more than the configured **Minimum allowed deviation**, an alert is generated. This contains information about which asset class is over- or underweighted and what volume should be rebalanced.

The rebalancing is not executed automatically but serves as a notification for the user, who can manually execute the recommended transactions.

## Parameters
Rebalancing is configured at the top level (portfolio strategy). At the asset class and security levels, an individual weighting can additionally be defined.

### Configuration at Portfolio Level

| Parameter | Description |
|---|---|
| **Number of redeployments per year** | Specifies how often per year the portfolio is checked for deviations. Allowed values: 1 to 53. |
| **Minimum allowed deviation** | The threshold in percent at which a deviation from the target share is considered significant and an alert is triggered. Allowed values: 1% to 49%. |

### Configuration at Asset Class / Security Level

| Parameter | Description |
|---|---|
| **Weighting** | The target weighting of this asset class or security within the parent level in percent. Allowed values: 1% to 100%. |

{{% notice style="info" title="Note" %}}
Integration of rebalancing into the existing portfolio reports with a target/actual comparison display is planned for a future version.
{{% /notice %}}
