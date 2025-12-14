---
title: "Transaction costs"
date: 2025-11-19T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
The **Transaction Costs** report provides a comprehensive analysis of all costs and taxes incurred in securities trading. It is available exclusively at **tenant level** and analyzes all security transactions across all portfolios and security accounts. All values are automatically converted to the main currency using historical exchange rates for precise cost analysis.
## Report Structure
The report is structured in two levels. The **main table** shows a summary per security account with aggregated cost values. Expanding a row reveals the individual transactions that contributed to these costs. This hierarchical structure enables both quick overview and detailed analysis at transaction level.
The main table shows the following columns for each security account: **Name** of the account, **Tax (every kind)** for the sum of all taxes paid in main currency, **Paid transactions** for the number of cost-incurring transactions, **Average transaction cost** for the average costs per transaction, and **Transaction cost** for the total sum of all order fees excluding taxes. The footer shows the totals across all accounts.
## Transaction Detail View
The detail view displays all individual transactions of a security account with their respective costs. The columns include **Date** and **Name** of the security, **Stock exchange** where the security was traded, **Transaction type** for the type of transaction, **Currency Cashaccount** and **Currency Security** for the currencies used, **Transaction cost** in the original currency, **Tax (every kind)** in main currency, **Net price** as transaction volume in main currency, **Exchange rate** for currency conversion, and **Transaction cost** in main currency. The detail table uses pagination and shows 20 transactions per page by default.
## Functions
The main table can be sorted by all columns to identify, for example, the most expensive accounts. Sorting by average transaction costs shows which accounts are cheapest relative to transaction size. In the detail view, transactions can be sorted chronologically or by cost amount.
Currency conversion is performed automatically to the tenant's main currency. The system uses historical exchange rates at the respective transaction time. The detail view shows both the original amount and the exchange rate used, so conversion can be verified at any time.
### Edit and Delete Transactions
In the detail view, individual transactions can be edited or deleted via the **context menu** or the **Edit** menu. This is useful when transaction costs need to be corrected retroactively. The available options correspond to those in the [Transactions](../transactionlist/) report. After each change, the report is automatically reloaded to display current values.
## Chart Visualization
Via the **View** menu and the **Show Chart** option, a scatter plot is displayed in the additional area. Each data point represents an individual transaction. The X-axis shows the net price of the transaction in main currency, the Y-axis shows the incurred transaction costs. Each account is displayed as a separate data series with its own color. The series are hidden by default and can be activated by clicking the legend. When you move your mouse over the chart, detailed information about the respective data points is displayed. Clicking a data point automatically selects the corresponding row in the detail table.
## Filter Criteria
The system automatically applies filter criteria to focus the report on relevant transactions. Only transactions with actual costs are considered. Transactions where the Transaction Cost field is null or empty are not included. Money market direct investments are excluded as this asset class typically does not incur traditional brokerage fees. All other security transactions are captured regardless of asset class or transaction type.
