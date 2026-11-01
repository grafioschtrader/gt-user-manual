---
title: "Performance view"
date: 2026-10-13T22:00:00+01:00
draft: false
weight: 10
archetype: "default"
---
By selecting a watchlist in the **navigation tree**, this view is displayed in the main area by default. The **watchlist view** shows the instruments with their open and traded positions. It is also the only option for **intraday price updates** in GT.

## Intraday price update
The selection of this watchlist view can cause an **intraday price update**, taking into account the specified time interval since the last price update. This **time interval** is also used for each individual instrument for a possible price update. The date of the last update of the intraday prices is shown in the title bar of the watchlist. The time interval can be found in the "gt.w.intra.update.timeout.seconds" property of the **global setting**.

If an instrument no longer receives intraday prices, it is valued with the youngest historical closing price. The **Timestamp** and **Price** columns indicate by their colouring how reliable the displayed price is, see [Price freshness](../pricefreshness/).

## Functions
This **performance view** has only the general functions implemented in the context menu.

### Menu "View" of the menu bar
**Time frame**: The setting via the **Time frame** menu item determines the period for the calculation of the "**Time frame %**" table column. If the time period is equal to or greater than one year, the **geometric average return** is used in the performance calculation. 

**Include dividends**: When this menu item is checked, the columns **Daily change**, **YTD**, **Time frame %** and **Annual time frame** contain the dividends of the securities as [total return](../../../glossar/#total-return). For every dividend whose ex-date lies between the starting price and the displayed price, the starting price is reduced by the factor of that dividend. When the ex-date falls on today's trading day, the daily change is corrected accordingly as well. The setting is stored like the time frame. **Currency pairs**, **indices** and **derived securities** are not converted. This makes a price index comparable with a performance index, or a distributing with an accumulating ETF, provided the performance index is chosen for the index. When the menu item is checked, the column **Total return** appears as well. It shows the warning sign ⚠ for a security that has a distribution frequency but no dividend in the time frame, or at least in the last year. Its values then equal the pure price development.

**Show position in header**: When this menu item is checked, the title bar of the watchlist shows next to its name, in small print, the open position of the selected table row. In the currency of the instrument these are **Holding**, **Total Position**, **Price gain** and **Price gain %**, in the main currency **Total Position**, **Price gain** and **Price gain %**, with the code of the main currency appended to the label. The price gain is the gain or loss if the open position were closed at the current price, without transaction costs. The title bar stays in place while the table scrolls, so these values remain visible even in a long watchlist. An instrument without a holding, such as a currency pair, shows no values. For a margin position the Total Position is its notional value, as in the table, and the price gain is the gain or loss of its open positions. The setting is stored.

## Values in the main currency
The menu item **Turn on/off columns** shows two further columns whose header carries the code of the main currency, for example "Total Position CHF" and "Price gain % CHF". The **Total Position** in the main currency is the value of the open position, converted at the current exchange rate. The **Price gain %** in the main currency also contains the [forex gain](../../../glossar/#forex-gain): the cost is converted at the exchange rates at which the money was invested, the value at the current rate. A security in the main currency therefore shows the same percentage in both price gain columns. For a margin position the percentage equals the one in the currency of the instrument. If a required exchange rate is missing, the values in the main currency remain empty.

## Expanding table row
An existing **expander icon** next to the instrument indicates its existing **transactions**. This is one of many places in Grafioschtrader where existing transactions can be edited. For more information, see [Securities Transactions](../../../transaction/security/).
