---
title: "Retry Counter and GTNet Fallback"
date: 2026-04-29T22:54:47+01:00
draft: false
weight: 25
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}

GT keeps two retry counters per instrument: the "Historical retries counter" for historical price data and the "Intraday retries counter" for intraday price data. Both counters continuously document whether the configured connector delivers reliably. As soon as GTNet is enabled and an instrument is opted in, GTNet can step in as a fallback without distorting these connector counters. The connector status therefore remains visible to monitoring while the user still receives price updates.

## Three bands per retry counter
The same counter passes through three consecutive bands. The boundaries are set by the global parameters `gt.history.retry` (or `gt.intra.retry`), which is the connector limit, and `gt.gtnet.quote.retry`, which is the additional GTNet budget. The following table describes the behaviour using historical prices as an example.

| Counter range | Meaning | Who tries to load? |
|---|---|---|
| 0 to gt.history.retry − 1 | Connector is considered healthy | The connector loads. For opted-in instruments GTNet can additionally provide data. |
| gt.history.retry to gt.history.retry + gt.gtnet.quote.retry − 1 | Connector limit reached, GTNet fallback active | Only GTNet attempts to load, provided the "Receive historical price data" flag is enabled for the instrument. The connector is skipped. |
| gt.history.retry + gt.gtnet.quote.retry or higher | Connector and GTNet exhausted | No further automatic updates. A manual reset is required. |

The same band structure applies to intraday prices with `gt.intra.retry` and the "Receive intraday price" option. The settings can be found under [Global Settings GTNet](../globalsettings/).

## Rules for changing the counter
To keep the counter a reliable signal about connector health, clear rules apply to every automatic update.

| Event | Effect on the counter |
|---|---|
| Connector success | Counter is set to 0. |
| Connector failure | Counter is incremented by 1. |
| GTNet success | Counter is capped at gt.history.retry. If the counter is already below the connector limit, it remains unchanged. If it is in the fallback band, it is set to exactly gt.history.retry, restoring the full GTNet budget. |
| GTNet failure | Counter is incremented by 1, but capped at the absolute limit. |

### When the counter is reset to 0
The value 0 normally results from a successful connector call. There are two further paths to reach it:

- **Successful connector call**: Both the regular background tasks and the manually triggered "Repair historical data" and "Repair intraday data" actions on the [Quote data feed view](../../../watchlistinstrument/watchlist/pricefeed/) reset the counter to 0 only if the connector actually returns data. If the connector stays silent or throws an error, the counter is left unchanged or even incremented. Editing an instrument also triggers a fresh data fetch as soon as the connector or URL extension changes; the counter is reset to 0 only if that connector call succeeds.
- **Background task 21 "Reset retry counters for connector(s) on active instruments"**: This task sets the counter directly to 0, without calling the connector first. It is the right tool for quickly re-activating the connector path after a connector outage has been fixed. See the [task description](../../taskdatachangemonitor/taskdescription/) for details.
- **Direct edit of the instrument**: When editing a security or currency pair, the retry counter field can be modified directly. This lets you reset the counter for a single instrument without invoking the connector.

## Visualisation of the bands
{{< mermaid >}}
stateDiagram-v2
    direction LR
    [*] --> Connector_active

    Connector_active : Connector active
    Connector_active : 0 ≤ counter < gt.history.retry
    Connector_active : Connector loads prices

    GTNet_Fallback : GTNet fallback
    GTNet_Fallback : gt.history.retry ≤ counter < gt.history.retry + gt.gtnet.quote.retry
    GTNet_Fallback : Only GTNet tries (receive option enabled)

    Exhausted : Connector and GTNet exhausted
    Exhausted : counter ≥ gt.history.retry + gt.gtnet.quote.retry
    Exhausted : Manual reset required

    Connector_active --> Connector_active : connector failure ⇒ counter + 1
    Connector_active --> Connector_active : connector success ⇒ counter = 0
    Connector_active --> GTNet_Fallback   : counter reaches gt.history.retry

    GTNet_Fallback --> GTNet_Fallback : GTNet failure ⇒ counter + 1
    GTNet_Fallback --> GTNet_Fallback : GTNet success ⇒ counter = gt.history.retry
    GTNet_Fallback --> Exhausted       : counter reaches absolute limit

    Exhausted --> Connector_active : counter reset (see "When the counter is reset to 0")
{{< /mermaid >}}

The same diagram applies to intraday prices, substituting `gt.intra.retry` and the "Receive intraday price" option.

## Practical consequences
A few intentional effects follow from these rules and matter in everyday use.

- An instrument can receive price data via GTNet even if its connector has never worked. The "Receive historical price data" or "Receive intraday price" option must be enabled, and at least one reachable GTNet supplier must offer the desired data.
- While GTNet is acting as fallback, the retry counter stays elevated. Connector monitoring (see [Background task monitor](../../taskdatachangemonitor/)) continues to detect the failure and notifies the administrator.
- The connector remains the first choice. This way, when adding a new instrument you can verify your connector is configured correctly before GTNet steps in.
- Once both bands are exhausted, GT stops trying automatically. The options described in "When the counter is reset to 0" remain: a successful connector call (e.g. via "Repair historical data" or "Repair intraday data" on the [Quote data feed view](../../../watchlistinstrument/watchlist/pricefeed/)), background task 21, or directly editing the instrument.

## Related topics
- [Global Settings GTNet](../globalsettings/) for configuring the limits.
- [Historical Price Exchange](../exchange/historicalprice/) and [Last Price Exchange](../exchange/lastprice/) for how GTNet exchanges data.
- [Quote data feed view](../../../watchlistinstrument/watchlist/pricefeed/) for manual repair actions.
- [External Data](../../../watchlistinstrument/externaldata/) for the list of available connectors.
