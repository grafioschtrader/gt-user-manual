---
title: "Margin based transaction"
date: 2021-06-16T22:54:47+01:00
draft: false
weight: 20
archetype: "default"
---
Transactions for **Forex** and **CFD** are **margin based transactions**. There is one transaction that **opens** the position and one or more transactions that **close** this open position. Only the closing of the position generates an account-relevant entry.

{{% notice warning %}}
The **margin** or **security deposit**, an important part of leverage trading, is not taken into account in GT. It is a **security deposit** and not a fee or **transaction cost**. It serves to ensure that the trader does not trade beyond his financial means. **GT is not suitable as a monitoring tool for taking high leveraged risks**.
{{% /notice %}}

## Why does GT support margin trading?
Sometimes there are certain opportunities in the market where it is difficult for the trader to gain an advantage with instruments such as shares or ETFs. Even without owning the underlying asset, a trader may be able to profit from a market situation. Here are two examples:
- Anyone who believes that the USD is undervalued against the CHF. Buy the USD against the CHF and sell it after it has appreciated. This **forex trade** is much cheaper than actually buying and selling USD.
- Assuming you think the SMI index is significantly overvalued, you can sell the SMI 20 as a CFD, i.e. you open a short position. Later, when the SMI index has fallen, you close this short position and buy back the index at a lower price.

**Market risks** arise while you **have** these **trading positions open**. GT tries to replicate these market risks. Ultimately, such transactions have an impact on the performance of a portfolio and must therefore be replicated appropriately.

Unfortunately only in German:
{{< youtube Rq7HK3dfeFA >}}

### Forex
**Forex** is a technically simple and financially favorable way to trade currency pairs, therefore this trade can be replicated in GT.
{{% notice warning %}}
The **open forex positions** are currently **not netted** against the existing **cash positions**. **Possible losses could therefore exceed the cash position**.
{{% /notice %}}

### CFD
If prices on the financial markets were only rising, then there would probably be no support for this financial instrument in **GT**. As we know, this is not the case. With **CFDs**, for example, you can hedge your share positions for a short time or generally bet on falling markets.

## Opening a position
As both short and long positions are supported, you can speculate on falling or rising prices. The **position** is opened by **selling** or **buying**. The corresponding instrument must be selected. These two functions can be accessed as usual via the context menu.

## Close position
When closing the position, one of the opening positions of the instrument must be selected. These become visible with the **expanding table row** of the instrument. The **open items** are displayed in the upper hierarchical structure in the **hierarchy table**. Open items are highlighted in yellow to make them easier to recognize. The **financing costs** and the **closing items** are shown as accounts of these **open items**.

## Financing costs
Forex and CFD positions may incur costs that have to be paid periodically or all at once. All these costs can be recorded using the **Financing costs** transaction.
- Several **financing costs** are possible for an open or closed position.
- Negative **financing costs** can also be recorded.

### Properties
Most of the properties are self-explanatory and are therefore not explained further; a tooltip is also available for certain properties.
- **Opening or closing a position**
  - Although the **tax** and **transaction costs** properties are unlikely to be used for this type of transaction, they are still made available.
  - **Security risk**: This is the calculated risk taken with this position.
