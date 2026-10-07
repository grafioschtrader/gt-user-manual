---
title: "Historical replay"
date: 2026-10-01T12:00:00+02:00
draft: false
weight: 15
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress - Target version V0.40.0" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
A historical replay evaluates the strategies of a simulation environment on every trading day after its opening date and books the resulting orders as ordinary transactions inside the simulation. This shows how your allocation and your strategies would have behaved in the past, without touching your real portfolio.

An existing simulation environment is required. How to open one is described under [Rule based trading](../algo/).

Regular deposits, withdrawals, cash-account interest, and fees as well as savings plans in individual securities can be modelled with [standing orders](./standingorders/) inside the simulation. Which commissions and custody fees are charged is described under [Fees in the replay](./fees/).

## Starting a replay
Choose **Start replay...** from the context menu of a simulation environment. Because every replay deletes all bookings outside the protected opening balance, including those entered by hand, GT first asks for a confirmation; only then does the dialog **Historical replay** open. The entry appears only in the main tenant and only with write permission: the replay books its executions as the owner of the environment, so it cannot be started from inside an entered simulation. If you have switched into a simulation, return first with **Switch to main tenant**.

In the dialog you supply the **End date**; the beginning is always the opening date of the environment and cannot be changed. This keeps it verifiable what a recorded result was calculated from. The end date must fall after the opening date and before the current day, because a day still in progress has no closing price to decide against. How many trading days a replay may cover at most is set by the administrator in the global settings with `gt.simulation.max.run.trading.days`. Between 300 and 7500 trading days are allowed, the default is 3000, roughly twelve years.
You can additionally switch on **Apply simulation tax models**. GT then estimates trade taxes and income withholding from the [simulation tax models]({{% ref "/algoalert/historicalrun/taxmodel" %}}) of the tax countries. The option is off by default. **Generate regular bond coupons** is independent of that, uses the data in the [Bond simulation]({{% relref "/watchlistinstrument/instrument/securityderived/security#tab-bond-simulation" %}}) tab of the security and creates contractual coupons when a direct fixed-rate bond has no stored distributions.

