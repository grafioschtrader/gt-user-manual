---
title: "Transactions"
date: 2026-07-16T10:00:00+02:00
draft: false
weight: 60
archetype: "default"
---
The **Transactions** report displays all financial movements in a tabular overview. This view is available at **tenant level** and at the **level of individual portfolios**. It enables the management and analysis of all security and account transactions.
## Accessing the Report
At **tenant level**, all transactions are displayed across all portfolios and accounts. At **portfolio level**, the view is limited to the transactions of the selected portfolio. This focused view facilitates the analysis of individual portfolio activities.
## Transaction Types
The report shows various types of transactions. For **securities**, purchases, sales, dividends and finance costs for margin positions are recorded. For **accounts**, these include deposits, withdrawals, account interest and fees. **Account transfers** are transfers between two accounts within the tenant and are displayed as connected transactions.
## Table Columns
The main columns of the report are **Date** for the time of the transaction, **Cashaccount** for the affected account and **Transaction Type** for the type of movement. For security transactions, **Security**, **Quantity** and **Quotation/Dividend** are additionally displayed. The **Tax Cost** and **Transaction Cost** columns show the corresponding costs. The **Cashaccount Amount** indicates the impact on the cash balance and is displayed in red when negative.
{{% notice note %}}
The **Quantity** for security transactions is always positive. The transaction type buy or sell determines the direction of the movement.
{{% /notice %}}
## Filter and Sort Functions
The table offers extensive filtering and sorting options. Each column can be filtered individually, with different filter options available depending on the data type. For **date columns**, you can select a date and specify whether you want to search for an exact date or a date range. For **text columns** such as cashaccount, transaction type or security, a selection list is available. For **numeric columns**, you can define numeric ranges. Sorting is by default by date descending, but can be adjusted by clicking on the column headers. Multi-column sorting is also possible using the **Ctrl key**.
## Edit Functions
Transactions can be edited or deleted via the **context menu** or the **Edit** menu. When **editing**, fields such as date, quantity, quotation, taxes or fees can be adjusted depending on the transaction type. The account and the security cannot be changed after creation. When **deleting** a transaction, the system requests confirmation as this process is permanent. For account transfers, both connected transactions are deleted together.
A special function is the **conversion to account transfer**. A single deposit or withdrawal can be converted to an account transfer retrospectively. This is useful when a withdrawal was originally recorded as a normal withdrawal but was actually an internal transfer. The system automatically creates the connected counter-entry on the target account.
## Special Features
For **margin positions**, separate transactions for finance costs are recorded which are linked to the opening transaction. These can be positive or negative. For **dividends**, an ex-date can be specified when the dividend is paid after the sale of the security. The security reference is retained. For **account transfers** with different currencies, the exchange rate is used for both transactions and the amount is converted accordingly.
## CSV export
The listed transactions can be downloaded as a CSV file via the menu item «**Export transactions as CSV**» in the **context menu** or the **Edit** menu. The scope follows the level of the view: at **tenant level** the transactions of every securities account are exported, at **portfolio level** only those of the securities accounts of the selected portfolio. The optional fields «Date from» and «Date to» limit the export to a period.
Because a transaction import always relates to a single **securities account**, one CSV file is created per securities account. If the export concerns only a single securities account, a CSV file is downloaded directly, otherwise a **ZIP archive** with one file per securities account. The generated files can be read back with the Grafioschtrader import templates, see [Grafioschtrader import](../../tenantportfolio/securityaccounts/transactionimport/grafioschtrader/).
{{% notice note %}}
Opening and closing positions of **margin trades** are exported but skipped on re-import and have to be entered manually.
{{% /notice %}}
If the two sides of an **account transfer** are imported through different files, they initially remain unconnected. At **tenant level** the menu item «**Connect cash account transfers**» restores this connection afterwards by reconnecting unambiguously matching deposits and withdrawals based on time and amount.
