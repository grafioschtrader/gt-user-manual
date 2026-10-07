---
title: "Security accounts"
date: 2026-10-05T22:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
The security accounts report provides a comprehensive overview of all security positions of a tenant. Using this report, various grouping views can be created to analyze the asset structure. Additionally, detailed information on holdings, prices, and gains is displayed for each security.

{{% notice style="info" title="Security accounts report on different levels" %}}
This report is available on three levels. For the whole tenant you find it in the tab "Securities accounts", which is described here. For a portfolio, click "Securities accounts" below the portfolio in the navigation tree; for a single security account, click the security account itself. If only one security account exists for a portfolio, the reports for the portfolio and for the security account deliver the same results.
{{% /notice %}}

## Functionality
The report evaluates all security positions of the tenant. All transactions up to the selected date are considered to ensure a point-in-time accurate presentation of holdings and gains. Positions are displayed in groups, with subtotals shown for each group and a grand total at the end.

## Grouping Options
The report offers various grouping options that can be selected via the dropdown menu next to the heading:

- **Grouped by currency**: Default view. Positions are grouped by their trading currency. For each currency, the exchange rate to the main currency is displayed. This provides a clear overview of currency exposures and facilitates the analysis of currency risks.
- **Grouped by asset class**: Positions are grouped by their asset class, for example equities, bonds, money market, commodities, or real estate. This enables asset allocation analysis.
- **Grouped by Financial instrument**: Grouping by financial instrument such as ETF, mutual fund, CFD, Forex or direct investment. This view is helpful for analyzing the investment instruments used.
- **Grouped by Sub-asset class**: Grouping by the sub-category of the asset class, which enables more detailed analysis within asset classes.
- **Grouped by combination**: Each combination of asset class, sub-category and financial instrument forms a group of its own. This is the finest grouping.

## Columns in the Report
Some columns are displayed in the security's currency, others in the main currency. Columns in the main currency carry its code as a suffix in the column header, for example "Gain security CHF". In the following list this is written as "(main currency)". The columns are the same in every grouping:
- **Name**: Designation of the security. The official name is used.
- **I**: Icon for the financial instrument. This symbol visualizes the type of instrument (e.g., stock, bond, ETF, currency pair) and facilitates quick identification in the table.
- **Holding**: Number of held units of the security. For currency pairs and short positions, this value can also be negative.
- **Currency**: The trading currency of the security.
- **Timestamp**: Time of the last available price. This information shows the basis for the valuation.
- **Price**: The price of the security on the date specified above. If no price is available for that day, the price from a previous date is displayed. A price shaded yellow was never traded but was calculated when closing gaps in the historical price data, see [Price freshness](../../watchlistinstrument/watchlist/pricefreshness/).
- **Gain security**: Profit or loss for this security in the trading currency. This value considers both realized and unrealized gains and losses. As it is shown in the instrument's trading currency, it contains no currency effect; that is in the **Forex gain** column. A subtotal exists only in the currency grouping, because amounts in different currencies cannot be added up.
- **Security risk (main currency)**: The risk exposure of the position in the main currency, that is the market value multiplied by the leverage factor. This column is particularly relevant for margin products and leveraged instruments, because for a CFD it shows the full market value of the position and not only its gain or loss. For example, you have a quantity of 10 of a double inverse ETF at a price of 100. This results in a negative security risk of 2,000. This helps you better assess the risk of your investments.
- **Levered Inverse**: There are leveraged ETFs. The factor is displayed here if it deviates from 1. A minus sign indicates that it is an inverse ETF.
- **Gain security (main currency)**: The profit or loss in the tenant's main currency. All values are automatically converted to the main currency.
- **Forex gain (main currency)**: The part of the result that arose purely from the movement of the exchange rate against the main currency. Together with **Gain security (main currency)** it gives the complete result of the position in the main currency. The value can be negative even though the instrument has risen. See [Forex gain]({{% ref "/reportportfolio#forex-gain" %}}).
- **Account relevant**: The amount that a sale at the reporting date would credit to the account, in the trading currency. For normal securities, this corresponds to the market value of the position. For CFD and Forex it is the unrealized gain or loss of the open positions, because only this amount would be booked on closing. A subtotal exists only in the currency grouping.
- **Account relevant (main currency)**: The same amount in the tenant's main currency. The grand total of this column is the total of the report.
- **Share %**: The share of the position in the total of the report, see the following section.
- **Disposal costs (main currency)**: Only visible when the disposal cost estimate is switched on. Estimated commission, transaction tax and currency conversion markup of selling the whole position at the reference date. A cell highlighted in yellow means that a part could not be estimated; the tooltip names the reason. See [Disposal costs]({{% ref "/reportportfolio#disposal-costs" %}}).
- **Value after disposal (main currency)**: **Account relevant (main currency)** less the estimated disposal costs. Also only visible when the estimate is switched on.

