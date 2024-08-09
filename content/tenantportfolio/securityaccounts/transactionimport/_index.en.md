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

### Initial import
**CSV import** is recommended for the initial import if the trading platform offers a complete CSV document where possible. Unfortunately, such exports often lack the fees or tax data and are only suitable to a limited extent for importing securities transactions. The other alternative is **GT-Transform-Import**, where all PDF documents of a directory structure can be imported, anonymized and the result exported as a text file. This text file is then uploaded to GT.

### Ongoing import
The ongoing import refers to the import after the initial import, whereby a transaction is imported relatively quickly after it has been executed on the trading platform. Most trading platforms provide a corresponding PDF document shortly after a transaction has been executed. This PDF document can possibly be imported into GT using drag & drop.

### Prerequisite for the import
The import is only offered if the **securities account** is linked to the corresponding **trading platform plan**. With this link, the import is imported with the corresponding **import templates** from the **import template group**.

### CSV import
Before uploading the CSV document, the appropriate import template must be selected if there are several import templates. When selecting, the "**templateId**" is displayed with the purpose of the template. To determine the correct import template, it is advisable to first compare the header of the CSV document with the import template, see also [Import transaction template](../../../basedata/imptranstemplate/createimptranstemplate/).

#### First line of the CSV document
The first line of the CSV file is used to assign the **columns** to the corresponding **fields**. This assignment is based on the field definition in the import template. As a rule, the user does not have to make any adjustments in this line.

### PDF import
The PDF import can currently only be used for securities transactions.

#### GT-PDF-Transform
This **desktop JavaFX application** can be used to transform and anonymize a large number of PDF documents. This application is recommended for the initial import, see also "[Import of securities transaction as PDF](../../../transaction/security/)" and "[GT-PDF-Transform](./gtpdftransform)".

#### Transfer PDF to GT with drag & drop
**Drag & drop** can be used to import a few or individual transactions. If the PDF import fails, an import group is created and switched to the **transaction import view**. The **name of the import group** is also derived from the current date and local time.  
{{% notice warning %}} 
If you have concerns about the data protection of the GT instance you are using, you should probably not use this variant. 
{{% /notice %}}

### Your documents for the GT project
There may not yet be any **import templates** for your **trading platform** in GT. You can create these yourself, see [Procedure for creating a PDF import template](../../../basedata/imptranstemplate). If you would like to leave this work to others, we would be grateful for your pseudonymized CSV and/or PDF documents. You can send these to us in accordance with the instructions on [Import transaction template](//github.com/grafioschtrader/gt-import-transaction-template). 
{{< youtube xzXmhcBHSHM >}}

### Transaction import view
**Transaction import** is an **intermediate stage** after the import and before the import becomes a transaction. Only the successful import with **drag & drop** does not require the **Transaction import view**. All other types of import are completed via this **view**. This view provides information about the **status** of a potential transaction. In addition, certain corrections can be made to the **import position**. Different imports of the same potential transactions can improve their quality. Ultimately, the **import item** is processed into a **real transaction** in this view.

#### Create and edit import group
There can be several **import groups**, these are created by the user or by the system when the **drag & drop** of **PDF** fails.
- **Create import group**: Create a new import group. In addition to the name, a comment can also be added. The **name** must be unique within a **securities account**.
- **Edit import group**: Name and comment can be changed.
- **Delete import group**: If there are no more **import items** within an **import group**, it can be deleted.

#### Prerequisite for transaction capability
The system carries out allocations and calculations based on the imported values. An **import position** becomes **transaction-capable** if the following steps can be carried out successfully.
- **Security allocation**: An existing security is allocated on the basis of the **ISIN** or **symbol/ticker**.
- **Bank account assignment**: By specifying the value of **field "cac"**, see [import template](../../../basedata/imptranstemplate/createimptranstemplate/), an existing bank account of this portfolio is assigned.
- **Total amount check**: The **total amount** is calculated from the details of the imported transaction; this must match the corresponding value of the **"ta" field**.

Some of the offered functions on the **import position** can help to achieve the above-mentioned operations.

#### Properties and table columns
Certain table columns can be shown or hidden, see [Control elements](../../../intro/userinterface/user_setting_ui_controls).
- **Has transaction**: The import position has already been processed into a transaction.
- **May have transaction**: Based on the **date of the transaction**, **number** and the **transaction type**, the system recognized a matching existing transaction in the current securities account. It is not possible to create a transaction for such an import position.

#### Functions marked transaction import view
There are functions that work independently of a line selected in the table, these are all the functions for uploading files and the functions described above regarding the **import group**.

#### Functions on selected import item(s)
This view supports multiple selection, so each selected import item must support the required prerequisite for the function to be selected, otherwise the menu item will not be activated.

##### Correction functions
The following functions are used to correct **import items** so that they **can be used for transactions**:
- **Accept difference total**: The value of the "**Calculated total**" is accepted and the corresponding import item becomes **transaction-enabled**. This function should only be used if the difference between the calculated total and the actual total is very small.
- **Correct multiplication to grand total**: This function can possibly solve the problem of **transaction capability** of the import item regarding the failed **grand total check**.
  - Sometimes the exchange rate is not exactly specified in the imported document, this function corrects the exchange rate accordingly.
  - In GT, the **interest in percent** is decisive for the calculation of the **total amount** of interest, this may be shown as annual interest, although the decisive interest covers a shorter period of time. This function can therefore adjust the **interest as a percentage** according to the **total amount**.
- **Assign instrument**: You can assign an instrument to a securities transaction. This opens the [search dialog for instruments](../../../watchlistinstrument/instrument/searchdialog).
- **Assign bank account**: Allows you to assign a bank account to the import item.

{{% notice style="info" title="Automatic correction of interest and dividends for securities" %}} 
In principle, GT expects a transaction in the currency of the instrument. ETFs in particular can be traded in different currencies, although they only have one fund currency. The payment of interest or dividends is made in the fund currency. In such cases, the trading platform may not perform a currency conversion. When importing such a transaction, GT supplements the transaction with a currency rate that is determined on the basis of the trade date. Of course, the interest or dividend amount and any taxes are automatically adjusted. This addition is made when the transaction is created and is therefore only visible in the created transaction. 
{{% /notice %}}

##### Transaction functions
The following functions relate to the creation of a transaction.
- **Create transaction**: The transformation of an import position into a transaction is only possible if the import position **does not** yet have a **transaction** and no "**maybe transaction**".
- **Pass the maybe transaction**: The system recognized the import item as a possible transaction and marked it accordingly, see property **Has maybe transaction**. This function can be used to deactivate this recognition by the system for the selected import item(s).
- **Check for existing transaction**: If the "**Pass possible transaction**" function was used, this function activates the check of the import item for existing possible transactions.

#### Expanding table row
The **expanding table row** has two different views:
- **Import position recognized**: The recognized result with the **assigned values**, the **import template** used and the **import status** are displayed.
- **Document not recognized**: There is a tabular view with all **import templates** of the **template group**. The last successfully recognized **field** per **import template** is displayed.

#### Transaction import in practice
{{< youtube uKzyETfcWRk >}}

