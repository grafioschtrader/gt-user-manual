---
title: "Exchange price data"
date: 2026-08-31T22:54:47+01:00
draft: false
weight: 30
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
This view allows configuring the price data exchange for individual instruments. The exchange is configured separately for securities and currency pairs in different views. A **private security** does not appear here: it belongs to a single tenant and its price data is never exchanged with a peer.

### Overview of Views
The configuration is done in two tabs:
- **Securities**: Shows all securities with additional information such as ISIN, ticker symbol, currency, active date, and asset class.
- **Currency pairs**: Shows all currency pairs with their names.

### The Four Exchange Options
Four independent options can be configured for each instrument:

| Option | Description |
|--------|-------------|
| **Receive intraday price** | The instrument receives intraday prices from GTNet partners |
| **Receive historical price data** | The instrument receives historical prices from GTNet partners |
| **Send intraday prices** | The own intraday prices of this instrument are sent to GTNet partners |
| **Send historical price data** | The own historical prices of this instrument are sent to GTNet partners |

{{% notice info %}}
The receive options ("Receive...") determine whether the instrument is considered in the GTNet price exchange. Only instruments with an enabled option are queried via GTNet during the watchlist update.
{{% /notice %}}

### Working with the Table

#### Filter and Search
- **Active securities only** (securities only): Shows only securities that are currently active (active date is in the future).
- **Search field**: Allows filtering the table by any terms.

#### Who May Change the Options
The exchange options belong to the instrument rather than to a tenant. The ordinary editing rights of the instrument therefore apply: an administrator and a «Privileged user» may change the options of every instrument, every other user only those of the instruments they created themselves. Rows one may not change are shown with disabled checkboxes.

#### Bulk Editing
All filtered entries can be changed simultaneously using the checkboxes in the table header:
- Enabling a header checkbox sets the corresponding option to active for **all currently filtered rows**.
- Disabling sets the option to inactive for all filtered rows.

{{% notice note %}}
Bulk editing only affects the currently visible (filtered) rows. Use the search field to specifically narrow down the selection. Only rows one could also change individually are covered; disabled checkboxes stay untouched.
{{% /notice %}}

#### Saving Changes
Changes are not saved automatically. After editing, the **Save** button must be pressed. The button is only active when there are pending changes.

When leaving the view with unsaved changes, a confirmation dialog appears.

Changes made by an administrator are synchronised with the peers immediately when the view is left. Changes made by every other user take effect with the daily synchronisation.

### Supplier Details
Clicking on the expansion arrow of a row displays details about the GTNet suppliers providing data for this instrument:
- **Remote Domain**: The domain of the GTNet server
- **Server Status**: The current status of the server (Not specified, In operation, Closed). An announced maintenance is not reflected in this status, it applies to the whole instance and is described under [Maintenance and Discontinuation](../setup/availability/).
- **Price Type**: Whether it is intraday or historical prices
- **Last Update**: When data was last received from this supplier

#### Role in Request Filtering
The supplier details are not just informational but play an active role in the price exchange process. When requesting price data from GTNet suppliers, the system uses these details to filter which instruments are sent to which suppliers:

- **Servers in "Open" mode**: Only instruments for which a supplier detail entry exists are sent to these servers. If no supplier detail is registered for an instrument with a particular supplier, that instrument is not included in requests to that supplier. This prevents unnecessary requests for instruments the supplier cannot provide.
- **Servers in "Push Open" mode**: These suppliers receive all instruments regardless of supplier details. They maintain active connections with fresh data and may be able to provide data even for instruments not yet registered in their supplier details.

The supplier details are populated through the [exchange synchronization background task](../../taskdatachangemonitor/taskdescription/#23---synchronize-gtnet-exchange-configurations). It runs daily, is scheduled automatically as soon as a data exchange with a peer has been agreed, and can also be triggered manually from the [GTNet setup](../setup/) interface. A newly agreed exchange therefore has its supplier details without waiting for the next daily run.

### Restrictions for Securities
For **inactive securities** (active date is in the past), the intraday options are disabled:
- "Receive intraday price" cannot be enabled
- "Send intraday prices" cannot be enabled

This is because current day prices are no longer fetched for inactive securities.

### Context Menu
Via the context menu (right-click on a row), the underlying security or currency pair can be edited. This opens the corresponding edit dialog.

### Interaction with Price Exchange
The options configured here determine the behavior during GTNet price exchange:
- Instruments with an enabled **"Receive..."** option are considered in the price exchange and can receive data from remote servers.
- Instruments with an enabled **"Send..."** option make their local data available to other GTNet instances.

The detailed workflows of the price exchange are described under:
- [Last Price Exchange](lastprice/)
- [Historical Price Exchange](historicalprice/)
