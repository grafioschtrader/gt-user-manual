---
title: "Import transaction template"
date: 2021-06-27T12:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
The import template for **PDF** and **CSV** have some similarities. An indication is required of where the relevant values can be determined from the document. This is done by assigning **fields**. In addition, a **configuration** is required so that the read values of the **fields** are interpreted correctly.

## Assignment of fields
The values determined for the transaction are read from the fields. The **mandatory** fields that are independent of the transaction type are shown **_in bold italics_**:
- **ac**: Accrued interest may be incurred when buying or selling a bond.
- **_cac_**: **Currency of the bank account**, an [ISO 4217](//de.wikipedia.org/wiki/ISO\_4217) value is expected. This is used to determine the bank account.
- **cct**: Currency of the transaction costs and tax, should be used if the costs are not incurred in the currency of the instrument.
- **cex**: The **exchange rate**.
- **cin**: The **currency** of the **instrument**.
- **_datetime_**: **Transaction time**, whereby the time is optional. Instead of **datetime**, the combination **date** and **time** can also be used.
  - **date**: If the date and time are separate, this field can be used. This is the part of the **date**.
  - **time**: If the date and time are separated, this field can be used. This is part of the **time**.
- **exdiv**: The ex-tag can be important for documents relating to dividends, see [Interest/Dividend](../../../transaction/security/cashbased/)
- **isin**: The ISIN is important as it allows a fairly precise determination of the instrument.
- **per**: For bonds, the **nominal value** is listed under **units** and divided by 100 if the **per** field is set, i.e. contains any character or character string. For example, if the nominal value is 10,000.00, it is divided by 100. The value of the **per** field itself is not evaluated.
- **quotation**: This is the **price**, **dividend per unit** or the **interest rate in percent for bonds**.
- **symbol**: If the document does not have an **ISIN**, the **symbol** of the instrument is the worse alternative.
- **sf1**: This field is reserved for **special transaction import implementations**. This is used to read a value from the document and feed it into a special implementation.
- **_ta_**: The **total sum**; a calculation is used to check whether the other details lead to this sum.
- **tc1**: The main transaction costs, i.e. the **fees of the trading platform**.
- **tc2**: Sometimes two items are required for the transaction costs.
- **reduce**: A trading platform may offer a discount on the transaction costs. This amount is deducted from the transaction costs.
- **_transType_**: This specification is used to determine the **transaction type** via a **corresponding configuration**.
- **tt1**: The main tax costs.
- **tt2**: Sometimes two items are required for the tax costs.
- **units**: The number of units in the transaction and for bonds the **nominal value**, see **per**.

### Mandatory fields depending on the transaction type
A distinction is made between **account transactions** and **securities transactions**. Only the requirements for **securities transactions** are described here.
- **Recognition of the instrument**: This is preferably done via the value of the **isin** field and, if necessary, with the value of the **symbol** field.
- **How much at what price**: For all **securities transactions**, the values of the **units** and **quotation** fields are required and are checked using the value of the **ta** field.

## Configuration
The **[END]** section follows at the end of the import template. This contains the **configuration** for processing the **template** or the **document** to be imported. A mandatory configuration is shown **_in bold italics_**.
- **ignoreTaxOnDivInt**: This dividend is not subject to tax. The text is then expected as for **transType**. For example, with the configuration "ignoreTaxOnDivInt=Capital Gain" in the **transaction**, the **Subject to tax** property is not set with this configuration for dividends or interest, whereby this refers to the text "Capital Gain". The functionality corresponds to **NO_TAX_ON_DIVIDEND_INTEREST**, whereby this configuration generally refers to an import template.
- **_transType_**: This is an assignment of the text to a **transaction type**. The configuration consists of the transaction type separated by "|" one or more texts. For example "transType=DIVIDEND|Interest,Dividend" whereby this also corresponds to the two line configuration "transType=DIVIDEND|Interest" and "transType=DIVIDEND|Dividend". The following transactions are supported in CSV and PDF templates:
  - **ACCUMULATE**: Is a purchase of an instrument.
  - **DIVIDEND**: Is for the interest or dividend of an instrument.
  - **REDUCE**: Is a sale of an instrument.
  - **transType only valid for CSV**: The following **transaction types** can also be used in an import template for **CSV**:
    - **DEPOSIT**: Is a withdrawal from an account.
    - **FEE**: Costs for account management or custody account fees.
    - **INTEREST**: Is the interest for an account.
    - **WITHDRAWAL**: Is a deposit from an account.
- **_dateFormat_**: The format is used for the **datetime** field. Two possible examples are "dd.MM.yyyy" or "dd.MM.yyyy HH:mm". The **date** field also uses this format, but time-specific format specifications must not be used.
  - **yyyy** or **yy**: For two or four-digit year.
  - **MMM** or **MM**: Month abbreviated to 3 letters or as a two-digit number.
  - **dd**: Day of the month as a two-digit number.
  - **HH**: The hours as a two-digit number from 0-23.
  - **hh**: The hours as 1-12.
  - **mm**: Minute as a two-digit number.
  - **ss**: Seconds as a two-digit number.
  - **a**: If the 12-hour format is used, this information should not be missing. So that the assignment of **AM** and **PM** works correctly.
- **timeFormat**: Sometimes the documents also provide an exact transaction time. The format corresponds to the format specified above for the time.
- **overRuleSeparators**: The expected country-specific **digit grouping** and **decimal separators** of the document are determined by the user's **country/language** settings. It is possible to override these with the configuration "**language-country<digit grouping|decimal separator>"**. **Several characters** can be specified for the **digit grouping**; only one character is possible for the **decimal separator**. For example, the configuration "**de-CH<'|.>de-DE<,|.>**" overwrites the **country/language** setting of the user with "de-CH" or "de-DE". A configuration with **ALL** sets the corresponding configuration regardless of the user. For example, "**All<''|.>**" expects the character "'" or "'" as the **digit grouping** and the **decimal point** as the **decimal separator**.
- **otherFlagOptions**: Specification of additional configurations, these are separated with "|".
  - **BOND_QUOTATION_CORRECTION**: Bonds can rarely pay interest two or more times a year. However, the document to be imported only shows the annual interest rate. In this case, the system may adjust this interest rate or the rate derived from it accordingly.
  - **CASH_SECURITY_CURRENCY_MISMATCH_BUT_EXCHANGE_RATE**: Sometimes a dividend is paid in a currency other than the security currency, but there is no conversion rate or it is not imported. The system sets the expected currency pair and calculates the exchange rate and adjusts the dividend according to the account currency per unit.
  - **BASE_CURRENCY_MAYBE_INVERSE**: The **base currency** may not be the expected currency of the instrument. It may be the base currency of the account.
  - **BOND_ADJUST_UNITS_AND_QUOTATION_WHEN_UNITS_EQUAL_ONE**: Certain trading platforms show the value 1 in the **units** field in the CSV document for **interest rate transactions** for bonds. **GT** wants the correct **number** as specified in the purchase. This configuration allows GT to adjust the **number** and **interest rate** via the **units** and **quotation** fields. This adjustment is only carried out when the transaction is created.
  - **NO_TAX_ON_DIVIDEND_INTEREST**: With this configuration, the **subject to tax** property is not set for dividends or interest in the **transaction**, i.e. the amount is tax-free. Otherwise the **Subject to tax** property is set.
