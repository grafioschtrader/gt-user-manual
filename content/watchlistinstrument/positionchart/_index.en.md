---
title: "Position chart"
date: 2026-11-18T22:00:00+01:00
draft: false
weight: 31
archetype: "default"
---
The [line chart of the end-of-day prices](../eodchart/) shows how an **instrument** develops. The **position chart**, by contrast, shows how your **own position** in a **security** develops: how much money is in it, what it is worth, what it has earned so far and how far it has fallen in between. It is shown in the **additional area** and is available for securities including CFDs and Forex, but not for currency pairs. A select switches between four chart types: **Result**, **Value development**, **Performance** and **Drawdown**.

{{% callout info %}}
GT calculates every figure from your own transactions and the end-of-day prices of the security. Positions are valued at the **closing price**, so today appears only once its closing price is available. The header names the date of the last valued day.
{{% /callout %}}

## Activating this view
The position chart is opened with the entry **Position chart** in the context menu, which appears right after **Add to line chart** in every view with one security per table row, for example in the watchlist, in a portfolio or in a security account. The same entry is in the context menu of the line chart when it shows exactly one security. The other way round, the entry **EOD as line chart** in the position chart leads back to the line chart of the same security.

### Scope of the evaluation
The position chart evaluates exactly the scope it was opened from. Opened from a **security account**, only the transactions and holdings of that account count. Opened from a **portfolio**, the security accounts of that portfolio count, and from the **watchlist** or a view of the client all security accounts count. The same security can therefore show different curves depending on where it was opened. That is why the header above the chart names the security, the scope, the holding phase and the date up to which it is valued.

## Holding phase
A **holding phase** starts on the first day that ends with units in the holdings after a day without units. It ends on the next day whose holdings end at zero. What counts is always the holdings at the end of the business day. A full sale and a repurchase on the same day therefore remain one phase, while a purchase and a sale of all units on the same day form no phase. A dividend paid only after the last sale still belongs to the ended phase.

The position chart always shows exactly one holding phase, because cost, return and drawdown start anew with every phase. The most recent phase is preselected: the open one or, when none is open, the last closed one. When there are several phases, the select **Holding phase** appears, in which every earlier phase can be opened on its own.

{{% callout note "Transfer between security accounts" %}}
GT books a transfer between two security accounts as a sale in one and a purchase in the other at the transfer price. When the position chart is opened from a single security account, the phase ends in the giving account and starts anew in the receiving one. When both accounts belong to the evaluated scope, it remains one phase. Invested capital, cost of all purchases and realized result then restart at the transfer value, as the transfer is booked. The drawdown is not affected, and a note appears below the chart.
{{% /callout %}}

## Selection in the header
The header contains four selects in two rows:
- **Chart type** chooses the chart. GT remembers the last choice; on the first call it is **Result**.
- **Basis** appears for **Result** and **Performance** and determines whether only the **Open position** or the result **Total since entry** is shown.
- **Holding phase** appears as soon as there is more than one holding phase.
- **Currency** appears when the currency of the security differs from the main currency of the scope and switches between **main currency** and **security currency**.

## Chart types
### Result
The **Result** shows the gain or loss of the position in money as an area around the zero line, green for a gain and red for a loss. With the basis **Open position** it is the value less the invested capital, that is the unrealized gain or loss. With **Total since entry** the income and the realized result from sales are added, so the curve shows everything the phase has earned so far.

Because the money put in is taken out, purchases and sales cause no jumps. A price move of three percent shows as the corresponding amount regardless of the size of the position and fills the full height of the chart. This makes the chart type particularly suitable for staggered purchases and savings plans.

### Value development
The **Value development** sets the **Value** of the position against the **Invested capital**. The invested capital is the cost of the units held including the transaction costs and taxes of the purchases and is drawn as a step line. The area between the two lines is the unrealized gain or loss. The last value and the last invested capital are shown as labels at the right edge. The legend can additionally show the lines **Value incl. income and realized result** and **Cost of all purchases**.

Every purchase raises this curve by its full amount. With staggered purchases it therefore rises in steps, and the actual price movement is small in comparison. The chart type **Result** is better suited for judging what the position has earned.

### Performance
The **Performance** shows the same as the result, but in percent and also as a green or red area. The basis determines what the percentage refers to:
- **Open position**: (value − invested capital) / invested capital. The percentage shows how the units still held on a given day stand against their cost. A dotted line shows the **Price change** since the start of the phase for comparison.
- **Total since entry**: (value − invested capital + income + realized result) / cost of all purchases of the phase. This figure does not depend on the [cost basis method]({{% relref "/reportportfolio/costbasismethod" %}}).

On the day of a full sale the open position has no cost left. That day therefore appears only with the basis **Total since entry**.

