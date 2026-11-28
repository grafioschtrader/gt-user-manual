---
title: "Asset class"
date: 2026-11-22T12:00:00+01:00
draft: false
weight: 5
archetype: "default"
---
When recording an **instrument**, the **selection** of an **asset class** has a considerable influence on its characteristics. It also influences the reporting and the type of transaction that can be applied to an instrument.

+ As soon as an **instrument** references the asset class, only the "sub-asset class" property can be changed.
+ An asset class referenced by an **instrument** cannot be deleted.

## Create and edit asset class
An **asset class** is created, edited and deleted in the **main area** in the **Asset class view**. You can access this **view** in the **navigation area** on the static element "Asset class".
+ **Create** an **asset class** via the **context menu**.
+ **Edit** an **asset class** via the **context menu** when **the asset class is selected**.
+ **Delete** an **asset class** via the **context menu** with **the asset class selected**.

### Properties and table columns
As mentioned above, the properties can be changed to a limited extent. The properties and the table columns are identical and therefore do not require a differentiated description.

#### Asset class
The **asset class** property influences the selection of **financial instrument**; the following combinations are possible:

| Asset class            |          Direct investment         |                     ETF                    |           Mutal Fund          |            Pension fund           |                  CFD                  |                Forex               |  Issuer risk Product ETC/ETN etc.  |        Index not investable       |
| ---------------------- | :--------------------------------: | :----------------------------------------: | :--------------------------------: | :-------------------------------: | :-----------------------------------: | :--------------------------------: | :--------------------------------: | :-------------------------------: |
| **Equties**             | {{< svg "eq.svg" svg-icon-size >}} |    {{< svg "etf_i.svg" svg-icon-size >}}   |  {{< svg "f.svg" svg-icon-size >}} | {{< svg "f.svg" svg-icon-size >}} | {{< svg "cfd_i.svg" svg-icon-size >}} |                                    | {{< svg "ir.svg" svg-icon-size >}} | {{< svg "i.svg" svg-icon-size >}} |
| **Fixed Income**              | {{< svg "bo.svg" svg-icon-size >}} |    {{< svg "etf_i.svg" svg-icon-size >}}   |  {{< svg "f.svg" svg-icon-size >}} | {{< svg "f.svg" svg-icon-size >}} |                                       |                                    | {{< svg "ir.svg" svg-icon-size >}} | {{< svg "i.svg" svg-icon-size >}} |
| **Commodities**        | {{< svg "co.svg" svg-icon-size >}} |    {{< svg "etf_c.svg" svg-icon-size >}}   |  {{< svg "f.svg" svg-icon-size >}} |                                   | {{< svg "cfd_c.svg" svg-icon-size >}} |                                    | {{< svg "ir.svg" svg-icon-size >}} | {{< svg "i.svg" svg-icon-size >}} |
| **Money market**       |  {{< svg "m.svg" svg-icon-size >}} |    {{< svg "etf_i.svg" svg-icon-size >}}   |  {{< svg "f.svg" svg-icon-size >}} |                                   |                                       |                                    |                                    |                                   |
| **Real estate**        |                                    |    {{< svg "etf_i.svg" svg-icon-size >}}   | {{< svg "fr.svg" svg-icon-size >}} |                                   |                                       |                                    | {{< svg "ir.svg" svg-icon-size >}} | {{< svg "i.svg" svg-icon-size >}} |
| **Credit derivatives** |                                    |    {{< svg "etf_i.svg" svg-icon-size >}}   |  {{< svg "f.svg" svg-icon-size >}} |                                   |                                       |                                    | {{< svg "ir.svg" svg-icon-size >}} | {{< svg "i.svg" svg-icon-size >}} |
| **Multi asset**        |                                    |    {{< svg "etf_i.svg" svg-icon-size >}}   |  {{< svg "f.svg" svg-icon-size >}} | {{< svg "f.svg" svg-icon-size >}} |                                       |                                    | {{< svg "ir.svg" svg-icon-size >}} | {{< svg "i.svg" svg-icon-size >}} |
| **Currency pair**      |                                    | {{< svg "etf_crypto.svg" svg-icon-size >}} |                                    |                                   |                                       | {{< svg "fx.svg" svg-icon-size >}} | {{< svg "ir.svg" svg-icon-size >}} |                                   |
| **Convertible bond**   | {{< svg "cb.svg" svg-icon-size >}} |    {{< svg "etf_i.svg" svg-icon-size >}}   |  {{< svg "f.svg" svg-icon-size >}} | {{< svg "f.svg" svg-icon-size >}} |                                       |                                    |                                    | {{< svg "i.svg" svg-icon-size >}} |

#### Sub-asset class

GT groups the supplied asset classes mainly by country or region. The sub-asset class identifies the region, for example “Equities USA”. You can choose the labels in German and English. Use the same label wherever you refer to the same group.

Switching to sectors later would mean reviewing each affected instrument and reassigning it where needed. Each instrument belongs to exactly one asset class in GT. A sector-based classification has not been tested in GT.

{{% callout style="note" title="How can you measure sector exposure across individual stocks and ETFs?" %}}
Suppose you hold CHF 2,000 in shares of a technology company and CHF 10,000 in an ETF with a 30% allocation to technology. Your total technology exposure is CHF 5,000. The shares contribute their full value and the ETF contributes CHF 3,000. This calculation needs the ETF’s sector weights on the date of the report. For an example of published sector weights, see the [iShares Core S&P 500 ETF](https://www.ishares.com/us/products/239726/ishares-core-sp-500-etf).

The *Global Industry Classification Standard* (GICS), developed by MSCI and S&P Dow Jones Indices, is a candidate for a future sector report in GT. It classifies companies by their principal business activity. For the total to be meaningful, “technology” must cover the same companies whether you hold them directly or through the ETF. A provider using another standard, such as the *Industry Classification Benchmark* (ICB), may draw the sector boundaries differently. [GICS at MSCI](https://www.msci.com/indexes/index-resources/gics) · [ICB at FTSE Russell](https://www.lseg.com/en/ftse-russell/industry-classification-benchmark-icb)

Obtaining the data is a challenge for GT. The GICS methodology is public, and use of the company classifications is subject to licensing terms. A combined report would require consistent, up-to-date data covering both stocks and ETFs, with permission for GT to use them. Such data are difficult to obtain free of charge. [MSCI terms of use](https://www.msci.com/legal/terms-of-use)
{{% /callout %}}

#### Financial instrument
As shown above, the selection of the financial instrument depends on the selection of the asset class.
+ **CFD** and **Forex**: Such an instrument is traded with **margin**. A position is opened or closed and the **profit/loss** becomes relevant to the account when it is closed. Further information on this topic in [Securities transactions](../../../transaction/security)
+ **Issuer risk Product ETC/ETN etc.:** If no other **financial instrument** can be assigned to the **instrument**, this one is used. These instruments include **ETCs**, **ETNs** and **structured products (certificates)**, etc. The issuer risk is their common feature.
