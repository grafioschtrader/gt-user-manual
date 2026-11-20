---
title: "Price freshness"
date: 2026-10-21T10:54:47+01:00
draft: false
weight: 40
archetype: "default"
---
An instrument does not receive prices forever. A security disappears from the exchange after bankruptcy, an ETF is closed, or a data source simply stops delivering for one instrument. The position still has a value and must continue to be valued. The colouring of individual cells in the watchlist tables and in the securities account report is therefore meant to show you at a glance where a displayed price may be inaccurate. Instead of comparing dates with each other yourself, you see immediately which instrument deserves a second look. Every colouring is also explained by a tooltip when you rest the mouse on the cell. An uncoloured cell means that GT found nothing conspicuous.

| Column | Colouring | Meaning | Watchlist views |
|---|---|---|---|
| **Timestamp** | reddish, increasing | The displayed price has not been renewed for some trading days. | Performance, Quote data feed, Indicators |
| **Price** | yellow | The price was never traded but calculated when gaps were filled. | Performance, Indicators |
| **Youngest EOD** | reddish, increasing | Historical prices of trading days are missing which should already have been delivered. | Quote data feed, Indicators, Performance |
| **Oldest EOD** | reddish, increasing | The historical prices only start after **Trading from**. | Quote data feed |
| **Oldest EOD** | yellow | The historical prices reach further back than **Trading from**. | Quote data feed |

## Reddish "Timestamp" column
The further back the displayed price lies, the stronger the **Timestamp** column is shaded red. What is measured are not calendar days but **trading days of the exchange** on which the instrument is listed. A weekend, a worldwide holiday or a holiday of exactly that exchange therefore never colours the cell — which is precisely what would happen if calendar days were counted, and an instrument supplied perfectly normally over Easter would then look alarming. Currency pairs, which belong to no exchange, are measured against the general trading calendar.

The current day deliberately does not count. An exchange which has not opened yet rightly shows the price of the last completed trading day, and that counts as current. From about ten missed trading days on, the colouring no longer intensifies; whether a data source has been silent for two months or for two years makes no difference to you any more.

## Yellow "Price" column
A **Price** shaded yellow means that this price was never traded. It comes from the historical price data and was itself only created there by closing gaps, either by you through [Linear filling of missing price data](../../externaldata/historyquote/pricedata/) or automatically, because the data source only delivers prices for days actually traded. The value is therefore an interpolated intermediate value and not market information.

A genuine closing price delivered by the data source stays uncoloured. It is a real price; how old it is, the **Timestamp** column already tells you.

## Which price is shown at all
In the [Performance view](../performance/) and in the [securities account report](../../../reportportfolio/securityaccountreport/) GT always shows the most recent price available. If the youngest historical closing price is newer than the last intraday price received, that closing price is shown and **Timestamp** names its date. Watchlist and securities account report therefore agree, and an instrument which is no longer traded is valued with the price you have maintained in the historical price data.

Only prices up to and including the current day are taken into account. That matters because a linear filling may well reach into the future, for instance with a bond whose **Active until date** is its maturity. A price for a trading day which has not yet occurred is never used as the current price.

{{< mermaid >}}
flowchart TD
    A[Price of an instrument] --> B{Historical close up to today newer than the intraday price?}
    B -->|No| C[Intraday price is shown]
    B -->|Yes| D{Was this closing price traded?}
    D -->|Yes| E[Closing price is shown]
    D -->|No, created by gap filling| F[Closing price is shown in yellow]
    C --> G[Timestamp reddish by missed trading days]
    E --> G
    F --> G
{{< /mermaid >}}

An example: a security has not been traded since a bankruptcy, and you have filled the missing prices linearly up to yesterday. The watchlist then shows a yellow **Price**, **Timestamp** shows yesterday's trading day and stays uncoloured, because the price is as current as it can be. If on the other hand you never filled, **Timestamp** shows the last real trading day and turns strongly red.

## Different behaviour in the "Quote data feed" view
In the [Quote data feed view](../pricefeed/) no price is substituted, on purpose. That view serves to monitor the data sources, and there **Timestamp** has to state when the intraday data source last delivered successfully — not how old the price is that you see elsewhere. Together with the two retry counters and the **Youngest EOD** column you therefore quickly recognise whether only the intraday supply has failed or whether an instrument receives no prices at all any more.

