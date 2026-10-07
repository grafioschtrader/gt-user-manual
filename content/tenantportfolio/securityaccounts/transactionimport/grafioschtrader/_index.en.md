---
title: "Grafioschtrader import"
date: 2026-09-28T10:00:00+02:00
draft: false
weight: 25
archetype: "default"
---
GT can read back its **own exports**. These include the [CSV export of the transactions](../../../../reportportfolio/transactionlist/) as well as the [transaction receipts as PDF](../../../../transaction/receipt/). A special **import template group** named «**Grafioschtrader**», shipped with GT, is used for this re-import.
{{% notice info %}}
Unlike the template groups for a trading platform that you create yourself, the «Grafioschtrader» template group is **unique** and is already delivered ready-made with GT. It should neither be edited nor created a second time. A general description can be found under [Import template group](../../../../basedata/imptranstemplate/).
{{% /notice %}}
## Prerequisite
For the re-import to be offered, two things have to come together. An administrator must have designated the delivered «Grafioschtrader» template group once for the whole installation, see [Import template group](../../../../basedata/imptranstemplate/), and on the [Client](../../../client/) the check box «**Enable Grafioschtrader import templates**» must be set. Only then is the selection described below available.
## Usage
When both prerequisites are met, the check box «**Use Grafioschtrader import templates**» appears at both places of the transaction import: on the one hand in the dialog for creating or editing an **import group**, on the other hand when dropping a PDF document via **drag & drop**. When the box is checked, GT uses the templates from the «Grafioschtrader» template group for this import instead of the templates of the securities account's regular **trading platform**. This allows a GT export to be read into any **securities account** without changing its trading platform assignment.
## One file per securities account
The CSV export creates one file per **securities account**. Import each file into the corresponding securities account. This way the transactions return to the same securities account they originate from.
## Connect account transfers
An **account transfer** between two portfolios is split across two files on export, because every portfolio has its own securities account. If these files are imported separately, two unconnected entries are created at first, a withdrawal and a deposit. A transfer between two accounts of the same portfolio, on the other hand, is contained in the same file and is already connected on import. The menu item «**Connect account transfers**» at **tenant level** in the [Transactions](../../../../reportportfolio/transactionlist/) report restores the missing connections afterwards. It may be run as often as you like, because transactions that are already connected are not touched again.

GT considers all unconnected deposits and withdrawals of the tenant. A withdrawal and a deposit only qualify as a pair if they carry **the same minute** and lie on two different accounts. First, pairs of **the same currency** whose amounts agree are searched for. Then the remaining transactions with **different currencies** are checked: the exchange rate that results from the two amounts may deviate by at most 8 % from the close of the currency pair on that day. The same limit also applies when you enter an account transfer manually. A transaction that already has a matching counterpart in the same currency is not additionally compared with a foreign currency. A pair is only connected if the assignment is **unambiguous**, meaning that neither of the two transactions has a second matching counterpart. Each pair is then checked in the same way as a manually entered account transfer.
{{< mermaid >}}
graph TD
    A[Unconnected withdrawal and deposit] --> B{Same minute and different accounts?}
    B -- No --> X[No pair]
    B -- Yes --> C{Same currency?}
    C -- Yes --> D{Same amount?}
    C -- No --> E{Rate at most 8 % from the close?}
    D -- No --> X
    E -- No --> X
    D -- Yes --> F{Only matching counterpart?}
    E -- Yes --> F
    F -- No --> M[Ambiguous, skipped]
    F -- Yes --> G{Check as for an entered account transfer}
    G -- Passed --> V[Connected]
    G -- Failed --> R[Rejected]
{{< /mermaid >}}
### The message
After the run GT reports four numbers. Note that two of them count individual **transactions** and two of them count **pairs** of two transactions each.

**Checked** is the number of unconnected deposits and withdrawals that were considered. It also includes transactions that have no counterpart at all, for example an ordinary deposit from outside. **Connected** is the number of pairs that now form an account transfer. **Ambiguous** is the number of transactions for which more than one matching counterpart exists. GT does not guess in this case but leaves these transactions unchanged. **Rejected** is the number of pairs that match unambiguously but did not pass the check of an account transfer. Reasons include an overdrawn account, a date in a period that has already been closed, or an exchange rate that deviates too far from the close. The message additionally names the transactions **without a counterpart**. They are the checked transactions minus the connected, ambiguous and rejected transactions.

An example: 58 checked transactions with 26 connected pairs, no ambiguous transaction and no rejected pair mean that 52 transactions were connected and 6 transactions remain without a counterpart. These are usually genuine deposits or withdrawals that were never part of a transfer.
### Limits of the assignment
The export stores the time of a transaction to the minute. If several transfers in the same currency with the same amount take place in the same minute, they cannot be told apart and remain ambiguous. With different currencies, the rate check can only take effect if a close exists for the currency pair on that day. If it is missing, any combination of amounts fits, and several transfers on the same day remain ambiguous. If two transfers between the same currencies with a similar rate lie in the same minute, the rate check cannot separate them either.

You connect ambiguous or rejected transactions manually. To do so, delete one of the two transactions and turn the other one into an account transfer with «**Conversion to account transfer**». GT creates the counter entry anew.
### Export from a simulation environment
If the files come from a [simulation environment](../../../../algoalert/historicalrun/environment/), a few particularities have to be considered. The transactions of a simulation carry only a date and no time of day. As a result, all transfers of a day fall into the same minute, and the amounts alone are often no longer sufficient for an unambiguous assignment. This mainly affects the transfers «**Cash moved to fund an order**» that GT carries out between the portfolios during a historical replay. There are often several of them on the same day, and they are mostly made into another currency. In these cases only the rate check separates the transfers from each other. The percentage markup on the exchange rate included in the simulation is usually well within the permitted deviation.

The deposits with which the opening balance of the environment starts become ordinary deposits on import. They have no counterpart and therefore rightly remain unconnected. In the message they appear among the transactions without a counterpart.

For the rate check to work, the tenant you import into must have closes for the affected currency pairs on the days of the transfers. This is the case for the common currencies. For a rarely used currency pair it is worth checking its price data before connecting.
## Limitation
Opening and closing positions of **margin trades** are marked in the export and skipped on import. Such positions have to be entered manually.
{{% notice note %}}
This route via export and import with the Grafioschtrader templates also serves **testing purposes**. It provides reproducible test data for checking the transaction import at any time, so that no real exports from banks have to be used.
{{% /notice %}}
