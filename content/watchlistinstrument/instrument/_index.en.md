---
title: "Instruments"
date: 2018-01-13T22:54:47+01:00
draft: false
weight: 25
archetype: "default"
---
In GT, the general term instrument is used because it is a collective term for security, currency pair, derived instrument and non-tradable "security".
- **Security**: A security is assigned to an **asset class**. This allocation determines how the security can be traded.
- **Derived instrument**: A derived instrument can be traded in GT if the assigned **asset class** permits this.
- **Currency pair**: The currency pair cannot be traded in GT. If different currencies are used in portfolios, these are primarily required for the calculation of performance.

## Price data
In GT, an **instrument** makes no sense without **price data**, whereby **price data** is a generic term for different price data.
- **Price data connector**: For most instruments, the price data is loaded via a **connector**. For a **currency pair**, the two connectors intraday and historical price data are mandatory.
- **Denomination**: The price of a **fixed-term deposit** does not change during the term, so the price corresponds to the **denomination**. More on this in [Securities](./securityderived/security/).
- **Historical prices for period**: It is also possible that an instrument has no public price data, but the price changes very rarely during the term. In this case, "**Historical prices for period**" is used. More on this in [Securities](./securityderived/security/).
- **Derived instrument**: In this case, both the **intraday** and the **historical price data** are derived from another existing instrument. More on this in [Derived instrument](./securityderived/derivedinstrument/).

Unfortunately only in German:
{{< youtube Zij10hSO9OQ >}}

## Connectors
Certain **price data**, **splits** and **dividends** are obtained from external data sources via **connectors**. Whether a **type of** connector is activated when editing an instrument is determined by the selection of the **asset class** and the **stock exchange**. The connector types have certain things in common:
- If the **data source is changed**, the existing data from the previous connector is deleted and read in again.
  - In the case of **historical price data** and **intraday**, saving activates the data source for import. The import is performed in the background so that the user interface is not blocked for too long.
  - For **dividends** and **splits**, saving creates a **background task** that is visible in the [batch processing monitor](../../admindata/taskdatachangemonitor/).
- Depending on the data source, a **URL extension** must be entered, which can be determined with varying degrees of difficulty. However, there is a button with help to the left of the input field.

### Connector type
For the different types of connectors there are **various settings**, **monitoring options** and in some cases **corrections** can be made to the **imported data**. The **historical price data** and **splits** can also be edited manually. Below is a list where you can find further information about the different types of connectors:
- **Intraday price data**: You will find a [data source](../externaldata/) for many instruments. This can be monitored with the watchlist view [price data feed](../watchlist/pricefeed/).
- **Historical price data**: In GT, historical quote data is used in a variety of ways. In addition to the cross-references mentioned for **intraday price data**, there are additional monitoring functions for [period yield](../../reportportfolio/periodperformance/) and [completeness of historical securities price data](../../admindata/historyquotequality/). The historical price data can be viewed as a [line graph](../eodchart/). They can also be edited in the **additional area** with the [Historical prices](../externaldata/historyquote/pricedata/) view
- **Dividends**: More information can be found under [Historical data](../externaldata/historyquote/) and [Dividend/Split feed](../watchlist/dividendsplit/).
- **Split**: More information can be found under [Security](./securityderived/security/), [Historical data](../externaldata/historyquote/) and [Dividend/Split feed](../watchlist/dividendsplit/).

### Invisible connectors
For **splits**, there are invisible connectors that monitor a **split calendar on a daily basis**. The user cannot make any changes to this type of connector. More on this topic can be found under [Historical data](../externaldata/historyquote/).

Unfortunately only in German:
{{< youtube UAHPG3zP-GA >}}
