---
title: "Market map"
date: 2026-11-11T10:00:00+01:00
draft: false
weight: 40
archetype: "default"
---
The **Market map** tab shows the stocks of a watchlist as a treemap. Each rectangle stands for one stock. Its **area** is the **market capitalisation** of the company, its **colour** the current **percentage price change**: red for a loss, green for a gain, grey for an unchanged stock. You see at a glance which large stocks move the market day of your watchlist.

## Layout of the map
The stocks are grouped by the **sub category of their asset class**, which for stocks is the country or region, for example "Stocks USA" or "Stocks Switzerland". The groups are ordered by their total market capitalisation, the stocks within a group by area. The colour of a group is the price change of its stocks weighted by market capitalisation. A click on a group zooms into it, the path bar above the map leads back.
The colour scale runs from −3 % over 0 % to +3 %. Larger changes take the end colour, so a single outlier does not wash out the other rectangles. The price change is also printed on every rectangle, so the colour is never the only carrier of the information.
Hovering over a rectangle shows the market capitalisation in the tenant's **main currency**, the price change and the time of the price. A right click on the rectangle of a stock opens the price history menu, see [End of day prices as line graph](../../eodchart). The **Refresh** button loads the current prices of the watchlist and redraws the map.

## Where the values come from
The area is the number of shares multiplied by the current price, converted into the main currency at the newest closing rate of the currency pair. The number of shares comes from the [fundamental data]({{% relref "/watchlistinstrument/watchlist/indicator#fundamental-data" %}}). If it is missing, GT falls back to the market capitalisation supplied by Yahoo. The price change is the change in the currency of the listing; exchange rate effects are not included.
Outside trading hours the colour shows the change of the last session of the respective exchange. A price change that lies more than one hour behind the reference time counts as outdated, and the stock is not drawn. This affects, for example, a stock that is hardly traded or a data source that has been failing for several sessions.

## What is not shown
Only **stocks held as direct investments** are considered. ETFs, funds, bonds and currency pairs have no market capitalisation; GT states their number below the map. Stocks that are missing for another reason are listed there under **Not shown**, by reason:
- **no current price change**, for example because a newer closing price than the intraday price is already available.
- **price change is outdated**.
- **market capitalisation missing**.
- **price or exchange rate missing**.

## Limitations
- The market capitalisation from Yahoo is confirmed by no second source and is as old as the last weekly load of the fundamental data.
- A stock newly added to the watchlist gets an area only after the next load of the fundamental data and is listed under **Not shown** until then. For administrators: the load is [background task 56]({{% relref "/admindata/taskdatachangemonitor/taskdescription/pricedata#JOB56" %}}) in the [batch processing monitor]({{% relref "/admindata/taskdatachangemonitor" %}}).
- If a company has several share classes with different ISINs, it appears once per class, and each rectangle may carry the market capitalisation of the whole company. Several listings of the same ISIN are shown only once, with the newest price.
- The grouping depends on the maintained asset classes. "Stocks World", for example, appears as a group of its own.
