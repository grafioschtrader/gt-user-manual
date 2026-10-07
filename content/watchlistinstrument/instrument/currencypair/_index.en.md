---
title: "Currency pair"
date: 2026-08-31T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
GT supports **portfolios**, **accounts**, **instruments** and **transactions** in different currencies. Therefore, the **currency pair** is essential for GT. GT attempts to treat **fiat currencies** and certain **cryptocurrencies** in the same way.

A **currency pair** **cannot be traded**; a corresponding **security** or **derived instrument** must be available for trading. This is based on an **asset class** with **Forex** as the **financial instrument**. Supported **cryptocurrencies** are BTC, BNB, ETH, ETC, LTC, XRP. It should be noted that cryptocurrency data sources only provide intraday or historical price data for the most important world currencies such as USD, EUR, JPY, GBP and CHF.

## When does a currency pair come into existence
A currency pair rarely has to be created by hand. In the vast majority of cases GT creates it itself as soon as an entry connects two different currencies. A **direction** is always decisive here: GT creates exactly the pair that is needed for the pending conversion. If the pair already exists in this direction, it is reused and no second one is created.

| Trigger | Currency pair created (from → to) |
|---|---|
| An [account](../../../tenantportfolio/cashaccount/) is saved with a currency that differs from the portfolio currency | portfolio currency → account currency |
| A [transaction](../../../transaction/) is entered in which the instrument and account currency differ, or an account transfer between two currencies | instrument → account currency, or account → account currency |
| A position with an exchange rate is processed during the [transaction import](../../../tenantportfolio/securityaccounts/transactionimport/) | as with the manual transaction |
| A [cash standing order](../../../transaction/standingorder/cash/) receives an **amount currency** that differs from the account currency | amount currency → account currency |
| The **currency** of a [portfolio](../../../tenantportfolio/portfolio/) is changed | client currency and all instrument currencies traded in this portfolio → new portfolio currency |
| The **currency** of the [client](../../../tenantportfolio/client/) is changed | all portfolio currencies and all traded instrument currencies → new client currency |
| The function **Currency client and portfolios** is executed | as the two preceding rows combined |
| A [report](../../../reportportfolio/) needs an exchange rate that is still missing — this concerns cross rates, the build-up of the holding tables, the instrument statistics and various portfolio reports | client currency → instrument or account currency |
| **Add newly created currency pair** is chosen from the context menu in the [watchlist](../../watchlist/) | freely chosen base and quote currency |

The following diagram summarises the sequence. What determines the waiting time is who triggered the currency pair.

{{< mermaid >}}
graph TD
    A["Trigger: account, transaction, import, standing order,<br/>currency change, report, manual creation"] --> B{"Currency pair present<br/>in this direction?"}
    B -->|yes| C["Existing currency pair is used"]
    B -->|no| D["New currency pair is created,<br/>default connectors are assigned"]
    D --> E{"Who triggered the currency pair?"}
    E -->|Report| F["Prices are loaded immediately,<br/>the report waits for them"]
    E -->|User action| G["Prices are loaded in the background<br/>right afterwards"]
    F --> J["Currency pair is ready for use"]
    G --> H{"Price history remained empty?"}
    H -->|no| J
    H -->|yes| I["Task 40 loads the prices later"]
{{< /mermaid >}}

## Direction and reverse pair
For GT, CHF/USD and USD/CHF are two independent currency pairs. When creating automatically, GT checks only whether the pair already exists in the required direction. If USD/CHF already exists and CHF/USD is needed, then CHF/USD is created in addition. Both pairs then exist side by side, are supplied with price data separately and both appear in the search.

Excluded from this are reports that form a cross rate via the client currency, as well as the instrument statistics with the return calculation. These deliberately search in both directions and calculate with the reciprocal value instead of creating a second currency pair.

{{% notice tip %}}
Before creating a currency pair manually, check whether the opposite direction is already present in a watchlist. Otherwise you maintain two pairs whose prices are loaded twice, although a single one would be sufficient for the conversion.
{{% /notice %}}

## Connectors of a new currency pair
An automatically created currency pair receives its data sources from the **global settings**. Which of the four settings applies depends on whether a cryptocurrency is involved. It is sufficient for one of the two currencies to be one of the supported cryptocurrencies, regardless of whether it appears as the base or as the quote currency.

| Type of currency pair | Historical price data | Intraday price data |
|---|---|---|
| Fiat currencies only, for example EUR/CHF | «gt.currency.history.connector» | «gt.currency.intra.connector» |
| At least one cryptocurrency, for example BTC/USD | «gt.cryptocurrency.history.connector» | «gt.cryptocurrency.intra.connector» |

