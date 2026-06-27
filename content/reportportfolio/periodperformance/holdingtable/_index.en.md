---
title: "Architecture of GT - Holding table"
date: 2024-03-03T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
In principle, a relational database should not contain any redundancies. However, redundancies are sometimes necessary to ensure sufficient performance.

## How GT calculates most reports
GT calculates most reports from the transactions and the historical or current price data. Each transaction is used for the calculation. If a transaction contains a foreign currency, this must of course also be taken into account. This procedure is currently also sufficiently performant for several thousand transactions.

## The problem of changing transactions or exchange rate data
In GT, transactions or exchange rate data can be changed at any time. For example, a transaction can be deleted, added or changed in the past in GT. This is not the case with a trading platform, which is why it handles its invalid transactions with a reversal. This has the advantage that you can easily calculate a total for a portfolio at any time in the past. This means that calculations can be carried out with a summation and the transactions from this point in time. Of course, such calculations could also be implemented in GT on the basis of such summations. However, such summations are rather invalid in GT, as the transactions can be changed at any time before this summation.

## Period yield report and redundant data
The Period Return report calculates the value of the portfolio over a given period on a daily basis. The calculation would take too long if only the transaction and price data of the instruments and currencies were available. For this reason, there are 3 database tables with redundant data that speed up the calculation. The data in these tables must of course also be updated according to changes in transactions, splits, etc. Typical for these tables are two fields that define a period. For example, a simple SQL query can be used to determine the stock of an instrument at any point in time for a client. When designing these tables, the focus was on optimizing the SQL queries for the Period Income report.
