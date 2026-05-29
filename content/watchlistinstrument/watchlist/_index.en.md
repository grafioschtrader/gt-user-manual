---
title: "Watchlist"
date: 2025-11-18T10:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
In the main area there are **4 tabs** for the different **watch list views**, these differ primarily in their content, whereby the first columns of the table for identifying the instrument do not differ.
- **Performance**: This provides an overview of the price data and the **performance** of the **open positions**. It is the best place to record, process, and delete securities transactions.
- **Quote data feed**: This is used to monitor the reliability of the **data sources for price data**.
- **Custom field**: User-defined additional fields can be displayed here.
- **Dividend/Split Feed**: This provides you with information regarding **dividends and splits**.

## Navigation area functions
You can create and delete a watchlist in the **navigation area**.

### Create watchlist
The watchlist is created using the **context menu** on the static **Watchlist** element in the **navigation area**. There is a limit to the number of watchlists a user can create.

### Delete watchlist
The watchlist is deleted using the **context menu** on the static **Watchlist** element in the **navigation area**. The watchlist must not contain any instruments.

### Performance Watchlist {{< svg "chart-line.svg" svg-icon-size >}}
In the **navigation area**, a specific watchlist is marked with the **performance** Watchlist. This watchlist should contain all your open positions. This automatically updates the dependent **currency pairs**.

## General functions of the watchlist views
There are functions that are implemented in all watchlists. On the other hand, the table columns of the watchlists differ, so there may be different functions for each watchlist. The functions that are available in all views are described below.

### Functions highlighted watchlist view
These functions can be used on the selected watchlist.
- **Move instrument to another watchlist**: An instrument can be moved to another watchlist using drag and drop. To do this, drag the **instrument type** icon onto your selected **watchlist** in the **navigation area**.
- **Add existing instrument**: An existing instrument can be added to an active watchlist in the **main area** via the **context menu**. The corresponding [search dialog for instruments](../instrument/searchdialog) opens for this purpose.
  - The selected instruments are added to the watchlist using the **Add** button.
- **Add new security**: This creates a new **security** and adds it to the watchlist, for more information see [Security and derived instrument](../instrument/securityderived).
- **Add new derived instrument**: This function is used to create a **derived instrument** and add it to the watchlist, for more information see [Security and derived instrument](../instrument/securityderived).
- **Add newly created currency pair**: This creates a new **currency pair** and adds it to the watchlist, for more information see [Currency pair](../instrument/currencypair).

### Selected instrument functions
This function can be used on a selected instrument. Some of these functions are self-explanatory and may have a help text in the menu.
- **Remove instrument**: This can be used to remove a single selected instrument from the watchlist.
- **Update intraday price**: This updates the intraday price of a selected instrument, whereby the **time interval** since the last update cannot be overridden.
- **Edit additional field**: This dialog can be used to fill the self-defined additional fields with content.
- **Buy, sell, interest/dividend**: There is more information on this under [Securities transaction](../../transaction/security).
- **Daily data as line graph or table**: This group of functions is not limited to the watchlist, therefore see [End of day prices as line graph](../eodchart).
- **Hyperlink stock exchange and product**: See [security and derived instrument](../instrument/securityderived/).
- **Intraday and historical data hyperlink**: These two hyperlinks are supportive when checking connector settings.
  - For certain data sources, a file is downloaded rather than an additional web page being opened.
  - For data sources with an API key, the behavior of these functions is different for a user with administrator rights than for all other users. For the administrator, the link is executed in the browser, while for the other users the execution takes place in the backend and the result is then displayed from the frontend in a new web browser tab.
- **Message to creator**: This can be used to send a message to the creator of this instrument, for more information see [Message system](../../admindata).

### "View" menu of the menu bar
**Show columns**: Individual columns can be shown or hidden for a better overview. Depending on the display device, the display can be optimized. This adjustment is retained at the end of the session.

## Properties and table columns
Columns are not described further if these **instruments** or **asset classes** have been mentioned. Some columns are self-explanatory or their tooltips provide sufficient explanation. Certain table columns can be shown or hidden, see [Control elements](../../intro/userinterface/user_setting_ui_controls).

- **Instrument type (I)**: The symbols of the instruments can be taken from the [asset class](../../basedata/assetclass). {{< svg "d.svg" svg-icon-size >}}: This additionally stands for a derived instrument.
