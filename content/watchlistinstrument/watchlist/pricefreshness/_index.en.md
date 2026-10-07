---
title: "Price freshness"
date: 2026-09-06T10:54:47+01:00
draft: false
weight: 40
archetype: "default"
---
An instrument does not receive prices forever. A security disappears from the exchange after bankruptcy, an ETF is closed, or a data source simply stops delivering for one instrument. The position still has a value and must continue to be valued. So that you do not trust a price which is no longer current, GT marks in the watchlist tables and in the securities account report how reliable the displayed price is. Two colourings are enough for that, and both are also available as a tooltip when you rest the mouse on the cell.

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

## Empty fields instead of outdated values
Every data source delivers only a part of the intraday values. If you switch the intraday data source of an instrument to one which, for example, only knows the last price, the remaining fields such as **yesterday** or **Daily change** stay empty from now on instead of continuing to show the values of the earlier data source. An empty field therefore means that the current data source does not deliver this value. The same applies to a price which comes from the historical price data: a closing price knows no course of trading, so those fields are empty there as well.

{{% notice style="info" title="Where you see the date of the substitute price" %}}
The **Youngest EOD** column names the day a price taken from the historical price data comes from. In the **Quote data feed view** it is visible by default; in the **Performance view** you show it when needed via **Show columns**, see [Control elements](../../../intro/userinterface/user_setting_ui_controls).
{{% /notice %}}
