---
title: "Transaction costs"
date: 2025-10-25T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
The **Transaction Costs Report** provides a comprehensive analysis of all costs and taxes incurred in securities trading. This report helps understand the impact of trading costs on investment performance and identify optimization opportunities. All values are automatically converted to the tenant currency using historical exchange rates for precise cost analysis.

## Accessing the Report
The Transaction Costs Report can only be accessed at tenant level. It analyzes all security transactions across all portfolios and security accounts that have incurred costs. Access is via Main Menu → **Accounts & Portfolios** → **Portfolios** → **Transaction Costs**. The system loads data asynchronously using parallel data queries for optimal performance.

## Report Content
The report focuses on cost analysis of securities trading and captures all transactions that have incurred fees or taxes. Intelligent filter criteria are applied to include only relevant transactions. Money market direct investments are excluded as they typically do not have traditional brokerage fees. The analysis includes order fees, taxes of all kinds, exchange fees, and other transaction-related costs.

The report is structured in two levels. The main table shows a summary per security account with aggregated cost values. Expanding a row reveals the individual transactions that contributed to these costs. This hierarchical structure enables both quick overview and detailed analysis at transaction level.

## Column Overview of Main Table
The main table groups all transaction costs by security account and displays the following aggregated values:

| Column | Description |
|--------|-------------|
| **Name** | Name of the security account (e.g., broker or bank) |
| **Tax (every kind)** | Sum of all taxes paid for this account in tenant currency |
| **Paid transactions** | Number of transactions that incurred costs |
| **Transaction average paid** | Average transaction costs per paid transaction in tenant currency |
| **Transaction cost** | Sum of all transaction costs (excluding taxes) for this account in tenant currency |

The table footer shows the totals across all accounts. This provides a quick overview of the tenant's total trading costs and allows comparison of different brokers regarding their cost efficiency.

## Transaction Detail View
Clicking the expand icon before an account displays all individual transactions that contributed to the aggregated costs. This detail table shows the following information:

| Column | Description |
|--------|-------------|
| **Date** | Date and time of the transaction |
| **Name** | Name of the traded security |
| **Stock exchange** | Name of the exchange where the security was traded |
| **Transaction type** | Type of transaction (Buy, Sell, Dividend, Finance Cost) |
| **Currency cashaccount** | Currency of the cash account used |
| **Currency security** | Currency of the security |
| **Transaction cost** | Order fees in the original transaction currency |
| **Tax cost** | Taxes paid in tenant currency |
| **Net value transaction** | Base price (quantity × quotation + accrued interest) in tenant currency |
| **Exchange rate** | Exchange rate used for currency conversion |
| **Transaction cost (MC)** | Order fees converted to tenant currency |

The detail table uses pagination and shows 20 transactions per page by default. Sorting is by transaction date descending but can be adjusted by clicking column headers. Multi-sorting is possible by holding the Ctrl/Cmd key.

## Functionality
The report offers various functions for analyzing and managing transaction costs:

### Sorting and Grouping
The main table can be sorted by all columns to identify, for example, the most expensive accounts. Sorting by average transaction costs shows which brokers are cheapest relative to transaction size. In the detail view, transactions can be sorted chronologically or by cost amount.

### Edit and Delete Transactions
In the detail view, individual transactions can be edited or deleted via the context menu (right-click). This is useful when transaction costs need to be corrected retroactively. The available options correspond to those in the Transaction Report. After each change, the report is automatically reloaded to display current values.

{{% notice tip %}}
Via the context menu in the detail view, transaction data can also be exported as a CSV file. This enables further analysis in spreadsheet applications or archiving of cost data.
{{% /notice %}}

### Currency Conversion
All costs and taxes are automatically converted to the tenant currency. The system uses historical exchange rates at the respective transaction time to ensure precise cost analysis independent of the originally used trading currencies. This is particularly important for international portfolios with transactions in various currencies.

The conversion is transparent and traceable. The detail view shows both the original amount and the exchange rate used, so conversion can be verified at any time. For transactions in the tenant currency, the exchange rate is 1.0.

## Chart Visualization
The report provides a graphical representation of transaction costs as a scatter plot. The chart is accessed via the menu option "Show Chart" and opens in a separate area below the main view.

In the scatter plot, each data point represents an individual transaction. The X-axis shows the net value of the transaction (transaction volume in tenant currency), the Y-axis shows the incurred transaction costs. Each account is displayed as a separate data series with its own color, with series hidden by default and activated by clicking the legend.

The chart uses Plotly and offers extensive interaction capabilities. You can zoom into the chart, inspect individual data points with the mouse pointer, and show or hide data series. The hover function displays detailed information about each transaction. Clicking a data point automatically selects the corresponding row in the detail table and the table area scrolls to the selected transaction.

{{% notice style="info" title="Cost Analysis with the Chart" %}}
The scatter plot is particularly useful for identifying cost outliers. Transactions with unusually high costs relative to transaction volume become immediately visible. Comparing different accounts in the chart shows which broker consistently offers better rates.
{{% /notice %}}

## Typical Use Cases
The Transaction Costs Report supports various analysis approaches for optimizing trading costs:

**Broker Comparison**: Grouping by account allows direct comparison of different brokers' cost structures. Average transaction costs per paid transaction show which broker is most cost-efficient. Especially with active trading strategies, these differences can be substantial.

**Identify Cost Outliers**: The detail view and chart help find individual transactions with disproportionately high costs. These could be recording errors or actually excessive fees that might warrant complaints.

**Optimize Trading Behavior**: The analysis shows whether certain transaction types or sizes are particularly cost-intensive. It may be worthwhile to consolidate smaller orders or prefer certain trading venues.

**Tax Documentation**: The complete listing of all taxes paid per account facilitates tax returns and serves as proof to tax authorities.

**Cost Budgeting**: The total cost overview helps plan future trading activities and assess whether trading costs are proportionate to returns achieved.

## Filter Criteria and Exclusions
The system automatically applies intelligent filter criteria to focus the report on relevant transactions:

Only transactions with actual costs are considered. Transactions where the "Transaction Cost" field is null or empty are not included in the report. This prevents cost-free transactions from distorting statistics.

Money market direct investments are excluded as this asset class typically does not incur traditional brokerage fees. Costs for these instruments are often already calculated into the price or charged differently. This filtering ensures meaningful comparability of actual trading costs.

All other security transactions are captured regardless of asset class or transaction type. This includes stocks, bonds, ETFs, funds, derivatives, and foreign exchange. Dividend transactions are also considered if taxes were incurred.

## Limitations and Notes
When interpreting the report, the following aspects should be considered:

The cost analysis is based on recorded transaction data. If transaction costs were not correctly entered during recording, this can affect the report's validity. Regular verification of cost data completeness and accuracy is therefore recommended.

Currency conversion uses historical exchange rates. With missing historical rates for certain currency pairs, conversion may be inaccurate. In such cases, the relevant exchange rates should be manually added.

The report only shows directly recorded transaction costs. Indirect costs such as spreads between bid and ask prices or opportunity costs from delayed order execution are not considered. Ongoing custody fees not assigned to individual transactions appear separately as account transactions.

Average transaction costs are an average across all transaction sizes. With highly varying order volumes, this value can be misleading. For detailed cost structure analysis, the chart should also be consulted.
