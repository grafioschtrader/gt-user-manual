---
title: "Client"
date: 2026-08-28T10:00:00+02:00
draft: false
weight: 2
archetype: "default"
---
The client was created using the **registration process**.

## Edit client
The properties of the client were recorded in the **registration process** and can be adjusted via the **navigation area** on the **main element Portfolios**.

### Properties
All properties of a **client** can be changed at any time.
- **Client name**: This **client name** is used for identification purposes in case **portfolios** can be shared with other users in future versions of GT.
- **Currency**: This is the **currency** of the **client**, i.e. the evaluations of all **portfolios** are carried out in this **currency**. If it is changed, GT creates the [currency pairs](../../watchlistinstrument/instrument/currencypair/) missing for the new client currency and rebuilds the holding tables. Both happen in a background task, so the evaluations are only complete once that task has finished.
- **Ignore tax on dividends/interest**: In Switzerland, a tax amount of 35% is automatically deducted with the dividend or interest payment for certain shares and bonds. This amount is refunded on the basis of the tax information and the income is taxed as ordinary income. If you select the skip interest/dividend tax option, the tax for the "Interest/dividend" transaction type is skipped when calculating the profit. This makes it easier to compare the profits from different securities.
- **Closed until**: In Grafioschtrader, all transactions can be edited at any time and past transactions can be added. While this flexibility is necessary for corrections, it carries the risk of historical transactions being accidentally changed or added. Users may unintentionally modify completed data from past periods. This can lead to the following problems:
- Unexplainable calculations of historical portfolio performance.
- Discrepancies with the trading platform's account data.
- Difficulties in tracking when data was actually changed and when transactions took place.
To solve this problem, this date was introduced. It prevents the addition or modification of transactions in the past.
  - **Configuration hierarchy**: The portfolio level takes precedence if the "Closed until" field has a date. The client level is used as a fallback if no date is set at portfolio level. No restrictions apply if both values do not contain a date.
  - **Exceptions**: Functions that only affect tax-related attributes of a transaction and change neither holdings nor account balances are deliberately exempt from this lock. These include **Toggle taxable status** and **Set ex-date from tax data** in the [Dividends and interest](../../reportportfolio/dividends/) report.
- **Country**: The **country** records where the client is liable to tax. The entry is optional and unlocks the country-specific tax functions; at present only the selection **Switzerland** has an effect. When it is set, the [Dividends and interest](../../reportportfolio/dividends/) report additionally shows the columns **ICTax dividends** and **ICTax value total**, the context menu **Tax year corrections...** on a security row, and the option **Export tax statement** in the **View** menu for the eCH-0196 tax statement. This requires an administrator to have imported the [tax data](../../admindata/taxdata/) of the Swiss Federal Tax Administration beforehand; without that data the columns stay empty. If the field is left empty or another country is selected, these functions are simply omitted; all other reports are unaffected.
- **Enable Grafioschtrader import templates**: With this check box you allow this **client** to use the delivered import template group «Grafioschtrader». When it is set, GT's own exports — the CSV export of the transactions and the transaction receipts — can be read back, see [Grafioschtrader import](../securityaccounts/transactionimport/grafioschtrader/). Which template group holds those templates is not your choice: an administrator settles that once for the whole installation, see [Import template group](../../basedata/imptranstemplate/).

{{% notice note %}}
As long as no administrator has designated a template group for this purpose, the check box **Enable Grafioschtrader import templates** does not appear at all. If it is designated while you are logged in, you will see the check box only after your next login.
{{% /notice %}}

{{% notice note %}}
The **country** can already be chosen in the **registration process** when the client is created. For existing clients with currency CHF it was preset to **Switzerland** during the update. A change only takes effect the next time the [Dividends and interest](../../reportportfolio/dividends/) report is built.
{{% /notice %}}

## Functions
- **Currency client and portfolios**: This function is available on the **main element Portfolios**, it sets the **currency** of the **client** and all his **portfolios**. Because this function switches both levels at once, the missing [currency pairs](../../watchlistinstrument/instrument/currencypair/) are created for the client and for every single portfolio, after which the holding tables are rebuilt, likewise in a background task.
