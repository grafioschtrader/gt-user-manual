---
title: "GT-PDF-Transform"
date: 2021-08-03T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
GT-PDF-Transform is an open source desktop **JavaFX application** that allows you to transform a variety of PDF files with the subsequent option of anonymization. For example, you can recursively read in all PDF files in a directory structure. The text lines that are not relevant for the import can then be removed in mass processing. If the majority of your securities transactions are available as PDF files, you can process them with GT-PDF-Transform and then import them into GT.

## Install GT-PDF-Transform
The GitHub repository [gt-pdf-transform](//github.com/grafioschtrader/gt-pdf-transform) contains information on the installation and creation of **GT-PDF-Transform**.

## Transform for transaction import
GT-PDF-Transform is particularly suitable for the initial import, see [Transaction import](../.) and [Import of securities transaction as PDF](../../../../transaction/security).

### Data protection of the GT instance
The transaction import is mainly processed in the **back-end**. The **transformation** is sufficient if you have no concerns regarding the data protection of the **GT instance** you are using. Otherwise, anonymization or pseudonymization must also take place.

#### Anonymize and pseudonymize
When anonymizing, most of the personal lines are removed from the document after the transformation, as they are irrelevant for the successful transaction import. These lines are usually found at the beginning and end of a PDF document. The lines with personal texts that are located within the [readable area](../../../../basedata/imptranstemplate/createimptranstemplate/pdf/) should only be deleted with caution. This data should be pseudonymized as a precaution.

## Pseudonymization for the GT project
With GT-PDF-Transform, transformed and pseudonymized PDF documents can be made available to the version management [Import transaction template](//github.com/grafioschtrader/gt-import-transaction-template) of the GT project on GitHub. The lines in the transformed PDF documents should not be deleted but **replaced** with **anonymized values** so that the document is **pseudonymized**. Please follow the instructions on [Import transaction template](//github.com/grafioschtrader/gt-import-transaction-template).

The video **Data protection and pseudonymization**: 
Unfortunately only in German:
{{< youtube sn024iNnG_E >}}

## Subdivision of the user interface
In addition to the **menu bar**, the user interface is divided into three areas: **Files**, **Replacement** and **PDF**.
- **Files**: The imported PDF documents are listed in a table. One PDF document per table row.
- **Replacement**: Text replacements are displayed in this table. The individual table cells can be edited.
- **PDF**: The transformed PDF document is displayed here according to the selection of **files**. The text field on the right shows the **transformed PDF with replacement**.

## General functionality
1. Starting GT-PDF-Transform
2. Import PDF documents with menu bar -> File -> "New PDF import".
   -   You can import individual PDF files or recursively import all PDF files in a directory structure.
   -   **Import directory**: There are additional input fields if this checkbox is selected, otherwise several selected files can be imported.
       - **Exclude file name patterns**: A regular expression can be specified for the exclusion of file names. For example, with the regular expression "(ver|sale)", all files containing "ver" or "sale" in the name are not imported.
       - **Include subdirectories**: With this option, the PDF documents are imported recursively from all subdirectories.
3. The explanations in this step depend on the purpose of the export. It is intended for **anonymizing and pseudonymizing**.
4. Select the files to be exported in the **Files area**.
5. Export the marked PDF files as text via the menu bar -> File -> "Export marked PDF as text"

**The basic video:**{{< youtube 5f3NuBP8\_T8 >}}

### Step 3: Anonymize and pseudonymize
As mentioned above, anonymization or pseudonymization should be used in certain constellations. In the left-hand **text field** of the **PDF** area, the "Remove text" function can be executed on a **selected text** using the context menu. This creates a new entry in the **Replacement** area. The result of this **replacement** can be viewed immediately in the right-hand **text field**. The **replacements** are applied to each individual document.

#### Edit replacement
The table rows in the **Replacement** area can be edited. A single click on the corresponding cell changes the value of the cell in the **selection box** or activates the **edit mode** for a **text input field**. The **Enter key** is used for the line break, so **Shift** + **Enter** **accepts** the **cell value** and ends editing. The use of [regular expressions](//www.tutego.de/blog/javainsel/2018/09/einfuehrung-in-regulaere-ausdruecke-mit-der-java-api/) is possible.

{{% notice info %}} 
For those familiar with the Java programming language, the replaceFirst method is used for the replacement. 
{{% /notice %}}

#### Save replacement
The content of the **Replacement** area can be saved for another session. The functions for saving and loading the replacement can be found under Menu bar -> File.
