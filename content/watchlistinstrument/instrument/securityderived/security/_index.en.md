---
title: "Security"
date: 2021-04-01T10:54:47+01:00
draft: false
weight: 8
archetype: "default"
---

## Properties and table columns
The combination of **ISIN** and **currency** is unique. This makes it impossible to enter the same security, i.e. ISIN/currency, with different stock exchanges. In addition, these can no longer be changed after they have been saved for the first time.
+ **Name**: For **bonds** and **convertible bonds**, the name should begin with the coupon rate. For example, "0.0219 IBK 19-25" for the Industrial Bank of Korea bond. In GT there is a functionality that reads the interest rate from the name, see [Yield to Maturity](../../../../basedata/udfmetadata/instruments/). In addition, the interest rate is not stored anywhere else in the security.
+ **Leveraged inverse**: Indicates a leveraged and/or inverse instrument. An open position increases the corresponding market exposure in the corresponding asset class according to the positive or negative leverage. This is only supported for ETF and ENT **financial instruments**. For an inverse security, this factor will be negative.

### Asset class
+ Selecting the **asset class** changes the **visibility** of the "**Security split/s**" tab. No split can be entered for bonds and convertible bonds. In addition, the selected **stock exchange** must provide price data, otherwise the security cannot have a split. In GT, a stock exchange can also be defined without price data.
+ The choice of the **asset class** property also influences the visibility of other properties such as **denomination**.
+ The **asset class** and **financial instrument** properties can no longer be changed for an existing **security** with one or more **transactions**.

### Denomination
For bonds this is the smallest tradable unit, for fixed-term deposits it is the price. There is currently no validation for purchases and sales. It is therefore possible that an insolvent creditor may choose other denominations for partial or full repayment. In the case of bonds, this information is therefore primarily of an informative nature.

### Stock exchange
+ If a **stock exchange** without price data is selected, the **"Historical prices for period"** tab appears in the dialog.

## Historical prices for period" tab
See also [Security without price data](./securitywithoutpricedata).

## Tab "Security split/s"
As mentioned in the chapter [Historical data](../../../externaldata/historyquote/), a split of data sources is read in. It can happen that the data source does not track the split accurately in terms of time. For this reason, a **split** can also be entered manually. A **split** that has been entered manually is not overwritten or removed by the system.
