---
title: "Historical Data"
date: 2026-11-20T22:54:47+01:00
draft: false
weight: 32
archetype: "default"
---
In GT, **historical price data** and **splits** are obtained from external data sources.

## Historical price data (end-of-day prices)
End-of-day prices are very important for GT, which is why the **Historical prices** view is available in the **additional area**. By default, this price data is updated **daily** via one or more **background tasks**.

GT stores the historical prices in the share basis after all splits of the security, that is split-adjusted. When a new split is added, GT reloads the price history in full as soon as the price source delivers the adjusted prices. Until then a background task checks this repeatedly for some days, see [Course of a split]({{% relref "/admindata/taskdatachangemonitor/taskdescription/splitdividend/#SPLITFLOW" %}}). A recorded [spin-off]({{% relref "/basedata/securityaction/spinoff" %}}) is the exception: GT stores the parent's prices before the ex-date unadjusted, as they were traded on those days.

## Splits
Splits can be entered manually with the security, but GT also monitors the splits in the [Invesing.com](https://www.investing.com/stock-split-calendar/) and [Yahoo](https://finance.yahoo.com/calendar/splits) calendars **on a daily basis**. The monitoring leads to the presumed split being transferred via the security's connector. The problem here is the correct recognition of the security via the name of the company or the symbol. The indirect route via the security's connector ensures that no "incorrect" splits are assigned to the security. The whole procedure is shown in [Course of a split]({{% relref "/admindata/taskdatachangemonitor/taskdescription/splitdividend/#SPLITFLOW" %}}). A [split entered by hand]({{% relref "/watchlistinstrument/instrument/securityderived/security" %}}) is never overwritten or removed by the system.

### The choice of data source
The following **data sources** deliver **splits**:
+ [Yahoo USA Finance](https://finance.yahoo.com/): Free of charge. The **URL extension** is determined according to the price data.
+ [EOD Historical Data](https://eodhistoricaldata.com) and [Eulerpool](https://eulerpool.com/financial-data-api): With an API key. The **URL extension** corresponds to that of the price data.
+ [DivvyDiary](https://divvydiary.com/): Only if an API key is stored.

The splits must match the price history. The historical data source must deliver its prices adjusted for exactly the splits the split data source reports. This is easiest when prices and splits come from the same data source.

### Spin-off as a split
Some data providers, such as Yahoo and EOD Historical Data, also report a spin-off as a split of the parent, for example a split of 1000 to 1281 for the spin-off of GE HealthCare from General Electric on 2023-01-04. As long as no spin-off is recorded, GT takes over such a split like any other. The parent then receives more shares by calculation, and the child is missing from the securities account. When an administrator records the [spin-off]({{% relref "/basedata/securityaction/spinoff" %}}), GT drops the split on the ex-date and brings the parent's price history into its unadjusted form, see [Provider split and price history of the parent]({{% relref "/basedata/securityaction/spinoff/#HISTORYBASIS" %}}). Do not enter a spin-off as a split by hand.

## Dividend
Dividends are used for the **annualized return** and in future in the not yet implemented historical simulation of the performance of one or more portfolios. There will be no automatic transfer of dividends to the real portfolios. Currently no dividends can be entered manually.

### The choice of data source
We have two free **data sources** for **dividends**:
+ [DivvyDiary](https://divvydiary.com/): This data source must certainly be checked manually:
  + Sometimes the history is not complete and only the most recent years are available.
  + The currency of the dividend does not always correspond to the currency of the instrument. This different currency must be specified.
  + The amount is not stock split adjusted.
+ [Yahoo USA Finance](https://finance.yahoo.com/): The **URL extension** is determined according to the share price data.
  + The amount is adjusted according to stock splits.

DivvyDiary additionally provides an **ex-dividend calendar** through which GT detects *when* a distribution has taken place. This is distinct from its role as a data source for an individual instrument's dividend history.

### How dividends enter the system
A **background task** that is **executed daily** by default determines, in two ways, the instruments for which new dividends should be queried.
```mermaid
graph TD
    A[Daily background task] --> B[Ex-dividend calendar]
    A --> C[Frequency fallback]
    B -->|Match by ISIN and currency| D[Instrument's connector loads the dividends]
    C -->|last ex-day and frequency plus 10 days| D
    B -->|no dividend connector assigned| E[Owner is notified]
```
The **first way** is the **ex-dividend calendar**. The task steps through the period since its last run day by day and examines the distributions announced in the calendar. An announced dividend is matched to an instrument held in GT by its **ISIN and currency**. On a match, the dividend history is reloaded – as already described for splits – via the **instrument's connector**; the calendar therefore only provides the hint about the ex-day, while the actual amounts come from the instrument's connector. If an instrument is found in the calendar that has **no dividend connector** assigned, the instrument's owner is notified so that a connector can be added.

The **second way** serves as a fallback for instruments that are not covered by any calendar. The selection is based on the calculated date of the last **ex-day** and the **distribution frequency** plus 10 days. This calculated date must be more recent than the "**Dividend Check**" date to avoid the same instrument being checked daily for new dividends over a certain period of time.

In addition, the entry of a transaction with a dividend can also be a trigger for querying a data source. If there is a more recent dividend for the instrument in the transactions than in the instrument's dividends, the corresponding data source is queried with a delay of 10 days.
