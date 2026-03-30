---
title: "Cash account"
date: 2026-02-26T22:54:47+01:00
draft: false
weight: 40
archetype: "default"
---
If you have opted for GT, you consciously accept that a **securities transaction** requires an **offsetting entry** in an **account**. GT allows accounts in different currencies.
+ A **transaction** on a **portfolio** is only possible if it contains at least one **account** - but can contain several.
+ Once a particular account has been used in a **transaction**, it can no longer be **deleted** or its **currency changed**.

## Create and edit account
An **account** is created, edited and deleted in the **Portfolio view** in the **main area**. You can access this **view** in the **navigation area** on the **name of** the **portfolio**.
+ **Create** an **account** via the **context menu**.
+ **Edit** an **account** in the **selected account**.
+ **Delete** an **account** from the **selected account**.

### Properties
As mentioned above, the properties can be changed to a limited extent.
+ **Name cash account**: The name of the bank account must be unique in a portfolio. It is useful to include the currency in the name.
+ **Currency**: Currency of the account.
+ **Borrowing rate**: Annual borrowing rate in percent for overdraft control. This field controls whether the account may have a negative balance. If this field is left **empty**, overdraft is **not allowed** and the backend rejects [transactions](../../transaction/) that would result in a negative account balance. A value of **0 or higher** allows overdraft at the specified interest rate.
+ **Securities account**: The input field **Securities account** only appears if your portfolio has several **securities accounts**. Within a **portfolio**, there is no restriction regarding the **securities transaction** when assigning a **bank account**. This can lead to ambiguities in the portfolio evaluations, so this assignment should be made for several securities accounts and bank accounts with the same currency.
