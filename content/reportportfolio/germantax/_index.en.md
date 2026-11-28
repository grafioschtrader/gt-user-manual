---
title: "German tax help"
date: 2026-11-20T18:00:00+01:00
draft: false
weight: 56
archetype: "default"
---
Below the table of years of the [Realized gains](../realizedgains/) report, a client with the main currency **euro** and the [country](../../tenantportfolio/client/#properties) **Germany** gets the **Tax help Germany**. For a tax year it derives the **starting values for the tax return**: the sections of Anlage KAP and Anlage KAP-INV, all in euro. It calculates **no tax**. The saver's allowance, tax rate, solidarity surcharge, church tax, most-favourable-tax test and loss carry-forwards are known only to the assessment. The help always calculates the whole client, by FIFO per securities account as the law requires, regardless of the cost basis method you use elsewhere. You choose the tax year in the selection **Tax year**, between 2018 and the current year. The current year is provisional.

{{% callout warning %}}
Starting values without warranty, neither a tax calculation nor tax advice. Bank certificates take precedence. Completeness depends on the recorded transactions.
{{% /callout %}}

## Prerequisites
- Assign the **Withholding tax** to every portfolio with accounts in the [portfolio dialog](../../tenantportfolio/portfolio/). As long as a portfolio is **Not assigned**, its income stands in a block of its own and in no form, and the help asks you to assign it. A portfolio should hold the accounts of exactly one institution.
- Assign the **Fund class (German tax)** to every ETF, Mutual Fund and Pension fund, see [Fund class](../../watchlistinstrument/instrument/securityderived/security/#fund-class). Without a fund class the help does not calculate the fund.
- For the advance lump sum the help needs the **base interest rate** of every year in which you hold a fund. An administrator maintains it in the tree of [tax data](../../admindata/taxdata/) under the country Germany. It is stored for the years 2018 to 2026.
- Classify ADRs and GDRs as **shares**, not as certificates. Only then do their losses count in the pot of shares.

## The sections
- **Shares** (Anlage KAP): gains and losses from selling shares, kept apart, plus the dividends. The help does not offset them. Whether the rule that losses on shares are only offset against gains on shares is constitutional is pending.
- **Interest and other capital claims** (Anlage KAP): interest of bonds and accounts, accrued interest received and paid, and gains and losses from selling bonds, each kept apart.
- **Investment funds** (Anlage KAP-INV): per fund class the distributions, the advance lump sum and the result of sales, each **before the partial exemption**, because the form expects that value. The amount after the partial exemption stands beside it as a reading aid. The result of a sale is reduced by the advance lump sums of the units sold. The advance lump sum of a year counts as income of the following year.
- **Foreign tax** (Anlage KAP): the recorded tax, a **calculated ceiling** of 25 % of the income and the rest above it. GT knows neither the kind of tax, the state nor a refund claim under a double taxation treaty. The ceiling is therefore no finally creditable amount.
- **With domestic withholding**: the same values of the portfolios where a domestic paying agent withholds the tax, for information only. The bank certificate is authoritative.
- **Portfolios not assigned yet** and **Purchases before 2009 and before 2018**: for information only. Shares and claims bought before 1 January 2009 and fund units before 1 January 2018 follow transition rules. Their sales stand in no form.

## Complete or without amount
Every sum is either derived completely or it has **no amount** and says **Not computable, see the warnings**. A missing number never shows as 0. The **warnings** name the cause, the year, the security and the sums that stay without an amount because of it. Typical causes are a missing exchange rate, a missing base interest rate, missing prices or distributions of a fund, sales that the purchases do not cover and ISIN changes of funds. A gap keeps acting: whoever sells a fund after years whose base interest rate of a year in between is missing gets no amount for the sale. The table **Derivation** shows every sale, every income and every advance lump sum with the amounts in euro, the exchange rate and its source. The list **Instruments outside the sums** names funds without a fund class, certificates, ETCs and CFDs with the value of their bookings. These bookings can be relevant for the tax return anyway, as can futures and crypto assets.

## Limits
- No tax professional has reviewed the calculation. It follows the law and the letters of the Federal Ministry of Finance to the best of our knowledge, and deviations are possible. Have a tax professional check the values before you file.
- The help replaces no certificate. Security accounts at domestic banks appear for information only.
- It uses booked rates and closing rates of the currency pair, no foreign exchange selling rate and no ECB reference rate, and for the advance lump sum the market price instead of the redemption price. The amounts can therefore differ from a bank statement.
- The advance lump sum is calculated in euro: prices and distributions are converted at the rate of their day. The law says nothing about converting funds in a foreign currency. Whether this conversion matches the view of the tax authorities has not been checked.
- For bonds and other capital claims the law ties the time before 2009 to the date the income flows, not to the purchase date. The help uses the purchase date before 1 January 2009 here as well. The sale of a bond bought before 2009 can therefore be taxable although the help shows it for information only.
- A [spin-off](../../basedata/securityaction/spinoff/#reports) applied as a security action follows the German tax treatment recorded there. As long as it is **Undetermined**, later sales of both securities stay without an amount. Compare the cash payment for fractional shares with the bank certificate.
- Capital increases, bonus shares and fund mergers are only mapped as far as they are recorded as transactions or ISIN changes. A later sale from such a purchase has no amount, because its tax cost is not established. The same holds for a purchase without consideration.
- A change of the fund class during the holding period is not treated under § 22 InvStG. If you change the fund class of a fund, earlier years change as well.
- With a partial transfer between security accounts FIFO takes the oldest units, even if the bank certificate names others.
- Not covered are loss certificates, business assets, joint assessment, futures, crypto assets and commodities.
- The help stores no filed states. If you change a transaction, the fund class or a base interest rate, years already evaluated change as well.
