---
title: "Security account report"
date: 2025-10-27T22:54:47+01:00
draft: false
weight: 30
archetype: "default"
---
The **Security Account Report** provides a comprehensive overview of all held security positions for a portfolio or tenant with flexible grouping options. This report is the central tool for analyzing current asset structure and performance of individual positions. All values are automatically converted to the main currency (portfolio or tenant currency) using historical exchange rates for precise valuation.

## Accessing the Report
The Security Account Report can be accessed at two levels:

1. **Tenant Level**: Via Main Menu → **Accounts & Portfolios** → **Portfolios** → **Security Account Report**
   - Shows all security positions across all portfolios and security accounts of the tenant
   - Values are displayed in tenant currency
   - Most comprehensive view of total assets

2. **Portfolio Level**: Within a single portfolio → **Security Account Report**
   - Shows only positions of the selected portfolio
   - Values are displayed in portfolio currency
   - Focused view on portfolio-specific holdings

## Report Content
The report displays a tabular overview of all held security positions with detailed information on holdings, current valuation, and performance. Positions are grouped by a selectable criterion, with subtotals shown for each group and a grand total at the end across all positions. The report considers both open and optionally closed positions and supports various financial instruments such as stocks, bonds, ETFs, funds, derivatives, and forex.

## Reference Date (Until Date)
A central feature of the Security Account Report is the **reference date** (until date), which enables valuation at a specific point in time. The reference date determines which closing price is used for position valuation and which transactions flow into the calculation. By default, the current date is used, so positions are valued with the most current available prices.

By changing the reference date, portfolio structure can be analyzed at any historical point in time. This is particularly useful for year-end closings, tax returns, or analyzing portfolio development at specific reference dates. The system uses the closing prices of the selected date and considers only transactions executed up to that date. The reference date is stored in session storage and persists across different reports until explicitly changed or reset to the current date.

{{% notice tip %}}
Via the menu option "Reset to Today", the reference date can be reset to the current date at any time to display the current portfolio structure.
{{% /notice %}}

## Column Overview
The main table displays the following information for each security position:

| Column | Description |
|--------|-------------|
| **Name** | Name of the security with instrument icon |
| **Instrument** | Icon visualizing the financial instrument type |
| **Holding** | Number of units held (for bonds × 100 for nominal value) |
| **Currency** | Trading currency of the security |
| **Date/Time** | Timestamp of the last price |
| **Last Price** | Current closing price in security currency |
| **Gain/Loss** | Gain or loss in security currency |
| **Gain/Loss (MC)** | Gain or loss in tenant/portfolio currency |
| **Account Value Security** | Current market value in security currency |
| **Account Value Security (MC)** | Current market value in tenant/portfolio currency |

**Important**: All monetary columns are color-highlighted. Positive values (gains) appear in green, negative values (losses) in red. Columns with the suffix "(MC)" show values in the main currency.

## Grouping
A particular strength of the Security Account Report is the flexible grouping function. Via the dropdown menu, positions can be grouped by various criteria:

### Grouping by Currency
The default grouping is by the trading currency of securities. This provides a quick overview of the portfolio's currency exposure. For each currency, positions are aggregated and a subtotal is formed across all positions in that currency. This grouping is particularly helpful for identifying currency risks and analyzing geographical diversification.

### Grouping by Asset Class
With this grouping, positions are sorted by their asset class (Asset Class Category Type). The system distinguishes between stocks, bonds, ETFs, funds, derivatives, and other categories. This view enables classic asset allocation analysis and shows how wealth is distributed across different asset classes.

### Grouping by Financial Instrument
This grouping is by special investment instrument. Positions are categorized by more specific characteristics such as direct equity investments, index ETFs, bond ETFs, actively managed funds, CFDs, forex positions, and others. This allows more detailed analysis than pure asset class grouping.

### Grouping by Sub Category
Grouping by sub category offers the most granular division. Positions are sorted by their specific sub category, for example by sectors for stocks, by maturities for bonds, or by regions for funds. This grouping enables very detailed analyses of portfolio structure.

### Grouping by Asset Class Combination
This combined grouping aggregates multiple asset class attributes and thus offers a balanced view between overview and detail. It is particularly suitable for portfolios with diverse instruments.

{{% notice style="info" title="Dynamic Group Changes" %}}
The selected grouping is saved in the system and persists on the next report access. Subtotals are automatically calculated for each group and displayed at the end of the grouping.
{{% /notice %}}

## Subtotals and Grand Total
For each group, subtotals are automatically calculated and displayed in a separate row below the last position in the group. The subtotal row is visually highlighted and shows the following aggregated values:

- Group designation (e.g., "EUR" for currency grouping or "Stock" for asset class grouping)
- Exchange rate (for currency grouping)
- Sum of gains/losses in group currency
- Sum of gains/losses in main currency
- Sum of account values in group currency
- Sum of account values in main currency

At the end of the table, a **grand total** is displayed across all groups. This row is specially highlighted and shows the aggregated values of the entire portfolio in the main currency. The grand total provides a quick overview of total assets and cumulative performance.

