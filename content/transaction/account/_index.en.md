---
title: "Cash transaction"
date: 2021-04-20T02:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
No instrument is involved in account transaction.

## Account transaction types
There are basically **two dialogs** for processing account transactions. One dialog is limited to one account and the other covers two accounts.

### Standard account transaction types
Standard account transaction types are always carried out in the currency of the corresponding account; this refers to the following account transactions:
- **Withdrawal**: The withdrawal of money from a specific account.
  - A withdrawal can have **transaction costs**.
- **Deposit**: The transfer of money into a specific account.
- **Account and securities account costs**: You may have to pay costs for account management. This is the purpose of this transaction type. If you specify a **securities account**, these expenses will be charged to the securities account and not to the account.
  - Negative costs are also possible, for example for a credit to the securyties account or for a reversal booking.
- **Account interest**: The savings interest you receive for your money. You can also enter a negative amount for negative interest.
  - A negative **account interest rate** is possible, for example for negative interest or a reversal booking.
  - Account interest can be subject to **tax**.

### Account transfer
This transaction creates a payment and deposit transaction, i.e. two transactions are created that are linked to each other. This transaction type can also be cross-portfolio and supports **currency conversion**. **Currency conversion** is often used if a deposit or withdrawal is made between two accounts in different currencies within a portfolio.

#### Determination of base currency and quote currency
A **currency pair** must be determined if a **currency conversion** takes place with the **account transfer**. The **base currency** is the currency of the **debit account** and the **exchange rate currency** is the currency of the **credit account**.

{{% notice note %}}
Currency gains are shown in the corresponding account from the time of deposit to the time of payout. Therefore, the exchange rate should reflect reality as accurately as possible, i.e. an exchange rate in the range of the daily low and high on the day of the transaction should be entered or adopted
{{% /notice %}}

## Creating a new account transaction
A new **account transaction** can only be created in the **portfolio view**. For the selected **account**, one of the account transaction dialogs can be called up via the **context menu**.

## Editing an existing account transaction
An **existing account transaction** can be edited via different views:
- In the [Portfolio](../../reportportfolio/portfolios/) view in the expanding table row of the corresponding **account**. This is a **balance view** and allows you to track the netted account balance. This is ideal for comparison with the account statement of your trading platform.
- In the [Portfolio transactions](../../reportportfolio/transactionlist/) view, at the level of the individual portfolios.
- In the [Portfolios transactions](../../reportportfolio/transactionlist/) view. This is the highest level in the navigation area.

## Video with account transaction practice
Brief overview of transactions, then practice with two portfolios. Recording of account transactions such as deposits, withdrawals, account or custody account costs and account interest. The main and foreign currencies are involved. Change an existing account transaction, view the balance history and transaction view. Finally, explain the account transactions made in the **Portfolio view**.

Unfortunately only in German:
{{< youtube C9awa_u1NaU >}}