**Opening custody balances (YAML)** uses the same [YAML editor]({{% ref "/intro/userinterface#yaml-editor" %}}), with field suggestions, hover help and **Validate**. The mapping keys are the names of securities accounts in the simulation; the editor suggests fields inside each account entry. **Validate** checks the document structure without starting a replay. Starting also checks the accounts and opening balances. See [Fees in the replay](./fees/#opening-custody-balances) for an example and when opening balances are required.

**Start replay** hands the work to the server. It continues in the background even if you close the dialog.

## Which state of the strategy a replay uses
A simulation environment holds no copy of its own of the strategy; it refers to the strategy in your main tenant. Every **Start replay** reads that strategy again, in the state it has at the start. The weightings and the selection of securities are fixed for the whole run at that moment. **Replay settings** shows exactly this fixed state.

While a replay of an environment is running, its strategy cannot be changed, not even in the main tenant. A change in the middle of a run would only apply to the remaining days, and the result would no longer match the recorded settings. Change the strategy before or after a run instead.

If you change the strategy between two replays, the second replay gives a different result as soon as the change affects a decision. The new result replaces the previous one. To compare two states of a strategy, note the first result before you change the strategy and replay again.

{{< mermaid >}}
graph LR
    A["Strategy R"] --> B["Create simulation environment"]
    B --> C["Start replay: reads R, result 1"]
    C --> D["Change R in the main tenant"]
    D --> E["Start replay: reads the changed R, result 2 replaces result 1"]
{{< /mermaid >}}

If the strategy is not ready at the start, for instance because the weightings of a level do not add up to 100%, GT refuses the start and names the reason. Which findings prevent a start is described under [Readiness of the strategy](../algo/#readiness-of-the-strategy).

## How a replay proceeds
Before the first trading day, GT returns the environment to its opening state: everything a previous replay produced is removed, while the opening holdings remain. A repeated execution therefore always begins from the same starting point instead of building on the positions of the previous run.

Each trading day is then evaluated in turn. GT first books due cash and security standing orders, followed by income and instrument end-of-life events. It then values the environment at that day's closing prices, checks whether a rebalancing is due, and evaluates the strategies of the individual securities. A rebalancing is due only at a checkpoint, which recurs after the interval derived from the number of redeployments per year; how the checkpoint and the tolerance work together is described under [Portfolio Rebalancing](../strategy/rebalancing/). An order that arises at the close of a day is never executed on that same day, but at the closing price of the security's next eligible trading day. There is no market execution before an instrument's start date or after its end date; for a direct bond, the end date itself is reserved for redemption.

{{< mermaid >}}
graph TD
    A["Restore the opening state"] --> B["Next trading day"]
    B --> C["Process standing orders, income, and instrument end dates"]
    C --> D["Value the environment at the close"]
    D --> E["Rebalancing due?"]
    E --> F["Evaluate the securities' strategies"]
    F --> G["Execute orders at the next eligible close"]
    G -->|yes| B
    G -->|no| H["Calculate the figures and finish the run"]
{{< /mermaid >}}

{{% notice style="info" title="Past prices only" %}}
A decision sees prices up to and including the day on which it is taken, and no further. Later prices and the current day's price are unreachable for the replay. Only this makes a result meaningful at all.
{{% /notice %}}

## Following the progress and cancelling
The dialog shows the **Status** of the run and, under **Trading days** and **Evaluated**, how far it has come. **Refresh** fetches the current state; while a run is working this also happens on its own. **Cancel replay** stops the run after the day it is currently evaluating. The environment stays locked until the run has actually ended, not merely until you ask for the cancellation. The executions booked until then remain in the environment, but no figures are reported for a cancelled run.

| Status | Meaning |
|---|---|
| **Running** | The run is queued or currently working. |
| **Completed** | The run reached its end date; the figures are valid. |
| **Cancelled** | You stopped the run. |
| **Failed** | The run ended on an error; the reason is shown in the dialog. |
| **Interrupted** | The server was stopped while the run was working. Such a run is not resumed but started anew. |

Only one replay can run per simulation environment at a time, and the server executes only a few at once overall. When that capacity is exhausted, GT says so and you try again later.

## Result
The card **Run** records what the run was calculated with: **Status**, **Opening date**, **End date**, the two chosen options, **Started**, **Finished** and the **Fallback dividend payment delay (calendar days)**, see [Income and instrument end dates](#income-and-instrument-end-dates). A completed run reports the following figures under **Result**. They refer to the equity of the simulation environment, valued on every evaluated day at that day's closing prices. The opening holdings are capital, not profit. Deposits and withdrawals from [cash standing orders](./standingorders/) are likewise treated as external capital flows rather than return.

| Figure | Meaning |
|---|---|
| **Total return** | Change in equity from the opening day to the end date. |
| **Annualized return** | The total return scaled to a calendar year. |
| **Maximum drawdown** | The largest decline from an intermediate peak of the equity. |
| **Maximum drawdown duration (days)** | The longest time in calendar days the equity spent below a previous peak: from the peak to the first day it is regained. Every such phase counts, not only the one of the largest decline. If the peak has not been regained by the end date, the phase counts up to the end date. Deposits and withdrawals neither end nor shorten a phase. |
| **Sharpe ratio** | Mean daily return divided by its deviation, scaled to a year. |
| **Paid gross dividends (tenant currency)** | Sum of the dividends paid out up to the end date, before tax deductions. |
| **Unpaid gross dividends (tenant currency)** | Dividends whose entitlement arose up to the end date but whose payment date lies after it. |
| **Trades** | Number of fully closed engagements. A position still open on the end date does not count; its value is part of the closing equity. |
| **Winning trades** and **Losing trades** | How many of the closed trades brought in more than they cost, and how many brought in less. |

A figure that cannot be determined stays empty instead of showing zero. Equity that never moved yields no Sharpe ratio, and a run with only one evaluated day yields no return.

### Equity curve
For a completed run, the menu entry **Equity curve** shows the course of the equity below the view as a chart. While a run is still running, or after it was cancelled or failed, the entry stays disabled. The chart contains two lines. **Equity** is the equity of the simulation environment at the closing prices of every evaluated day, the same series from which GT calculates the return, the maximum drawdown and its duration. **Invested capital** starts at the equity on the opening day and changes only through deposits and withdrawals, for example from [cash standing orders](./standingorders/). The gap between the two lines is therefore what the replay has earned or lost up to that day; a deposit raises both lines alike and does not appear as a gain.

A day that GT could not value completely, for instance because a closing price or an exchange rate was missing, is not a point of the curve; the [course of the replay](#course-of-the-replay) names it. If the end date is not a trading day, its valuation is added as the last point. The slider below the chart zooms into a period.

## Calculation assumptions
Figures are only as meaningful as the assumptions they were produced under. GT therefore records these with every run and lists them with the result.

Execution takes place at the closing price of the security's next eligible trading day, without price deviation on execution. If the securities account used or its trading platform plan has a configured fee model, its commissions and any custody fees are included; otherwise no fees are charged. If the model does not cover an execution date, the replay stops instead of booking the trade free of charge. Details are given under [Fees in the replay](./fees/). The gain or loss of a trade is measured in the currency of the security. The Sharpe ratio is calculated against a risk-free rate of zero; the return is annualized over calendar days, the deviation over 252 trading days.

### Cash accounts and currencies
An order is always paid by exactly one cash account and is never split across several accounts. A security the environment already holds stays with the securities account and cash account through which it was opened or first bought. For a new security, the **Primary securities account** or **Secondary securities account** of the security or its asset class in rule-based trading applies, provided that account may trade the security on that day. Otherwise GT chooses by itself: first come the portfolios that hold a cash account in the currency of the security. Within a portfolio, GT tries the cash account in the currency of the security first, then the one in the currency of the environment, and then the rest. The first account that can pay the whole order is taken.

A purchase through a cash account in another currency needs no preceding currency exchange. The conversion takes place within the buy or sell transaction itself, at the closing rate of the currency pair on the execution day, which is the same day as the price of the security. If the captured fee model supplies an FX tariff, its percentage markup is applied; otherwise the conversion uses mid and its tariff coverage is reported. Intraday timing deviations are not modelled. See [Currency conversion markups](./fees/#currency-conversion-markups). If the exchange rate for that day is missing, the order is not executed, with the reason "No exchange rate for the settlement currency".

When several purchases follow one another, an earlier purchase can use up the cash account in the currency of the security. A later purchase of another security in the same currency is then settled entirely through a different account, for example the one in the currency of the environment, with the conversion inside the transaction. This requires that this account can pay the whole order.

If no single account can pay the order although the environment as a whole holds enough money, GT moves only the missing amount from other cash accounts of the environment. For a new security the target is the cash account in the currency of the environment in the same portfolio; for a security already held, it is that security's own cash account. The money comes first from accounts in the currency of the target, then from the same portfolio, and only then from other portfolios. The transfer is booked on the decision day, because money arriving only on the execution day would not yet cover the purchase. It is therefore converted at the closing rate of the decision day. In the course of the replay the transfer appears as **Cash moved to fund an order**. From the first such transfer on, the currency mix of the environment no longer matches the one at its opening. If the missing money is held in a foreign currency, it can be converted twice: when it is moved into the currency of the environment, and when the purchase converts it back into the currency of the security. Both conversions can carry the applicable percentage markup. The funding transfer uses the beneficiary securities account's tariff; the change in the mid rate between the two days also affects the result.

If the money is still not enough, the purchase is cut to the amount that can be paid ("Reduced to what the cash account could pay"). If not even one tradable unit can be paid, it is omitted ("The environment holds no money left for this order"). Money is moved for orders only during rebalancing and the initial purchase. Orders of a strategy such as the [Mean Reversion Dip](../strategy/meanreversiondip/) are paid only from the chosen account and are reduced if necessary. The proceeds of a sale become available only from the following day. GT completes a rebalancing purchase that fell short for this reason on the following trading days.

{{< mermaid >}}
graph TD
    A["Purchase order"] --> B{"Can a single cash account<br>pay the whole order?"}
    B -->|yes| C["Buy through this account,<br>converting a foreign currency at the<br>closing rate of the execution day"]
    B -->|no| D["Move the missing amount on the decision day<br>from other cash accounts"]
    D --> E{"Enough money now?"}
    E -->|yes| C
    E -->|no| F["Reduce the order or<br>do not execute it"]
{{< /mermaid >}}

### Overdraft interest
An account of the simulation environment whose **Borrowing rate** is above zero bears interest for every calendar day on which it is negative at the end of the day. GT calculates by the usual convention for current account overdrafts: negative balance times borrowing rate divided by 360. A weekend or holiday counts with the balance of the last day before it. The accrued interest is charged at the last calendar day of every month and on the end date as account interest with a negative amount; from the following day on it is itself part of the overdrawn balance and bears interest too. The opening day belongs to the starting state and bears none.

The borrowing rate at the start of the replay applies. An account with a borrowing rate of zero may be overdrawn but costs nothing. Overdraft interest is an expense and not a capital flow: it lowers the equity and the return, but not the invested capital of the [equity curve](#equity-curve). If an interest charge cannot be booked, the replay stops, because its result would be wrong without the interest.

### Income reconciliation and warnings
When tax models or generated bond coupons are involved, a completed run additionally shows **Income reconciliation**. **Gross payments**, **Withheld tax**, and **Net payments** are reported separately for Interest/Dividends and Securities interest. Outstanding claims appear as **Gross receivables and coupon accrual**, **Estimated withholding**, and **Net receivables and coupon accrual**. **FX and rounding reconciliation** discloses differences that currency conversion and rounding can introduce between gross, tax, and net amounts.

Under **Tax and income warnings**, GT groups equivalent warnings and shows their count and affected period. If a country lacks a model section, period, required input, or matching rule, GT omits only that country's contribution; valid contributions from other countries remain and the replay continues. An explicit zero rule instead counts as complete coverage. If estimated withholding exceeds gross income, that withholding is not booked, preventing negative net income. No tax warnings are generated while tax models are switched off.

{{% notice style="warning" title="Not a substitute for a real trading simulation" %}}
Price deviations on execution are not included. Without a configured fee model, transaction costs are absent as well. A result can therefore be more favourable than actual trading, especially for frequently traded strategies.
{{% /notice %}}

## Income and instrument end dates
A stored dividend creates an entitlement on the ex-date for the units held before that day and is booked as cash on the payment date. If the payment date is missing, GT uses the ex-date plus a fallback delay in calendar days set by the administrator with `gt.simulation.dividend.payment.delay.days` (1 to 32, default 16). The run records the delay used. For direct fixed-rate bonds, GT can additionally generate regular coupons from the bond terms, which are entered in the **Bond simulation** tab of the [security]({{% relref "/watchlistinstrument/instrument/securityderived/security" %}}). Without a stored day-count convention, the usual convention of the currency is applied. Stored distributions take precedence. Generation supports regular fixed-rate periods; irregular first or last periods, floating rates, and business-day adjustments are not modelled.

A direct bond is redeemed at par on its end date. If no par value per unit is configured, GT uses 100. This principal repayment is independent of **Generate regular bond coupons**. When regular coupons are generated and redemption falls on a coupon date, the full coupon is booked separately. When it falls between coupon dates, the redemption includes interest accrued up to that date. The redemption itself carries no securities-account fee or trade tax; any withholding on the coupon remains separate.

Other non-margin positions are closed at the last available closing price on or before the instrument's end date. The normal fee and tax models apply. If there is no usable price, the position stays open and the course shows **Unavailable**. If the instrument ended before the replay's first trading day, GT catches up the redemption or close on the first evaluated day.

If a security is marked under [Instrument without price data]({{% relref "/basedata/bankruptsecurity" %}}) with **No trading since**, the replay no longer trades it from that day on: neither strategies nor rebalancing buy or sell it. From that day, regular bond coupons are no longer generated and no further accrued interest is recognised; stored distributions, by contrast, are kept. If the date lies before the instrument's end, the redemption at maturity and the close at instrument end are omitted as well. The position stays held and is valued at its last price. Only **No trading since** counts; **No data since** has no effect on the replay. The run records the date when it starts, so a later change only affects a new run.

## Course of the replay
Below the result GT lists what happened on which day. The course explains a run rather than merely summarising it: it equally records why nothing happened, because otherwise a run without trades and a run without price data would look the same.

| Column | Content |
|---|---|
| **Date** | The closing day the entry belongs to. |
| **Event** | The kind of entry, see below. |
| **Reason** | Why it was decided this way, for example a triggered entry or an exhausted investment limit. |
| **units**, **Price**, **Amount**, **Currency** | Size and price, where the entry has them. |
| **Details** | Supplementary information, such as the reason for a cancellation. |

The entries **Replay started** and **Replay ended** enclose the run. Between them, **Opening liquidation** marks a CFD, Forex or leveraged position of the opening state that is closed before ordinary simulation trading, **Cash standing order** a recurring cash booking, **Security standing order** a recurring purchase or sale of a security, **Decision** a triggered entry or exit, and **Execution** its booking on the following trading day. **Rebalancing due** and **Rebalancing execution** record the same for the alignment with your target weightings. **Initial purchase due** and **Initial purchase execution** appear only for an environment that held no positions on its opening date: it buys into its target weightings once at the beginning of the run, and only afterwards does the checkpoint interval of the rebalancing start counting. An environment that opens with positions goes straight to its first rebalancing instead. **Cash moved to fund an order** marks a transfer between two cash accounts of the environment so that an order can be paid for; it names the security it was made for. **Dividend entitlement** and **Dividend payment** record the entitlement arising on the ex-date and its payout. **Maturity redemption** identifies a direct bond repayment and **End-of-life close** the closing of another position. **Custody fee** records a billed custody fee, **Overdraft interest** the interest charged to an overdrawn account, **Trading credit used** a commission reduced by a credit. **Blocked** means an action was deliberately omitted, for instance because of a cooldown or an exhausted investment limit. **Unavailable** means no decision could be taken at all, because price data or an exchange rate was missing.

## Limitations
CFDs, Forex and instruments with a leverage factor other than 1 are not traded in a replay. If the opening state contains such positions, they are closed before ordinary simulation trading (**Opening liquidation**), are not bought again afterwards, and their weighting is redistributed to the other securities for that run. An order for which no further eligible trading day follows within the replay stays unexecuted. Where closing prices or exchange rates are missing for a day, no decision is taken for the affected security; the run nevertheless continues its work and records the reason in the course.

Money can only be moved when a currency pair exists in the direction from the giving account to the receiving account. GT usually keeps currency pairs from a foreign currency into the currency of the environment, for example USD/CHF, but not the reverse. Money can therefore be brought into the account in the environment currency, but hardly ever out of it into a foreign currency account. If a security already held is settled through a foreign currency account, that account can as a rule only be topped up from accounts in the same currency. If their money is not enough, the purchase is reduced even though the environment might still hold enough money in its own currency.

Executions of a rebalancing carry no assignment to a strategy, because the accounting keeps such an assignment only for strategies of the tactical trading level. The strategy involved is visible in the course instead. Where a security is claimed both by an asset class with a target weighting and by a strategy of its own, that strategy reports the security as undecidable. Assign a security to one asset class only.

{{% notice style="info" title="Results are replaced" %}}
Exactly one run is kept per simulation environment. Replaying again restores the opening state and replaces the previous result together with its course. To compare two periods side by side, create a second simulation environment. **Delete simulation** removes its run as well; while a replay is working, deleting is refused. What else is possible in an environment, and what is protected there, is described under [Working in a simulation environment](./environment/).
{{% /notice %}}
