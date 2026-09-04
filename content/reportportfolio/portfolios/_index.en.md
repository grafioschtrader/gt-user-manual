---
title: "Portfolios and Portfolio"
date: 2026-09-01T22:54:47+01:00
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

## How the evaluation is calculated
Both evaluations include **every transaction ever recorded**. The calculation starts with the very first transaction and runs chronologically up to the chosen cut-off date. No intermediate results are stored or carried forward; the evaluation is recalculated in full every time it is opened.

Each transaction in a foreign currency is converted using the exchange rate of its own transaction day, not the rate of the cut-off date. Securities still held on the cut-off date are valued at that day's closing price. Stock splits are taken into account so that the quantities remain comparable across the entire period.

Three points follow from this that matter in daily use. The cut-off date can be chosen freely, because the result always refers to the complete history up to that day. If a transaction far in the past is corrected afterwards, all values affected by it change immediately. And as the number of transactions grows, the evaluation takes correspondingly longer.

{{% notice tip %}}
The [Period performance](../periodperformance/) evaluation determines its figures in a different way, which is why the two evaluations can be used as a plausibility check against each other. Some columns are not meant to agree to the last cent. Which ones and why is described under [Comparison with the Portfolios evaluation]({{% relref "/reportportfolio/periodperformance" %}}#comparison-with-the-portfolios-evaluation).
{{% /notice %}}

## Table columns
Only the non-self-explanatory columns are described:
- **Account and custody account costs**: Separately booked fees of the account and of the custody account. Trading costs contained in a purchase or sale do not belong here, they are part of the securities result; neither do the financing costs of margin positions, even though they are charged to the account. They are a cost of the position and appear in **Gain securities**.
- **Account interest**: Interest credited to or charged on the cash account. Income from securities does not belong here.
- **Forex gain main currency**: The part of the result that arose purely from the movement of the exchange rate. With an account transfer in a foreign currency, currency gains or losses are accrued from the transaction time. The calculation is described under [Forex gain]({{% ref "/reportportfolio#forex-gain" %}}).
- **Gain on securities in main currency**: The gain on securities is calculated by adding the price gain and dividends. Taxes and trading costs are deducted accordingly.
- **Gain on securities**: The hypothetical gain that is and was realized on the securities, i.e. if all securities were sold on the **reference date**. This amount includes the income from interest and dividends as well as the expenses of the recognized transaction and tax costs.

{{% notice note %}} 
The values **Securities** and **Cash balance** result in the **total** in the corresponding portfolio currency.\
The values **External cash deposit/withdrawal** - **Account transaction costs** - **Account and custody account costs** + **Account interest** + **Forex gain** + **Gain securities** must also result in the **total** in the corresponding portfolio currency. 
{{% /notice %}}

## Comparison with Period performance

Four figures of this evaluation also appear in [Period performance](../periodperformance/), there under slightly different labels.

| Portfolios and Portfolio | Period performance |
|---|---|
| **Account and custody account costs** | **Account/Depot real cost** |
| **Account interest** | **Account interest real** |
| **External cash deposit/withdrawal** | **External cash inflows, outflows** |
| **Cash balance** | **Cash balance** |

They are the same bookings in each case, and the amounts can still deviate from one another. The reason is the moment of the currency conversion: this evaluation converts every booking with the exchange rate of its own booking day, while period performance converts the accumulated amount once, with the rate of the evaluation day. For an account held in the main currency this makes no difference; for a foreign-currency account a deviation of several percent can arise. A deviation of a few cents is always just rounding.

The full explanation, with an example and the remaining reasons, is given under [Comparison with the Portfolios evaluation]({{% relref "/reportportfolio/periodperformance" %}}#comparison-with-the-portfolios-evaluation).
