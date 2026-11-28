---
title: "Performance by security"
date: 2026-11-24T18:00:00+01:00
draft: false
weight: 22
archetype: "default"
---
The [period performance](../periodperformance/) answers how a portfolio or the whole tenant developed over a period. The **Performance by security** view breaks the same period down by security. For every security held or traded in the period it shows two figures: the **Profit period** in the main currency, where the money was earned, and the **Time-weighted return** of the position in percent, how well the security ran while you held it.

## Access
At **tenant level** the view is the tab **Performance by security** beside **Performance** in the **Period performance**; amounts are in the tenant currency. Within a **portfolio** the tab **Performance by security** follows directly after **Period performance**; amounts are in the portfolio currency. At tenant level a security held in several portfolios is combined into one row.

## Period
**Date from** and **Date to** work as in the period performance: both dates refer to the close of trading, the day of the start date is the base of comparison and not part of the period. Only days on which every held security has a price can be chosen. The two views share the period: a start date chosen in one view is already entered in the other. When the period ends on the latest available trading day, the end date moves on to a newer trading day the next time the view is opened. The calculation starts only with **Apply**.

## The table
Each row is a security. The table is sorted by **Profit period**, descending; a click on a column header sorts by that column, for example by the return. Above the table the calculated period is shown, so it stays clear what the result refers to even after the dates in the form were changed. Amounts in the main currency carry its code in the column header. Some columns are hidden at first and can be shown through the column selection of the table.

| Column | Meaning |
|---|---|
| **Name**, **ISIN**, **Currency** | The security and the currency it is traded in. |
| **Days held** | Calendar days within the period on which the security was held at the close. When it was held throughout, the number equals the length of the period. A security bought and sold on the same day has zero days held. |
| **Units at start**, **Units at end** | Units held on the start or end date as traded. A split changes the units, not the value. |
| **Value at start**, **Value at end** | Value of the position at the closing price of the start or end date, converted with the closing exchange rate of the same day. |
| **Invested** | Purchases of the period including their cost. |
| **Returned** | Sales and income of the period after cost. |
| **Profit period in security currency** | The profit of the period in the currency of the security. |
| **Profit period** | Value at end minus value at start minus invested plus returned. It contains price gain, income, transaction cost and the currency result of the position. |
| **Time-weighted return in security currency** | The return of the position in the currency of the security, without the exchange rate movement. |
| **Time-weighted return** | The return of the position in the main currency over the days it was held, measured from close to close. |
| **Remark** | The reason when a figure of the row is missing, see below. |

The footer **Grand total** sums the main currency amounts of all rows, independent of a table filter. When at least one row lacks its profit, it reads **Partial total**, because those rows are not included. Expanding a row shows the transactions of the security up to the end date.

## Charts
**Show** → **Show Chart** opens the charts of the calculated period in the lower display area. The **Chart type** chooses between **Return against profit** and **Treemap of profits and losses**; the choice is remembered. After a new calculation the chart shows its result. When you switch to another tab the chart stays; when you return, the table shows the last calculation again as well. Only the view of another portfolio or of the client empties it. A click on a security in the chart expands its row in the table.

### Return against profit
Every security is a point: horizontally the **Time-weighted return**, vertically the **Profit period** in the main currency. The two zero lines divide the chart into four fields:
- **top right**: performed well and earned money;
- **bottom right**: good return but little earned, usually a small or briefly held position;
- **top left**: weak return but much earned, because much capital was invested;
- **bottom left**: loss.

The more days a security was held, the larger its point. A small point with a high return is therefore recognisable as a briefly held position. A loss is red. Hovering over a point shows name, profit, return and days held. Securities without return or profit, such as margin instruments, are missing from this chart; their number is shown below its title.

### Treemap of profits and losses
The treemap shows where the profit came from and where money was lost. It has two branches, **Profits** in green and **Losses** in red. The area of a tile is the profit or loss of the security in the main currency, independent of its return. The two branches stand in the ratio of their sums: a small total loss next to a large total profit is small as well. A click on a branch enlarges it, so small tiles become readable; the bar above leads back. Below the title stand the sum of the profits, the sum of the losses and the balance, which equals the grand total of the table. Margin instruments are included. Securities without profit or with a profit of zero have no area; their number is shown below the title.

