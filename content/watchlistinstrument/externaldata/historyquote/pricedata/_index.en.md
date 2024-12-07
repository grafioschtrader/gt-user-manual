---
title: "Historical price data"
date: 2021-05-22T22:54:47+01:00
draft: false
weight: 12
archetype: "default"
---
If an **instrument** is selected in a **watch list** or **securities account**, the **end-of-day prices** function is available **as a table**. They can also be accessed via "**Missing end-of-day prices**" or "**Completeness of historical securities price data**". This shows a table with the historical price data and other useful information relating to this **historical price data** in the **additional area**.

### Additional monitoring of historical price data
There are several functions in GT for monitoring **historical price data**. These can be found under [Completeness of historical price data](../../../../admindata/historyquotequality/) or [Price data feed](../../../watchlist/pricefeed/).

### Functions on historical price data
In addition to the usual editing functions of an information class, there is additional support for the most complete possible history of price data.

#### Creating and editing historical price data
Only the **creator of** the **security** or a user with sufficient rights can create or delete historical price data.
+ **Create** a **historical price** via the **context menu**.
+ **Edit** a **historical price** on the **selected historical price**.
+ **Delete** a **historical price** on the **selected historical price**.

#### Export as CSV file
The historical daily data can be exported. The export format corresponds to the import format.

#### Importing historical data
You can find the import format in the export format. When importing historical daily data, the date and the closing price must be available. The assignment of the column to the import fields is determined from the first line. In the following example, the date was expected in the first column and the closing price in the second column. A **semicolon** is expected as the field delimiter.
```
date;close;volume;open;high;low
18.02.2021;78;;;;
```
Existing historical daily data cannot be overwritten with an import.

#### Linear filling of missing price data
For example, it is possible that an instrument is removed from the exchange in order to merge it with another instrument. During this time, no price data is supplied, but GT needs the historical price data as completely as possible to calculate the periodic performance. The **Linear fill missing price data** function is used to fill the prices of the missing trading days. Optionally, possible weekend prices can be shifted to the Friday of the same week if this Friday had no price data.

### Properties and table columns
The properties are not discussed further here as they are self-explanatory or supported by Quckinfo.