## Show Closed Positions
By default, only currently open positions with holdings greater than zero are displayed. Via the menu option "Show Closed Positions", historical, already closed positions can also be included in the report. This is useful for analyzing past investments or for traceability of selling decisions.

Closed positions are displayed with zero holdings and show realized gains or losses. When the option is activated, a checkmark symbol appears next to the menu item, and the table is automatically reloaded to also display closed positions.

## Row Expansion for Transaction Details
Each position in the table can be expanded by clicking the arrow symbol before the security name. The expanded view displays all transactions that contributed to this position. This includes purchases, sales, dividend payments, and possibly financing costs for margin positions.

Transaction details show the complete history of the position and enable direct editing actions. Via the context menu (right-click) or the main menu, new transactions can be recorded:

- **Buy**: Record new purchase transaction for the position
- **Sell**: Record sales transaction (only if holdings exist)
- **Dividend**: Record dividend payment (not for margin products)

After recording or editing a transaction, the report is automatically reloaded, and the affected position remains expanded so changes are immediately visible.

## Chart Visualization
Via the menu option "Show Chart", a graphical representation of the portfolio structure can be accessed. The chart opens in a separate area below the main view and offers different visualizations depending on the selected grouping.

The chart display uses Plotly and offers extensive interaction capabilities. You can zoom into the chart, inspect individual data points with the mouse pointer, and show or hide data series. The hover function displays detailed information about each group or position.

Different chart types are used depending on the grouping:

**For currency grouping**: A pie chart shows the percentage distribution of assets across different currencies. This enables quick visual assessment of currency exposure.

**For asset class grouping**: A bar chart shows the weighting of different asset classes and their performance. Positive and negative gains are displayed with different colors.

**For other groupings**: Depending on the selected grouping, suitable visualizations are generated that optimally represent the structure and performance of the portfolio.

{{% notice style="info" title="Chart Synchronization" %}}
The chart is automatically updated when the grouping is changed or data is reloaded. The chart definition is passed to the chart component, enabling consistent presentation across different views.
{{% /notice %}}

## Context Menu and Additional Functions
Right-clicking on a position opens a context menu with additional functions:

**Transaction Recording**: The already described functions for recording purchases, sales, and dividends are also available via the context menu.

**Show Time Series**: For each security, historical price data can be displayed as a time series. This opens a new view with an interactive chart of price development.

**Set Up Alarm**: Price alarms can be configured for the selected security, which trigger a notification when certain price thresholds are reached.

**URL Links**: For many securities, external links are available that lead to detailed information on financial websites. These are dynamically generated based on the security and its exchange.

## Special Treatment of Margin Products
Margin positions such as CFDs or forex are treated specially in the report. For these products, the account value differs from the pure market value because leverage and margin requirements must be considered.

The **Account Value Security** shows for margin products the actual value of the position including gain or loss, while the market value would represent valuation without leverage. Financing costs (positive or negative) are recorded separately as their own transaction type and flow into overall performance.

For margin products, dividend recording is not available in the context menu because these instruments do not pay dividends. Instead, financing costs can be documented via regular transaction recording.

## Currency Conversion and Exchange Rates
All securities and their valuations are automatically converted to the main currency. The system uses historical exchange rates at the respective reference date to ensure precise valuation independent of the originally used trading currencies.

Conversion is transparent and traceable. For currency grouping, the used exchange rate is displayed in the group header row. For positions in the main currency, the exchange rate is 1.0. Columns without the "(MC)" suffix always show values in the original currency of the security, while columns with "(MC)" represent the converted values in the main currency.

## Typical Use Cases
The Security Account Report supports various analysis approaches for portfolio management and optimization:

**Asset Overview**: The most important function is the quick overview of total assets and their current valuation. The grand total shows portfolio total assets at a glance.

**Asset Allocation Analysis**: By grouping by asset class, you can verify whether strategic asset allocation is being maintained or adjustments are required.

**Currency Risk Analysis**: Currency grouping shows exposure to different currencies and helps identify and manage currency risks.

**Performance Tracking**: The gain/loss columns show the performance of individual positions and enable identification of top performers and loss makers.

**Rebalancing Planning**: By comparing actual weightings with target weightings, necessary reallocations can be identified.

**Tax Planning**: The overview of realized and unrealized gains supports tax decisions, for example for utilizing loss offset options.

**Portfolio Cleanup**: By displaying closed positions, you can verify which securities have already been sold and how past investment decisions developed.

## Limitations and Notes
When using the Security Account Report, the following aspects should be considered:

Valuation is based on available closing prices at the selected reference date. Missing price data can lead to inaccurate valuations. It is recommended to regularly verify the completeness of price data and add missing prices if necessary or switch the data provider.

For margin products, account values are determined based on complex calculations that consider leverage, financing costs, and margin requirements. These calculations can vary depending on product and broker, and displayed values should be reconciled with broker statements.

Grouping and total calculation are based exclusively on master data of securities stored in the system. Changes to asset classes or categorizations immediately affect grouping.

Closed positions are displayed with zero holdings and realized gains. For positions completely closed through multiple purchases and sales, cumulative performance is calculated across all transactions.

The reference date affects all reports globally that use this date. Changing the reference date in one report also influences other reports such as the Period Performance or Transaction Costs Report.
