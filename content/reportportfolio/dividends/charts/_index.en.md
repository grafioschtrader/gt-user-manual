---
title: "Charts"
date: 2026-10-02T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
The [Dividends and interest]({{% relref "/reportportfolio/dividends" %}}) report comes with six charts that present the income and costs of the table graphically. They show in which months distributions arrive, how income and costs develop over the years and from which asset classes the income originates. Individual securities are deliberately not broken down, since a chart with many securities would become unreadable; the **Securities Detail View** of the table serves this purpose.
## Opening and operating the charts
Via the **View** menu or the context menu of the report, **Show charts** opens the charts in the [Additional Area]({{% relref "/intro/userinterface" %}}). Above the chart, **Chart type** selects one of the six charts; the selection is stored in the browser and restored the next time.
Chart and table work together. Three of the charts refer to a single year, which is shown in the chart title. You choose this year by clicking a year row in the main table; a note below the chart type reminds you of this. As long as you have not clicked a row, the chart shows the most recent year with distributions. If you restrict the report to certain security accounts and cash accounts via **Evaluation of depots**, the selection also applies to the charts and they are reloaded. The same happens when you change a transaction in the table.
As with all charts in GT, clicking an entry of the legend hides or shows the corresponding data series, and hovering with the mouse shows the exact amounts. If a selected year has no income, the note **No income in this year** appears instead of an empty chart.
## What the charts count
All amounts are stated in the tenant's main currency and are calculated from the same transactions and with the same exchange rates as the table. The yearly totals of the charts therefore match the main table: net dividends and net interest together make up the column **Received Div/Interest**, **Account interest** corresponds to the column of the same name, and the finance costs of CFD and Forex together make up the column **Margin finance cost**.
The charts distinguish between **dividends** and **interest**. Interest means the distributions of instruments in the asset class categories **Fixed income** and **Convertible Bond**. This applies to directly held bonds as well as to bond funds and bond ETFs. All other distributions, for example from shares, equity ETFs or real estate funds, count as dividends. What matters is therefore the [asset class]({{% relref "/basedata/instrumentbased/assetclass" %}}) of the instrument.
Income is shown **net**, that is with the amount actually credited to the cash account. The withholding tax deducted from the distribution appears as a lighter segment in the color of the income directly above it. Net amount and withholding tax together make up the gross amount. Income is assigned to the month of its booking date, not to the **Ex-Date**.
{{% notice style="note" title="Withholding tax on account interest" %}}
The withholding tax in the charts only covers the distributions of securities. Tax deducted from account interest is not included. The total withholding tax in the charts can therefore be lower than the column **Taken tax** of the main table.
{{% /notice %}}
## The charts
### Income per month
For the selected year, this chart shows the distributions of the twelve months as stacked bars: **Dividends net** with the **Withholding tax on dividends** on top, followed by **Interest net** and the **Withholding tax on interest**. This shows in which months a lot of money arrives and in which hardly anything, which helps for example when planning withdrawals. **Account interest** is hidden at first and can be switched on via the legend.
### Income and costs per year
Here all years stand side by side. The income, that is dividends, interest, account interest and the related withholding tax, grows upwards from the zero line. The costs, that is **Finance costs CFD** and **Finance costs Forex**, grow downwards; negative account interest also appears below the zero line. The **Net income** line connects, for every year, the sum of net dividends, net interest, account interest and finance costs. This shows at a glance whether income grows from year to year and how much the finance costs of margin positions reduce it.
{{% notice style="info" title="Account/Depot cost" %}}
The **Account/Depot cost** is hidden in the chart at first and can be switched on via the legend. It is not included in the **Net income**, because it arises independently of the income.
{{% /notice %}}
### Distributions by asset class per year
This chart shows the net distributions of every year as stacked bars, split by asset class. Each asset class is labelled with its category, subcategory and instrument type. This lets you follow how the origin of the income shifts over the years, for example from shares towards bonds.
### Share of the asset classes in the year
For the selected year, the ring chart shows the percentage share of each asset class in the net distributions. It answers the question how strongly the income of a year depends on individual asset classes.
{{% notice style="note" title="Limits of the asset class charts" %}}
To keep the colors distinguishable, at most seven asset classes are shown individually, namely those with the largest distributions. All others are combined under **Other asset classes**. In addition, the ring chart cannot show negative shares; an asset class with a negative total is missing from it.
{{% /notice %}}
### Income year × month
This overview shows every year as a row and every month as a column. The color of a cell stands for the sum of net dividends, interest and account interest in that month: the darker the blue, the higher the income. Hovering over a cell additionally shows the deducted withholding tax. This makes recurring distribution months and the growth of income over many years visible at the same time. Finance costs are not included in this view, and a month with negative income appears as light as a month without income.
### Cumulative income compared with previous years
For every year with income, a line shows how the sum of net dividends, interest and account interest builds up from January to December. The selected year is highlighted with a strong blue line, the previous year in orange; all other years appear in gray so that the comparison remains readable even with many years. If the line of the current year lies above that of the previous year, you have received more up to that month than in the previous year. Finance costs are not included in this view.
