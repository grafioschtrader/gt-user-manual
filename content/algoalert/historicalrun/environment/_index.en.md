---
title: "Working in a simulation environment"
date: 2026-10-01T10:00:00+01:00
draft: false
weight: 5
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress - Target version V0.40.0" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
A simulation environment is a separated copy of your portfolios and accounts as of a day in the past. It is a tenant of its own: whatever you book, change or delete inside it stays inside it and never touches your real portfolio. How an environment comes about is described under [Rule based trading](../../algo/).

Use **Switch to simulation** to open an environment and **Switch to main tenant** to return. Both commands reload the application, so that the navigation tree, the menus and the reports really belong to the selected environment instead of still showing figures of the tenant you came from.

## What is possible in an environment
The overview below shows what you can work with in an opened environment while no replay is running, and what stays outside it. What decides is always where the data belongs: everything tenant-specific belongs to the environment, while the strategy and your user account belong to you.

| Area | In the environment |
|---|---|
| Portfolios, accounts, watchlists, correlation matrices, imports, tax corrections | Editable; they belong to the environment alone. Accounts and portfolios holding bookings of the opening balance cannot be deleted, however. |
| Name, currency and the other details of the environment itself | Editable as for your own portfolio. |
| Transactions | You can record, change and delete your own transactions, but they are transient. The transactions of the opening balance are protected. Please read the following sections. |
| The strategy with its asset classes, securities and rules | Read only. Change the strategy in your own portfolio and replay the simulation afterwards. |
| Alerts, including their evaluation and notifications, as well as switching monitoring on and off | Not available. Alerts watch current prices and therefore belong to your own portfolio only. |
| Instruments, currency pairs, asset classes, stock exchanges, import templates, historical prices | Editable within your usual permissions. This data is shared and belongs to no single environment. |
| Private instruments | Can be recorded; they belong to the environment and are deleted with it. |
| Cash and security standing orders | Can be recorded; they are executed by the [historical replay](../standingorders/) alone. |
| **Export personal data** and **Deleting my data and user account** | Not available. Both concern your user account and act on your own portfolio only, which is why the menu entries are not shown in an environment. |
| **Share read access** and client management | Not available. A share applies to your own portfolio, not to a copy of it. |

Should you nevertheless attempt one of these blocked functions, Grafioschtrader refuses it with a message instead of quietly changing something in the wrong place.

{{% notice style="info" title="The strategy is shared, but protected" %}}
The strategy is used jointly by your portfolio and by all of its simulation environments. So that a recorded replay stays verifiable, it cannot be changed from within an environment. Switch to the main tenant, adjust the strategy there and start the replay again afterwards.
{{% /notice %}}

{{% notice style="warning" title="Shared data takes effect everywhere" %}}
If you change an instrument, a currency pair or its prices in an environment, you change this data for your own portfolio and for all other users who use it. Grafioschtrader treats such a change exactly as it would in the main tenant and gives no additional warning inside an environment.
{{% /notice %}}

## The protected opening balance
When an environment is created, Grafioschtrader sets up its opening balance: the transactions copied as of the opening date, or the deposits an environment without positions starts with. Every replay resets to this balance. It is therefore protected and can be neither edited nor deleted.

In the transaction list, Grafioschtrader offers no command that would change a booking of the opening balance: **Edit**, **Delete**, **Conversion to account transfer** and **Toggle taxable status** are disabled. **Create Standing order** remains available, because a new standing order does not change the booking itself. Likewise, an account or portfolio holding bookings of the opening balance cannot be deleted; Grafioschtrader refuses this with the message "This account or portfolio contains opening transactions and cannot be deleted."

The opening date, the opening mode and the opening balance cannot be changed afterwards. If you want to calculate from a different starting point, create a new environment.

## Effect of the historical replay
A replay always begins at the opening balance of the environment. Before the first trading day is evaluated, Grafioschtrader returns the environment to exactly that state. Everything that is not part of the opening balance is deleted in the process:

- All transactions that do not stem from the opening. This affects the executions, income and standing order bookings of the previous replay as much as transactions you recorded in the environment yourself.
- A **Security Transfer** recorded in the environment and the **Security Actions** applied in the environment. The security action itself is shared and remains; you can apply it again afterwards.
- The previous result with its course.

The holdings are then rebuilt from the opening balance. Your settings, in contrast, are not reset: portfolios, accounts, watchlists and the details of the environment itself remain unchanged. A replay therefore does not restore the whole environment, only its transactions.

{{< mermaid >}}
graph TD
    A["Start replay..."] --> B{"Confirmation"}
    B -->|No| C["Nothing happens"]
    B -->|Yes| D["Historical replay dialog"]
    D --> E["Delete transactions outside the opening balance and the previous result"]
    E --> F["Rebuild holdings from the opening balance"]
    F --> G["Evaluate trading days"]
    G --> H["Completed: result with key figures"]
    G --> I["Cancelled or failed: partial result without key figures"]
{{< /mermaid >}}

