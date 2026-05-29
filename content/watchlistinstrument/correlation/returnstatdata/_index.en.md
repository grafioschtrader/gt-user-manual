---
title: "Return and statistical data"
date: 2026-05-13T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
This evaluation contains the **annual return** and several **statistical metrics** for a **single instrument**. It is shown when a row in the [correlation matrix]({{< relref "/watchlistinstrument/correlation" >}}) is expanded.

- **Period**: The **return** is computed over the entire span of available end-of-day prices. The period for the **statistical metrics** can be narrowed via the date fields of the correlation matrix.
- **Client and instrument currency**: The currency of the instrument may differ from the client's currency (the main currency). The results are therefore presented in two columns — once in the instrument's currency and once in the main currency. For currency pairs and Forex the main-currency column is not produced.

## Return
The calculations are based on the last available closing prices within each year — ideally from December 31 of the previous year to December 31 of the year under evaluation. If **dividends** are available for the instrument they are added in, with the **ex-date** determining the year allocation. When the instrument currency differs from the main currency, GT uses historical exchange rates to produce the second column in the main currency.

#### Return per calendar year
The return is computed for every fully available calendar year and for the current year. The result is a percentage total performance that reflects both the price change and any dividends received.

#### Annualized return
The annualized return condenses the individual yearly returns via the **geometric mean** (Compound Annual Growth Rate). GT computes it for the standard horizons of **1, 3, 5 and 10 years**, plus an additional entry covering the **entire available period** if that is longer than 10 years. The current year is excluded so an incomplete year does not distort the multi-year values.

## Statistical metrics
The hierarchy table groups the statistical metrics by **sampling period** (daily, monthly, annual). Each sampling period exposes one or more properties, presented once in the instrument currency and once in the main currency.

- **Standard deviation** — dispersion of the percentage changes. It is the basis for a volatility assessment and is derived from daily, monthly and annual values respectively.
- **Minimum** and **Maximum** — the smallest and largest daily percentage change in the observation window. These two values are only reported on a daily basis.

{{% notice style="info" title="Annual standard deviation" %}}
The annual standard deviation is not computed directly from yearly returns. Instead, it is scaled up from the monthly standard deviation using the factor √12. This makes it possible to annualize the volatility meaningfully even when only a few complete years are available.
{{% /notice %}}

{{% notice style="note" title="Effect of the date restriction" %}}
The date fields of the correlation matrix affect every statistical metric — including the Sharpe ratio described in the next section. The return calculation above continues to use the entire span of available closing prices regardless.
{{% /notice %}}

## Sharpe ratio
The **Sharpe ratio** is a risk-adjusted metric that compares **excess return** against **volatility**. A higher Sharpe ratio means that the instrument has delivered more return above the risk-free rate per unit of risk taken.

```
Sharpe = (annualized return − avg risk-free rate) ÷ annualized standard deviation
```

The metric is shown per horizon in the **Annualized return** part of the hierarchy table — once in the instrument currency and once in the main currency. When both currencies coincide the two values are identical. When the instrument currency differs, the main-currency variant uses the risk-free rate of the **client's main currency**, whereas the instrument-currency variant uses the rate of the **instrument's currency**.

The average risk-free rate per horizon is the arithmetic mean of the daily rate values in the window `[today − number of years, today]`. If the window contains no data point at all, GT falls back to the **last known** rate before the end date (carry-forward).

{{% notice style="warning" title="Prerequisite: Risk-free rate mapping" %}}
The calculation needs a valid row in the [Risk-free rate mapping]({{< relref "/basedata/riskfreeratemapping" >}}) for each currency involved. If a mapping is missing — or if the linked security has no historical closing prices yet — the Sharpe ratio remains empty in the corresponding column.
{{% /notice %}}

{{% notice style="info" title="Current simplification" %}}
All horizons currently share the same annualized standard deviation — the value shown one row above in the statistics table. A horizon-specific standard deviation would require an extra database query per horizon and is planned as a future refinement.
{{% /notice %}}

The Sharpe ratio also stays empty when the annualized standard deviation is zero or non-finite — for example because the closing prices are constant or too few data points are available for a meaningful dispersion calculation.

{{% notice style="note" title="Only one consumer today" %}}
At present the Sharpe ratio is shown only here. Further evaluations that consume the risk-free rate are conceivable but have not yet been implemented.
{{% /notice %}}