The administrator can adjust these four settings under [Global settings](../../../admindata/globalsettings/). They only affect **newly** created currency pairs; existing currency pairs keep their connector. For an individual currency pair the connector can be overridden at any time via the context menu **Edit Currency pair** in the watchlist.

## Loading the price data
How quickly the prices of a newly created currency pair become available depends on the trigger.

If the currency pair is triggered by a **user action**, that is by an account, a transaction, an import, a standing order or the manual creation, it is saved first and the price data is loaded in the background right afterwards. This does not delay the entry. It may however happen that the view briefly still shows the currency pair without a price; after reloading the view the values are present.

If the currency pair comes into existence as part of a **report**, the price data is loaded immediately, because the report cannot deliver a result without a price. This one report therefore takes noticeably longer than usual. The next time it is called the currency pair already exists and the report runs at its usual speed again.

### Changing the connector of an existing currency pair
If the connector for the historical price data or its URL extension is changed while editing a currency pair, GT discards the existing historical prices and reads them in again from the new data source immediately. This also happens in the background, so the editing dialog closes at once.

{{% notice note %}}
For a currency pair, **no** entry appears in the [batch processing monitor](../../../admindata/taskdatachangemonitor/) for this reload, and the previous prices are not transferred into the [archive of historical price data](../../externaldata/historyquote/pricedata/archive/). For a security both is the case, see [Changing the connector and reloading the price data](../securityderived/#changing-the-connector-and-reloading-the-price-data). Therefore only change the connector of a currency pair if the new data source actually covers the required price history.
{{% /notice %}}

### When a currency pair remains without prices
If the data source delivers nothing on the first load, the currency pair is left without historical prices. There is a safety net for this case: together with the daily price update, GT searches for all currency pairs without historical prices and queues the task **Load historical price data of an empty currency pair** for each of them. This task is visible in the [batch processing monitor](../../../admindata/taskdatachangemonitor/) and is described under number 40 in the [background tasks](../../../admindata/taskdatachangemonitor/taskdescription/).

The safety net only applies to a completely empty price history. If only individual prices were read in after a connector change, the currency pair counts as supplied. In that case check the completeness in the [quote data feed view](../../watchlist/pricefeed/) of the watchlist and trigger the repair of the historical data there if required.

## Prices for every calendar day
A currency pair needs a price for **every calendar day** and not only for the trading days. The reason lies with the accounts: a transaction, an account transfer or a standing order can fall on a Saturday, a Sunday or a public holiday, and a price must be available to convert that day. This question does not arise for a security, where only the trading days of its stock exchange count.

The data sources, however, usually deliver a price for a currency pair on working days only. GT therefore closes these gaps itself: if individual days are missing between two existing prices, the last price before the gap is carried over to each of those days. This happens together with the daily price update, see [background tasks](../../../admindata/taskdatachangemonitor/taskdescription/) under number 30.

How a gap is treated depends on its length. If it covers at most as many days as the global setting «gt.history.max.filldays.currency» specifies — five days by default, which covers a long weekend — the preceding price is carried over directly. If the gap is longer, GT first asks the data source again for exactly that period, and only when it delivers nothing is the preceding price carried over here as well.

{{< mermaid >}}
graph TD
    A["Gap between two existing prices"] --> B{"Longer than «gt.history.max.filldays.currency»?"}
    B -->|no| C["Last price before the gap is carried over to every missing day"]
    B -->|yes| D["Data source is asked again for this period"]
    D --> E{"Prices delivered?"}
    E -->|yes| F["Delivered prices are stored,<br/>remaining gaps are checked again"]
    E -->|no| C
{{< /mermaid >}}

These carried-over prices deliberately stay inconspicuous. They appear neither in the table of the [historical price data](../../externaldata/historyquote/pricedata/) nor in the chart of the currency pair, and the statistical evaluations leave them out as well, so that a price which stays unchanged over the weekend does not distort the key figures. They are counted only in the completeness statistics of the same dialog, under **Saturday prices** and **Sunday prices**. If you correct a price by hand, the carried-over days that immediately follow it are corrected along with it; you do not have to adjust them one by one.

{{% notice note %}}
Only the gaps **between** two existing prices are filled. No entry is created before the first and after the last delivered price. If a transaction lies before the price history of a currency pair begins, that day remains without an exchange rate and the reports skip it, see [period performance](../../../reportportfolio/periodperformance/). The only remedy is then a data source that actually covers the required price history.
{{% /notice %}}

Unfortunately only in German:
{{< youtube ORLgkY4YiX0 >}}