## Reddish "Youngest EOD" column
The **Youngest EOD** column names the day of the youngest stored historical closing price. It is shaded the more strongly red, the more trading days after it are missing whose closing price should already be available. This lets you recognise a historical data source which no longer delivers, even while the intraday prices keep arriving. That matters because evaluations such as return, correlation or the indicators are based on the historical prices.

A closing price from yesterday is not automatically a problem. The historical prices of a trading day are only loaded after the market has closed, depending on the setting at a fixed time or per exchange some time after its close, see [Automatic update after market close]({{% relref "/watchlistinstrument/externaldata/historyquote/pricedata" %}}). GT therefore does not go by the clock but by the **Last quote update** of the exchange, which you see in the table of [stock exchanges]({{% relref "/basedata/instrumentbased/stockexchange" %}}). A trading day is only expected when this update took place at least three hours after its close. Between the market close and the next update the cell therefore stays uncoloured, even if yesterday's price is still missing.

{{< mermaid >}}
flowchart TD
    A[Youngest EOD of a security] --> B{Historical data source or derived instrument?}
    B -->|No| N[No colouring]
    B -->|Yes| C[Youngest trading day whose close lay at least three hours before the last quote update of the exchange]
    C --> D[At most up to the active until or maturity date]
    D --> E{Are trading days missing between Youngest EOD and this day?}
    E -->|No| N
    E -->|Yes| R[Reddish by the number of missing trading days]
{{< /mermaid >}}

As with **Timestamp**, only trading days of the exchange are counted; weekends and holidays never colour the cell. From ten missing trading days on, the colouring no longer intensifies. For a security which is no longer traded, GT expects no prices after the **Active until or maturity date**. If that date lies more than about three months back, the column is no longer assessed. Instruments without a historical data source, whose prices you maintain yourself, are not coloured.

Currency pairs belong to no exchange. They are measured against the general trading calendar. Because their historical prices are loaded once a day, the last completed trading day does not yet count as missing for them.

{{% callout style="note" title="A light shade of red" %}}
Some data providers only publish the closing price some time after the market close. If the quote update ran earlier, one trading day is missing until the next update fetches it. A single lightly coloured day is therefore usually harmless. Only a colouring which grows stronger from day to day points to a data source which no longer delivers. In that case a look at the **Historical retries counter** in the [Quote data feed view](../pricefeed/) helps.
{{% /callout %}}

## Reddish or yellow "Oldest EOD" column
The **Oldest EOD** column of the [Quote data feed view](../pricefeed/) names the day of the oldest stored historical closing price. It is compared with **Trading from** of the security, because historical prices should exist from that date on.

If the historical prices start later, the cell is shaded reddish, the more strongly the larger the gap is. The data source has in this case never delivered the beginning of the price history, and the prices are missing for evaluations over long periods or for transactions from that early time. Calendar days are counted here. A light shade of a few days arises for example when **Trading from** falls on a weekend, and is harmless. From a gap of one year on, the colouring no longer intensifies.

If on the other hand the historical prices reach further back than **Trading from**, the cell is shaded yellow. That is no problem of the data source but a hint that **Trading from** may have been set too late; it is worth checking this date. Currency pairs have no **Trading from** date and are never coloured in this column.

## Empty fields instead of outdated values
Every data source delivers only a part of the intraday values. If you switch the intraday data source of an instrument to one which, for example, only knows the last price, the remaining fields such as **yesterday** or **Daily change** stay empty from now on instead of continuing to show the values of the earlier data source. An empty field therefore means that the current data source does not deliver this value. The same applies to a price which comes from the historical price data: a closing price knows no course of trading, so those fields are empty there as well.

{{% callout style="info" title="Where you see the date of the substitute price" %}}
The **Youngest EOD** column names the day a price taken from the historical price data comes from. In the **Quote data feed view** and in the **Indicators** view it is visible by default; in the **Performance view** you show it when needed via **Show columns**, see [Control elements](../../../intro/userinterface/user_setting_ui_controls).
{{% /callout %}}
