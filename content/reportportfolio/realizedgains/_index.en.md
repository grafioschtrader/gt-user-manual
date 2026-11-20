---
title: "Realized gains"
date: 2026-11-10T18:00:00+01:00
draft: false
weight: 55
archetype: "default"
---
The **Realized gains** report shows the capital gains and losses from sales, ordered by the year of the sale. It is available at **tenant level**, in every main currency and for every country, and converts all amounts to the main currency. Dividends and interest are not part of it, they are in the [Dividends and interest](../dividends/) report. The report is not a tax calculation.

## Choosing the method
The selection of the **Cost basis method** stands above the result. It applies only to this report and is independent of the method set for the client or a portfolio, see [Cost basis method](../costbasismethod/). The same sale can therefore be compared by **Moving average**, **FIFO** or **LIFO** without changing the other views. After the choice, the **Calculate** button calculates the report again.

The option **Method per securities account (client and portfolio)** calculates each securities account by the method that applies to it. If securities accounts use different methods, the text **Mixed, differs per security account** appears above the table. The column **Cost basis method** of a row shows which method calculated a sale.

With the menu item **Evaluation of depots** you choose the securities accounts whose sales are listed. The selection does not change the gain of a sale: with moving average, all securities accounts that use this method form one pool for a security, and a sale in a single securities account also calculates with this pool. The gain is the same for every selection of accounts.

## Table of years
The table has one row per calendar year. The sums over all years stand above the table. All amounts are in the main currency.
- **Year**: The year in which the sales took place.
- **Gains**: The sum of all sales with a gain.
- **Losses**: The sum of all sales with a loss, as a positive amount.
- **Net**: Gains minus losses.
- **Sales**: The number of sales. A sale without gain is counted, it is neither a gain nor a loss.
- **System bookings, net** and **System bookings, number**: Gains minus losses of the system bookings and their number, see below.
- **Incomplete**: Shows that not every sale of the year was fully included in the sums, see below.

Expanding a year row shows its sales. With FIFO and LIFO, a sale can be expanded once more. It then shows the **purchases consumed by this sale**.

## Sales of the year
Every row is a sale with its date, security, currency and **Quantity** as it stands in the transaction. **Proceeds** are the proceeds of the sale after its costs, **Cost basis** is the cost of the units sold according to the chosen method, and **Gain/loss** is the difference, each in the currency of the security. **Gain/loss %** relates the gain to the cost basis.

The column **Gain/loss at the rate of the sale day** converts the gain to the main currency at the **Exchange rate** of the day of the sale. If the security is quoted in the main currency, the exchange rate is 1. The column **Realized** of the securities account report converts the same sales at the rate of the reporting date. For a security in a foreign currency the two amounts therefore differ.

For a margin product such as a CFD, a row is the close of a position. It shows the gain of the closed position and neither proceeds nor cost basis, and the method does not matter for it.

## System bookings
A securities account transfer and an ISIN change are booked by GT as a sale followed by a purchase at the closing price. The sale is a **system booking**: nothing was sold on the market, and yet a gain or loss arises against the previous cost basis. These rows are marked in the table, and their amounts are not in the columns **Gains**, **Losses** and **Net** but in columns of their own.

The reason is that a system booking shifts gain between securities accounts and years. The later sale in the target securities account gains only against the price of the transfer. Only the sum of the sales on the market and the system bookings over all years gives the gain you actually made economically. A single year does not show it.

## When data is missing
If the purchases do not cover all units sold, for example because a purchase was not recorded, the gain of the missing units counts as 0. The row and the year then carry the mark **Incomplete**. If the exchange rate of the security currency is missing for the day of the sale, the amount in the main currency stays empty and the sale is not part of the sums in the main currency. It makes the year incomplete as well.

{{% callout note %}}
The report calculates no taxes and applies no tax rules of a country. It shows what the chosen method yields as realized capital gain. For a tax return, the certificates of your bank and the rules of your country are authoritative.
{{% /callout %}}

## German tax help
For a client with the main currency euro and the country Germany, the **German tax help** also appears below the table of years. For a tax year it derives the starting values for Anlage KAP and Anlage KAP-INV, see [German tax help](../germantax/).

