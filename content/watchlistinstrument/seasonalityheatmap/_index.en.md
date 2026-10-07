---
title: "Seasonality heat map"
date: 2026-06-24T10:00:00+02:00
draft: false
weight: 35
archetype: "default"
---
The **seasonality heat map** presents the returns of an **instrument** across several years as a colored table. Each row stands for a calendar year and each column for a month or a quarter. A cell is the greener the more positive the return of that period was, and the redder the more negative it was. This makes recurring patterns over the course of the year visible, such as a traditionally weak summer phase or a year-end rally. The view is available for **securities**, **derived securities** and **currency pairs**, provided a sufficiently long price history exists.

{{% notice info %}}
The calculation is based on the **closing prices** of the historical price data. For each month the last available trading day is used; a quarterly or annual value is derived from the last monthly value of the respective period.
{{% /notice %}}

## Activating this view
The seasonality heat map appears in the **additional area**, that is, in the same place as the [end-of-day chart](../eodchart/) and the end-of-day data as a table. It is opened from the **context menu** of an instrument via the entry **Seasonality** and thereby replaces whichever view was previously shown in the additional area. As with the chart, the entry is available wherever a table contains one row per instrument, in practice mainly in the watchlist.

## Settings
Three inputs above the table recalculate the heat map on every change.

- **Period**: Choice between **Monthly** and **Quarterly**. The monthly display contains twelve columns, the quarterly one four.
- **Include dividends**: When this option is selected, dividends and interest are included in the return. A distribution is attributed to the month or quarter of its ex-date. The option can only be selected when the instrument actually pays dividends or interest.
- **In main currency**: This converts the returns into the **client's main currency**. The option is not available for currency pairs, nor when the instrument is already held in the main currency.

## Annual return and footer
As its last column the table shows the **Annual return** of the respective year. The footer reports three figures per column across all years: the **Average**, the **Median** and **Positive %**, that is, the share of years in which the period in question closed positively. **Positive %** in particular is one of the most meaningful figures of a seasonality analysis, as it shows how reliably a pattern has repeated over the years.

{{% notice note "Coloring is instrument-specific" %}}
The coloring follows the distribution of the returns **within the instrument under consideration**: the strongest loss is shown fully red, the strongest gain fully green, and the remaining values lie in between. The annual-return column is colored independently, since annual values are naturally larger than monthly or quarterly ones. For this reason the colors are **not comparable between different instruments** – the same color represents different percentage values for a calm instrument and for a highly volatile one.
{{% /notice %}}

{{% notice warning %}}
### Price or total return
As mentioned in the [glossary](../../glossar/), a distinction must be made between **distributing** and **accumulating** ETFs, and between **price** and **performance indices**; the same applies to **shares** with or without **dividends**. By default the heat map is based on the plain **closing prices**. The **Include dividends** option lets you add the distributions to the return, as far as corresponding data is available for the instrument. The user should therefore always be aware of which kind of return is currently being viewed.
{{% /notice %}}

{{% notice info %}}
Retrieving the price data for this view counts toward the daily quota of different instruments, see [Daily quota for retrieving historical price data](../externaldata/historyquote/pricedata/).
{{% /notice %}}
