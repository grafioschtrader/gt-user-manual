---
title: "Widgets"
date: 2026-10-06T15:00:00+01:00
draft: false
weight: 10
archetype: "default"
---
This page describes the widgets that every signed-in user can see on the [Dashboard]({{% relref "/intro/dashboard" %}}). The additional cards of the **Administrator** role are under [Widgets for administrators]({{% relref "/intro/dashboard/adminwidgets" %}}). The investment cards **Biggest winners**, **Biggest losers**, **Last trading days** and **Development of the total value** appear only after a client has been set up.

Each card has stored settings that you change in draft mode with **Configure**. Some cards also have a setting that applies only to the current reading and is not saved.

## Unread messages
The card shows unread messages from your inbox. Opening the dashboard does not mark a message as read. The stored setting **Maximum rows** limits the table, defaulting to five rows and allowing 1 to 20. The heading names the total number of unread messages, even when the table is shorter.

The table contains **Nickname**, **Subject** and **Creation time**. If there are no entries, **No pending items.** is shown. **Open full view** leads to the [messaging system]({{% relref "/admindata" %}}), where you read, reply and set the read state.

## Open data-change requests
The card shows open [data change requests]({{% relref "/basedata" %}}). It has two lists, **For you** and **Submitted by you**. A request can appear in both lists, for example when you own the affected entity and submitted the request. **Maximum rows** applies to each list separately, also from 1 to 20, defaulting to five.

The tables contain **information object**, **Remark data change** and **Creation time**. **Open full view** leads to the tab **Data change requests for you** or **Your data change requests**.

Which requests appear under **For you** follows the same rights as the full view. Users in the roles **Users with limits** and **User without limits** see the open requests for entities they own. Users in the roles **Privileged user** and **Administrator** see every open data-change request of the installation, because they may edit shared data regardless of ownership.

## Biggest winners and Biggest losers
The two cards share the same ranking of held instruments and show opposite ends. **Biggest winners** lists the instruments with the strongest positive move, **Biggest losers** those with the strongest negative move. A move of exactly zero appears on neither card. Amounts are in the client currency.

Each card has three periods: **Intraday** for the current session, **Last trading day** and **Chosen day**. Each period is ranked twice, once **By change** as a percentage price move and once **By value of the move** as the effect on the client. The period nodes themselves have no total; summed ranking rows would not be a period figure.

The stored setting **Instruments per period** limits how many instruments appear per ranking and period, defaulting to three and allowing 1 to 5. You set the **Chosen trading day** on the card itself. That day is not stored; **Refresh** returns to the default, the trading day before the last completed one. A date that is not a trading day, or one after the last completed trading day, is moved back to the nearest valid trading day. Which periods you leave expanded is remembered in this web browser per card.

{{% notice note %}}
These cards answer which held instruments moved the most. They are not a period result and not an alternative to the **Last trading days** card.
{{% /notice %}}

Instruments without a usable pair of prices are missing from the ranking; the card states how many held instruments that concerns. Held margin positions are not covered by the ranking, also with a count. If no price is available for the dates of a period, the instrument stays in the list, the row carries an asterisk and the tooltip names the dates actually used, for example **Valued from … to …, because no price was available for the dates of this period.** Intraday last prices may be older than today; the tooltip then reads **Last price of …**.

An empty period is not silent. If no market held a session today, the Intraday period says so. If the trading calendar does not reach back far enough to name the two sessions of a closing-price period, that explanation appears instead of an empty ranking.

## Last trading days
The card shows what the client, or a single portfolio, gained or lost over the last completed trading days, next to the change of total value and the deposits and withdrawals that explain the difference. The calculation is the same as in the [Period performance]({{% relref "/reportportfolio/periodperformance" %}}) report.

The stored setting **Trading days shown** sets how many completed trading days the table contains, defaulting to five and allowing 2 to 10. If the history does not reach that far, the card shows the days that exist, without quietly choosing a different span. **Portfolio** restricts the card to one of the client's portfolios; the empty choice applies to the whole client. That choice is not stored. **Refresh** returns to the whole client.

Above the table stand the totals **Total Gain**, **Change of total value** and **External cash inflows, outflows**. The table itself, newest session first, additionally contains **Account/Depot real cost**, **Account interest real** and **Securities + Balance**. The days walk the same holidays and days with missing prices that the period-performance date picker blocks.

{{% notice note %}}
Fees and account interest are already contained in the result of the day and are shown to explain a movement no market caused. Do not add them to **Total Gain**.
{{% /notice %}}

