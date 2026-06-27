---
title: "Period performance"
date: 2025-10-28T22:54:47+01:00
draft: false
weight: 20
archetype: "default"
---

The **Period Performance Report** provides a detailed analysis of a portfolio's or tenant's performance over a defined time period. The report allows tracking value development on a daily, weekly, or monthly basis and identifying the various factors influencing overall performance.

## Accessing the Report
The Period Performance Report can be accessed at two levels:

1. **Tenant Level**: Via Main Menu → **Accounts & Portfolios** → **Portfolios** → **Period Performance**
   - Analyzes the combined performance of all tenant portfolios
   - Values are displayed in the tenant currency

2. **Portfolio Level**: Within a single portfolio → **Period Performance**
   - Analyzes only the selected portfolio
   - Values are displayed in the portfolio currency

## Input Parameters
Before calculating the report, the following parameters must be specified:

### Date From
The start date of the analysis period (inclusive). Important: The date refers to the close of trading, meaning the income of the start day is **not** included in the calculation.

**Restrictions:**
- Must be a valid trading day (no weekend, holiday, or day with missing quotes)
- Cannot be before the first trading day with available data

### Date To
The end date of the analysis period (inclusive). The date refers to the close of trading.

**Restrictions:**
- Must be a valid trading day
- Must be after the start date
- Cannot be after the last available trading day

### Period Splitting
Determines how data is aggregated in the report:

- **Week**: Displays performance by weekdays (Monday-Friday)
  - Only available if the period spans at most the configured number of weeks
  - Ideal for detailed short-term analysis

- **Year/Month**: Displays performance by months
  - Only available if the period spans at least the configured number of months
  - Ideal for longer-term overviews

The availability of options is dynamically adjusted based on the selected time period.

## Report Content
The Period Performance Report consists of several main sections:

### 1. Summary (Period Comparison)
This section displays three columns:

- **First Day**: All values at the period start date
- **Last Day**: All values at the period end date
- **Difference**: The change between start and end

**Displayed Metrics:**

| Column | Description |
|--------|-------------|
| **Date** | The respective date |
| **Dividend Real** | Realized dividend income (cumulative) |
| **Fee Real** | Realized fees for account management and transactions (cumulative) |
| **Interest Cash Account Real** | Realized interest income on cash accounts (cumulative) |
| **Accumulate Reduce** | Net amount from security purchases and sales |
| **Cash Balance** | Total cash balance across all accounts |
| **External Cash Transfer** | Deposits and withdrawals |
| **Securities** | Market value of all security holdings |
| **Margin Close Gain** | Realized gains/losses from closed margin positions |
| **Security Risk** | Market value of open positions (unrealized) |
| **Gain** | Total gain/loss from securities |
| **Securities and Margin Gain** | Combined value of securities and margin gains |
| **Total Gain** | Total gain including regular and margin gains |
| **Total Balance** | Total portfolio wealth (cash + securities + margin gains) |

**Important**: All values are displayed in the main currency (MC = Main Currency) - either tenant or portfolio currency, depending on the access context.

### 2. Detailed Period Windows (Table View)
This section shows daily changes structured by periods (weeks or months):

**Weekly Split:**
- Each row represents one week
- Columns show days Monday through Friday
- At the end: Sum column for the week

**Monthly Split:**
- Each row represents one year
- Columns show the 12 months
- At the end: Sum column for the year

**Each cell displays the following information:**

| Value | Description |
|-------|-------------|
| **Deposit** | External transfers on this day (positive = deposit, negative = withdrawal) |
| **Gain** | Daily gain/loss from all sources |
| **Cash Balance** | Change in cash balance |
| **Securities** | Change in securities value |
| **Total Balance** | Daily change in total balance |
| **Missing Days** | Number of consecutive days with missing data before this day |

**Color Coding in the Table:**
- 🟩 **Green**: Regular trading day with data
- 🟨 **Yellow**: Holiday (no trading activity)
- 🟥 **Red**: Trading day with missing historical quote data
- ⬜ **Gray**: Weekend or non-relevant day

### 3. Column Sums
At the end of each column (weekday or month), the sum of gains for all corresponding days/months is displayed. This allows pattern recognition (e.g., "Is Monday a bad day for my portfolio?").

### 4. Graphical Representation
The report offers the option to display performance as a chart:

**Access**: "Show Chart" button in the menu

**Displayed Lines:**
- **External Cash Transfer Diff**: Cumulative deposits/withdrawals
- **Gain Diff**: Cumulative gains/losses
- **Cash Balance Diff**: Change in cash balance
- **Securities Diff**: Change in securities value
- **Total Balance**: Total wealth development

