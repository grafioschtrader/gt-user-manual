---
title: "Client and report portfolios"
date: 2026-08-10T22:54:47+01:00
draft: false
weight: 12
archetype: "default"
---
The analysis is what the user of GT is ultimately interested in. The evaluations have very different focuses, which is why there are different reports.

## Performance calculation
... 
{{% notice warning %}} 
Many **certificates** can also be replicated as instruments in **GT**. However, **GT** cannot determine the **risks** for most **structured products** (certificates), as these often have an **asymmetrical payout profile** and GT is not aware of the characteristics of the individual products. 
{{% /notice %}}

## Why no percentage change in assets
GT does not specify a percentage change in assets. This has been omitted as the informative value is low in the case of a non-permanent 100% investment. 
{{% notice note %}}
**Benchmarking future GT version**\
It is possible that there will be a benchmarking option in future GT versions. An idea of a simulation: What would the performance be if the same amounts were invested in the benchmark compared to the investments that were actually made. 
{{% /notice %}}

## Deviation from reality
Hypothetical transactions are carried out to calculate the performance. For example, securities would have to be sold and the foreign currencies bought against the main currency. Hypothetical sales and currency transactions are carried out for this purpose. The middle rate is used for the foreign currency exchange rates. Expenses for transaction costs and taxes are not taken into account. 
{{% notice note %}}
**Disposal costs future GT version**\
In a future GT version, the previously neglected disposal costs are taken into account. The **disposal costs** will be derived from the history of transactions carried out. 
{{% /notice %}}

## Foreign currency problem
As soon as an application supports accounts and trading with foreign currencies, there will be discussions about different approaches to performance calculation. GT itself is knowingly not consistent throughout with regard to this calculation. For example, the income and expenses for **account interest** or **account and custody account costs** are calculated differently in the **portfolio** report than in the period income report. In the former, the income is converted into the main currency on the transaction date, whereas in the period income report the date to which the calculation relates is used. This does not apply to the **Forex gain**, which is described in the next section and is determined in the same way in every report.

## Forex gain
Anyone investing in a foreign currency achieves their result from two sources: from the instrument itself and from the movement of the exchange rate. The **Forex gain** shows the second part separately. It answers the question of how much of the result arose purely because the rate of the foreign currency changed against your main currency.

The principle is simple. What matters is the money that is still tied up in the foreign currency on the reference date. This is valued at the rate of the reference date and compared with the value that the same money had on the days it actually moved. Purchases, sales, dividends and accrued interest all count towards this. Money that has already flowed back, from a sale or a dividend for instance, no longer takes part in the rate movement from that moment on.

### Example
An example makes the relationship clear. The main currency is CHF, and 100 units of an instrument are bought at USD 10.00 with a USD/CHF rate of 1.00. By the reference date the instrument has risen to USD 12.00, while at the same time the dollar has fallen to a rate of 0.80.

| | USD | CHF |
|---|---:|---:|
| Amount invested at purchase | 1'000.00 | 1'000.00 |
| Value on the reference date | 1'200.00 | 960.00 |
| **Gain security** | **+200.00** | **+160.00** |
| **Forex gain** | | **-200.00** |
| **Total** | | **-40.00** |

The instrument gained 20 percent, and the position is still down by 40 francs in CHF. The price gain of USD 200.00 is worth CHF 160.00 on the reference date, and against it stands a currency loss of CHF 200.00 on the capital invested. A negative Forex gain while the instrument rises is therefore not a contradiction, but the normal case with a weakening foreign currency.

{{% notice note %}}
**The two columns together give the result**\
**Gain security** in the main currency and **Forex gain** together always give exactly the result of the position in your main currency. If the figures differ from your expectation, price data of the instrument or of the currency pair is usually missing.
{{% /notice %}}

### Where the Forex gain appears
The Forex gain is calculated for accounts and for securities using the same procedure, which is why the reports can be used to check one another. It appears in [Portfolios and Portfolio]({{% ref "/reportportfolio/portfolios" %}}) for the foreign currency accounts, in [Security accounts]({{% ref "/reportportfolio/securityaccountreport" %}}) and [Asset Classes with Cash]({{% ref "/reportportfolio/securitycashaccountreport" %}}) for the individual securities, and in the transaction list of a [Securities transaction]({{% ref "/transaction/security" %}}) for each individual transaction.

The column header carries the main currency after the label, for example «Forex gain CHF».

## General representations in reports
Certain forms of representation in the reports are generally applied and are described here.

### Color representation
For better readability and faster identification of gains and losses, the report uses consistent color coding:
- **Positive values** (gains): Are displayed in the standard text color or in green
- **Negative values** (losses): Are displayed in red

This color coding applies to the profit columns, making it easy to see at a glance which asset classes and securities show positive performance and which show losses.

### Decimal places per currency
Amounts are displayed with the number of decimal places appropriate for the respective currency. For most currencies this is two decimal places. However, one unit of the Japanese yen (JPY) has a low value, so amounts in JPY are displayed without decimal places. Conversely, one unit of the cryptocurrency Bitcoin (BTC) has a very high value, which is why amounts in BTC are displayed with eight decimal places. This applies to monetary amounts such as balances, costs, taxes or gains in the reports. Prices and exchange rates are exempt from this, as they require a finer resolution with up to eight decimal places.
The entry of amounts behaves accordingly: in the dialogs for transactions and standing orders, the number of decimal places is limited by the currency of the respective account or instrument.
{{% notice note %}}
**Global setting**\
The administrator defines the decimal places per currency via the global parameter «gt.currency.precision». Currencies without an entry use two decimal places; an entry has the form «BTC=8,ETH=7,JPY=0». A change takes effect for the user after logging in again.
{{% /notice %}}
