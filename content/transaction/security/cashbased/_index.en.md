---
title: "Asset value recognized transaction"
date: 2026-11-20T22:54:47+01:00
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
Some **events** are recorded with several transactions. The **account balance** changes temporarily while they are entered. What matters is whether the event changes the account balance in the end or leaves it unchanged, and how it affects the securities account(s). From this starting point and the desired result, the necessary transactions can usually be derived easily.

### Security transfer
To move a security into another securities account, use the [Security transfer]({{% ref "/basedata/securityaction/securitytransfer" %}}) function. In one step it creates the sale in the previous securities account, the transfer between the cash accounts of the two portfolios and the purchase in the new securities account, all at the closing price of the transfer date. The account balance afterwards is the same as before. The **profit or loss** up to the transfer date stays with the previous securities account, and the entire history of **all trades** of an **instrument** remains visible at **portfolio** level under **Securities account** and in the **watchlist**. The function does not record transfer fees. They are booked separately as **Account/Depot cost**.

### Spin-off
In a spin-off, a company separates a business unit into a company of its own and allots its shares to its shareholders, for example one new share for every four shares held. The shares of the parent company stay in the securities account and the new shares are added. Part of the previous cost basis passes to the new shares. The company usually publishes this share. If it does not, it follows from the ratio of the closing prices on the first day both shares trade.

Do not record a spin-off yourself with sales and purchases. An administrator records it once as a [Spin-off]({{% ref "/basedata/securityaction/spinoff" %}}) among the security actions, and each tenant applies it to their securities accounts. GT then books the necessary transactions, divides the cost basis and keeps the original purchase dates. If you booked a spin-off by hand earlier, the preview recognizes these bookings and proposes to skip the securities account.
