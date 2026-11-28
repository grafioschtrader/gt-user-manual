---
title: "GT after installation"
date: 2026-11-25T12:00:00+01:00
draft: false
weight: 60
archetype: "default"
---
After installing GT, the **first start** of the **back end** fills certain shared **information classes** with some **entities**. The user whose email address was entered during installation in `application.properties` under the property `g.main.user.admin.mail` receives the rights of the [main administrator](../../userrights/#hauptadmin) on registration. A few minutes later, this user becomes the owner of these entities. The following figures refer to a new installation of GT 0.40.2.
- **Instruments**: The selection comprises 65 **securities** and three **currency pairs**. They include indices whose prices GT can use to identify the trading days of individual stock exchanges. The instruments use a variety of different **connectors** and can serve as a template or cheat sheet for your own instruments.
- **Splits** and **dividends**: Some of the included securities already have **splits** recorded. GT downloads the **dividends** through the configured connectors after the first start.
- **Historical price data**: No prices are included. GT downloads the **historical price data** of the included instruments after the first start and then keeps it continuously up to date. How far the price history goes back depends on the instrument and the data provider.
- **Basic data**:
  - **Asset classes**: GT includes 84 **asset classes** whose **sub asset class** is aligned mainly with countries and regions. The chapter on asset classes explains why this [geographic grouping](../../../basedata/instrumentbased/assetclass/#sub-asset-class) is difficult to change later.
  - **Stock exchange**: The 24 "most important" [stock exchanges](../../../basedata/instrumentbased/stockexchange/) are recorded together with their trading calendars.
  - **Trading platform plan**: The 18 [trading platform plans](../../../basedata/tradingplatformplan/) are used to assign the **import template group**.
  - **Import template group**: For importing PDF receipts and CSV files, 44 templates are available in seven groups, for example for Swissquote and DEGIRO. The Grafioschtrader group is for receipts and transaction lists exported from GT itself. If GT does not recognise a receipt from your trading platform, check the corresponding [import template group](../../../basedata/imptranstemplate/).
- **Administrative data**:
  - **Global trading calendar**: The [global trading calendar](../../../admindata/tradingcalendar/) is the **superset** of all trading days. An individual stock exchange may close on additional days, such as a local holiday. Depending on the exchange, GT determines these exceptions from a set of calendar rules or from the prices of a reference index. The years 2000 through 2027 as well as 2029 and 2030 are prefilled. For 2026 and 2027, Monday to Friday are possible trading days, excluding 1 January and 25 December. During an update, GT fills each of these two years only if it has no entries yet.
  - **Global settings**: The [global settings](../../../admindata/globalsettings/) allow the administrator to influence the system-wide behaviour of GT.

{{% callout style="warning" title="Trading calendar 2028" %}}
The year 2028 is empty on a new installation. Before recording transactions for 2028, open the [global trading calendar](../../../admindata/tradingcalendar/) as an administrator, select 2028, mark the trading days and save the year.
{{% /callout %}}

Video in German.

{{< youtube tsW2oCACLLA >}}
