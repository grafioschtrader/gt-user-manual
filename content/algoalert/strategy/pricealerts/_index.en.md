---
title: "Price Alerts"
date: 2026-09-26T12:00:00+02:00
draft: false
weight: 20
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress - Target version V0.38.0" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
Price alerts are checked on fresh intraday observations and through the configurable background schedule. Period-change alerts also require a historical reference quote. The [alert evaluation rules](../../alert/) explain exchange hours, weekends and missing data. They are evaluated only in the [portfolio monitoring](../../algo/#portfolio-monitoring) of your own portfolio and never in a simulation, because an alert only reports that a condition has occurred and states neither the direction nor the quantity of an order. Why this is so and which strategy trades in a simulation instead is explained in [Where each strategy takes effect](../../#where-each-strategy-takes-effect).

## Absolute price gain/lose
This strategy monitors the current price of a security against a lower and upper price limit. If the price falls below the lower limit or exceeds the upper limit, an alert is triggered. This alert type is exclusively available at the security level and is suitable for monitoring fixed price thresholds, for example for buy prices or profit targets.

| Parameter | Description |
|---|---|
| **Lower limit (L)** | The lower price limit, from 0. An alert is triggered when the price falls below this value. |
| **Upper limit (U)** | The upper price limit, from 0. An alert is triggered when the price exceeds this value. |

**Example:** A security is traded at 100 CHF. With a lower limit of 90 CHF and an upper limit of 120 CHF, the user is notified as soon as the price falls below 90 CHF or rises above 120 CHF.

## Holdings gain/lose
This strategy monitors the percentage gain or loss of a held position relative to its cost basis. The alert is triggered when the gain or loss exceeds the configured percentage value. If the security is not held, no evaluation takes place. In addition, price limits can be set as for the absolute alert; they are evaluated only while the position is held. At least one of the four fields must be filled in. This strategy is available on all three levels, that is on the portfolio based strategy, the asset class and the security. On the portfolio based strategy it applies to the securities of its watchlist, on an asset class to the securities of that asset class.

| Parameter | Description |
|---|---|
| **Gain (G)** | Gain threshold in percent (1-500%). An alert is triggered when exceeded. |
| **Lose (L)** | Loss threshold in percent (1-500%). An alert is triggered when exceeded. |
| **Upper limit (U)** | Price limit from 0. An alert arises when the price crosses it upwards. |
| **Lower limit (L)** | Price limit from 0. An alert arises when the price crosses it downwards. |

**Example:** A position was acquired at a cost basis of 50 CHF. With a gain threshold of 20% and a loss threshold of 10%, the user is notified when the position value rises by more than 20% or falls by more than 10%.

## Gain/loss in a period
This strategy monitors the price change of a security over a defined time period. The alert is triggered when the price rises or falls by more than the configured percentage within the specified number of days. The reference is the last closing price on or before the day the period points back to; the period counts calendar days. This strategy is exclusively available at the security level.

| Parameter | Description |
|---|---|
| **Period (P)** | Number of days for the observation period (1-999 days). |
| **Gain (G)** | Gain threshold in percent (1-500%). |
| **Lose (L)** | Loss threshold in percent (1-500%). |

**Example:** With a period of 30 days, a gain of 15% and a loss of 10%, the user is notified when the price has moved more than 15% upward or 10% downward within 30 days.