If at least one held instrument was valued with a filled price on that day because it had no price of its own, a tooltip on the row explains this. If there are not two trading days with complete prices, **There are not two trading days with complete prices, so no change can be calculated.** is shown. If nothing was held over these trading days, that explanation appears.

## Development of the total value
The card draws the total value of the client, or of a single portfolio, as a line over time, next to the invested capital. The total value is **Securities + Balance** of the [Period performance]({{% relref "/reportportfolio/periodperformance" %}}) report; on every day both show, the values agree. The **Invested capital** is the deposits less the withdrawals accumulated up to that day. The distance between the two lines is therefore the gain or loss of the investments. All amounts are in the client currency or, when a portfolio is chosen, in the currency of that portfolio.

The stored setting **Period** sets the period the card opens with: **1 month**, **3 months**, **Year to date**, **1 year**, **3 years**, **5 years** or **Since the beginning**, defaulting to **1 year**. On the card itself, **Portfolio** restricts the chart to one of the client's portfolios; the empty choice applies to the whole client. The **Period** can be switched there as well. Neither choice on the card is stored; **Refresh** returns to the whole client and the stored period.

When you move the mouse over the chart, the tooltip shows the date, **Securities + Balance**, **Invested capital** and their **Difference**. Below the chart stands the **Newest day** with a value. For long periods, one point per trading day would be more than a chart of this size can show. If the period covers more than about 500 trading days, the card therefore shows the last value of each week and notes **One value per week is shown.** If that is still too many, it shows the last value of each month. The newest day is always included.

{{% notice info %}}
The card does not calculate anything itself. The daily values are computed in the background by [task 55]({{% relref "/admindata/taskdatachangemonitor/taskdescription" %}}#JOB55) and stored. After a new transaction or prices loaded later, the affected days are computed again; until then **The newest days are still being computed.** is shown below the chart. A day on which a price or an exchange rate of a held instrument is missing has no value, and the chart connects the neighbouring days.
{{% /notice %}}

Without values the card is not silent but states the reason. If the background task has never computed the client, for example shortly after it was set up or after an import, **The daily values have not been computed yet. A background task computes them, so try again later.** is shown. If nothing was held in the chosen period, **Nothing was held in this period.** is shown. No daily values are computed for a simulation environment, which the card reports as well.

### Maximize
This is the only card that can be maximized. The **Maximize** icon in the header of the card enlarges it to the whole dashboard area, much like a maximized window; the navigation tree on the left stays visible. The other cards are only hidden and keep their data. The **Restore** icon or the Esc key returns to the usual layout. The maximized state is not stored and also ends when you choose **Edit dashboard** or switch the client. The chart adapts to the size, also when you then move the splitter or resize the window.

## Rebalancing monitoring
The card shows whether the strategy you have assigned to [portfolio monitoring]({{% relref "/algoalert/algo" %}}#portfolio-monitoring) needs attention. It is available only when rule-based trading is switched on for this instance, and it has no settings. The card does not calculate anything itself but summarises the last daily evaluation, so opening the dashboard does not value any positions. Even while you work in a simulation environment, the card shows the monitoring of your own portfolio.

If no strategy is assigned, **No strategy hierarchy is assigned to monitoring.** appears, and if the assigned strategy has not been evaluated yet, **The monitored strategy hierarchy has not been evaluated yet.** Otherwise the card names the strategy and one of three states:

- **Nothing to do** means that the last evaluation proposes no trade.
- **Checkpoint due** states the number of proposed purchases, sales and blocked trades. A checkpoint of the [portfolio rebalance]({{% relref "/algoalert/strategy/rebalancing" %}}) is the time to act.
- **Drift beyond the tolerance, traded at the next checkpoint** means that the allocation deviates from its targets, but the next checkpoint has not been reached yet. GT reports the drift but proposes the trades only at the checkpoint.

Below that appear the **Valuation date**, the **Next checkpoint** and the **Largest bucket deviation (pp)** together with the name of the group concerned, the totals of the proposed **Sales** and **Purchases** in the tenant currency the number of **Mean reversion signals** that ask for a trade, and under **Outside the tolerance** the number of asset classes, securities and the investment ceiling currently beyond their tolerance, see [Allocation outside its tolerance]({{% relref "/algoalert/strategy/rebalancing" %}}#allocation-outside-its-tolerance). A figure without a value is left out rather than shown as zero. At most three of the largest proposed trades follow, sales before purchases, because a purchase made before the sale that funds it can exceed the investment ceiling.

**Open rebalancing report** leads to the report [Asset Classes with Cash]({{% relref "/reportportfolio/securitycashaccountreport" %}}) with the monitored strategy compared. There the comparison is calculated anew from the current holdings and shows every row.
