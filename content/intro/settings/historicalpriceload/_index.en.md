---
title: "Historical prices after the import"
date: 2026-10-20T12:00:00+02:00
draft: false
weight: 20
archetype: "default"
---
An [export of the personal data]({{% relref "/intro/settings" %}}) does not contain the historical prices of instruments that are still active, because these can be obtained again. The new instance loads them itself after the import. This page describes where the prices come from, what has to be prepared for it and what to do when prices are still missing after the first run.

## The prices come from the connectors
The historical prices are first loaded through the **connectors** of the instruments, that is through the data sources entered as historical data source for each instrument. Even if your instance is already connected to [GTNet]({{% relref "/admindata/gtnet" %}}), an instrument without price history is requested from its connector first. This shows right from the start whether the connectors of the new instance are set up correctly.
GTNet steps in only later. If the connector of an instrument fails repeatedly and its **Historical retries counter** reaches the value of `gt.history.retry`, GTNet takes over the delivery as a substitute, provided the option **Receive historical price data** is set for the instrument. For instruments that already have a price history, the daily update asks GTNet first when GTNet is set up, and the connector only afterwards. The details are described under [Historical Price Exchange]({{% relref "/admindata/gtnet/exchange/historicalprice" %}}).

## Enter the API keys of the connectors first
Many data sources deliver only with a valid API key. The API keys belong to the instance and are **not exported**. In the new instance you therefore have to enter them again under [Connector-API-Key]({{% relref "/admindata/connectorapikey" %}}). If the API key of a data source is missing, its connector fails and the **Historical retries counter** of the instruments concerned rises.
It is therefore best to enter the API keys immediately after the first start, before the loading of the prices begins.
You may not have all the API keys in the new instance that the imported instruments need, for example because the source instance used a paid subscription of a data source. In this case you have to adjust the connectors of the instruments concerned and select, under **Historical data source** and if necessary **Intraday data source** of the instrument, a data source that is available in the new instance. Which instruments use a particular data source is shown in the **Quote data feed view** of a watchlist, which can be filtered by these two columns.

## The loading starts automatically
About five to ten minutes after the first start of the back-end, GT creates the background task [30 - Historical and intraday course update]({{% relref "/admindata/taskdatachangemonitor/taskdescription/pricedata#JOB30" %}}), which loads the missing prices. Afterwards this task runs daily at the time defined in `gt.eod.cron.quotation`.
If the API keys were not yet complete at the time of the automatic run, add them and trigger task 30 by hand in the [Batch processing monitor]({{% relref "/admindata/taskdatachangemonitor" %}}) by creating a new background task of this type. You therefore do not have to wait for the next daily run.

{{< mermaid >}}
flowchart TD
    A[Import completed, back-end started] --> B[Enter API keys under Connector-API-Key]
    B --> C[Task 30 starts automatically after 5 to 10 minutes]
    C --> D{Prices still missing?}
    D -- No --> E[Price history complete]
    D -- Yes, API keys were missing --> F[Add API keys]
    F --> G[Create task 30 by hand]
    G --> D
    D -- Yes, counters exhausted --> H[Task 49 resets the retry counters]
    H --> G
{{< /mermaid >}}

## The process can take a long time
The entire price history is loaded for every active instrument, often over many years. With several hundred instruments and free data sources with request limits this takes a long time, on a small computer such as a Raspberry Pi in particular. A single run of task 30 is limited to one hour; what is still missing afterwards is loaded by the next run. Several runs may therefore be needed until all instruments have a price history.
As long as the price history of an instrument is missing, columns such as **YTD** and **Time frame** stay empty in the **Performance view** of a watchlist, although the last price is updated continuously.

{{% callout warning %}}
Do not restart the back-end while task 30 is running. An interrupted run is not resumed. The missing prices are loaded only by the next run or by a task 30 created by hand.
{{% /callout %}}

## When a connector has been suspended
In every run in which the connector of an instrument fails, its **Historical retries counter** rises by one. When the counter reaches the value of `gt.history.retry`, GT no longer requests this connector. If the API keys were missing over several runs, this may already affect many instruments. In the **Quote data feed view** of a watchlist you recognise these instruments by a retry counter greater than zero. Once the API keys are entered, the background task [49 - Reset retry counters for connector(s) on active instruments]({{% relref "/admindata/taskdatachangemonitor/taskdescription/pricedata#JOB49" %}}) resets the counters for a single connector or for all connectors. Afterwards you trigger task 30 by hand again.
Instruments whose connector no longer delivered prices already in the source instance are listed in the file `broken_history_connectors.txt` of the export. Their prices are included in the export, but they still need a new connector so that future prices are added.
