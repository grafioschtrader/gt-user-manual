---
title: "Price Alerts"
date: 2026-02-12T12:00:00+01:00
draft: false
weight: 20
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
Price alerts are simple threshold-based strategies evaluated after each update of intraday price data (Tier 1). They do not require historical price data and can be configured directly via dynamic form fields.

## Absolut Price Gain/Lose Alert
This strategy monitors the current price of a security against a lower and upper price limit. If the price falls below the lower limit or exceeds the upper limit, an alert is triggered. This alert type is exclusively available at the security level and is suitable for monitoring fixed price thresholds, for example for buy prices or profit targets.

| Parameter | Description |
|---|---|
| **Lower limit** | The lower price limit. An alert is triggered when the price falls below this value. |
| **Upper limit** | The upper price limit. An alert is triggered when the price exceeds this value. |

**Example:** A security is traded at 100 CHF. With a lower limit of 90 CHF and an upper limit of 120 CHF, the user is notified as soon as the price falls below 90 CHF or rises above 120 CHF.

## Holdings Percentage Gain/Lose Price Alert
This strategy monitors the percentage gain or loss of a held position. The alert is triggered when the gain or loss exceeds the configured percentage value. This strategy is available at all levels (portfolio, asset class, security).

| Parameter | Description |
|---|---|
| **Gain** | Gain threshold in percent (1-500%). An alert is triggered when exceeded. |
| **Lose** | Loss threshold in percent (1-500%). An alert is triggered when exceeded. |

**Example:** A position was acquired at a cost basis of 50 CHF. With a gain threshold of 20% and a loss threshold of 10%, the user is notified when the position value rises by more than 20% or falls by more than 10%.

## Gain/Loss Warning in a Period
This strategy monitors the price change of a security over a defined time period. The alert is triggered when the price rises or falls by more than the configured percentage within the specified number of days. This strategy is available at all levels.

| Parameter | Description |
|---|---|
| **Period** | Number of days for the observation period (1-999 days). |
| **Gain** | Gain threshold in percent (1-500%). |
| **Lose** | Loss threshold in percent (1-500%). |

**Example:** With a period of 30 days, a gain of 15% and a loss of 10%, the user is notified when the price has moved more than 15% upward or 10% downward within 30 days.
