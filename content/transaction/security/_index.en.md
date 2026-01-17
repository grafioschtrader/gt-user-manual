---
title: "Securities transaction"
date: 2021-04-22T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
A securities transaction also includes the transaction of an instrument. GT supports different booking approaches. For example, the trading of Forex is booked differently than the stock exchange deal with an ETF.

## General plausibilities
- The quantity in a transaction is checked. For example, you cannot sell more shares of a stock held in the securities account than you hold at the time of the sale.

## Determination of base currency and quote currency
A currency pair must be determined if the currency of the instrument and the account are different. The **base currency** is determined by the **instrument** and the **quote currency** by the **account**. If an instrument has the currency USD and the account the currency CHF, this results in the currency pair USD/CHF.

### Transaction statement with different base currency
Sometimes the securities transaction documents have a different **base currency** than expected by **GT**. In such cases, the additional input field to the right of the **rate/div/usw**, **tax** etc. input fields can be used to accept the value in the **rate currency**. The value of this input field is only used to **calculate** the corresponding value of the base currency. This calculation only takes place from the value of the **exchange rate currency** towards the **base currency**, i.e. the input value of the **base currency** is adjusted according to the **exchange rate** and the input value of the **base currency**.

{{% notice note %}} 
Sometimes the dividend is not paid in the currency of the instrument or the **portfolio** does not have an account in the relevant currency. In such cases, this will automatically result in a currency pair according to the rules above. The exchange rate entered or applied should fairly accurately reflect an exchange rate on that day, otherwise this will result in inaccurate currency gain calculations
{{% /notice %}}

## Different booking approaches
Different investment products can be traded with GT. Due to the type of **transaction**, there are basically two different booking approaches in GT.

### Booking of the underlying asset
When trading shares, ETFs, etc., the underlying **asset** is **booked** when buying or selling.

### Booking the price difference
When booking the price difference, there is a transaction that opens the position, plus one or more transactions that close this open position. Only the closing of the position generates an account-relevant posting, in which the **price difference** is posted. This **transaction** is also referred to in this documentation as **a margin-based transaction**.

## Influence of splits
Splits do not require any activity on your part if you hold a security at the time of a split. GT always calculates the number of units held according to the splits assigned to the security.

## Creating and editing transactions
In **GT**, a previously untraded instrument can only be purchased for the first time via a watchlist. Otherwise, purchases are possible via the **securities account** or **named securities account** view.

### Create purchase transaction
A previously untraded instrument can only be purchased for the first time via a watchlist. Otherwise, **purchases** are possible via the **securities account** or **named securities account** view. The instrument of the corresponding **watchlist** can be selected in the**"Security**" input field. The instrument selected in the **watchlist** is preselected in this input field.

### Edit and delete existing transactions
The "**Edit transaction**" function via the context menu is offered in many different views; the corresponding transaction must be selected. There are special views for **transactions** and general views that refer to one **account** or one **instrument** per table row. The latter often contains a list of transactions via the expanding table row. The "**Edit transaction**" function also works in these **expanded table rows**.

## Properties and table columns
Regardless of the booking approach, all transactions share certain properties. These are described here unless they are self-explanatory:
- **Exchange rate**: See above "**Transaction statement with different base currency**".
- **Account booking**: This is calculated and is also used to check the other entries.

## Import securities transaction as PDF
Many trading platforms offer the executed transactions as a **PDF file**. These can be downloaded immediately or a few hours after the transaction has been executed. GT supports the import of such **PDF files**, whereby a PDF must be available for each transaction. It should be noted that such PDF files are processed on the **back-end**. The following flowchart provides information on how transactions are processed if they are available as PDF files. Further information on this topic can be found under [Import template group](../../basedata/imptranstemplate/) and [Transaction import](../../tenantportfolio/securityaccounts/transactionimport). 
{{< mermaid >}} 
graph TB 
  A(Securities transaction as PDF) --> B{Many transactions?}; 
  B -- Quantity >= 10 --> C[GT-PDF-Transform]; 
  C -- Import/anonymize/export --> C; 
  C -- Import --> D[Transaction import]; 
  B -- Quantity < 10 --> G{GT Instance trustworthy?}; 
  G -- No --> I{Despite import}; 
  I -- Yes --> C; I -- No --> K[Enter manually]; 
  K --> F[(GT Database)]; 
  G -- Yes -> Drag and Drop ----> E{"Transaction(s)" recognized?};
  D -- Import add/correct --> D; 
  E -- No --> D; 
  D -- Processed to transaction --> F; 
  E -- Processed to transaction --> F; 
{{< /mermaid >}}
