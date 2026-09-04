---
title: "Import template CSV"
date: 2026-07-15T12:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
An **import template** for a CSV file makes the connection between the **table columns** of the CSV file and the **fields** required for GT. It also contains CSV document-specific **configurations**. A CSV file can contain **several transactions** compared to the PDF document. It will generate a maximum of one transaction per table row in GT. Certain transactions are made over several lines, for example a partial sale of a security. Due to the import template, the user does not have to make any adjustments to columns in the CSV file.
{{% notice note %}}
**Why a general CSV importer is not the solution?** Similar tools like GT only have a CSV importer, i.e. the user has to create a CSV file according to the specification of this tool. With GT, we refrain from this error-prone and unnecessary work that the user is forced into. GT is the avoidance of bullshit jobs and therefore there is the solution with these **CSV import templates**.
{{% /notice %}}

## Structure of the template
Like the PDF template, a CSV import template consists of two sections separated by the line **[END]**. Before **[END]**, there is one mapping line of the form "**field=column header**" per value to be read. The GT field name is on the left, the **exact column header** from the header line of the CSV file is on the right. The **configuration** follows after **[END]**. The general fields such as **datetime**, **isin** or **units** as well as the general configurations such as **transType**, **dateFormat** or **overRuleSeparators** are described in the parent chapter [Import transaction template](../) and are not repeated here.

When processing a CSV file, GT compares its header line with the mapping lines of the template. The following applies:
- The column header must match exactly, only leading and trailing spaces are ignored.
- Every column listed in the template must be present in the CSV file, otherwise the template cannot be applied.
- Columns of the CSV file without a mapping are skipped, and the order of the columns does not matter.

Since a template group can contain several CSV templates, the user selects the appropriate template via its **templateId** when uploading the CSV file.

## Assignment of fields
In addition to the general fields, there are additional fields that can only be usefully supported by CSV documents.
- **order**: This field is intended for the linking of several table rows or for the comprehensive import of several different CSV documents. Sometimes two different CSV documents have to be imported so that this can be mapped as a transaction in GT.
- **sn**: This field contains the name of the instrument. It is not currently used to recognize the instrument.

## Configuration
In addition to the general configuration, additional configurations are required due to the document type:
- **bond**: Possibly the CSV document contains bonds with a **price value**. This price value could be marked with a "%". Therefore, this configuration requires the field name and the corresponding label. For example, with "quotation|%" it is expected that the values of the "quotation" field are supplemented with a "%".
- **_delimiterField_**: The field delimiter, which is used to separate data fields (table columns) within the data records (row).
- **_templateId_**: This supports the unique assignment of the import template to the CSV document. It must be unique within the template group and is offered for selection when the CSV document is uploaded.
- **ignoreLineByFieldValue**: It is possible to skip certain data lines during import based on a **field value**. The configuration **Field||String** contains the **field** and the corresponding **character string** as an exclusion of the line for the import. **Regular expressions** can also be used for the character string, whereby the expression must cover the **entire field value**. As an exception, the character string "**||**" is used as a separator. For example, the configuration "ignoreLineByFieldValue=ta||-" ignores all lines that contain exactly a "-" in the "ta" field.

