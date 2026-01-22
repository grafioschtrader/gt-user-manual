---
title: "Performance view"
date: 2025-11-18T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
By selecting a watchlist in the **navigation tree**, this view is displayed in the main area by default. The **watchlist view** shows the instruments with their open and traded positions. It is also the only option for **intraday price updates** in GT.

## Intraday price update
The selection of this watchlist view can cause an **intraday price update**, taking into account the specified time interval since the last price update. This **time interval** is also used for each individual instrument for a possible price update. The date of the last update of the intraday prices is shown in the title bar of the watchlist. The time interval can be found in the "gt.w.intra.update.timeout.seconds" property of the **global setting**.

## Functions
This **performance view** has only the general functions implemented in the context menu.

### Menu "View" of the menu bar
**Time frame**: The setting via the **Time frame** menu item determines the period for the calculation of the "**Time frame %**" table column. If the time period is equal to or greater than one year, the **geometric average return** is used in the performance calculation. 
{{% notice info %}} 
Dividends are not yet included. This complicates the comparison between a price index and a performance index. The **price index**, also known as the **price index**, only takes price movements into account. The **performance index**, on the other hand, also includes distributions. 
{{% /notice %}}

## Expanding table row
An existing **expander icon** next to the instrument indicates its existing **transactions**. This is one of many places in Grafioschtrader where existing transactions can be edited. For more information, see [Securities Transactions](../../../transaction/security/).