## Profit and return
An example: a security was worth 10'000 on the start date. During the period further units were bought for 5'000, units were sold for 3'000 and a dividend of 200 was credited. On the end date the position is worth 13'000. The **Profit period** is 13'000 − 10'000 − 5'000 + 3'200 = 1'200.

The **Time-weighted return**, by contrast, measures how the security itself performed, independent of how much of it was held when. It links the returns of the individual days. A purchase counts from the previous close, a sale and income at the close of the day. Spans in which the security was not held do not count. When a position was sold and bought again later, the return links both holding phases.

The return in the main currency contains the exchange rate movement, the one in the security currency does not. The difference between the two columns therefore shows how much the currency changed the return.

## Comparison with the benchmark
The field **Benchmark instrument** shows the stored [benchmark]({{% relref "/reportportfolio/periodperformance" %}}#comparison-with-a-benchmark) of the portfolio or the client. The button in the field chooses another benchmark for the next calculation only, and the button **Save as default** behind it stores it, as in the period performance, as default of the portfolio or the client. **Hide benchmark for this report** in the **View** menu calculates without a comparison.

With a benchmark the columns **Benchmark return** and **Difference to benchmark (pp)** are added. The benchmark is linked over exactly the days on which the position held capital. When a position was sold in March and bought again in October, the move of the benchmark in between does not count. If the comparison period of the benchmark does not cover a day of holding, both columns of the row stay empty, and the tooltip of the cell names the reason. In the chart **Return against profit** the tooltip of a point shows the benchmark return as well.

## Margin instruments
For CFD and Forex the value of the position is the open profit or loss, not invested capital. A return on it would be meaningless, so these rows show the profit only. For them **Invested** contains the settled losses and cost, **Returned** the settled gains. The profit period is the change of the open result plus what was settled during the period.

## Remarks
A figure that cannot be determined stays blank and is never shown as zero. The column **Remark** names the reason:
- **No price at the start or end of the period**: the security was held on the start or end date, but the price or the exchange rate is missing. Values, profit and return stay blank.
- **Exchange rate missing for a booking**: a purchase, sale or income of the period lacks the exchange rate into the main currency. The main currency amounts and the return in the main currency stay blank; profit and return in the security currency are still shown.
- **Negative units, no return**: on a day of the period the units were negative. The profit is shown.
- **Not held at any close, no return**: there was no invested capital in the period, for example because only a dividend was credited after the sale. That dividend counts towards the profit.
- **Margin instrument, gain only**: see above.

## Limitations
- Start and end are valued with closing prices and closing exchange rates. When the period ends on the latest trading day, the profit can therefore differ from the view [Security accounts](../securityaccountreport/), which uses the latest intraday price.
- Purchases, sales and income in a foreign currency are converted with the closing exchange rate of their day, not with the rate of the booking. The profit period in the main currency is therefore not the difference of the gain and currency gain columns of the view **Security accounts** on the two dates, which converts the gain over the whole holding period at the rate of the report date and shows the rest as currency gain.
- The return measures the security over the days it was held. A position held for only three days can therefore show a high return; the days held are shown beside it.
- Because a purchase counts from the previous close, a large purchase on a day with a large price move noticeably shifts the return of that day. The same applies to a span across a day without a price.
- The return magnifies inconsistent data: a trade booked far from the closing price of its day, or a split that the price history does not reflect, produces an implausible return, while the profit shows the same deviation as the view **Security accounts**.
- Profit and return are after transaction cost and, for dividends, before or after withholding tax according to the setting of the client. Account and custody fees are not attributed to a security.
- A dividend counts on its booking date, not on its ex-date. The price drop and the income can therefore fall on different days.
- Income received while the security is not held, for example a dividend paid after the sale, counts towards the profit but not towards the return.
- Finance cost of a CFD or Forex position also counts on its booking date. When it is booked for the whole holding period at the close, the share of earlier periods also falls into the period of that booking.
- An ISIN change, a spin-off or a rights issue booked as a sale and a purchase appears as two securities.
- The profits of the rows do not add up to the result of the period performance, which also contains interest, fees and the currency result of the cash accounts.
- A portfolio whose trades were all opened and closed on the same day has no holding and therefore offers no selectable period.
