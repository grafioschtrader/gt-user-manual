---
title: "Client and report portfolios"
date: 2025-11-19T22:54:47+01:00
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
As soon as an application supports accounts and trading with foreign currencies, there will be discussions about different approaches to performance calculation. GT itself is knowingly not consistent throughout with regard to this calculation. For example, the income and expenses for **account interest** or **account and custody account costs** are calculated differently in the **portfolio** report than in the period income report. In the former, the income is converted into the main currency on the transaction date, whereas in the period income report the date to which the calculation relates is used.

## General representations in reports
Certain forms of representation in the reports are generally applied and are described here.

### Color representation
For better readability and faster identification of gains and losses, the report uses consistent color coding:
- **Positive values** (gains): Are displayed in the standard text color or in green
- **Negative values** (losses): Are displayed in red

This color coding applies to the profit columns, making it easy to see at a glance which asset classes and securities show positive performance and which show losses.