Because your own bookings are lost in the process, Grafioschtrader asks before every replay: "Start replay? All transactions outside the protected opening balance, including your manually entered transactions, will be deleted and previous results replaced." If you decline, nothing is started and nothing is deleted.

{{% notice style="warning" title="Your own bookings are lost with the next replay" %}}
Transactions you record by hand in an environment are deliberately transient: the next replay removes them together with the executions of the last run. If you want to keep a booking permanently, it belongs in your own portfolio, or in an environment that you do not replay again.
{{% /notice %}}

A cancelled or failed run keeps what it has booked and recorded up to that point. You can look at this partial result, but no key figures are shown for it, because the run did not reach its end date. The next replay starts again from the opening balance.

## While a replay is running
A running replay books transactions continuously and rebuilds the holdings of the environment. While it works, the environment is therefore locked: it can neither be opened nor deleted, and shared data cannot be edited from within it either. **Switch to simulation** and **Delete simulation** are then not offered. A session that already has the environment open, in a second browser window for instance, receives the message "This simulation environment is currently being replayed. Wait until the replay has finished before opening it." on its next step and returns to the main tenant.

You follow the status and the course from the main tenant instead, and **Cancel replay** likewise. A cancellation ends the run after the day it is currently calculating; the executions booked until then remain. The environment becomes available again only once the run has actually ended, not already at the moment of the cancellation. Wait therefore until the status changes from **Running** to **Cancelled** before you open or delete the environment.

{{< mermaid >}}
stateDiagram-v2
    [*] --> Idle
    Idle --> Running: Start replay
    Running --> Cancelling: Cancel replay
    Cancelling --> Idle: Run ended
    Running --> Idle: Run ended
    Idle --> [*]: Delete simulation
    note right of Idle
        Opening, editing and
        deleting possible
    end note
    note right of Running
        Locked: neither opening
        nor deleting
    end note
{{< /mermaid >}}

Only one replay runs per environment at a time, and the server executes only a few at once overall. When that capacity is exhausted, Grafioschtrader says so and you try again later.

## Quantity limits
Every environment has a budget of its own: portfolios, accounts, watchlists and transactions count against the usual upper limits, but separately from your own portfolio and separately from the other environments. An environment can therefore not take away the room in your main tenant.

Grafioschtrader already checks at creation whether the copy fits into this budget. If it would contain more portfolios, accounts, watchlists, watchlist positions or opening transactions than allowed, the environment is not created, and the message names the exceeded limit, for example "The simulation would contain 3 portfolios; the limit is 2." A copy is never silently shortened.

A replay also counts its bookings against the transaction limit. When it reaches that limit, it books nothing further and ends with the status **Failed**. What was booked and recorded until then is kept as a partial result. In that case, choose an earlier end date or a strategy that trades less, or ask the administrator for a higher limit.

Shared data behaves differently. An instrument you create in an environment counts against the same budget as one you create in your own portfolio. The number of simulation environments per portfolio is limited as well; by default there are five. How these limits are managed is described under [Quantity limitation]({{% ref "/admindata/entitylimit" %}}).

The course of a replay has an upper limit too. Once it is reached, Grafioschtrader records no further entries, but the run itself finishes its calculation. The course is then incomplete while the result stays valid.

## Deleting an environment
**Delete simulation** removes the environment with everything that belongs to it alone: portfolios, accounts, transactions, watchlists as well as the result of the last replay with its course. Your own portfolio, the shared instruments and the strategy remain untouched. While a replay of the environment is running, Grafioschtrader refuses the deletion; cancel the run or wait for it to end.

Private instruments you created inside that environment are deleted along with it, including their prices. A private instrument belongs to the tenant it came about in and would be meaningless without it. If you create an instrument you want to keep beyond the simulation, record it in your own portfolio.

**Deleting my data and user account** first removes all simulation environments of your portfolio and then your own data and the user account. Both happen in one step: if anything fails, everything is kept. If a replay is currently running in one of your environments, the deletion is refused until the run has ended. The same applies when the data of a managed client is deleted: that client's environments are removed first as well.

{{% notice style="info" title="Environments are not part of the personal export" %}}
**Export personal data** delivers the data of your own portfolio. The contents of a simulation environment, that is its settings, private instruments, own bookings and the results of a replay, are not included in it and cannot be restored from the export either. This is intentional: an environment is a copy that can be created anew at any time, not a data set in its own right. Shared data such as instruments follows the usual export rules, regardless of whether you created it in an environment or in the main tenant.
{{% /notice %}}
