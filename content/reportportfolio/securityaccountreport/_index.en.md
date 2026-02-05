---
title: "Security accounts"
date: 2025-11-17T22:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
The security accounts Report provides a comprehensive overview of all security positions of a tenant. Using this report, various grouping views can be created to analyze the asset structure. Additionally, detailed information on holdings, prices, and gains is displayed for each security.

{{% notice style="info" title="Security account Report on different levels" %}}
This report is available at different levels. Click on a single security account in the navigation tree to access the report for that security account. Additionally, the report exists one level higher, where it is called "Securities accounts". If only one security account exists for a portfolio, these reports on both levels deliver the same results.
{{% /notice %}}

## Functionality
The report evaluates all security positions of the tenant. All transactions up to the selected date are considered to ensure a point-in-time accurate presentation of holdings and gains. Positions are displayed in groups, with subtotals shown for each group and a grand total at the end.

## Grouping Options
The report offers various grouping options that can be selected via the dropdown menu:

- **Grouped by Currency**: Default view. Positions are grouped by their trading currency. For each currency, the exchange rate to the main currency is displayed. This provides a clear overview of currency exposures and facilitates the analysis of currency risks.
- **Grouped by Asset Class**: Positions are grouped by their asset class, for example equities, bonds, money market, commodities, or real estate. This enables asset allocation analysis.
- **Grouped by Financial Instrument**: Grouping by specialized investment instrument such as ETF, mutual fund, CFD, options, or futures. This view is helpful for analyzing the investment instruments used.
- **Grouped by Sub-Asset Class**: Grouping by asset subcategory, which enables more detailed analysis within asset classes.

## Columns in the Report
Some columns are displayed in the security's currency, others in the main currency. This can be identified by the suffix of the column header. The report displays the following columns for each position:
- **Name**: Designation of the security. The official name is used.
- **I**: Icon for the financial instrument. This symbol visualizes the type of instrument (e.g., stock, bond, ETF, currency pair) and facilitates quick identification in the table.
- **Holding**: Number of held units of the security. This column shows the current holding in the respective trading currency. For currency pairs, this value can also be negative.
- **Currency**: The trading currency of the security.
- **Timestamp**: Time of the last available price. This information shows the basis for the valuation.
- **Price**: The price of the security on the date specified above. If no price is available for that day, the price from a previous date will be displayed..
- **Gain security**: Profit or loss for this security in the trading currency. This value considers both realized and unrealized gains and losses including currency gains. This column is only displayed in the currency grouping.
- **Security Risk (Main Currency)**: The risk exposure of the position in the main currency. This column is displayed for currency grouping and is particularly relevant for margin products and leveraged instruments. For example, you have a quantity of 10 of a double inverse ETF at a price of 100. This results in a negative security risk of 2,000. This helps you better assess the risk of your investments.
- **Leveraged Inverse**: There are leveraged ETFs. The factor is displayed here if it deviates from 1. A minus sign indicates that it is an inverse ETF.
- **Gain Securities (Main Currency)**: The profit or loss in the tenant's main currency. All values are automatically converted to the main currency.
- **Account Relevant**: The accountable value of the position in the trading currency. For normal securities, this corresponds to the market value of the position. For margin products, the account value is displayed considering the leverage. This column is only displayed in the currency grouping.
- **Account Relevant (Main Currency)**: The accountable value of the position in the tenant's main currency.

### Group Totals and Grand Totals
Subtotals are displayed for each group. For currency grouping, the exchange rate to the main currency is additionally displayed. At the end of the report, the grand total appears across all groups.

## Filter Options
The report offers various filter options to customize the view:

- **Until Date**: The report can be set to a specific date, showing the portfolio view at a historical point in time. This is particularly useful for comparisons or year-end closings. Clicking the repeat icon next to the date field resets the date to today.
- **Include Closed Positions**: Via the context menu, you can choose whether already closed positions should also be displayed in the report. By default, only active positions are shown.

## Context Menu on Selected Instrument
Most functions that are also available in the watchlist are available here. See [Functions for Selected Instrument](../../watchlistinstrument/watchlist/#funktionen-markierte-watchlist-ansicht)

## Expandable Rows
Each position can be expanded by clicking the expander icon. The expanded view displays all **transactions** for this security. This enables detailed tracking of all purchases, sales, dividends, and other transactions that led to the current position.

### Context Menu on Transactions
At this point, individual existing transactions can also be edited. For more information, see [Security Transactions](../../../transaction/security/).

## Chart View
The "Show Chart" function in the context menu generates a graphical representation of the portfolio allocation. The pie chart visualizes the percentage distribution according to the selected grouping. This enables a quick assessment of diversification and shows at a glance how assets are distributed.

## Special Features
- **Securities-oriented view**: Unlike the account overview, which emphasizes accounts, this report focuses on securities and their performance.
- **Multi-currency support**: The report supports positions in different currencies. All values are automatically converted to the tenant's main currency.
- **Margin products**: For leveraged instruments, the accountable account value is calculated considering the leverage. The leverage factor is displayed in a separate column.
- **Currency gains**: For currency grouping, currency gains and losses are displayed separately, enabling precise analysis of currency effects.
- **Transaction details**: By expanding rows, all underlying transactions can be viewed, enabling complete tracking.
