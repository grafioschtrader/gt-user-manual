---
title: "Import transaction template"
date: 2025-04-22T12:54:47+01:00
draft: false
weight: 35
archetype: "default"
---

Most trading platforms offer an export function for transactions. The result of this export can possibly be imported into GT. As each trading platform implements its own **document** design, one or more **import templates** must be created in GT according to these documents. GT can process the two document types **PDF** and **CSV**.

{{% notice note %}} 
GT's aim with these import templates is to generalize the transaction import as much as possible. This avoids many specific implementations per trading platform. In addition, a document change of the trading platform only leads to an update of GT in a few cases, which is very useful for the user and software developer. 
{{% /notice %}}

## Purpose of the import template group
There can be one or more import templates for a trading platform, so these are kept in a **template group**.

## Template group properties
- **Template group name**: Name of the template group.
- **Transaction implementation**: In some cases, the processing of **PDF/CSV documents** requires a **special transaction import implementation** in the **back-end**. In most cases, the general import function implemented in GT with the corresponding import templates is sufficient for processing the transaction documents of a trading platform. In all other cases, an import function must be implemented in GT.

## Create and edit template group
An **import template group** is created, edited and deleted in the main area in the "**Import template group**" view.
- **Delete import template group**: This menu item only becomes active if no import templates for this template group exist.

## Functions on selected view Import template group
There is additional functionality in addition to editing the template group. These functions support the creation of import templates. 
Unfortunately only in German:
{{< youtube DXVgkHk1qc4 >}}

### Export and import of import templates
The **import templates** can be exported or imported. The [gt-import-transaction-template](//github.com/grafioschtrader/gt-import-transaction-template) project on **GitHub** is intended for exchanging import templates. You may find the import templates for your trading platform there. If you want to make your self-created import templates available to the public, you can export them and add them to this project.

{{% notice style="info" title="File name and uniqueness" %}}
The properties Category, Language, Template, Document type and Valid from are used for the export and import of import templates. The combination of these four properties is unique. In addition, the file name of an import template is generated from these properties.
{{% /notice %}}

### Transform PDF to text
This function allows you to transform a PDF document to text so that it can be used for other functions. 
{{% notice warning %}} 
The transformation of the PDF document to text takes place in the **back-end**. If you have concerns about the data protection of the GT instance you are using, you should probably not use this function. 
{{% /notice %}}

### Create and edit import template
- Create an **import template** via the **context menu**.
- Edit an **import template** via the **context menu** when **an import template is selected**.
- Delete an **import template** via the **context menu** when **an import template is selected**.

### Check import templates with PDF as text
This function can be used to check a text created with the "**Transform PDF to text**" function for correct recognition. There are two fundamentally different results:

- **Document recognized**: The text or document was recognized, i.e. an import template could be found whose mandatory fields were filled with a field value.
- **Document not recognized**: There is a tabular view with all **import templates** in the **template group**. The last successfully recognized **field** per **import template** is displayed.

## Properties and table columns
The "PDF/CSV template" property is by far the most complicated input field in GT and is described in detail in the [Import transaction template](./createimptranstemplate/) chapter. The **purpose of the template** property is a free text that describes the purpose of the template for the **user**. The other four properties must be unique per **template group**, as they form the key when **importing** an **import template**. If the property values are congruent, the existing template is overwritten, otherwise a new import template is created.
- **Category**: The category best describes the purpose of the import template for the **system**.
- **Document type**: This specification defines whether it is a template for CSV or PDF documents.
- **Valid from**: This is an indication for the user to assign the template of changing document design to the same type of transaction.
- **Template language:** This is the language of the template. The import templates work with anchor points and these are often fixed to **static texts** in the **document**.

## Procedure for creating a PDF import template
If there are already **import templates** in the **template group**, you should first check whether an existing import template can be generalized. PDF documents from the purchase and sale of instruments are often very similar and can be handled with a single **import template**. An import template can be created interactively as follows:
1. Double login to GT. The first (**S1**) session for creating the import template and the second (**S2**) for checking it. Then select the **Import template group** view in both sessions.
2. **S1/S2**: Select the corresponding template group or create the template group in one of the sessions. In the other session, selecting the **import template group** again forces the loading of the template group just created.
3. **S1**: Convert the **PDF document** to be processed into text using the "**Transform PDF to text**" function.
4. **S2**: Trigger the "**Check templates with PDF as text**" function and copy the document available as text into the "**PDF form as text**" input field.
   - If you want to publish the import template, certain text sections in the transformed text should also be anonymized. This certainly includes the address and any numbers or text referencing your account and/or securities account.
5. **S1**: Trigger the "**Create import template**" function and copy the document available as text into the "**PDF/CSV template**" input field of the dialog.
6. **S1**: Fill in the **Category** etc. input fields as described above.
7. **S1**: In the text of the **PDF/CSV template** input field, replace the values to be read with the corresponding fields, see [Import transaction template](./createimptranstemplate/).
8. **S1**: Save import template.
9. **S2**: Press the "**Check templates with PDF as text**" button. According to the result, the import template must be edited again in S1 by repeating **steps 7-9**.

Unfortunately only in German:
{{< youtube YB8ZcY35tY4 >}}

### Publish the import template
You can help GT by publishing the import template you have created via the GitHub [gt-import-transaction-template](//github.com/grafioschtrader/gt-import-transaction-template) project. For example, follow the instructions of ["Send us your PDF or CSV documents with or without import templates](//github.com/grafioschtrader/gt-import-transaction-template)." 

Unfortunately only in German:
{{< youtube MhRXGeNBe1A >}}