### Share %
The column **Share %** shows which part of the total a position, a group or a cash holding represents. It is based on the column **Account relevant (main currency)**: the value of the row is divided by the grand total of that column. The shares of all positions therefore always add up to 100%, and the grand total row shows 100.

A position with a negative value has a negative share. This mainly concerns CFD and Forex at a loss, but also a short position or an overdrawn cash holding. The remaining positions then add up to more than 100%. An example: you hold a security worth 10,000 and a CFD that is 5,000 in the red. The total is 5,000, the security has a share of 200% and the CFD one of −100%. The security is worth twice the net assets, because the loss of the CFD reduces them.

The column shows the share of the assets and not the risk. A CFD with a market value of 100,000 and a gain of 100 has only a small share, although it can move the assets considerably. The risk is shown in the column **Security risk**. If the total is zero or negative, the column stays empty, because shares of a negative total would have inverted signs and carry no meaning.

{{% notice style="tip" title="Total of the security accounts report" %}}
The security accounts report contains securities only, so **Share %** is the share of the securities here. The share of the entire assets including cash holdings is shown in the report [Asset Classes with Cash](../securitycashaccountreport/).
{{% /notice %}}

### Group Totals and Grand Totals
Subtotals are displayed for each group. For currency grouping, the group row additionally shows the exchange rate to the main currency. At the end of the report, the grand total appears across all groups. Negative amounts appear in red in all numeric columns.

## Filter Options
The report offers various filter options to customize the view:
- **To date**: The report can be set to a specific date, showing the portfolio view at a historical point in time. This is particularly useful for comparisons or year-end closings. Clicking the repeat icon next to the date field resets the date to today.
- **Show closed positions**: Via the context menu, you can choose whether already closed positions should also be displayed in the report. A checkmark shows that the option is active. By default, only open positions are shown.

## Context Menu
The context menu always contains the entries **Turn on/off columns...**, **Show Chart**, **Show closed positions** and **PDF report...**. **Turn on/off columns...** shows or hides individual columns; the selection is saved.

### Context Menu on Selected Instrument
When a security is selected, most functions that are also available in the watchlist are added, see [Functions for Selected Instrument](../../watchlistinstrument/watchlist/#funktionen-markierte-watchlist-ansicht). In addition, **Buy**, **Sell** and **Interest/Dividends...** record a transaction for this security directly. **Sell** is only possible with an existing holding, and **Interest/Dividends...** is missing for CFD and Forex.

## Expandable Rows
Each position can be expanded by clicking the expander icon. The expanded view displays all **transactions** for this security. This enables detailed tracking of all purchases, sales, dividends, and other transactions that led to the current position. For CFD and Forex the transactions appear as a tree: below each opening stand the transactions with which it was fully or partly closed.

### Context Menu on Transactions
At this point, individual existing transactions can also be edited. For more information, see [Security Transactions](../../../transaction/security/).

## Chart View
**Show Chart** in the **View** menu or in the context menu displays a graphical representation of the asset allocation in the [Additional Area]({{% relref "/intro/userinterface" %}}). In the currency grouping, a pie chart shows the share of each currency in the total. In all other groupings a bar chart with the **Security risk** of each group is added on the left, with the pie chart of each group's share in the total on the right. This enables a quick assessment of diversification and shows at a glance how assets are distributed.

{{% notice style="note" title="Negative values in the pie chart" %}}
A pie chart cannot display negative shares. A group with a negative value, such as CFDs at a loss, is therefore missing from the pie, and the percentages in the pie refer only to the groups with a positive value. In that case the values of the column **Share %** are authoritative.
{{% /notice %}}

## Special Features
- **Securities-oriented view**: Unlike the account overview, which emphasizes accounts, this report focuses on securities and their performance.
- **Multi-currency support**: The report supports positions in different currencies. All values are automatically converted to the tenant's main currency.
- **Margin products**: For CFD and Forex only the unrealized gain or loss counts towards the assets, whereas the full market value counts towards the risk. This is why the columns **Account relevant** and **Security risk** differ considerably for these products.
- **Forex gains**: Currency gains and losses are shown as a separate column in every grouping, enabling precise analysis of currency effects. For instruments already denominated in the main currency the value is zero.
- **Transaction details**: By expanding rows, all underlying transactions can be viewed, enabling complete tracking.

## PDF report
The entry **PDF report...** in the context menu creates a statement of assets at the date that is set. It covers the whole tenant or the whole portfolio with all its cash accounts, not only the displayed security accounts. See [PDF report]({{% relref "/reportportfolio/periodperformance/pdfreport" %}}).
