---
title: "Price data and connectors"
date: 2026-10-19T12:00:00+02:00
draft: false
weight: 20
archetype: "default"
---
These background tasks load the price data of the instruments and monitor the connectors through which it is obtained. The daily task 30 updates all prices and then carries out a series of further work, among it reading the split calendars. Tasks 35 and 40 load the prices of a single security or currency pair, task 45 the exchange rates of the European Central Bank. If a connector fails repeatedly for an instrument, its retry counter rises. Tasks 46 and 47 notify the main administrator when this happens frequently for one connector, and task 49 resets the counters once the fault has been fixed. The weekly task 56 loads no prices but the fundamental base values of equities, with which the indicators of a watchlist compute ratios such as the price/earnings ratio.

## 30 - Historical and intraday course update with completeness tracking and split calendar update {#JOB30}
This task type is used to process various tasks in the following order:
1. Historical and intraday price data update.
2. The price gaps of the currency pairs are closed, so that an exchange rate is available for every calendar day, see [Prices for every calendar day](../../../../watchlistinstrument/instrument/currencypair/#prices-for-every-calendar-day)
3. Missing trading days of instruments marked as [Instrument without price data]({{% relref "/basedata/bankruptsecurity" %}}) are filled linearly, so that a single silent issuer does not remove whole days from the period performance of everyone holding it.
4. [Completeness of historical securities price data](../../../../admindata/historyquotequality/)
5. The split calendars are read. If a split of a security of this GT instance is found there, [task 32](../splitdividend/#JOB32) is created.
6. Cash holdings table, for more information see [Architecture of GT - Cash holdings table](../../../../reportportfolio/periodperformance/holdingtable/)
7. A new task with ID 40 is created for currency pairs without historical price data

## 35 - Loading or reloading historical price data of the instrument {#JOB35}
When a security is created, the intraday and historical price data is read. If the connector for the historical price data is changed, the prices must also be reloaded. When the security is created, this can be done synchronously or asynchronously with this task. The historical price data is only loaded synchronously for unit tests or according to the settings in `application.properties`. The price data for currency pairs is always loaded synchronously. If the connector for the historical price data is changed, or if this task is triggered for a security, the existing prices are first copied to the [archive of historical price data]({{% relref "/watchlistinstrument/externaldata/historyquote/pricedata/archive" %}}) before the table is rebuilt from the new data provider.

## 40 - Load historical exchange rate data of an empty currency pair {#JOB40}
This task type is mentioned in [Currency pair and cryptocurrencies](../../../../watchlistinstrument/instrument/currencypair/).

## 45 - Loading the ECB's historical exchange rate data {#JOB45}
For more information, see [European Central Bank](../../../../watchlistinstrument/externaldata/)

## 46 - Monitoring the connectors for historical exchange rate data {#JOB46}
This task type checks whether a connector for **historical exchange rate data** may no longer be functioning. Certain parameters of the check can be adjusted via the global settings. In the event of a possible malfunction of a connector, the **main administrator** receives a message. This task should be performed daily.
- **gt.history.observation.retry.minus** transmission error at maximum retry**(gt.history.retry**) minus this number. If `gt.history.retry` contains the value 4, this value should be 0 or 1. Thus, an instrument with 4 or 3 repetitions would be considered non-functioning.
- **gt.history.observation.days.back**: Instrument is only taken into account if a successful transmission has taken place within the current date minus this number of days. In this way, instruments that are no longer active but are listed as active do not distort the calculation.
- **gt.history.observation.falling.percentage**: Message if at least this percentage of a connector has failed. At 100 %, the connector probably no longer works at all.

## 47 - Monitoring of connectors for intraday price data {#JOB47}
This task type checks whether a connector for **intraday price data** may no longer be working. As with the monitoring of historical price data, the **main administrator** receives a message. The following parameters can be changed using global settings:
- **gt.intraday.observation.retry.minus**: See **gt.history.observation.retry.minus** of task type 46.
- **gt.intraday.observation.or.days.back**: A connector of an instrument is classified as faulty if the number of repetitions is too high or if no update has taken place since the current date minus this value as the number of days.
- **gt.intraday.observation.falling.percentage**: See **gt.history.observation.falling.percentage** of task type 46.

## 49 - Reset retry counters for connector(s) on active instruments {#JOB49}
This task type resets the retry counters for historical and intraday price data on active instruments. It is used to recover from situations where connectors have exceeded their retry limits due to temporary issues such as network outages. The task can be executed either for a single connector or for all connectors.

## 56 - Load the fundamental base values of equities {#JOB56}
Once a week this task loads earnings per share, book value and revenue per share, the number of shares and net debt, which are available as fundamental data in the [indicators of a watchlist]({{% relref "/watchlistinstrument/watchlist/indicator" %}}). It covers active equities held as a direct investment that are on a watchlist of any tenant or appear in a transaction. The values of securities that no longer meet this condition are deleted. When the task is created by hand with the information object **Security** and its ID, it loads only that security; without an information object it loads all of them.
The sources are Yahoo, Finnhub and Eulerpool. On its free plan Finnhub covers listings on US exchanges only, Eulerpool finds listings by ISIN, and both are asked only when their [connector API key]({{% relref "/admindata/connectorapikey" %}}) is stored. Each source is processed on its own with its own pause between two requests, so that its free request limit is respected; a run for about 250 equities takes around five minutes. When a source refuses the key or the plan, it is not asked again for the rest of the run. When a source gives no usable answer for an equity, the values of that source from the last run remain. GT then checks the new values against the current price, compares the sources with each other and decides which value is used.
When a source delivers not a single usable answer or was refused, the task ends as failed. Its error message then lists, per source, the number of usable, failed and unsupported equities and the time of the last usable answer. The values of the other sources are stored all the same. When such a task is already running, a second one ends at once without doing anything.
The schedule is set with the application property `gt.fundamental.load.cron`, by default Saturday at 04:23 UTC. On installation this time is moved to a random minute between 03:00 and 05:00 UTC while it still equals the default, so that not all GT instances ask Yahoo at the same time. With `-` the automatic run is disabled, but the task can still be created by hand. The property `gt.fundamental.sources` selects which of the sources `yahoo`, `finnhub` and `eulerpool` are asked. When it is empty, no request for fundamental data leaves the instance, not even from a task created by hand.
