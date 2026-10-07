---
title: "Standing order cash"
date: 2026-08-07T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
The cash account standing order automatically creates recurring account transactions. It is the simpler of the two standing order types, as only a fixed amount and an account need to be configured.

## Transaction types
Four transaction types are available for cash standing orders:
- **Deposit**: Regular credit to the account. A typical use case is a monthly money transfer to the trading account.
- **Withdrawal**: Regular debit from the account. It is suitable for regular withdrawals with which you actually take capital out of the portfolio.
- **Account interest**: Regular interest credit to the account.
- **Account/Depot cost**: A recurring account or custody fee charged by the bank. The amount is always entered as a positive value and posted as a debit.

For a recurring bank fee, select **Account/Depot cost** and not **Withdrawal**. A withdrawal counts as capital taken out of the portfolio and therefore distorts the performance calculation, whereas account and custody costs are correctly treated as an expense.

## Input fields
When creating or editing a cash standing order, the following information must be provided:
- **Transaction type**: Deposit, withdrawal, account interest or account/depot cost.
- **Account**: The account on which the transaction is posted. The selection list shows all accounts of the tenant, grouped by portfolio and currency. An account deactivated via the **Active until or maturity date** field is not offered for selection depending on the **valid from** date. See [Deactivating an account]({{% ref "/tenantportfolio/cashaccount#deactivating-an-account" %}}).
- **Amount**: The fixed amount that is posted with each execution. Without an amount currency, this is the currency of the selected account.
- **Amount currency** (optional): The currency in which the bank charges the amount, if it differs from the account currency. See [Amount in a foreign currency](#amount-in-a-foreign-currency).
- **Amount formula** (optional): Formula for calculating the amount to be posted. See [Amount in a foreign currency](#amount-in-a-foreign-currency).
- **Transaction cost** (optional): A fixed transaction cost that is applied to each generated transaction. This can be used, for example, to account for bank fees on recurring transfers.

Additionally, the common fields of the [repeat configuration](../) are available: repeat unit, repeat interval, day position, execution day, execution month, non-trading-day adjustment, quote tolerance, valid from, valid to and an optional note. For a cash standing order the non-trading-day adjustment skips weekends only; exchange holidays are irrelevant, because a bank posts on those days as well.

Once the standing order has created transactions, the transaction-specific fields (transaction type, account, amount, amount currency, amount formula and transaction cost) are locked. Only the scheduling fields and note can still be edited. A margin factor that has once been used can therefore no longer be adjusted afterwards; if the bank changes its terms, end the existing standing order via the **valid to** field and create a new one. See [Editing and deletion restrictions](../#editing-and-deletion-restrictions) for details.

## Amount in a foreign currency
Some banks charge a recurring fee in one currency but debit it from an account in another currency. An example is a custody fee of CHF 3.00 per month that is debited from an account in US dollars. The debited amount is then slightly different every month because it depends on the exchange rate.

For such a case, enter the amount in the billing currency under **Amount**, that is 3.00, and set the **Amount currency** to CHF. Already when the standing order is saved, GT creates the required [currency pair](../../../watchlistinstrument/instrument/currencypair/) from the amount currency to the account currency, if it does not exist yet. Its price data is therefore loaded long before the first execution. At each execution, GT determines the exchange rate of the execution date and converts the amount into the account currency. If no rate is available on the execution date, GT moves to a neighbouring day within the [quote tolerance](../#quote-tolerance). Since a cash standing order may well fall on an exchange holiday, on which no exchange rates are delivered, the quote tolerance should not be set to 0 here. If no rate is found within the window either, the execution is skipped and recorded in the [failure log](../#execution-failures).

### Amount formula
The plain exchange rate rarely corresponds to the rate the bank actually charges, as the bank adds a margin. The **Amount formula** lets you model this markup. Two variables are available in the formula:

| Variable | Meaning | Example |
|---|---|---|
| `a` | Amount as entered in the **Amount** field | 3.00 |
| `r` | Exchange rate on the execution date | 1.2366 |

| Formula | Description |
|---|---|
| `a * r` | Plain conversion, corresponds to the behaviour without a formula |
| `a * r * 1.019` | Conversion with a bank markup of 1.9% |
| `ROUND(a * r * 1.019, 2)` | As above, additionally rounded to two decimal places |

The formula is checked when the standing order is saved, so a typing error is reported immediately. If the formula uses the variable `r`, an amount currency must be set.

{{% notice style="info" title="Accuracy" %}}
The amount is an approximation and usually does not match the bank's amount to the cent. A bank's margin is not constant but varies from month to month. For a real account fee of CHF 3.00, the margin averaged around 1.9% over three years, and the calculated amount typically deviated by about two cents from the actual debit. If you need exact values, correct the created transaction after the execution.
{{% /notice %}}

{{% notice note %}}
If the formula cannot be evaluated or no exchange rate is available, no transaction is posted. The execution is skipped and the reason is recorded in the standing order's failure list.
{{% /notice %}}

## Created transactions
Each transaction created by the standing order corresponds to a normal [cash transaction](../../account/). It can be viewed in the usual transaction views and edited if necessary. Additionally, the created transactions are visible directly in the standing order table via the row expander, where they can also be edited or deleted via the context menu. See [Viewing created transactions](../#viewing-created-transactions) for details.