## Example of a CSV template
The following import template processes the **transaction export** of **E-Trading** of the trading platform **Postfinance**. With a single template, it covers both securities transactions and account transactions. Since the template language is German, the column headers and transaction type texts are German as well.
{{< highlight markdown "linenos=true, hl_lines=2 3 13 14 15 16 29 32" >}}
datetime=Datum
order=Auftrag #
transType=Transaktionen
symbol=Symbol
sn=Name
isin=ISIN
units=Anzahl
quotation=Stückpreis
tc1=Kosten
ac=Aufgelaufene Zinsen
ta=Nettobetrag in der Währung des Kontos
cac=Währung
[END]
templatePurpose=Transaktions Export
templateId=1
transType=WITHDRAWAL|Auszahlung,Forex-Belastung
transType=WITHDRAWAL|Fx-Belastung Comp.
transType=DEPOSIT|Vergütung
transType=DEPOSIT|Forex-Gutschrift
transType=DEPOSIT|Fx-Gutschrift Comp.
transType=INTEREST_CASHACCOUNT|Zins
transType=FEE|Jahresgebühr
transType=ACCUMULATE|Kauf
transType=REDUCE|Verkauf
transType=REDUCE|Rückzahlung
transType=DIVIDEND|Dividende
transType=DIVIDEND|Coupon
dateFormat=dd.MM.yyyy HH:mm
delimiterField=;
bond=quotation|%
overRuleSeparators=All<’'|.>
ignoreLineByFieldValue=ta||-
otherFlagOptions=BOND_ADJUST_UNITS_AND_QUOTATION_WHEN_UNITS_EQUAL_ONE
{{< / highlight >}}
- _Lines 1-12_: Each line maps the exact column header of the CSV file to a GT field. For example, "units=Anzahl" reads the value of the **units** field from the "Anzahl" column.
- _Line 2_: The "Auftrag #" column supplies the **order** field. This merges several rows of the same order, for example partial executions of a purchase, into one transaction.
- _Line 3_: The text of the "Transaktionen" column determines the transaction type via the **transType** field. The possible texts are mapped in _lines 16-27_ of the configuration.
- _Line 14_: The line "templatePurpose=" is inserted automatically when an import template is **exported** and fills the **Purpose of template** property on **import**. It does not have to be entered when creating a template manually.
- _Line 15_: The mandatory **templateId** of this template. When uploading a CSV document, the template is selected via this number.
- _Lines 16-27_: The texts of the "Transaktionen" column are mapped to the transaction types. _Line 16_ shows the comma list, both "Auszahlung" and "Forex-Belastung" result in a withdrawal (WITHDRAWAL). Several lines per transaction type are allowed, for example "Verkauf" and "Rückzahlung" both lead to a sale (REDUCE). The account-related transaction types WITHDRAWAL, DEPOSIT, INTEREST_CASHACCOUNT and FEE are only possible in CSV templates.
- _Line 28_: The date format also contains the time, as the "Datum" column supplies both.
- _Line 29_: The mandatory field delimiter, in this export the semicolon.
- _Line 30_: For bonds, the value of the "Stückpreis" column may be supplemented with a "%".
- _Line 31_: Regardless of the user's **country/language** setting, "’" or "'" is expected as the digit grouping and the point as the decimal separator.
- _Line 32_: Rows whose "Nettobetrag in der Währung des Kontos" column contains only a "-" are skipped. Postfinance marks bookings without effect on the account this way.
- _Line 33_: This option is described in the parent chapter [Import transaction template](../).

### Excerpt of a matching CSV file
The following **fictitious** excerpt shows a header line and three data rows as expected by this template:
{{< highlight markdown >}}
Datum;Auftrag #;Transaktionen;Symbol;Name;ISIN;Anzahl;Stückpreis;Kosten;Aufgelaufene Zinsen;Nettobetrag;Nettobetrag in der Währung des Kontos;Saldo;Währung
02.05.2023 09:31;39230910;Kauf;VOO;Vanguard S&P 500 ETF;US9229083632;12;370.85;25.30;;-4'475.50;-4'475.50;12'510.35;USD
15.06.2023 00:00;;Dividende;VOO;Vanguard S&P 500 ETF;US9229083632;12;1.4874;;;17.85;17.85;12'528.20;USD
30.06.2023 00:00;;Titeleingang;VOO;Vanguard S&P 500 ETF;US9229083632;5;;;;-;-;12'528.20;USD
{{< / highlight >}}
The first data row results in a purchase (ACCUMULATE) via the text "Kauf" in the "Transaktionen" column, whereby GT checks the calculation 12 × 370.85 plus costs of 25.30 against the value 4'475.50 of the "Nettobetrag in der Währung des Kontos" column. The "Nettobetrag" and "Saldo" columns are not mapped in the template and are ignored. The third row is skipped due to the configuration "ignoreLineByFieldValue=ta||-", as the "Nettobetrag in der Währung des Kontos" column contains only a "-".
