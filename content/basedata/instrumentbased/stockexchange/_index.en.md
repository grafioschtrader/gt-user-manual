---
title: "Stock exchange"
date: 2026-09-03T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
The instruments are traded on the stock exchanges. In GT, these are important for the following reasons:
+ The trading calendar of a stock exchange can be used to evaluate the **completeness** of **historical price data**.
+ The assignment of the stock exchange to an **instrument** determines whether there is any publicly accessible price data for this instrument.
+ In a future GT version, the update of **historical price data** will be staggered over the periods of the **closed stock exchanges**.
The **country** and **time zone** can no longer be changed for an existing stock exchange; this is intended to prevent the total rewriting of a stock exchange. The **No market value** setting can only be corrected as long as no instrument is assigned to the stock exchange.

## Create and edit asset class
A **stock exchange** is created, edited and deleted in the main area in the **stock exchange view**. You can access this **view** in the **navigation area** on the static "stock exchanges" element. The view is split into two tabs: the **stock exchange** itself and the [trading calendar rules](./tradingcalendarruleset/), which offer a second, shared source for the trading calendar.
- **Create** a **stock exchanges** via the **context menu**.
- **Edit** a **stock exchanges** via the **context menu** when **a stock exchanges is selected**.
- **Delete** a **stock exchange** via the **context menu** with **the stock exchange selected**.

## Properties and table columns
- **MIC**: When the **MIC (Market Identifier Code)** is selected, the **name**, **country** and **website** are automatically assigned. For the **main stock exchanges**, the **time zone** is also set. The MIC can be used in certain price data connectors. The MIC dropdown includes a **search field** that allows you to quickly filter MICs by name or code.
- **Country**: The **country** is controlled by the selection of the **MIC** and cannot be changed.
- **Name**: The **name of** the stock exchange is automatically specified in lower case by the selection of the **MIC**. This should be corrected accordingly. This may change over time due to mergers of stock exchanges.
- **Secondary market**: Some securities are not traded on the secondary market, but there is price data that can be downloaded from a provider. However, most securities are traded on the secondary market.
- **No market value**: If there is no publicly available price data for the instruments of this stock exchange, this checkbox should be marked. It activates the option to enter **"Historical prices for period"** when entering the **instrument**; a price data connector is not available for such an instrument. As long as no instrument is assigned to the stock exchange, the setting can still be corrected. As soon as the first instrument is assigned, the checkbox is shown greyed out and the setting can no longer be changed, because the prices already recorded would become unusable. The only remaining way to switch is to create a new stock exchange with the desired setting and assign the instruments to it. Only instruments for which no prices exist yet and which have not been traded can be moved in this way, see [Stock exchange](../../../watchlistinstrument/instrument/securityderived/#stock-exchange).
- **Opening time, closing time and time zone**: In a future GT version, the update of historical price data will be staggered in the time periods of the closed stock exchanges. These properties determine the period in which the update can take place.

## Additional properties when editing
- **Main exchanges only**: The selection of the **MIC** is restricted or extended to the **main exchanges** or all known MICs according to this selection box. It is partly arbitrary what is defined as a **main exchange** in GT, we hope that nobody feels negatively affected by this selection.

### Calendar source: index or rule set
The **trading calendar** of a stock exchange can be maintained automatically from one of two sources, and the two are **mutually exclusive** — an exchange uses either an index or a rule set, never both. Selecting one source disables the input field of the other.

The first source is an **index** traded on this stock exchange, entered in the "**Index for trading calendar**" field. The dropdown only shows **non-investable indices from the country** of the stock exchange, loaded dynamically when a country is selected. Every candidate trading day on which the index delivered no price becomes a closed day. If the required index instrument **already exists**, it can be selected directly when creating the stock exchange. Otherwise, the workflow is:
1. Create the stock exchange.
2. Optionally create an asset class for the corresponding country and index.
3. Create an instrument for this index.
4. Edit the stock exchange and set the created index in the "**Index for trading calendar**" input field.

The second source is a "**Trading calendar rule set**", selected from the dropdown of the same name. Choosing a MIC for which a matching rule set exists preselects that rule set automatically. Unlike the index, a rule set derives the closed days from holiday rules and therefore also covers future years without price data. See [trading calendar rules](./tradingcalendarruleset/) for how these rule sets are created and maintained.

## Trading calendar
The **trading calendar** of a **stock exchange** is a prerequisite for the **completeness check** of the historical price data. The "**automated marking**" for closed days is only applied after the most recent day marked by the user. This means that the system never marks a day that is older than the most recent **blue** marker.
- **Open days**: Days marked in **green** are defined from the **global trading calendar**.
- **Closed days**: Days marked in **red** were created by the system from the chosen calendar source — the **index** or the **rule set**. Days marked in **blue** were created by the user. The copy functions do not change these characteristics.

### Functions
There are two copy functions for copying an entire trading calendar or an annual calendar to the selected annual calendar. Existing calendars are overwritten.
