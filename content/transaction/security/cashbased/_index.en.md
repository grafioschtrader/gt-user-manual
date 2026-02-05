---
title: "Asset value recognized transaction"
date: 2021-06-16T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
With the exception of **Forex** and **CFD** instruments, the asset is booked when **the purchase** is made, i.e. the account balance decreases by the transaction amount and the corresponding securities account increases more or less by this asset.

## Buying and selling
The **visibility of** the **properties** can change depending on the choice of instrument. In the case of a bond and convertible bond, for example, the accrued interest can be entered.

### Buy and sell properties
- **Accrued interest**: For **bonds** and **convertible bonds**, this input field appears for the accrued interest.

## Selling
Only held positions with their maximum number of units can be sold.

## Interest/dividend
When entering interest or dividends, the system checks whether the specified number of units was held **at the time of the transaction**. In the case of a **dividend**, there is the **ex date** and the **payment date**, whereby the payment date is always the more recent date. Ultimately, you can still receive **dividends** for an **instrument that has been sold**, as the **ex-date** was before and the **payment date** after the sale. In such a case, the **ex-date** must be entered in the **transaction** so that the plausibility check of the transaction is successful.
- A **negative dividend or interest** can be recorded for a **reversal**. But it should not be used for accrued interest.

### Interest/dividend properties
- **Subject to tax**: Most income is subject to tax, therefore the checkbox is activated. However, there is also tax-exempt income. For example, tax-exempt capital gains in Switzerland, in such cases deactivate the checkbox.

## Video with practice of securities transactions
Where are securities transactions recorded and processed in GT. This is explained using examples with an ETF and a bond. The dividend includes a currency that differs from the instrument and the accrued interest is explained for the bond.
- Purchase, dividend and sale of an ETF
- Purchase, interest and sale of a bond

Unfortunately only in German:
{{< youtube BiEIfaGTus4 >}}

## Special events with multiple transactions
Sometimes there are **events** that can be replicated with multiple transactions. When replicating these events in transactions, **the account balance** will be **temporarily** affected. Ultimately, it must be considered whether the replication of the event changes the account balance or leaves it unchanged and what the effects are on the securities account(s). The necessary transactions can usually be easily derived from the consideration of this initial situation and the desired result.

### Securities transfer
In GT, a **securities transfer** with a **securities delivery** and **securities deposit** is carried out as follows:
1. On the day of the **securities transfer**, a **sell transaction** is executed on the held position of this **instrument**. Possible transfer fees are entered as **transaction costs**.
2. The amount resulting from this sale transaction, excluding transaction costs, is **transferred** to an **account** of the **securities depository** with an **account transfer**.
3. Buy the same **instrument** again from the **securities** **deposit** **account**, using the same **price** and the same **date** for the transaction time as for the sale.
4. Repeat **steps 1-3** for all instruments in this **securities transfer**. **Step 2** can be the sum of several **securities sales**.

Using this method, it is still possible to see in **GT** how successfully the corresponding **instrument** was traded. In **GT**, the entire history of **all trades** for an **instrument** can be viewed at **portfolio** level under **Securities account** and in the **watchlist**. Additional advantages of this possibly unique method:
- The **loss or profit** is credited to the securities account of the **securities delivery** up to the time of the **securities transfer**.
- The possible **transfer fees** are correctly allocated to the **instrument** and **custody account** **involved**.
- The **balance** of the **bank accounts** involved is **the same** before step 1 and after step 3. Excluding the transfer fees, of course.

### Spin-off
...
