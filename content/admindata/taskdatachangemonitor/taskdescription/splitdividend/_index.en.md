---
title: "Splits and dividends"
date: 2026-10-16T12:00:00+02:00
draft: false
weight: 30
archetype: "default"
---
These background tasks take over the splits and dividends of the instruments from external data sources and update the data depending on them. A split changes the held quantity and requires split-adjusted historical price data; tasks 32, 38 and 36 are involved in this. Tasks 31 and 43 read the dividends.

## Course of a split {#SPLITFLOW}
The daily [task 30](../pricedata/#JOB30) reads one or more split calendars. A calendar names a split with ticker symbol, company name and usually also the exchange or the country of the listing. GT assigns it to one of its own securities only if the ticker symbol matches, the listing fits the stock exchange of the security and the company name is sufficiently similar. The check of the listing is necessary because depositary receipts such as the Canadian CDRs often carry the ticker symbol of the underlying stock but split independently of it. On 16 October 2026, for example, a split calendar reported splits of the CDRs of Advanced Micro Devices and CrowdStrike under the ticker symbols AMD and CRWD; the stocks themselves were not affected.

It can take several days until an announced split is also reflected by the connector of the security and in its historical price data. The following simplified flowchart shows this process, where a hexagon node stands for a background task:
{{< mermaid >}}
graph TD;
    A{{Task 30: daily}} --> B(Read one or more split calendars)
    B --> |Split of a matching listing found| C{{+2 minutes: task 32}}
    C --> D(Read the splits of the security through its connector)
    D --> E{Announced split delivered?}
    E --> |No, at most 5 days after the split date| G{{+1 day: task 32}}
    G --> D
    E --> |Yes| I(Check whether the historical price data of the data supplier contains the split)
    I --> K{Split in the price data?}
    K --> |No| H{{+ some days: task 38}}
    H --> I
    K --> |Yes| R(Reload historical price data)
    R --> L{{+0: task 36}}
    L --> M(Update the holding of the security of the affected clients for this split)
{{< /mermaid >}}

## 32 - Read splits again because the connector has been changed or the instrument has undergone a split {#JOB32}
Is triggered if the data connector of the split has been changed or if a possible new split has been detected in the split calendar. It reloads all splits for a specific security. If the instrument's splits have been changed, the historical price data is also completely reloaded. If the connector does not yet deliver a split announced in the split calendar, the task is repeated on the next day, at most until five days after the split date. The whole process is shown in [Course of a split](#SPLITFLOW).

{{% callout style="info" title="Incorrect split detection" %}}
Splits of another listing with the same ticker symbol are not taken over, for example the split PLTR.NE reported on 14 November 2025 in the split calendar of Yahoo Finance. It did not concern the Palantir share but its depositary receipt traded on a Canadian exchange. If a split calendar does not name the listing, or if two products on the same exchange carry the same ticker symbol and a similar name, a split can still be assigned to the wrong security. Task 32 is then repeated without success for some days. Such a task with the status "Waiting" can be deleted in the monitor, which also stops its further creation; at the latest five days after the split date the system stops it by itself.
{{% /callout %}}

## 38 - Check repeatedly after a split whether the historical price data reflects this and can be loaded {#JOB38}
When a split is added for a security, it can take a few days for the adjusted historical price data to be available from the data provider. This task is therefore repeated until the historical price data for the security has been successfully imported.

## 36 - Position quantity of a security has changed due to a split, all dependent securities positions of the clients are rebuilt {#JOB36}
The change of splits can affect the position table of the instruments. See more on this topic under"[Report on periodic income and redundant data](../../../../reportportfolio/periodperformance/holdingtable)".

## 31 - Read dividend, as connector has been changed {#JOB31}
For more on this topic, see [Dividend](../../../../watchlistinstrument/externaldata/historyquote/).

## 43 - Tracks possible new dividends of the instruments via the connectors {#JOB43}
The loading of dividends for the individual instruments via connectors is currently carried out using two different algorithms. One algorithm monitors one or more dividend calendars, the other works with the expected periodicity of dividend payments:

- **Dividend calendar**: the dividend calendar is loaded daily on the trading days via the corresponding connector. The dividends are added if the instruments listed in this calendar are also contained in this GT instance.
- **Periodicity**: This is used to add dividend income to the dividend entity. The following algorithm is used to determine any missing dividend income in the Dividend entity for securities. This is done on the basis of the date of the last dividend payment and the periodicity of the expected payments. In addition, the dividend payment in the Transaction entity is also taken into account if the dividend payment is more recent than the date in the Dividend entity.
