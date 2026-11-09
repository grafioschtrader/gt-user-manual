---
title: "Transaction receipts"
date: 2026-07-15T10:00:00+02:00
draft: false
weight: 20
archetype: "default"
---
GT can generate its own **transaction receipts** as **PDF documents** for the **security transactions** of an **instrument**. This makes it possible to hand over a single transaction as a document or to archive several transactions of an instrument as documents in one step. The layout of the receipt is modeled on the statements of Swiss trading platforms, with Grafioschtrader as the sender.

## Generating the receipts
The menu item "**Transaction receipts**" is located in the **context menu** of a selected **instrument**, for example in the [watchlist](../../watchlistinstrument/watchlist/) or in other views with one instrument per table row. The menu item is only offered for **securities**, not for **currency pairs**.

After the call, a dialog shows the security transactions of the instrument in a table with the columns **Date**, **Transaction type**, **Cash account**, **Quantity**, **Price/Div/etc.** and **Total amount**. Depending on the view from which the function is called, the table only contains the transactions of the corresponding **portfolio** or **securities account**. One or more transactions are marked using the checkboxes. The "**Download**" button becomes active as soon as at least one transaction is marked.

The transaction types **Buy**, **Sell**, **Interest/Dividends** and **Finance cost** are supported. Receipts cannot be generated for **account transactions** such as deposits or withdrawals.

## Downloaded files
If a single transaction is marked, the receipt is downloaded directly as a **PDF document**. If several transactions are marked, a **ZIP archive** is created with one PDF document per transaction. The file name of a receipt is composed of the transaction date and the transaction type, for example "20240315_Buy.pdf". If several marked transactions fall on the same day, the time is additionally inserted into the file name.

## Content of the receipt
Each receipt comprises one page. The header contains the **nickname** of the user as well as the involved **securities account** and **cash account**. The title line names the type of stock exchange transaction together with a reference consisting of "GT-" and the transaction number. This is followed by the name of the security with **ISIN**, the instrument and settlement currency, quantity and price or distribution per unit, **accrued interest** for bonds where applicable, **commission**, **taxes and duties** and the **currency exchange rate** for different currencies. The highlighted total line shows the debited or credited amount.

The language of the receipt follows the language set by the user: German-speaking users receive a German receipt, all others an English one. For the transaction type **Interest/Dividends**, the receipt distinguishes between **Interest** for interest-bearing instruments such as bonds or money market and **Dividend** for all other instruments.
