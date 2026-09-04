---
title: "Historical price data"
date: 2026-08-19T22:54:47+01:00
draft: false
weight: 12
archetype: "default"
---
If an **instrument** is selected in a **watch list** or **securities account**, the **end-of-day prices** function is available **as a table**. They can also be accessed via "**Missing end-of-day prices**" or "**Completeness of historical securities price data**". This shows a table with the historical price data and other useful information relating to this **historical price data** in the **additional area**.

## Automatic update after market close
GT offers two methods for automatically updating historical price data. The administrator can specify which method to use via the **gt.update.price.by.exchange** setting in the [global settings](../../../../admindata/globalsettings/). A value of **0** uses the time-controlled update, a value greater than **0** uses the exchange-dependent update.

### Time-controlled update (Job 30)
With this classic method, all instruments are updated at a fixed time defined in `application.properties`. This background task performs other important tasks in addition to the price update and therefore cannot be deactivated. More on this under [Background tasks](../../../../admindata/taskdatachangemonitor/taskdescription/).

### Exchange-dependent update
This method takes into account the individual trading hours of the various stock exchanges. The historical price data is updated separately for each exchange once a certain time has elapsed after its market close. This offers the following advantages:
- **Earlier data availability**: Price data from exchanges with early closing times is available more quickly without having to wait for exchanges that close later.
- **Better data quality**: The update only takes place when the price data is expected to be complete at the data provider.
- **Distributed load**: Requests to data providers are spread throughout the day instead of all occurring simultaneously at a fixed time.

The following diagram shows the process of exchange-dependent updating:
{{< mermaid >}}
flowchart TD
    A[Background process checks exchanges] --> B{Exchange closed + waiting time elapsed?}
    B -->|No| C[Calculate waiting time until next exchange ready]
    C --> D[Wait until next exchange ready]
    D --> A
    B -->|Yes| E[Fetch price data from data provider]
    E --> F{Successful?}
    F -->|Yes| G[Save price data]
    F -->|No| H[Mark for later retry]
    G --> A
    H --> A
{{< /mermaid >}}

Instruments whose update fails are marked for a later retry. This allows temporary problems with the data provider to be resolved automatically.

{{% notice style="info" title="Logging for monitoring" %}}
A log is temporarily kept for monitoring the exchange-dependent update. This shows for each exchange when the update was performed and how many instruments could be successfully updated.
{{% /notice %}}

### Interaction of both methods
Even if the exchange-dependent update is activated, Job 30 remains active. It continues to perform important tasks such as maintaining completeness data, updating holding tables, and creating tasks for currency pairs without historical price data. However, when the exchange-dependent method is activated, the actual price update is handled by this method.

## Additional monitoring of historical price data
There are several functions in GT for monitoring **historical price data**. These can be found under [Completeness of historical price data](../../../../admindata/historyquotequality/) or [Price data feed](../../../watchlist/pricefeed/).

## Functions on historical price data
In addition to the usual editing functions of an information class, there is additional support for the most complete possible history of price data.

### Creating and editing historical price data
Only the **creator of** the **security** or a user with sufficient rights can create or delete historical price data.
+ **Create** a **historical price** via the **context menu**.
+ **Edit** a **historical price** on the **selected historical price**.
+ **Delete** a **historical price** on the **selected historical price**.

### Export as CSV file
The historical daily data can be exported. The export format corresponds to the import format.

### Importing historical data
You can find the import format in the export format. When importing historical daily data, the date and the closing price must be available. The assignment of the column to the import fields is determined from the first line. In the following example, the date was expected in the first column and the closing price in the second column. A **semicolon** is expected as the field delimiter.
```
date;close;volume;open;high;low
18.02.2021;78;;;;
```
Existing historical daily data cannot be overwritten with an import.

### Linear filling of missing price data
For example, it is possible that an instrument is removed from the exchange in order to merge it with another instrument, or that after a bankruptcy no prices are supplied at all any more. During this time, no price data is supplied, but GT needs the historical price data as completely as possible to calculate the periodic performance. The **Linear fill missing price data** function is used to fill the prices of the missing trading days. Optionally, possible weekend prices can be shifted to the Friday of the same week if this Friday had no price data; the option **Move weekend quotes to Friday** can only be chosen if there are any prices on a Saturday or Sunday at all.

