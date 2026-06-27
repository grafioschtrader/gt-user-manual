---
title: "Dividends and interest"
date: 2026-03-12T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
The **Dividends and Interest** report provides a year-by-year overview of all dividend and interest income as well as associated costs and taxes. It is available exclusively at **tenant level** and analyzes all transactions across all portfolios. All values are automatically converted to the main currency.

{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The tax data functionality has not been finally checked and may still change.
{{% /notice %}}

## Report Structure
The report is structured in three levels. The **main table** shows a summary per year with aggregated values for dividends, interest, costs and taxes. Expanding a row reveals two detail views: the **securities view** shows all dividend payments per security for the selected year, the **cash account view** shows all interest income and fees per account. This hierarchical structure enables both a quick overview over the years and detailed analysis at security and account level.
The main table shows the following columns for each year: **Year**, **Account interest** for the sum of all interest income from cash accounts in main currency, **Tax cost transaction** for all taxes incurred on transactions, **Paid transactions** for the number of cost-incurring transactions, **Average transaction cost** for the average costs per transaction, **Transaction cost** for the total sum of all order fees, **Fee** for account and custody fees, **Auto paid tax** for withholding taxes automatically deducted on dividends, **Taxable amount** for the taxable portion of income, and **Real received** for the dividends and interest actually received after deduction of all taxes. The **Value at end of year** column shows the total value of all securities at year end. The footer shows the totals across all years. For Swiss tenants (tenant country CH), the additional columns **ICTax income** (CHF) and **ICTax tax value total** (CHF) are displayed. These columns are based on imported [tax data](../../admindata/taxdata/) and enable a direct comparison with the official tax values.
## Securities Detail View
The first detail view lists all securities that paid dividends in the selected year. The columns include **Name** and **ISIN** of the security, **Currency** of the security, **Exchange rate** at year end, **Units** at year end, **Close** price at year end, **Tax free income** for tax-exempt dividend portions, **Auto paid tax** for deducted withholding taxes, **Taxable amount** for the taxable portion, **Real received** for the dividends actually received, and **Value at end of year** for the total position value. For Swiss tenants, the additional columns **ICTax income** and **ICTax tax value total** are displayed per security. Expanding a security row further displays all individual dividend transactions of that security for the year. These can be edited directly.
## Cash Accounts Detail View
The second detail view lists all cash accounts that generated interest or incurred fees in the selected year. The columns include **Name** and **Currency** of the account, **Close** as balance at year end, **Fee cashaccount** and **Fee security account** each in account currency and in main currency, **Auto paid tax** for deducted taxes, **Taxable amount** for the taxable portion, **Real received** for the interest actually received, **Cash balance** as current account balance, and **Cash balance** in main currency. Expanding an account row further displays all interest and fee transactions of that account for the year. These can be edited directly.
## Functions
The main table can be sorted by all columns. Sorting is by default by year ascending. In the detail views, securities and accounts can also be sorted by all columns.
In the detail views, individual transactions of a security or account can be displayed and edited by expanding. The editing functions correspond to those in the [Transactions](../transactionlist/) report. After each change, the report is automatically reloaded to display current values.
## Tax statement
The following functions support the preparation of an eCH-0196 tax statement for Swiss tenants. Each setting is persisted in a different scope — pay attention to the notes below.
### Exclude securities
At the beginning of each row in the securities detail view there is an **Excl.** (Exclude) checkbox: when activated, the security is excluded from the tax statement export for the respective year. This setting is persisted in the **database per tenant, year and security** (table `tax_security_year_config`) and is therefore shared across all users of the same tenant.
### Account selection
Via the **View** menu and the **Evaluation of depots** option, a dialog can be opened to restrict the evaluation to specific security accounts and cash accounts. In this dialog you can select which security accounts should be considered for dividend evaluation and which cash accounts for interest evaluation. The table header shows how many accounts are currently selected. This setting is persisted in the **browser's local storage** per user — it is not shared with other users and resets when switching to a different browser or device.
### Filter to year end
Via the **View** menu and the **Show transactions only until end of year** option, a toggle can be activated that shows only transactions up to December 31 of the respective year in the securities detail view. This is useful for limiting the tax data to the relevant calendar year.
### Export
Via the **View** menu and the **Export tax statement** option, a dialog can be opened to generate an eCH-0196 tax statement. The dialog contains the following fields: **Tax year** (defaults to the previous year), **Canton** (selection from 26 Swiss cantons), **Institute name** (required), **LEI** (optional), **Customer number** (required), **First name**, **Last name**, **TIN/AHV number** (optional). The export is downloaded as a ZIP file containing an XML file (eCH-0196 v2.2.0) and a PDF overview. The export settings (canton, name, customer number, etc.) are persisted in the **database per tenant** and are therefore pre-filled and shared for all users of the same tenant.

{{% notice note %}}
The tax statement export and ICTax columns are only available when the tenant country is **CH** (Switzerland). The ICTax data must first be imported by the **administrator** under [Tax data](../../admindata/taxdata/). The export and the exclusion function are available to every user.
{{% /notice %}}