### Drawdown
The **Drawdown** shows how far the money in the position lies below its own previous high. A purchase is new money and raises this high by its full amount. A sale or a distribution takes part of the position out and lowers the high in the same proportion, so a sale at the current price does not change the drawdown. A full sale ends with its result against the high and not with −100 %. A dotted line shows the **Price decline** from the highest closing price of the phase for comparison. The distance between the two lines is the effect of your own purchases.

{{% callout note "In profit and still in drawdown" %}}
Performance and drawdown measure different things. The performance measures the distance to the **cost**, the drawdown the distance to the previous **high** of the position. A position can therefore be in profit and below its high at the same time. For a security that moves sideways the drawdown is almost always red, because only a new high brings it back to zero.
{{% /callout %}}

Below the chart GT summarizes the drawdown. The **Maximum drawdown** is shown together with the **High before the drawdown**, the **Low of the drawdown**, the day the **High regained** and the **Duration (days)** of this episode. The longest time below the high is often a different episode than the deepest drawdown, because a shallow decline can last much longer. In that case the **Maximum drawdown duration (days)** appears in addition with its own dates. Last comes the **Current drawdown** or, for a closed phase, the **Drawdown at sale**.

## Events and tooltip
Purchases, sales, transfers, income and finance costs appear in every chart type as small markers at the bottom, labelled with a letter: **B** for purchase, **S** for sale, **T** for transfer, **I** for income and **F** for finance cost. Several events of the same kind on one day share one marker. Each kind can be hidden in the legend, which calms the curve for savings plans with many purchases. The tooltip of a marker names units and amount.

The tooltip of the curve shows for the chosen date the **Value**, the **Invested capital**, the **Open result**, the **Realized result**, the **Income**, the **Total result**, the percentage of the chosen basis and the drawdown. This lets you retrace, for example, a jump on the day of a partial sale.

## Section and scaling
The chart always shows the whole holding phase. The time slider below the chart or dragging with the mouse enlarges a section. The y-axis follows the visible section, so even a short period uses the full height. For result, performance and drawdown the zero line remains visible. A double click into the chart resets the section.

The section changes no figures. Percentages and the drawdown summary always refer to the whole phase, which is why the axis of the performance is labelled **% since start of phase**.

## Dividends, income and costs
**Income** comprises dividends and interest as well as accrued interest paid and received for bonds. Whether it counts gross or net of withholding tax follows the setting of the client, as in the other reports. Income and results are booked on the **payment date**.

| Chart type | Open position | Total since entry |
|---|---|---|
| Result | without income | with income and realized result |
| Performance | without income | with income and realized result, relative to all purchases of the phase |
| Value development | The value line contains no income; the line **Value incl. income and realized result**, which can be shown, does. | |

For the drawdown a dividend is an outflow from the position and lowers the high in the same proportion. When an **ex-date** is recorded for the dividend, this outflow takes effect on the ex-date, on which the price also falls by the dividend; the price drop therefore does not count as a drawdown. Without an ex-date the outflow takes effect only on the payment date, and between ex-date and payment the drawdown shows a temporary dip of the size of the dividend. When withholding tax is deducted, the price falls by the gross dividend, but only the net amount counts as outflow. The tax withheld therefore remains as a small drawdown.

## CFDs and Forex
For CFDs and Forex the **invested capital** is the **Entry value** of the open positions, that is the value of the units still open at the opening price without costs, and the **value** is entry value plus open result. For a long position the value thus equals units times value per point times price; for a short position it rises when the price falls. Long and short positions open in the same security account at the same time are valued individually. The costs of opening and closing count towards the realized result, finance costs as negative income.

The chart type **Value development** is not offered for CFDs and Forex. The entry value is the whole exposure, and the movements of the result would not be readable next to it. The performance refers to this exposure and not to the margin deposited with the broker, which GT does not know. Finance costs flow into the position without raising its value and therefore deepen the drawdown.

## Main currency
In the main currency the value is converted at each day's closing rate of the currency, the invested capital at the rate of each purchase. When the cash account is in the main currency, the booked rate applies, otherwise the closing rate of the business day. The realized result of a sale is the proceeds at the rate of the sale less the cost at the rates of the purchases. The curves in the main currency therefore contain the currency result and can drift apart even when the price of the security stands still.

## Notes below the chart
Missing prices or exchange rates are never drawn as zero. Instead a note appears below the chart:
- When the security was never held overnight in the evaluated scope, a note replaces the chart.
- When the price is missing on a trading day of the security's stock exchange, that day is left out. A holiday of the exchange, by contrast, is an expected pause and causes no note.
- When exchange rates are missing, the dependent main currency figures stay empty while the figures in the security currency are complete.
- Days with negative holdings have no value and are left out.
- A transfer between security accounts of the evaluated scope is shown as described above.

## Limitations
The position chart shows one holding phase after the other; several phases on the same curve are not offered. A comparison with an index, a monthly overview and an internal rate of return of the phase are not included. Percentages always refer to the whole phase; a time window with its own base, such as year to date, is not calculated.
