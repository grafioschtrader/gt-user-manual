---
title: "Performance view"
date: 2026-10-09T22:00:00+01:00
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

## Expanding table row
An existing **expander icon** next to the instrument indicates its existing **transactions**. This is one of many places in Grafioschtrader where existing transactions can be edited. For more information, see [Securities Transactions](../../../transaction/security/).
