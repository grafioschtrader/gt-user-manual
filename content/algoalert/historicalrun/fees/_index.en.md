---
title: "Fees in the replay"
date: 2026-09-24T12:00:00+02:00
draft: false
weight: 15
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress - Target version V0.40.0" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
A [historical replay](../) models **commissions** on every buy and sell, recurring **custody fees**, and percentage **currency conversion markups**. They come from the same fee model, so a replay needs no configuration of its own. This page explains which model applies, how the fees are booked and what you have to supply when starting a replay. How a fee model is written is described under [Trading platform plan]({{% ref "/basedata/tradingplatformplan" %}}), the FX tariff under [Currency conversion markups]({{% ref "/basedata/tradingplatformplan#currency-conversion-markups" %}}). Transaction taxes such as the Swiss stamp duty are not part of the fee model. They are estimated by the [simulation tax model]({{% ref "/algoalert/historicalrun/taxmodel" %}}) when **Apply simulation tax models** is switched on; whether the stamp duty is due depends on the [dealer country]({{% ref "/basedata/tradingplatformplan#dealer-country" %}}) of the trading platform plan.

## Which fee model applies
A simulation environment is a copy of your portfolio. Every securities account of the copy keeps its trading platform plan and its own fee model, if it has one. For commissions and custody fees, GT determines the applicable model as follows:
{{< mermaid >}}
graph TD
    A["Securities account of the simulation"] --> B{"Own commission rules<br/>of the account?"}
    B -->|yes| C["Account commissions and custody apply"]
    B -->|no| D{"Fee model in the<br/>trading platform plan?"}
    D -->|yes| E["Plan commissions and custody apply"]
    D -->|no| F["No commissions and<br/>no custody fees"]
{{< /mermaid >}}
An [account-specific fee model]({{% ref "/tenantportfolio/securityaccounts#fee-model" %}}) replaces commissions and custody together when it contains commission rules. When overriding that part, copy both the commission rules and the custody section. The FX section inherits separately; an FX-only account document keeps the plan's commissions and custody.

At the start GT freezes the models of all accounts. If you change a plan while a run is working, the replay still calculates with the state at the start. Saving a model never creates charges in a real account.

## Commissions
For every execution GT calculates the commission from the rules of the applicable model, using the execution date. With a model that uses time periods, the tariff valid on that day applies. The commission is calculated in the currency of the security, as for an ordinary transaction.

If the model cannot return an amount for an execution, the replay stops with an error. This happens when no period covers the execution date, when no rule matches, or when the result is negative. GT deliberately does not treat such a trade as free, because otherwise exactly the trades the model does not describe would be the cheapest ones of the run. Cover the whole simulation period with a catch-all rule (condition `"true"`) and periods without gaps.

Free-trade allowances such as one free trade per quarter also count the buys and sells booked in the same calendar year before the opening date. Only executions that are actually booked use up an allowance, not calculations made to size an order. Counting is per securities account; an allowance granted across several accounts of the same banking relationship cannot be modelled.

## Custody fees
Custody fees are calculated only when the applicable model contains a `custody` section. Without it, the custody fees of that account count as **unmodelled**, not as free. To model free custody, use a dated period with the amount `"0"`. The model must cover the whole simulation period. An unresolved period (`UNRESOLVED`), a missing historical period or missing prices for the valuation stop the replay.

The fee is charged to the cash account that belongs to the same portfolio, is held in the fee currency and stays active during the replay. If there is no such account, or more than one, name it with `cashaccount` in the opening balances (see below). If the selected account is held in another currency, GT converts with the historical exchange rate. If the balance is insufficient, the usual bounded internal funding transfers are available. If the fee still cannot be paid, the replay fails with a clear message.

Until they are billed, custody fees accumulate as an unpaid accrual. This accrual reduces the simulated equity even before the charge. Billing reduces cash and clears the accrual, so the fee is not counted twice. If the replay ends before the next invoice, the accrual remains in the result as a liability.

A fee per stock exchange counts, for the first billing period, the exchanges of the positions held on the opening date and those of trades booked in that period before the opening date. This applies even if these positions are sold before the period ends.

The replay cannot process two kinds of tariff change and refuses to start. If the fee currency changes within the simulation period, a new simulation is needed. If a charged tariff changes within a billing period, model this in an account-specific model with suitably split periods.

