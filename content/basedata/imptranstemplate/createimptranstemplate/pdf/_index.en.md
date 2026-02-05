---
title: "Import template PDF"
date: 2025-05-04T12:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
An **import template** for **PDF** is a definition for a single securities transaction per document. An import template for a PDF consists of a PDF document of the trading platform converted into text. The values relevant to the transaction are then replaced by variables with anchor points. A field with anchor points is enclosed with "**{}**", the anchor points are separated with a "**|**" from the field name and among themselves. An example of a valid field with anchor points, also called **field position**, could look like this **{datetime|P|N}**.

{{% notice note %}} 
With these **import templates for PDF**, GT follows a **declarative** instead of an imperative **path** for processing the PDF documents of the various trading platforms. This means that many GT updates can be avoided. In addition, hardly any ambitious software developer will volunteer to program this large number of pattern recognitions. This work is mostly simple-minded and completely contradicts the idea of generalization. GT does not create a bullshit job, which is useful for the user but only creates unnecessary work for the operator of a GT instance and the software developer
{{% /notice %}}

## Anchor points
The values relevant to the transaction are distributed throughout the entire PDF document. Each of these transaction documents has a fixed structure that only changes slightly for the same type of transaction. GT can use this similar structure with anchor points to recognize values. The **anchor point** **references** a **static text part** that does not change per document.
- The value can be recognized from a document line. If the structure in a line hardly changes, the following anchor points can be used.
  - **P**: Previous word. Also works if the value is at the beginning of a line. This anchor point can refer to a regular expression.
  - **Pc**: The value has a prefix, i.e. there is no space between the value and the prefix.
  - **N**: Following word. Also works if the value is at the end of a line. This anchor point can refer to a regular expression.
  - **Nc**: The value has a suffix, i.e. there is no space between the value and the suffix.
  - **SL**: The first word of the same line.
- **PL** and **NL** are used if the anchor point cannot be fixed to a static text part within the line to be processed. It is possible that the beginning of the preceding or following line is static text.
  - **PL**: The first word of the beginning of the line of the previous line.
  - **NL**: The first word of the beginning of the following line.

The number of words and numbers in the anchor point line must match if **SL**, **PL** and **NL** are set as the only anchor point.

### Readable area
The **readable area** defines the contiguous area of lines in the document where the **relevant values** are located and which are **referenced** by **anchor points**. Normally, the first and last few lines are not included in the readable area, so these may be missing both in the import template and in the transformed PDF document.

### Anchor points referenced set of static words at the beginning of the line
Sometimes you want to reference two variants of a static word with one anchor point. This flexibility is available for the beginning of the **same** line, the **preceding** **line** and the **following line**. This construct therefore works for **PL**, **SL** and **NL**. The possible words at the start of the line are enclosed in brackets and separated with "|" **\[w1|w2|w\...]**. In the following example, the start of the line of field **tt1** can begin with "Withholding tax" or "Anticipatory tax": 
{{< highlight markdown >}} 
[Withholding tax|Anticipatory tax] 35% (CH) CHF {tt1|SL|N|O} 
{{< / highlight >}}

### More variance for preceding and following words
The anchor point can be very flexible for the preceding and following word. The configuration can be quite demanding, as it allows the use of **regular expressions**. For anchor points **P** and **N**, **(?:)** can be used. This allows the use of a regular expression that is not processed as a group. It is a very powerful construct, which allows us a certain variance for the preceding and following word. Here are two examples: 
{{< highlight markdown >}} 
AMOUNT WE CHARGE TO THE ACCOUNT (?:CREDIT|DEBIT) {cac|P|SL} {ta|SL|N} 
{units|P|NL} Vanguard FTSE Japan ETF {quotation|NL|N} (?:\[A-Z]{3}\$) 
{{< / highlight >}} 
The first line expects either "CREDIT" or "DEBIT" as the preceding word in the **Currency of bank account (cac)** field. The second line is more complicated and states that the **quotation** field must be followed by a character string consisting of three capital letters at the end of the line.

### Field position configuration
Sometimes a field position must be additionally configured, these are expressed like the anchor points in the field position.
- **O**: There are optional values such as tax costs that only occur in certain documents. This can be configured with the "**O**" configuration.
- **R**: The content of this line can be repeated several times. This configuration is helpful when selling and buying a security, as sometimes the transaction is executed over several stock exchange transactions. The configuration must be in the first field position. The following repetitions cannot be handled as the number of columns in the two lines differs:
```
17 AT A PRICE OF 136.5"
53 OF 136,5
```

### Example of a PDF Template
Below is a template for the purchase and sale of securities.
{{< highlight markdown "linenos=true, hl_lines=1 2 5 8 10-13" >}}
Gland, {datetime|P|N}
(?:Transaction:|Stock Trade:) {transType|P|N} Our Reference: 12121212
According to your purchase order from 12.12.2012, we have executed the following transactions:
Title Place of Execution
ISHARES $ CORP BND ISIN: {isin|P} SIX Swiss Exchange
Sec. No.: 1613957
Quantity Price Amount
{units|PL|R} {quotation} {cac} 8’000.00
Total USD 8’250.00
Commission Swissquote Bank AG USD {tc1|SL|N|O}
[Duty (Federal Stamp Tax)|Federal Stamp Tax] USD {tt1|SL|N}
Stock Exchange Fees USD {tc2|SL|N}
Charged to Your Account USD {ta|SL|N}
[END]
transType=ACCUMULATE|Purchase
transType=REDUCE|Sale
dateFormat=dd.MM.yyyy
overRuleThousandSeparators= '’
{{< / highlight >}}
- _Line 1_:  The **{datetime|P|N}** refers to the transaction date, with the previous and following words used as anchor points. In this case, **P** refers to "**Gland,**" and **N** to the **end of the line**.
- _Line 2_: This line contains a [Regular Expression](//en.wikipedia.org/wiki/Regular_expression). For it to work correctly, the expression must be defined as a non-capturing group **(?:…)**; otherwise, the grouping in GT may not function correctly.
- _Line 5_: The **ISIN** is extracted from this line. Typically, at least two anchor points should be set. In this case, **{isin|P}** is unique across the entire document, so one anchor point is sufficient. Here, **P** references the static text "ISIN:".
- _Line 8_: **{units|PL|R} {quotation} {cac}** This line can be repeated. The R configuration must be in the first field position.
- _Line 10_: This field position for the main tax costs **{tc1|SL|N|O}** is optional. The anchor points refer to the word "**Commission**" and the **end of the line**.