---
title: "Dividends and interest"
date: 2026-08-09T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
The **Dividends and Interest** report provides a year-by-year overview of all dividend and interest income as well as associated costs and taxes. It is available exclusively at **tenant level** and analyzes all transactions across all portfolios. All values are automatically converted to the main currency.

{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The tax data functionality has not been finally checked and may still change.
{{% /notice %}}

## Report Structure
The report is structured in three levels. The **main table** shows a summary per year with aggregated values for dividends, interest, costs and taxes. Expanding a year row reveals two detail views: the **securities detail view** shows all securities with their income for the selected year, the **cash accounts detail view** shows all interest income and fees per account. Expanding a security or account row once more finally shows the individual transactions. This hierarchical structure enables both a quick overview over the years and detailed analysis at security and account level.
### Columns of the Main Table
The main table contains one row per calendar year. All amounts are stated in the tenant's main currency. The footer shows the totals across all years.
- **Year**: The calendar year to which all values of the row refer.
- **Account interest**: The sum of all interest income from cash accounts in this year.
- **Tax cost transaction**: The sum of taxes incurred on buy and sell transactions, for example stamp duties.
- **Paid transactions**: The number of transactions for which transaction costs were paid.
- **Average transaction cost**: The average costs per paid transaction, i.e. the transaction costs divided by the number of paid transactions.
- **Transaction cost**: The sum of all order fees (brokerage) of the year.
- **Account/Depot cost**: The sum of fees recorded as fee transactions for account and custody management.
- **Margin finance cost**: The sum of financing costs of margin products such as CFDs. This column is hidden by default and only shown when such positions exist.
- **Taken tax**: The sum of taxes deducted directly on dividend and interest payments, for example withholding tax.
- **Taxable**: The taxable gross income of the year. It comprises the account interest as well as those dividend and interest transactions where the **Subject to tax** checkbox is activated. The automatically deducted tax is added back to the received amount.
- **Received Div/Interest**: The dividends and interest actually credited, i.e. after deduction of the automatically paid taxes.
- **Value at end of year**: The total value of all security positions on December 31 of the respective year. Margin products such as CFD and Forex are not included in this total: their unrealized gain/loss is already contained in the cash accounts' column **Balance + margin + fin. cost** and would otherwise be counted twice when reading the total assets.

For Swiss tenants, i.e. when the client's [country](../../tenantportfolio/client/#properties) is set to **Switzerland**, the additional columns **ICTax dividends** (CHF) and **ICTax value total** (CHF) are displayed. These columns are based on imported [tax data](../../admindata/taxdata/) of the Swiss Federal Tax Administration and enable a direct comparison with the official tax values.
## Securities Detail View
The first detail view lists all securities that paid income in the selected year or were held at year end. Expanding a security row further displays all individual transactions of that security for the year; these can be edited directly.
- **Excl.**: A checkbox to exclude the security from the tax statement export, see [Exclude securities](#exclude-securities).
- **Name**: The name of the security. The background color indicates how the holding changed in the respective year, see [Background color of the Name column](#background-color-of-the-name-column).
- **Instrument**: An icon showing the product type of the security, for example stock, bond or ETF.
- **ISIN**: The international securities identification number. A yellow background indicates that a [tax year correction](#tax-year-corrections) exists for this security in the displayed or the previous tax year; the tooltip shows the entered note.
- **Currency**: The currency in which the security is traded.
- **Exchange rate end of year**: The conversion rate from the security currency to the main currency at year end. It is used to convert the amounts to the main currency.
- **Units at end of year**: The number of units on December 31 after all transactions and splits. For a position fully sold during the year this shows zero.
- **Price end of year**: The closing price of the security at year end in the security currency.
- **Tax free**: The sum of distributions that do not count as taxable because the **Subject to tax** checkbox of the transaction is deactivated. An example is a return of capital.
- **Finance cost**: The financing costs of this position for margin products. This column is hidden by default.
- **Taken tax**: The taxes deducted directly from the distributions in the security currency. A second column shows the same value converted to the main currency; its header is supplemented with the currency code of the main currency.
- **Taxable**: The taxable gross income of this security, once in the security currency and once in the main currency.
- **Received Div/Interest**: The distributions actually received after tax deduction in the main currency.
- **Value at end of year**: The value of the position at year end in the main currency, i.e. units times price, converted with the exchange rate at year end. For margin products (CFD, Forex) this column instead shows the unrealized gain/loss of the positions still open at year end — as if they had been closed at the year-end price. This value deliberately does not flow into the column's year total; the tooltip of the column header points this out.

For Swiss tenants, the additional columns **ICTax dividends** and **ICTax value total** are displayed per security.
### Background color of the Name column
The background of the **Name** column shows at a glance how the holding of the position changed in the respective year. The reason for the coloring is also shown in the tooltip when hovering over the name.
- {{% badge color="#B6EDB6" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} **Green**: The position was opened this year, i.e. the holding went from zero to a positive number of units.
- {{% badge color="#FFF48F" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} **Yellow**: The position was fully sold this year, the holding at year end is zero.
- {{% badge color="LightPink" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} **Light pink**: The holding was zero both at the beginning and at the end of the year. This is the case when a position was opened and fully closed within the same year, or when after a complete sale in an earlier year only a trailing distribution was credited.
- {{% badge color="#FFD39B" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} **Light orange**: A directly held bond was redeemed on or after its maturity date this year.

If several states apply, the order light orange before light pink before yellow before green applies.
### Coloring of the ICTax dividends column
For Swiss tenants, the cell of the **ICTax dividends** column is shaded as soon as its value deviates from the value of the **Taxable** column. This makes deviations between the recorded transactions and the official tax data immediately visible.
- {{% badge color="#FBE9E9" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} to {{% badge color="#F0A8A8" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} **Red**: The ICTax value is higher than the value in **Taxable**.
- {{% badge color="#E9FBE9" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} to {{% badge color="#A8F0A8" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} **Green**: The ICTax value is lower than the value in **Taxable**.

The larger the deviation, the stronger the color; full intensity is reached at a deviation of 20 percent. If an ICTax value is present but there is no amount in **Taxable**, the cell is shown in the strongest red shade — the **Subject to tax** flag is probably missing on the transaction.

If a [tax year correction](#tax-year-corrections) exists for the security in the displayed year, the following markings apply instead. The tooltip of the marked cells shows the note of the correction.
- {{% badge color="#B6EDB6" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} **Green** in the **Taxable** column: This amount replaces the ICTax value because **Use taxable amount** is activated in the correction.
- {{% badge color="#FFF48F" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} **Yellow** in the **ICTax dividends** column: The value was replaced by a directly entered **Taxable income**; the deviation shading does not apply in this case.
## Cash Accounts Detail View
The second detail view lists all cash accounts that generated interest or incurred fees in the selected year. The columns include **Name** and **Currency** of the account, **Price end of year** for the exchange rate of the account currency at year end, **Cash account cost** and **Securities Account cost** each in account currency and in main currency, **Taken tax** for deducted taxes, **Taxable** for the taxable portion, **Received Div/Interest** for the interest actually received, and **Cash balance** as account balance at year end in account currency and in main currency. For margin accounts, the columns **Margin earnings**, **Hypothetical finance cost** and **Balance + margin + fin. cost**, hidden by default, are additionally available. **Margin earnings** is the unrealized gain/loss of the margin positions still open at year end that are settled through this account. **Hypothetical finance cost** is the projected financing cost since the last booked financing cost transaction. **Balance + margin + fin. cost** yields the account balance including both values — together with the securities' **Value at end of year** this allows reading the total assets at year end directly from the report. Expanding an account row further displays all interest and fee transactions of that account for the year. These can be edited directly.
## Functions
The main table can be sorted by all columns. Sorting is by default by year ascending. In the detail views, securities and accounts can also be sorted by all columns.
In the detail views, individual transactions of a security or account can be displayed and edited by expanding. The editing functions correspond to those in the [Transactions](../transactionlist/) report. After each change, the report is automatically reloaded to display current values. Expanded year and security rows remain expanded during the reload.
Via the **context menu** of a dividend or interest transaction, the **Subject to tax** checkbox can be switched directly with **Toggle taxable status**, without opening the edit dialog. This function is only available for saved dividend and interest transactions. It also works for periods locked via ["Closed until"](../../tenantportfolio/client/#properties), as it changes neither holdings nor account balances.
## Tax statement
The following functions support the preparation of an eCH-0196 tax statement for Swiss tenants. Each setting is persisted in a different scope — pay attention to the notes below.
### Exclude securities
At the beginning of each row in the securities detail view there is an **Excl.** (Exclude) checkbox: when activated, the security is excluded from the tax statement export for the respective year. This setting is persisted in the **database per tenant, year and security** (table `tax_security_year_config`) and is therefore shared across all users of the same tenant.
### Tax year corrections
In rare cases the computed value of the **ICTax dividends** column is not correct for a security, for example after corporate actions or when individual distributions are valued differently. Via the **context menu** of a security row in the securities detail view, the entry **Tax year corrections...** opens a dialog in which this value can be overridden per tax year. The table in the dialog shows all corrections of this security across all tax years and allows creating, editing and deleting entries. Exactly one entry is possible per security and tax year. For a new entry, the **Tax year** of the year whose detail view the dialog was opened from is suggested; after saving it can no longer be changed.
Two mutually exclusive options are available for the override. With the **Use taxable amount** checkbox, the value of the **Taxable** column replaces the ICTax value. Alternatively, an amount can be entered directly under **Taxable income**. In addition, a **Note** with up to 1024 characters can be stored; the full text is shown when hovering over the cell. For tax years without imported [tax data](../../admindata/taxdata/) no override is possible, but a note is — this allows documenting a tax position even without a correction.
The corrected value flows into the year total **ICTax dividends** of the main table as well as into the [export](#export) of the tax statement. After closing the dialog, the report is automatically reloaded. The corrections are persisted in the **database per tenant, year and security** (table `tax_year_correction`) and therefore apply to all users of the same tenant.
The following diagram shows which amount per security and tax year flows into the tax statement.
{{< mermaid >}}
graph TD
  A[Tax position:<br/>security + tax year] --> B{Correction<br/>entered?}
  B -- no --> C[Computed ICTax value]
  B -- yes --> D{Use taxable<br/>amount?}
  D -- yes --> E[Value of the<br/>Taxable column]
  D -- no --> F{Taxable income<br/>entered?}
  F -- yes --> G[Directly entered amount]
  F -- no --> C
{{< /mermaid >}}
### Account selection
Via the **View** menu and the **Evaluation of depots** option, a dialog can be opened to restrict the evaluation to specific security accounts and cash accounts. In this dialog you can select which security accounts should be considered for dividend evaluation and which cash accounts for interest evaluation. The table header shows how many accounts are currently selected. This setting is persisted in the **browser's local storage** per user — it is not shared with other users and resets when switching to a different browser or device.
### Filter to year end
Via the **View** menu and the **Show transactions only until end of year** option, a toggle can be activated that shows only transactions up to December 31 of the respective year in the securities detail view. This is useful for limiting the tax data to the relevant calendar year.
### Set ex-date from tax data
Via the **context menu** on a year row of the main table, the **Set ex-date from tax data** function is available. It is only active when [tax data](../../admindata/taxdata/) has been imported for the selected year. The function fills in a missing **Ex-Date** on the year's dividend transactions from the official ICTax data. The assignment is based on the ISIN and the temporal proximity of the transaction to the respective distribution, accepting a deviation of at most seven days. An already entered **Ex-Date** is never overwritten. After execution, a message shows how many ex-dates were set, how many were already present and how many transactions found no match; the report is then reloaded. The function also affects periods locked via ["Closed until"](../../tenantportfolio/client/#properties), as the ex-date is only used for tax evaluation.
### Export
Via the **View** menu and the **Export tax statement** option, a dialog can be opened to generate an eCH-0196 tax statement. The dialog contains the following fields: **Tax year** (defaults to the previous year), **Canton** (selection from 26 Swiss cantons), **Institute name** (required), **LEI** (optional), **Customer number** (required), **First name**, **Last name**, **TIN/AHV number** (optional). The export is downloaded as a ZIP file containing an XML file (eCH-0196 v2.2.0) and a PDF overview. The export settings (canton, name, customer number, etc.) are persisted in the **database per tenant** and are therefore pre-filled and shared for all users of the same tenant.

{{% notice note %}}
The tax statement export and ICTax columns are only available when the client's [country](../../tenantportfolio/client/#properties) is set to **Switzerland**. The ICTax data must first be imported by the **administrator** under [Tax data](../../admindata/taxdata/). The export and the exclusion function are available to every user.
{{% /notice %}}