The chart uses Plotly and offers interactive features such as zoom, hover details, and range selector.

## Missing End-of-Day Quotes
An important aspect of the Period Performance Report is the handling of missing quote data. Missing quote data occurs when no historical quotes are available for held securities on a trading day. This can have various causes: The quote provider did not deliver data, technical problems occurred during data retrieval, or the security was actually not traded on that day.

The impact on the report is significant. Missing days are marked red in the period calendar, and performance cannot calculated on these days. The system counts consecutive missing days and displays them in the "Missing Days" field. During date selection, days with missing quotes are automatically marked as invalid trading days, so they cannot be selected as start or end dates.

{{% notice warning %}}
Without complete historical quote data, accurate performance calculation is not possible. It is strongly recommended to resolve missing quotes before analysis to obtain meaningful results.
{{% /notice %}}

### Overview and Interaction with Missing Quotes
The report provides a separate view for analyzing and resolving missing quote data. This can be accessed via the menu under "Missing End-of-Day Quotes" and displays two interconnected areas: A year calendar in the upper area and a securities table in the lower area.

The **year calendar** displays all trading days of the selected year in a clear overview. The color coding immediately reveals where problems exist: Green-marked days indicate that all quotes are available. Red-marked days show that at least one security has missing quotes. Yellow-marked days are days with missing quotes that have been selected by the user. When you click on a red-marked day, the securities table reacts immediately and displays only the securities that have missing quotes on that specific day.

The **securities table** lists all securities that have missing quotes in the selected period. In addition to the security name, the number of missing days is also displayed. The interaction also works bidirectionally here: When you click on a security in the table, all days on which this specific security has missing quotes are automatically marked yellow in the calendar. This bidirectional interaction enables quick identification of systematic data gaps and targeted remediation.

{{% notice tip %}}
The combination of calendar and table allows two analysis approaches: You can either start from a problematic day and see which securities are affected, or you can select a problematic security and see on which days quotes are missing.
{{% /notice %}}

### Resolving Missing Quote Data Issues
Several options are available for resolving missing historical quote data. The simplest method is **manually reloading** the quote data. Historical quotes can be retrieved again from the data provider via securities management. Select the affected security and trigger an update of the historical data.

A particularly elegant solution for individual missing days between available quotes is **linear filling of missing quote data**. The system can fill quote gaps through linear interpolation, whereby the missing quote is calculated from the adjacent available quotes. This method is particularly suitable for individual missing days in otherwise complete quote series, for example when the data provider had an outage on a single day.

{{% notice style="info" title="Linear Filling of Quote Gaps" %}}
For detailed instructions on linear filling of missing quote data, see [Linear Filling of Missing Quote Data](/gt-user-manual/en/watchlistinstrument/externaldata/historyquote/pricedata/). This function interpolates missing values based on surrounding quotes and is ideal for individual gaps in the time series.
{{% /notice %}}

If a data provider systematically has gaps, it may make sense to switch to an **alternative data provider**. GT supports various data providers, and often another provider offers better coverage for certain markets or securities. The data provider can be changed in the settings of the respective security.

For a few missing days, especially for exotic securities with poor data coverage, **manual quote entry** is also possible. While this option is time-consuming, it may be the only way to achieve a complete quote history in individual cases.

## Technical Details
### Trading Days and Holidays
The system automatically considers:
- **Global Holidays**: Worldwide non-trading days
- **Exchange-Specific Holidays**: Holidays of exchanges where held securities are traded
- **Weekends**: Saturday and Sunday are always excluded

### Currency Conversion
- All securities and accounts are automatically converted to the main currency
- Conversion uses historical exchange rates for the respective date
- For tenant evaluation: Conversion to tenant currency
- For portfolio evaluation: Conversion to portfolio currency

### Performance Optimization
- The system uses a cache with 2-minute validity for trading day metadata
- Data queries are executed in parallel (CompletableFuture)
- Results are reused for repeated requests within the cache period

## Typical Use Cases
1. **Performance Analysis**: How has my portfolio developed over the last quarter?
2. **Impact Analysis**: Which factors (gains, deposits, price development) influenced performance?
3. **Pattern Recognition**: Are there specific weekdays or months with particularly good/bad performance?
4. **Data Quality**: Are there systematic gaps in my historical quote data?
5. **Comparison**: How does the performance of different portfolios differ?

## Limitations and Notes
- The report requires at least one transaction in the system
- For meaningful analysis, the period should span at least several trading days
- Missing quote data can affect calculation accuracy
- Period splitting selection is automatically restricted based on the time period
- For very large periods (multiple years), calculation may take several seconds
