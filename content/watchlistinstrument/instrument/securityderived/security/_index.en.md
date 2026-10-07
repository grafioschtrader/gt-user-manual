---
title: "Security"
date: 2026-09-22T12:00:00+02:00
draft: false
weight: 8
archetype: "default"
---

## Properties and table columns
The combination of **ISIN** and **currency** is unique. This makes it impossible to enter the same security, i.e. ISIN/currency, with different stock exchanges. In addition, these can no longer be changed after they have been saved for the first time.
+ **Name**: For **bonds** and **convertible bonds**, the name should begin with the coupon rate. For example, "0.0219 IBK 19-25" for the Industrial Bank of Korea bond. In GT there is a functionality that reads the interest rate from the name, see [Yield to Maturity](../../../../basedata/udfmetadata/instruments/). The edit dialog also proposes this rate as **Annual coupon (%)** in the **Bond simulation** tab.
+ **Leveraged inverse**: Indicates a leveraged and/or inverse instrument. An open position increases the corresponding market exposure in the corresponding asset class according to the positive or negative leverage. This is only supported for ETF and ENT **financial instruments**. For an inverse security, this factor will be negative.

### Asset class
+ Selecting the **asset class** changes the **visibility** of the "**Security split/s**" tab. No split can be entered for bonds and convertible bonds. In addition, the selected **stock exchange** must provide price data, otherwise the security cannot have a split. In GT, a stock exchange can also be defined without price data.
+ The choice of the **asset class** property also influences the visibility of other properties such as **denomination** and **issuer country**, as well as of the **Bond simulation** tab.
+ The **asset class** and **financial instrument** properties can no longer be changed for an existing **security** with one or more **transactions**.

### Denomination
For bonds this is the smallest tradable unit, for fixed-term deposits it is the price. There is currently no validation for purchases and sales. It is therefore possible that an insolvent creditor may choose other denominations for partial or full repayment. In the case of bonds, this information is therefore primarily of an informative nature.

### Issuer country
The **issuer country** is optional and is used by the [simulation tax models]({{% relref "/algoalert/historicalrun/taxmodel" %}}), for example for a withholding tax that depends on the domicile of the issuer. It is offered for every security with an issuer, but not for the financial instruments CFD, Forex and Index not investable.

While you enter the **ISIN**, the dialog proposes its first two letters as issuer country. As soon as you select a country yourself, the proposal no longer changes it. For existing securities, the issuer country was filled in the same way during the update. Check the value for bonds in particular: the first two letters of an ISIN name the country that assigned the ISIN, which is not always the domicile of the issuer. CHF bonds of foreign issuers listed on SIX, for example, usually carry an ISIN beginning with CH.

### Stock exchange
+ If a **stock exchange** without price data is selected, the **"Historical prices for period"** tab appears in the dialog.
+ As soon as prices exist for the security or a transaction has been entered, the selection list only offers stock exchanges of the same kind, see [Stock exchange](../#stock-exchange).

## Historical prices for period" tab
See also [Security without price data](./securitywithoutpricedata).

## Tab "Security split/s"
As mentioned in the chapter [Historical data](../../../externaldata/historyquote/), a split of data sources is read in. It can happen that the data source does not track the split accurately in terms of time. For this reason, a **split** can also be entered manually. A **split** that has been entered manually is not overwritten or removed by the system.

A split is added by entering the **Split date**, the **From quantity** and the **To quantity** and then clicking **Apply**. The new entry appears in the table below the form. The ratio *From quantity : To quantity* describes the split: with `2 : 1` one share becomes two, `3 : 2` corresponds to a consolidation. Selecting a row in the table lets you edit or delete the entry. If an already-existing date is entered again, the entry overwrites the existing one for that date instead of duplicating it. The changes are only persisted when the whole **security** is saved.

{{% notice style="info" title="Maximum number of splits" %}}
Up to 20 splits per instrument can be entered by default. An administrator can adjust this limit through **Security / Split** of the limit type **Total number**, see [Limit information class]({{% relref "/admindata/entitylimit" %}}). The table footer shows the current count and the allowed maximum.
{{% /notice %}}

## Tab "Bond simulation"
This tab appears for bonds and convertible bonds with the financial instrument **Direct investment**. Its entries are used only by the [historical replay]({{% relref "/algoalert/historicalrun" %}}) when **Generate regular bond coupons** is switched on and no distributions are stored for the bond. They have no influence on your real transactions, whose accrued interest always comes from the booking itself. If the asset class is changed to a different kind of instrument, the entries are removed when saving.

Regular coupon dates are derived backwards from the **Active until date**, and the number of coupons per year comes from the **Distribution Frequency**. The nominal amount is always 100 per unit. Irregular first or last periods, calls, floating rates and business-day adjustments are not supported.

+ **Annual coupon (%)**: The yearly interest rate of the bond. The dialog proposes the number at the beginning of the name, as long as you have not entered a rate yourself. A rate of 0 describes a zero-coupon bond, for which no coupons are generated.
+ **Coupon day-count convention**: Determines how the accrued interest is calculated when the simulation buys or sells the bond between two coupon dates. The amount of a full coupon does not depend on it. **Actual/Actual ICMA** counts the actual days of the coupon period, **30E/360 (European)** counts every month as 30 days.

The applicable convention is stated in the prospectus of the bond. As it depends on the bond market in which the bond was issued rather than on the domicile of the issuer, the dialog proposes it from the **currency** of the security: **30E/360 (European)** for CHF, **Actual/Actual ICMA** for all other currencies. This matches the usual practice for CHF bonds on SIX and for government bonds and most corporate bonds in EUR, GBP and USD. The proposal only fills an empty field; a stored or manually selected convention remains unchanged. If the field is left empty, the historical replay applies the same proposal, so securities saved earlier or created by an import also receive generated coupons as soon as they have a coupon rate.

{{% notice style="info" title="Small deviations are possible" %}}
USD corporate bonds usually accrue on 30/360 (US) and JPY bonds on Actual/365; neither is offered, and **30E/360 (European)** or **Actual/Actual ICMA** is used instead. The two conventions differ by at most one or two days of interest, so the effect on a simulation result is minor.
{{% /notice %}}
