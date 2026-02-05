---
title: "Client"
date: 2026-01-06T22:54:47+01:00
draft: false
weight: 2
archetype: "default"
---
The client was created using the **registration process**.

## Edit client
The properties of the client were recorded in the **registration process** and can be adjusted via the **navigation area** on the **main element Portfolios**.

### Properties
All properties of a **client** can be changed at any time.
- **Client name**: This **client name** is used for identification purposes in case **portfolios** can be shared with other users in future versions of GT.
- **Currency**: This is the **currency** of the **client**, i.e. the evaluations of all **portfolios** are carried out in this **currency**.
- **Ignore tax on dividends/interest**: In Switzerland, a tax amount of 35% is automatically deducted with the dividend or interest payment for certain shares and bonds. This amount is refunded on the basis of the tax information and the income is taxed as ordinary income. If you select the skip interest/dividend tax option, the tax for the "Interest/dividend" transaction type is skipped when calculating the profit. This makes it easier to compare the profits from different securities.
- **Closed until**: In Grafioschtrader, all transactions can be edited at any time and past transactions can be added. While this flexibility is necessary for corrections, it carries the risk of historical transactions being accidentally changed or added. Users may unintentionally modify completed data from past periods. This can lead to the following problems:
- Unexplainable calculations of historical portfolio performance.
- Discrepancies with the trading platform's account data.
- Difficulties in tracking when data was actually changed and when transactions took place.
To solve this problem, this date was introduced. It prevents the addition or modification of transactions in the past.
  - **Configuration hierarchy**: The portfolio level takes precedence if the "Closed until" field has a date. The client level is used as a fallback if no date is set at portfolio level. No restrictions apply if both values do not contain a date.

## Functions
- **Currency client and portfolios**: This function is available on the **main element Portfolios**, it sets the **currency** of the **client** and all his **portfolios**.
