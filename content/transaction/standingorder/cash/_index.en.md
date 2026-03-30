---
title: "Standing order cash"
date: 2026-02-28T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
The cash account standing order automatically creates recurring account transactions. It is the simpler of the two standing order types, as only a fixed amount and an account need to be configured.

## Transaction types
Two transaction types are available for cash standing orders:
- **Deposit**: Regular credit to the account. A typical use case is a monthly money transfer to the trading account.
- **Withdrawal**: Regular debit from the account. Can be used, for example, for recurring fees or regular withdrawals.

## Input fields
When creating or editing a cash standing order, the following information must be provided:
- **Transaction type**: Deposit or withdrawal.
- **Account**: The account on which the transaction is posted. The selection list shows all accounts of the tenant, grouped by portfolio and currency.
- **Amount**: The fixed amount in the currency of the selected account that is posted with each execution.
- **Transaction cost** (optional): A fixed transaction cost that is applied to each generated transaction. This can be used, for example, to account for bank fees on recurring transfers.

Additionally, the common fields of the [repeat configuration](../) are available: repeat unit, repeat interval, day position, execution day, execution month, weekend adjustment, valid from, valid to and an optional note.

Once the standing order has created transactions, the transaction-specific fields (transaction type, account, amount and transaction cost) are locked. Only the scheduling fields and note can still be edited. See [Editing and deletion restrictions](../#editing-and-deletion-restrictions) for details.

## Created transactions
Each transaction created by the standing order corresponds to a normal [cash transaction](../../account/). It can be viewed in the usual transaction views and edited if necessary. Additionally, the created transactions are visible directly in the standing order table via the row expander, where they can also be edited or deleted via the context menu. See [Viewing created transactions](../#viewing-created-transactions) for details.
