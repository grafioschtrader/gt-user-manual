---
title: "Portfolios and Portfolio"
date: 2021-05-20T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
The two evaluations **Portfolios** and **Portfolio** are very similar. They are therefore documented together. A **cut-off date** for the day of the evaluation can be set for both evaluations.

## Portfolios
The **Portfolios** evaluation provides an overview of the total sum of all portfolios. The two groupings **Currency grouped** and **Portfolio grouped** are supported. If there is no account for a currency, the corresponding currency is not assigned to an account.

## Portfolio
The evaluation is carried out according to the accounts or the currency.

### Portfolios function
There is only the selection of grouping.

### Portfolio function
It is only possible to enter an [account transaction](../../account/transaction/) here. The following functionality is also available:
- The [account](../../tenantportfolio/cashaccount/) is processed here.
- The transactions of the corresponding account are displayed in the expanding table row. These can be edited, see [Transaction](../../transaction/).

## Table columns
Only the non-self-explanatory columns are described:
- **Currency gain main currency**: The currency gain is calculated as if the transaction had taken place in the main currency and not in the foreign currency.
  - With an account transfer in a foreign currency, currency gains or losses are accrued from the transaction time.
- **Gain on securities in main currency**: The gain on securities is calculated by adding the price gain and dividends. Taxes and trading costs are deducted accordingly.
- **Gain on securities**: The hypothetical gain that is and was realized on the securities, i.e. if all securities were sold on the **reference date**. This amount includes the income from interest and dividends as well as the expenses of the recognized transaction and tax costs.

{{% notice note %}} 
The values **Securities** and **Cash balance** result in the **total** in the corresponding portfolio currency.\
The values **External cash deposit/withdrawal** - **Account transaction costs** - **Account and custody account costs** + **Account interest** + **Currency gain** must also result in the **total** in the corresponding portfolio currency. 
{{% /notice %}}
