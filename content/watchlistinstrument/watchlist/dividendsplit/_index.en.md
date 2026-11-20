---
title: "Dividend/Split Feed view"
date: 2026-11-18T22:54:47+01:00
draft: false
weight: 30
archetype: "default"
---
This watchlist view contains information on **dividends** and **splits**.

## Functions
This **watchlist view** allows **multiple selection** of **instruments**. The **Remove selected instruments** function has been implemented to simplify the removal of multiple instruments.

## Expanding table row
An existing **expander symbol** on the instrument is an indication of existing **dividends** or **splits**. Further information under [Historical Data](../../externaldata/historyquote). The expanded row shows the dividends on the left and the splits on the right. If the instrument has dividends, the **dividend key figures** follow below the splits.

## Dividend key figures
The box **Dividend key figures** sums up the dividend history of the instrument. It is based on the dividends of the instrument as listed in the table on the left, not on the distributions you received in your portfolios. All amounts are adjusted for splits and stated in the currency of the instrument, which is shown in the title of the box. A dividend in another currency is converted with the exchange rate of the ex-date. Annual figures refer to calendar years. A figure whose input is missing is left out.

| Figure | Meaning |
|---|---|
| **Last ex-date**, **Last dividend** | The most recent dividend up to today. |
| **Next ex-date**, **Next dividend** | A future dividend that is already known. These lines only appear if the data source has delivered such a dividend. |
| **Dividends of the last 12 months** | Sum of the dividends with an ex-date in the last 365 days. |
| **Price** | The last price, to which the yields refer. |
| **Dividend yield TTM %** | Dividends of the last 12 months divided by the price. It corresponds to the function `DIVYIELD()` of the [indicators](../indicator/#benchmarks-and-dividends). |
| **Payments per year** | The distribution frequency of the instrument, provided it is regular. Otherwise GT counts the dividends of the last 365 days. |
| **Forward dividend yield %** | Last dividend times payments per year, divided by the price. It is a projection, not an announced dividend. |
| **Dividend growth 1Y**, **Dividend growth CAGR 3Y**, **5Y**, **10Y %** | Average annual growth of the annual dividend up to the last completed year, like the function `DIVGROWTH()`. The current year does not count. If the start year lies before the first dividend, the value is missing. |
| **Years without cut** | Completed years in a row with a positive annual dividend that is not lower than in the year before. The rule corresponds to `DIVYEARS()`, but has no upper limit of 20 years. |
| **Last cut in year**, **Last cut %** | The most recent completed year with a lower annual dividend than the year before, and its change. A value of -100 % means a suspended dividend. |
| **Chowder value (yield + CAGR 5Y)** | Dividend yield TTM plus dividend growth over five years. A common rule of thumb considers a value of 12 or more attractive, 8 or more for high-yield stocks such as utilities. |
| **Payout ratio %** | Dividends of the last 12 months divided by the earnings per share of the last 12 months. It only appears for direct equities for which GT has checked earnings per share from the [fundamental data](../indicator/#fundamental-data). |
| **First ex-date**, **Number of dividends** | Start and extent of the recorded dividend history. |

{{% notice style="note" %}}
The dividend table does not distinguish between regular and special dividends. A special dividend therefore raises the yield of the last 12 months and the growth, and in the following year it appears as a cut. If the exchange rate is missing for a dividend in another currency, the figures that need this year remain empty.
{{% /notice %}}

## Dividend chart
A click on the box **Dividend key figures** activates it. The menu **View** then offers the functions **Dividend chart** and **Add to dividend chart**. The two symbols at the top right of the box perform the same functions. The chart appears in the lower area of the application and remains when you collapse the row again.

### One instrument
**Dividend chart** shows the chart for the instrument of the box. The bars show the annual dividend: a year with a cut is red, the current and therefore incomplete year is light. The line shows the dividend yield TTM at each month end on the right axis. Splits appear as dashed vertical lines labelled only with their factor, for example 1:4. Moving the mouse over them also shows the date. A click on the legend entry **Split** hides the splits.

### Comparing several instruments
As long as the dividend chart is shown, **Add to dividend chart** adds the instrument of a further box, up to ten instruments. If no dividend chart is open yet, the function starts a new one. **Dividend chart** always starts again with the one instrument.

From two instruments on, the upper part of the chart shows the dividend yield TTM and the lower part the annual dividend as an index. The index is 100 in the base year. The base year is the first completed year in which all compared instruments paid dividends for a whole year; the year of the first dividend is considered possibly incomplete. Both measures have no unit, so instruments in different currencies can be compared without conversion. Comparing the amounts themselves would say little, because they depend on the price level and the number of shares. Each instrument has one legend entry that hides both of its lines together.
