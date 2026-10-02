---
title: "Instrument without price data"
date: 2026-09-25T22:54:47+01:00
draft: false
weight: 67
archetype: "default"
---
[Period performance]({{< relref "/reportportfolio/periodperformance" >}}) values a client only on trading days for which every held instrument has a closing price. If a single instrument has no price that day, the whole day drops out of the report. Once that gap reaches the present, no usable trading day remains.

That is exactly what happens when the issuer of an instrument no longer supplies prices — for example a bond after a bankruptcy whose **Active until date** is still the maturity. The connectors keep trying and keep finding nothing. [Linear filling of missing price data]({{< relref "/watchlistinstrument/externaldata/historyquote/pricedata" >}}) closes the gap once; the next trading day is another gap, and the reports of everyone holding the instrument stall again.

## What the marker does
A row in this table is a statement about the **shared instrument**, not about a single client. That is why an instrument appears only once. After the connectors have run, background task [30 - Historical and intraday course update with completeness tracking and split calendar update]({{% relref "/admindata/taskdatachangemonitor/taskdescription" %}}#JOB30) fills the missing trading days of every marked instrument with the same linear filling as **Linear filling missing EOD**, up to the last completed trading day. If the data provider still delivers a price on a given day, that price remains; only the days around it are created. A gap at the end of the series is carried forward from the last known close. Prices already written are not changed again.

The data connector stays on the instrument. A price that is delivered later is stored in the usual way.

{{< mermaid >}}
flowchart TD
    A[End-of-day task 30 starts] --> B[Connectors fetch available prices]
    B --> C[Marked instruments: fill missing trading days linearly]
    C --> D[Period performance has complete days]
{{< /mermaid >}}

A linearly filled price is not a traded price. If GT values an instrument with such a price, the **Price** column appears yellow, see [Price freshness]({{< relref "/watchlistinstrument/watchlist/pricefreshness" >}}).

## Effect on the simulation
Whether an instrument still delivers prices and whether it can still be traded are two different questions. The bond of a failed issuer can go on being quoted although the exchange and the brokers stopped trading it long ago. That is why there is a second, independent date next to **No data since**: **No trading since**.

From the day **No trading since** on, the [historical simulation]({{% relref "/algoalert/historicalrun" %}}) neither buys nor sells the instrument. It generates no further coupons and does not repay a bond at maturity. An existing position stays held and is valued at its last price. Without this date the simulation treats the instrument as tradable. **No data since** has no effect on the simulation.

If the data provider still delivers prices for an instrument that can no longer be traded, leave **No data since** empty and set only **No trading since**. A simulation run that has already completed keeps the date it was calculated with; a change only affects a new run.

## Operation
The table is reached from the **navigation tree** under **Base data → Instrument without price data**. For each marked instrument it shows:

- **Security** — name of the instrument.
- **ISIN** and **Currency** — to identify it.
- **No data since** — from which day you consider the issuer to have stopped delivering prices. The date is documentation only; filling always starts from the last closing price that is actually present.
- **No trading since** — from which day the instrument can no longer be traded. From that day on the historical simulation no longer trades it, see [Effect on the simulation](#effect-on-the-simulation).
- **Newest delivered price** — the most recent closing price that a data provider, an import or you yourself recorded, so not a filled price.
- **Newest price** — the most recent closing price of any kind, filled days included. When this date is later than **Newest delivered price**, the end-of-day task is doing its work.
- **Note** — why the instrument was marked.

A new row is added through the **plus icon** in the table header. The instrument is chosen in the **Set security** dialog; the search is restricted to securities, derived instruments and currency pairs do not appear. After saving, the instrument can no longer be changed — the row is about that instrument. An existing row is edited through the **pencil icon** and deleted through the **trash icon**.

{{% notice style="info" title="At least one closing price" %}}
An instrument without any closing price cannot be marked. Filling needs a known price to start from and would otherwise silently do nothing.
{{% /notice %}}

If the instrument is already marked, GT rejects the second row with a message.

## User rights
Only an instrument that you may edit yourself can be marked. Users with the role **User with limits** and **User without limits** may therefore mark only instruments they created. Users with the role **Privileged user** and **Administrator** may mark any instrument.

Unlike other shared data, **no data change request** is created here. Someone who may not edit the instrument is refused. A waiting change request would leave the reports of every holder broken until someone approved it — which is the situation this marker exists to end.

For the role **User with limits** the limits of the information class **Instrument without price data** apply in addition: three changes per day and 200 rows per creator. The administrator maintains both values under [Limit information class]({{% relref "/admindata/entitylimit" %}}).
