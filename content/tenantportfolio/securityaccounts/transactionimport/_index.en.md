---
title: "Transaction import"
date: 2021-03-30T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
Many users have been trading on the stock exchange for several years and prefer to import their transactions rather than enter them manually. There are therefore several options for this **import**, whereby one method is better suited for an **initial** **import** and the other for an **ongoing import**.

{{% notice note %}}
**Why is the transaction import at the level of a single securities account?** The CSV import also enables account transactions, but the transaction import is still at the level of a single securities account. A portfolio can contain several securities accounts; this forced preselection means that the securities account does not have to be determined from the documents to be imported. 
{{% /notice %}}

## Initial import
**CSV import** is recommended for the initial import if the trading platform offers a complete CSV document where possible. Unfortunately, such exports often lack the fees or tax data and are only suitable to a limited extent for importing securities transactions. The other alternative is **GT-Transform-Import**, where all PDF documents of a directory structure can be imported, anonymized and the result exported as a text file. This text file is then uploaded to GT.

## Ongoing import
The ongoing import refers to the import after the initial import, whereby a transaction is imported relatively quickly after it has been executed on the trading platform. Most trading platforms provide a corresponding PDF document shortly after a transaction has been executed. This PDF document can possibly be imported into GT using drag & drop.

## Prerequisite for the import
The import is only offered if the **securities account** is linked to the corresponding **trading platform plan**. With this link, the import is imported with the corresponding **import templates** from the **import template group**.

## CSV import
Before uploading the CSV document, the appropriate import template must be selected if there are several import templates. When selecting, the "**templateId**" is displayed with the purpose of the template. To determine the correct import template, it is advisable to first compare the header of the CSV document with the import template, see also [Import transaction template](../../../basedata/imptranstemplate/createimptranstemplate/).

### First line of the CSV document
The first line of the CSV file is used to assign the **columns** to the corresponding **fields**. This assignment is based on the field definition in the import template. As a rule, the user does not have to make any adjustments in this line.

## PDF import
The PDF import can currently only be used for securities transactions.

### GT-PDF-Transform
This **desktop JavaFX application** can be used to transform and anonymize a large number of PDF documents. This application is recommended for the initial import, see also "[Import of securities transaction as PDF](../../../transaction/security/)" and "[GT-PDF-Transform](./gtpdftransform)".

### Transfer PDF to GT with drag & drop
**Drag & drop** can be used to import a few or individual transactions. If the PDF import fails, an import group is created and switched to the **transaction import view**. The **name of the import group** is also derived from the current date and local time.  
{{% notice warning %}} 
If you have concerns about the data protection of the GT instance you are using, you should probably not use this variant. 
{{% /notice %}}

## Your documents for the GT project
There may not yet be any **import templates** for your **trading platform** in GT. You can create these yourself, see [Procedure for creating a PDF import template](../../../basedata/imptranstemplate). If you would like to leave this work to others, we would be grateful for your pseudonymized CSV and/or PDF documents. You can send these to us in accordance with the instructions on [Import transaction template](//github.com/grafioschtrader/gt-import-transaction-template). 
Unfortunately only in German:
{{< youtube xzXmhcBHSHM >}}