## Opening custody balances
If a simulation opens in the middle of a billing period, fees have already accumulated up to the opening date that GT cannot know. Enter them in the replay dialog in the field **Opening custody balances (YAML)**. The keys are the names of the securities accounts **in the simulation**. The [YAML editor]({{% ref "/intro/userinterface#yaml-editor" %}}) suggests and explains the suitable fields inside an entry. The following example is not a template for your amounts; replace name and amounts with the state on the opening date:
```yaml
My depot:
  accruedFees: 0
  remainingCredits: 0
  billedThisYear: 0
  assumption: "Balances verified against the opening statement."
```
| Field | Meaning |
|-------|---------|
| `accruedFees` | Custody fees accrued up to the opening date but not yet charged. |
| `remainingCredits` | Unused commission credit of the current billing period. Must not exceed the credit per period. |
| `billedThisYear` | Custody fees already billed in the current calendar year, for an annual cap. |
| `assumption` | A short explanation of where the amounts come from. |
| `cashaccount` | Optional: identifier of the cash account in the simulation that the fees are charged to. |

All amounts use the fee currency and exclude VAT. If you list an account, all three amounts and the explanation are required; zero must also be entered explicitly. Without an entry GT assumes zero. That is not enough, however, when the model charges fees and the simulation opens inside a billing period or the model has an annual cap. The start then requires an entry for that account. A fee already paid before the opening is part of the opening cash and is not charged again.

**Validate** checks only the structure of the document. When starting, GT additionally checks whether the names belong to active securities accounts of the simulation with a custody model and whether the amounts fit the model. An unknown name is rejected.

## Closing an account during the replay
If a securities account has an active-until date within the simulation period and the fee is billed in arrears in the middle of a billing period, set `closingPolicy` in the tariff to define what is charged at closing. Without it, GT refuses to start.

| `closingPolicy` | Charge at closing | Allowed with valuation |
|-----------------|-------------------|------------------------|
| `ACCRUED` | The daily or monthly observations accumulated up to the closing. | daily, monthly |
| `FULL_PERIOD` | The full fixed or period-end fee; the holdings on the closing date are valued. | none, period end |
| `PRORATED` | Fixed and period-end fees as well as the per-bill minimum and maximum, scaled by elapsed calendar days. | all |

Validation of the model rejects any combination outside this table. Fees paid in advance are not refunded at closing.

## Commission credits
Some brokers turn part of the custody fee into a credit for later commissions. In the replay, such a credit only reduces the commission of eligible trades, never transaction taxes. It is not cash and does not increase equity. Unused credit expires at the end of the billing period, and only successfully booked trades use it up.

To allocate the credit correctly, executions within a billing period must be processed in date order. If different exchange calendars cause an earlier execution to follow an already booked later one, the replay stops with a message rather than allocating the credit incorrectly.

## Avoiding double charges
Do not also schedule the same custody fee as a [cash standing order](../standingorders/). Mark an existing custody standing order with `[custody]` in its note. If that standing order charges the same cash account a custody model settles to, GT refuses to start until you have removed the standing order in the simulation. Other account charges are not part of the custody model; tax statements, postage, transfers and cash-account maintenance are not included.

## In the course and the result
In the [course of the replay](../#course-of-the-replay), every charge appears as **Custody fee** with its period, VAT and tariff source, and every use of a credit as **Trading credit used**. The charge itself is an ordinary fee booking on the cash account, linked to the simulated securities account. Under the calculation assumptions GT records whether custody fees are modelled.

Replaying again restores the opening state and recalculates all fees instead of duplicating earlier simulation charges.

## Currency conversion markups
The replay also uses the percentage FX tariff captured at its start. FX inherits independently: an account can override only the FX section and keep the plan's commissions and custody fees, or override commissions and custody while retaining the plan's FX tariff. A matching zero-percent account rule explicitly overrides inherited markup.
For a buy, sell or income payment that converts currencies, GT applies the percentage to the currency pair's EOD mid rate on the execution or payment day. The amount used for tariff tiers is the net amount before markup, including the relevant commissions, taxes and accrued interest. Funding transfers use the beneficiary securities account's tariff and the close on the funding day. Merely expressing a dividend in the instrument currency or valuing an amount in the reporting currency adds no separate markup. Intraday timing deviations are not simulated.
The calculation assumptions state whether conversions use EOD mid only or fee-model markup. Without a matching section, period, rule or tier-currency rate, the conversion uses mid; where FX modelling is active, uncovered conversions are counted and reported. The course of the replay then shows **Conversion has no FX tariff coverage** once for each account, currency direction, conversion kind and reason, and a booking with markup names its percentage and rule in its details. An invalid configuration fails the replay with **FX markup model failed**. **FX markup paid (tenant currency)** reports the economic cost of committed conversions, while **Conversions without FX tariff** counts their coverage gaps. Rejected orders and preliminary sizing calculations add nothing to either total. Captured runs from before FX modelling retain their mid-only convention.
Only percentage markups from zero up to, but excluding, 5% are supported; fixed conversion commissions and minimums are outside the model. If a commission rule already charges conversion through its settlement-currency condition, add an explicit zero FX override for those conversions to avoid duplicate modelling. Use the [observed FX deviations]({{% ref "/reportportfolio/transactioncosts" %}}) to compare recorded bookings with the tariff, while allowing for the report's execution-timing bias.
