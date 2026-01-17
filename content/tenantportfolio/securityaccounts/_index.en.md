---
title: "Security accounts"
date: 2021-03-23T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
The securities account is a "container" for the instruments you hold; no **transactions** with instruments can be carried out without it.
+ A **portfolio** can contain several **securities accounts** - as a rule, it is usually only one.
+ A portfolio can be evaluated on its own or across all **portfolios** in a **portfolio**.
+ There is a limit to the total number of securities accounts.
+ A securities account cannot be deleted as long as it is still referenced by a transaction.

## Creating and editing securities accounts
A **securities account** is created, edited and deleted via the **navigation area**.
+ **Create** a **securities account** via the **context menu** on the **static element** Securities accounts.
+ **Edit** a **securities account** via the **context menu** on the corresponding **element** of the securities account.
+ **Delete** a **securities account** via the **context menu** on the corresponding **element** of the securities account.

### Properties
All properties of the portfolio can be changed completely at any time.
+ **Name securities account**: A name for your securities account is displayed in various places in GT.
+ **Trading platform plan**: Selection of the trading platform plan for the corresponding securities account. Further information under [Trading platform plan]({{% ref "/basedata/tradingplatformplan" %}}).
+ **Supported until input fields**: The input fields **"supported until"** are used for the future component **algorithmic trading**. These input fields are currently ineffective in transaction processing.
+ **Cheapest transaction costs**: This input is currently not used. Will be used in a future GT version for the implementation of the problem solution ["Deviation from reality"]({{% ref "/reportportfolio" %}}).
