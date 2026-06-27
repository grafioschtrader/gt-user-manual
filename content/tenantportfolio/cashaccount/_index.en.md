---
title: "Cash account"
date: 2026-06-19T22:54:47+01:00
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
+ **Active until or maturity date**: Optional date up to which entries can be booked on this account. If the field is left **empty**, the account stays active indefinitely. If a date is set, the account is considered deactivated afterwards. See [Deactivating an account](#deactivating-an-account) for details.

## Deactivating an account
Once an account has been used in a **transaction**, it can no longer be deleted. The **Active until or maturity date** field still lets you remove an account that is no longer used from everyday work without losing its existing history. This applies equally to **cash accounts** and [securities accounts]({{% ref "/tenantportfolio/securityaccounts" %}}).

If the field is left empty, the account stays active indefinitely. A date that is set may not lie before the account's most recent **transaction**. After this date no **transaction** can be booked on the account: when entering or editing a [transaction]({{% ref "/transaction" %}}) or a [standing order]({{% ref "/transaction/standingorder" %}}), the account is no longer offered for selection depending on the chosen date, and the backend rejects any entry dated after the active-until date.

A deactivated account stays visible in the navigation tree but is shown struck through, so the reports and history remain reachable. During a [transaction import]({{% ref "/tenantportfolio/securityaccounts/transactionimport/viewtransactionimport" %}}) as well, a position whose date is after the active-until date is rejected with a per-row error message and skipped; the remaining positions are still imported.
