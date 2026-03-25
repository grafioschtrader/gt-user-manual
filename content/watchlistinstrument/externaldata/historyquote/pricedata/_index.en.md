---
title: "Historical price data"
date: 2026-01-26T22:54:47+01:00
draft: false
weight: 12
archetype: "default"
---
If an **instrument** is selected in a **watch list** or **securities account**, the **end-of-day prices** function is available **as a table**. They can also be accessed via "**Missing end-of-day prices**" or "**Completeness of historical securities price data**". This shows a table with the historical price data and other useful information relating to this **historical price data** in the **additional area**.

## Automatic update after market close
GT offers two methods for automatically updating historical price data. The administrator can specify which method to use via the **gt.update.price.by.exchange** setting in the [global settings](../../../../admindata/globalsettings/). A value of **0** uses the time-controlled update, a value greater than **0** uses the exchange-dependent update.

### Time-controlled update (Job 0)
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
Even if the exchange-dependent update is activated, Job 0 remains active. It continues to perform important tasks such as maintaining completeness data, updating holding tables, and creating tasks for currency pairs without historical price data. However, when the exchange-dependent method is activated, the actual price update is handled by this method.

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
For example, it is possible that an instrument is removed from the exchange in order to merge it with another instrument. During this time, no price data is supplied, but GT needs the historical price data as completely as possible to calculate the periodic performance. The **Linear fill missing price data** function is used to fill the prices of the missing trading days. Optionally, possible weekend prices can be shifted to the Friday of the same week if this Friday had no price data.
- The possible trading days are determined on the basis of the trading calendar of the respective instrument.  The tracking date of the corresponding trading calendar is also taken into account. Dates later than this do not trigger a fill. The data up to the current date minus 1 day or as long as the instrument is traded is taken into account.

## Properties and table columns
The properties are not discussed further here as they are self-explanatory or supported by Quckinfo.
