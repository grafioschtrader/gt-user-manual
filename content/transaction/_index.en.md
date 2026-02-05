---
title: "Transaction"
date: 2021-04-20T02:54:47+01:00
draft: false
weight: 20
archetype: "default"
---
A distinction is made between **transactions** that relate only to **bank accounts** and those involving an **instrument**. In addition, some transactions can be **imported** via **PDF** or **CSV** but can certainly be **entered manually**.

## Properties and plausibilities
Certain plausibility checks are omitted or carried out regardless of the **transaction type**.
- An account can be overdrawn, i.e. a negative account balance is possible.
- **Transaction time**:
  - Transaction times for securities transactions are permitted on trading days defined in the global trading calendar.
  - There is no restriction on the transaction time for account transactions.

## Import of securities transactions as CSV
**CSV** is probably the best alternative for the initial import of a portfolio's transactions, provided that the **CSV file** also contains information such as the **transaction costs** and the **tax amount** for the **securities transactions**. Unfortunately, experience has shown that many **trading platforms** only provide incomplete **CSVs**. **CSV files** can only become transactions indirectly via [transaction import](../tenantportfolio/securityaccounts/transactionimport); as with **PDF import**, this requires an **import template** from the [import template group](../basedata/imptranstemplate/). The basic advantages of **CSV** are:
- Anonymization is probably not necessary or easy to accomplish.
- All information required for a transaction could be available.

## Accuracy of amounts and prices
A compromise was found between the Iranian Rial and Bitcoin currencies for the numerical format of amounts and prices. The decimal places are used for the Iranian rial and the decimal places for Bitcoin.

#### Pre-decimal places
A maximum of 14 digits before the decimal point is possible for amounts and exchange rates. This should be sufficient for displaying the Iranian rial currency.

#### Decimal places
Amounts and exchange rates are always saved internally with 8 decimal places. This can lead to minor inaccuracies. A real example: There is an interest rate of 1355.70 for the quantity of 1300 shares, this results in an interest rate of "1.04284615384615..." per share. Only 1.04284615 can be entered and saved in GT. The calculation 1.04284615 x 1300 results in 1355.699995. This small inaccuracy is still visible in the current GT version.

#### Different accuracies for currencies
Most currencies have a subdivision of 100, such as the CHF, EUR and USD. The Kuwait Dinar is a thousand-dollar currency, so an accuracy of 3 decimal places is expected. For cryptocurrencies such as Bitcoin, 8 decimal places are expected. Finally, there are also currencies that do not use decimal places, such as the Japanese yen. In the current version, GT is not yet able to display this correctly.

{{% notice style="info" title="Automatic correction of exchange rate or rate/div/etc" %}}
Rounded or truncated decimal places may be specified in the transaction documents. Due to the decimal places of a currency, the GT backend attempts to achieve the desired amount of the account entry by adjusting the exchange rate or exchange rate. For example, an amount of CHF 2887.85 is sought from the account entry CHF 2887.84995323 by adjusting the exchange rate, if available, or otherwise by adjusting the exchange rate/div/etc. In reality, an amount of CHF 2887.84999998 may be reached in this example
{{% /notice %}}
