---
title: "Grafioschtrader import"
date: 2026-08-28T10:00:00+02:00
draft: false
weight: 25
archetype: "default"
---
GT can read back its **own exports**. These include the [CSV export of the transactions](../../../../reportportfolio/transactionlist/) as well as the [transaction receipts as PDF](../../../../transaction/receipt/). A special **import template group** named «**Grafioschtrader**», shipped with GT, is used for this re-import.
{{% notice info %}}
Unlike the template groups for a trading platform that you create yourself, the «Grafioschtrader» template group is **unique** and is already delivered ready-made with GT. It should neither be edited nor created a second time. A general description can be found under [Import template group](../../../../basedata/imptranstemplate/).
{{% /notice %}}
## Prerequisite
For the re-import to be offered, two things have to come together. An administrator must have designated the delivered «Grafioschtrader» template group once for the whole installation, see [Import template group](../../../../basedata/imptranstemplate/), and on the [Client](../../../client/) the check box «**Enable Grafioschtrader import templates**» must be set. Only then is the selection described below available.
## Usage
When both prerequisites are met, the check box «**Use Grafioschtrader import templates**» appears at both places of the transaction import: on the one hand in the dialog for creating or editing an **import group**, on the other hand when dropping a PDF document via **drag & drop**. When the box is checked, GT uses the templates from the «Grafioschtrader» template group for this import instead of the templates of the securities account's regular **trading platform**. This allows a GT export to be read into any **securities account** without changing its trading platform assignment.
## One file per securities account
The CSV export creates one file per **securities account**. Import each file into the corresponding securities account. This way the transactions return to the same securities account they originate from.
## Connect cash account transfers
An **account transfer** between two portfolios is split across two files on export. If these files are imported separately, two unconnected entries are created at first, a withdrawal and a deposit. The menu item «**Connect cash account transfers**» at **tenant level** in the [Transactions](../../../../reportportfolio/transactionlist/) report restores the connection afterwards. Only unambiguously matching pairs are connected based on time and amount.
## Limitation
Opening and closing positions of **margin trades** are marked in the export and skipped on import. Such positions have to be entered manually.
{{% notice note %}}
This route via export and import with the Grafioschtrader templates also serves **testing purposes**. It provides reproducible test data for checking the transaction import at any time, so that no real exports from banks have to be used.
{{% /notice %}}
