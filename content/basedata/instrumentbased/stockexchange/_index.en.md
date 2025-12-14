---
title: "Stock exchange"
date: 2021-03-29T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
The instruments are traded on the stock exchanges. In GT, these are important for the following reasons:
+ The trading calendar of a stock exchange can be used to evaluate the **completeness** of **historical price data**.
+ The assignment of the stock exchange to an **instrument** determines whether there is any publicly accessible price data for this instrument.
+ In a future GT version, the update of **historical price data** will be staggered over the periods of the **closed stock exchanges**.
The **country** and **time zone** can no longer be changed for an existing stock exchange; this is intended to prevent the total rewriting of a stock exchange.

## Create and edit asset class
A **stock exchange** is created, edited and deleted in the main area in the **stock exchange view**. You can access this **view** in the **navigation area** on the static "stock exchanges" element.
- **Create** a **stock exchanges** via the **context menu**.
- **Edit** a **stock exchanges** via the **context menu** when **a stock exchanges is selected**.
- **Delete** a **stock exchange** via the **context menu** with **the stock exchange selected**.

## Properties and table columns
- **MIC**: When the **MIC (Market Identifier Code)** is selected, the **name**, **country** and **website** are automatically assigned. For the **main stock exchanges**, the **time zone** is also set. The MIC can be used in certain price data connectors.
- **Country**: The **country** is controlled by the selection of the **MIC** and cannot be changed.
- **Name**: The **name of** the stock exchange is automatically specified in lower case by the selection of the **MIC**. This should be corrected accordingly. This may change over time due to mergers of stock exchanges.
- **Secondary market**: Some securities are not traded on the secondary market, but there is price data that can be downloaded from a provider. However, most securities are traded on the secondary market.
- **No course data**: If there is no price data, this checkbox should be marked. This activates the option to enter **"Historical prices for period"** when entering the **instrument**.
- **Opening time, closing time and time zone**: In a future GT version, the update of historical price data will be staggered in the time periods of the closed stock exchanges. These properties determine the period in which the update can take place.

## Additional properties when editing
- **Main exchanges only**: The selection of the **MIC** is restricted or extended to the **main exchanges** or all known MICs according to this selection box. It is partly arbitrary what is defined as a **main exchange** in GT, we hope that nobody feels negatively affected by this selection.

### Index for trading calendar
The **trading calendar** can be updated automatically via an **index** traded on this stock exchange. Of course, this can only be set with the second processing of the **stock exchange**. This means:
1. Enter the stock exchange.
2. Optionally create an asset class for the corresponding country and index.
3. Create an instrument for this index.
4. Add the stock exchange with the created index in the "**Index for trading calendar**" input field.

## Trading calendar
The **trading calendar** of a **stock exchange** is a prerequisite for the **completeness check** of the historical price data. The "**automated marking**" for closed days via the index is only applied after the most recent day marked by the user. This means that the system never marks a day that is older than the most recent **blue** marker.
- **Open days**: Days marked in **green** are defined from the **global trading calendar**.
- **Closed days**: Days marked in **red** were originally created by the **index for trading calendars**. Days marked in **blue** were created by the user. The copy functions do not change these characteristics.

### Functions
There are two copy functions for copying an entire trading calendar or an annual calendar to the selected annual calendar. Existing calendars are overwritten.
