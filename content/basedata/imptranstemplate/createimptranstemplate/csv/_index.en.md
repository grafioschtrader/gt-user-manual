---
title: "Import template CSV"
date: 2021-06-27T12:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
An **import template** for a CSV file makes the connection between the **table columns** of the CSV file and the **fields** required for GT. It also contains CSV document-specific **configurations**. A CSV file can contain **several transactions** compared to the PDF document. It will generate a maximum of one transaction per table row in GT. Certain transactions are made over several lines, for example a partial sale of a security. Due to the import template, the user does not have to make any adjustments to columns in the CSV file.
{{% notice note %}}
**Why a general CSV importer is not the solution?** Similar tools like GT only have a CSV importer, i.e. the user has to create a CSV file according to the specification of this tool. With GT, we refrain from this error-prone and unnecessary work that the user is forced into. GT is the avoidance of bullshit jobs and therefore there is the solution with these **CSV import templates**{{% /notice %}}

## Assignment of fields
In addition to the general fields, there are additional fields that can only be usefully supported by CSV documents.
- **order**: This field is intended for the linking of several table rows or for the comprehensive import of several different CSV documents. Sometimes two different CSV documents have to be imported so that this can be mapped as a transaction in GT.
- **sn**: This field contains the name of the instrument. It is not currently used to recognize the instrument.

## Configuration
In addition to the general configuration, additional configurations are required due to the document type:
- **bond**: Possibly the CSV document contains bonds with a **price value**. This price value could be marked with a "%". Therefore, this configuration requires the field name and the corresponding label. For example, with "quotation|%" it is expected that the values of the "quotation" field are supplemented with a "%".
- **_delimiterField_**: The field delimiter, which is used to separate data fields (table columns) within the data records (row).
- **_templateId_**: This supports the unique assignment of the import template to the CSV document. It is offered for selection when the CSV document is uploaded.
- **ignoreLineByFieldValue**: It is possible to skip certain data lines during import based on a **field value**. The configuration **Field||String** contains the **field** and the corresponding **character string** as an exclusion of the line for the import. **Regular expressions** can also be used for the character string. As an exception, the character string**"||**" is used as a separator. For example, the configuration "ignoreLineByFieldValue=ta||-" ignores all lines that contain a "-" in the "ta" field.
