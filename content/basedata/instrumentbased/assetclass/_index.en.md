---
title: "Asset class"
date: 2021-05-09T22:54:47+01:00
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
The property is freely definable, whereby consistent texts are expected. In the case of a regional categorization, for example, the text "Equities USA" should be used consistently for the corresponding asset classes. GT currently supports the two languages **German** and **English**.

#### Financial instrument
As shown above, the selection of the financial instrument depends on the selection of the asset class.
+ **CFD** and **Forex**: Such an instrument is traded with **margin**. A position is opened or closed and the **profit/loss** becomes relevant to the account when it is closed. Further information on this topic in [Securities transactions](../../../transaction/security)
+ **Issuer risk Product ETC/ETN etc.:** If no other **financial instrument** can be assigned to the **instrument**, this one is used. These instruments include **ETCs**, **ETNs** and **structured products (certificates)**, etc. The issuer risk is their common feature.
