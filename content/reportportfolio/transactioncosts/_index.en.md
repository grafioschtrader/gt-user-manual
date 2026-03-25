---
title: "Transaction costs"
date: 2026-02-26T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
The **Transaction Costs** report provides a comprehensive analysis of all costs and taxes incurred in securities trading. It is available exclusively at **tenant level** and analyzes all security transactions across all portfolios and security accounts. All values are automatically converted to the main currency using historical exchange rates for precise cost analysis.
The report uses a tab menu with two tabs:
- **Transaction cost**: Shows the cost overview and detail analysis per securities account (documented in the sections below).
- **Fee model comparison**: Compares the fee model's estimated costs against actual recorded transaction costs. See [Fee model comparison](#fee-model-comparison) at the end of this page.
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
## Fee model comparison
The fee model comparison validates whether the YAML-based fee model correctly predicts actual transaction costs for historical buy and sell transactions. It requires a configured fee model — either on the [Trading platform plan]({{% ref "/basedata/tradingplatformplan" %}}) or as a per-account override on the [Securities account]({{% ref "/tenantportfolio/securityaccounts" %}}).
{{% notice style="warning" title="Prerequisite" %}}
For the comparison to produce results, a fee model must be configured for the selected securities account — either on its trading platform plan or as an account-level fee model.
{{% /notice %}}
### Input controls
Two controls determine the scope of the analysis:
- **Securities account**: Selects the account to analyze. The dropdown shows accounts in "Portfolio / Securities account" format.
- **Exclude zero cost**: Hides transactions with no transaction costs (enabled by default).
### Summary
After selecting a securities account, two summary panels appear.
The **Overview** panel shows the following fields:
| Field | Description |
|-------|-------------|
| **Plan** | Name of the trading platform plan. If the account has its own fee model, this is indicated accordingly. |
| **Total** | Total number of buy/sell transactions for the account. |
| **Compared** | Number of successfully compared transactions. |
| **Skipped** | Number of skipped transactions (zero cost). |
| **Errors** | Number of transactions where the estimation failed (e.g. no matching rule). |

The **Accuracy** panel shows statistical metrics for model quality:
| Field | Description |
|-------|-------------|
| **Mean absolute error** | Average absolute difference between actual and estimated costs. |
| **Mean relative error** | Average relative deviation in percent. |
| **RMSE** | Root mean squared error — weights larger deviations more heavily. |
### Detail table
The detail table shows the comparison at individual transaction level. The following columns are available:
| Column | Description |
|--------|-------------|
| **Date** | Date of the transaction. |
| **Transaction type** | Type of transaction (buy/sell). |
| **Security** | Name of the traded instrument. |
| **Category type** | Asset class category (e.g. equities, fixed income). |
| **Special investment instrument** | Special instrument type (e.g. ETF, direct investment). |
| **MIC** | MIC code of the stock exchange. |
| **Actual cost** | Actually recorded transaction costs. |
| **Estimated cost** | Costs calculated by the fee model. |
| **Relative error %** | Percentage deviation between actual and estimated value. The column uses color coding: green indicates low deviation (~0%), through yellow (~25%) to red (50%+). |
| **Currency** | Currency of the transaction. |
| **Trade value** | Total trade amount (quotation × units). |
| **Quotation** | Price at the time of the transaction. |
| **Units** | Number of traded shares/units. |
| **Matched rule** | Name of the fee model rule that matched for this transaction. |
### Model hierarchy
When a securities account defines its own fee model, it takes priority over the trading platform plan's model. The **Overview** panel indicates which model was used for the comparison. For more information on per-account overrides, see [Securities accounts — Fee model]({{% ref "/tenantportfolio/securityaccounts" %}}).
