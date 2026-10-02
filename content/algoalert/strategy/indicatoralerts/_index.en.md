---
title: "Indicator Alerts"
date: 2026-09-26T12:00:00+02:00
draft: false
weight: 30
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress - Target version V0.38.0" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
Indicator alerts use technical indicators calculated from historical prices. They share the configurable background schedule and can also be checked on fresh intraday observations. The [alert evaluation rules](../../alert/) explain timing and data requirements. They are evaluated only in the [portfolio monitoring](../../algo/#portfolio-monitoring) of your own portfolio and never in a simulation, because an alert only reports that a condition has occurred and states neither the direction nor the quantity of an order. Why this is so and which strategy trades in a simulation instead is explained in [Where each strategy takes effect](../../#where-each-strategy-takes-effect).

## Moving average crossing
This alert is triggered when the current price of a security crosses a moving average. The user configures the type of moving average, the calculation period and the crossing direction.

| Parameter | Description |
|---|---|
| **Indicator type (I)** | Type of moving average: SMA (Simple Moving Average) or EMA (Exponential Moving Average). The value is typed in as text, there is no selection list; exactly `SMA` or `EMA` in capital letters is accepted. |
| **Period (P)** | Number of trading days for calculating the moving average (1-999). |
| **Cross direction (C)** | The direction of the crossing that triggers the alert: ABOVE (price crosses from below to above) or BELOW (price crosses from above to below). This value is typed in as text too; exactly `ABOVE` or `BELOW` is accepted. |

**Example:** An alert with indicator type SMA, period 200 and cross direction BELOW notifies the user when the price falls below the 200-day SMA. This is a classic signal for a possible downtrend.

## RSI threshold
This alert is based on the Relative Strength Index (RSI), a momentum indicator with values between 0 and 100. The alert is triggered when the RSI falls below the lower threshold (oversold) or rises above the upper threshold (overbought). At least one of the two thresholds should be configured.

| Parameter | Description |
|---|---|
| **RSI period (R)** | Number of trading days for RSI calculation (1-999), typically 14. |
| **Lower threshold (L)** | RSI value from 0 to 100 below which an oversold alert is triggered (e.g. 30). |
| **Upper threshold (U)** | RSI value from 0 to 100 above which an overbought alert is triggered (e.g. 70). |

**Example:** With an RSI period of 14, a lower threshold of 30 and an upper threshold of 70, the user is notified when the RSI falls below 30 (possible buying opportunity) or rises above 70 (possible sell signal).

## Custom expression
This alert enables defining a free-form expression evaluated against current price data and technical indicators. The expression language is based on the EvalEx library and supports mathematical operators, comparisons and logical operations.

| Parameter | Description |
|---|---|
| **Expression (E)** | The expression to evaluate (maximum 500 characters). |

### Available Variables
The following intraday price data variables are available in the expression:
- `price` — Last traded price
- `prevClose` — Previous closing price
- `open` — Opening price
- `high` — Day high price
- `low` — Day low price
- `volume` — Trading volume

### Indicator Functions
Additionally, technical indicators can be used as functions. These are computed from historical closing prices:
- `SMA(period)` — Simple Moving Average, e.g. `SMA(200)`
- `EMA(period)` — Exponential Moving Average, e.g. `EMA(50)`
- `RSI(period)` — Relative Strength Index (0-100), e.g. `RSI(14)`

### Expression Examples
- `price < SMA(200)` — Price below the 200-day SMA
- `price < SMA(200) AND RSI(14) < 30` — Price below SMA and RSI in oversold range
- `EMA(50) > EMA(200)` — Golden cross (short-term EMA above long-term)
- `price > 100` — Simple price threshold

{{% notice style="info" title="Note" %}}
Indicator alerts require sufficient historical price data for calculation. Ensure that a historical price data connector is configured for the affected securities.
{{% /notice %}}