You decide yourself up to which day the filling reaches, using the field **Fill up to**. The current day is proposed, so that prices for days that have not happened yet never arise by accident. The selectable period is the one allowed by the **Active until date** of the instrument and by the [trading calendar](../../../../basedata/instrumentbased/stockexchange/) of its exchange; the earlier of the two dates limits the choice. The trading days to be filled are derived from that same trading calendar.

This latest date quite often lies in the future, for instance for a bond whose **Active until date** is its maturity. Prices for future trading days can therefore very well be created — but you have to move the date forward deliberately. If the filling is to reach beyond the **Active until date**, that date has to be adjusted on the instrument.

If the trading calendar of the exchange is not yet maintained up to the last completed trading day, a note appears in the dialogue. It names the date up to which the calendar is maintained and the number of trading days that are therefore not covered. The filling cannot go beyond that date, because the exchange holidays past it are unknown and a holiday would otherwise receive a price. To fill those trading days as well, the trading calendar of the exchange has to be maintained first.

### Delete imported and/or linear filled
This function is the counterpart to **Linear filling of missing price data**: it removes the previously linear-filled end-of-day prices as well as the manually imported prices, leaving the quotes supplied by the **data provider** untouched. Existing fills or faulty imports can therefore be reset.

The fields **Date from** and **Date to** determine the period for which the deletion takes place. The whole range from the oldest to the most recent existing price is always proposed, so that without any further input the function resets everything selected in a single step. If a linear filling reaches into the future, those future prices belong to it as well. Outside that range the two dates cannot be set, because there are no prices there anyway, and the two fields limit each other, so a period with start and end swapped cannot arise.

The period is above all useful when a linear filling is to be withdrawn only in part. An example: you have filled an instrument that is no longer traded up to the current day and then learn that from a certain date on a new price applies, which falls in the middle of the range already created. You delete the prices from that date on, enter the new price by hand and afterwards fill linearly again.

Only the **creator of** the **security** or a user with sufficient rights may execute this function. For all other users the menu entry remains visible but is disabled, mirroring the behaviour of **Linear filling of missing price data**. Because the action is a bulk operation on shared price data, executing it does not consume the daily quota of a **limit user**.

### Archive of historical price data
When a security's data provider changes, or when a split triggers a complete reload, older prices that the new provider no longer supplies are preserved in a dedicated archive. See [Archive of historical price data]({{< relref "archive" >}}). Inside the archive view archived data can be inspected, individual prices edited or deleted, exported, imported, corrected with a forgotten split, or the whole archive cleared.

## Daily quota for retrieving historical price data
GT shall not be misused as a free data provider. Therefore, the number of **different instruments** for which historical price data can be retrieved is limited per user and day. Repeated retrievals of the same instrument on the same day count only once. Every view that loads the price history of an instrument counts, i.e. **EOD as table**, [EOD as line chart](../../../eodchart/) including the technical indicators, and the [Archive of historical price data]({{< relref "archive" >}}).

The administrator defines the quota in the [Limit information class]({{% relref "/admindata/entitylimit" %}}) view through the limit **Historical price retrieval (instruments per day)** of the limit type **Retrievals per day** (default 250 instruments for the role **User with limits**). For users with elevated rights no such limit is recorded from the outset, so they are unrestricted. In addition, the administrator can grant individual users a different daily quota for a limited time; this is done in the same dialog used to adjust the daily change limits of a **limit user**. Anyone exceeding the quota receives an error message and cannot load any further instruments not yet retrieved that day until the following day. Repeated violations can lead to the account being locked, which only the administrator can release.

{{% notice style="info" title="No impact in everyday use" %}}
A normal user will not notice this limit, as the quota is far above typical usage. Instruments already retrieved can be viewed again as often as desired.
{{% /notice %}}

## Properties and table columns
The properties are not discussed further here as they are self-explanatory or supported by Quckinfo.
