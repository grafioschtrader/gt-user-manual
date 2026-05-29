---
title: "General definition of additional fields"
date: 2024-06-23T20:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
GT specifies the corresponding **properties** for the **information classes**. Sometimes, however, it would be practical if the user could define their own properties for an information class. Such a function would be particularly helpful in the area of the **information class Instruments**. For this reason, the user can create corresponding **user-defined properties** for certain information classes.

This description shows that this functionality must consist of **two parts**. There is a part in which the properties of the **additional fields** are **defined** and another part in which these **additional fields** are **filled with content**. The content is filled when the information class is processed. For example, instruments and currency pairs are filled and displayed in the watchlist. Accordingly, this functionality is also described there, see [additional field](../../watchlistinstrument/watchlist/udf/).

{{% notice style="info" title="Additional field for all users" %}}
There are also additional fields for all users. Why this? A relational database management system, on which GT is based, has certain limits when mapping the different characteristics of instruments. A bond has completely different characteristics than a share. For this reason, there is a special implementation of [additional fields for instruments](./instruments/). Some additional fields are defined there for all users. These relate to bonds, shares as direct investments and CFDs
{{% /notice %}}

## Create and edit
Some functions can always be executed, others depend on the definition selected in the table.

### Functions in the view
The names of the menu items may differ slightly from reality, as this description is intended for a specific implementation.
An **additional field** is created, edited and deleted in the **main area** in the **General additional field view**. You can access this **view** in the **navigation area** on the static element "general definition of additional field".
- **Create** a "**Definition additional field**" via the **context menu**.
- **Edit** an **additional field** via the **context menu** when **the "Definition additional field" is selected**.
- **Delete** a "**Definition additional field**" via the **context menu** when **"Definition additional field" is selected**.

### Functions via general additional fields
The **general additional fields** may not be of interest to you. You therefore have the option of deactivating these general fields. They are then no longer displayed in the corresponding **information classes**. They are activated or deactivated via the **context menu** of the selected row of a **general additional field definition**.

## Properties and table columns
Some information is required to define an additional field. Data types are used to maintain the structure of the data. This serves to check the plausibility of the entries when filling the additional fields later. Sorting, filtering and possibly a future calculation based on this data can only function correctly if the data type is specified.
- **Information object**: Currently only currency pairs and instruments are supported, although there is a special implementation for instruments.
- **Order GUI**: The input fields are sorted by this ascending number.
- **Property name**: This is the text that appears in the column header of the table or to the left or above the input or output field.
- **Help text**: The number of characters of "Property name" is limited, therefore a more detailed description of the property can be entered. This is displayed as a tooltip in the column header of the table, in the input field or next to the property name of an output field.
- **Data type**: Only certain combinations are possible for specifying the **data type** and its field length. The input length is not required for certain data types. The description of these data types follows first:
  -   **Date**: Date property without time specification.
  -   **Date and time**: Property for date with time.
  -   **Yes and No**: A checkbox can be used to display a Yes/No property.
  -   **Website URL**: GT can only display a small part of the information relevant for an investment decision. Other useful sources can be added via the URLs of the websites. The maximum length of a URL must not exceed 256 characters.
  -   **Input values or limit**: An input value or limit must be specified for the following data types.
      - **Numeric**: The **total length of the decimal number** and the **number of decimal places** separated by a comma must be specified here. The separator is not included in the total length. Example: The range for "5.4" is -9.9999 - 9.9999. In this example, "." is the separator.
      - **Integer**: The **minimum** and **maximum** integer **values** separated by commas must be entered here. The value range can also include negative numbers. Example: "-10.99" covers the value range from -10 to 99.
      - **String**: Enter the **minimum** and **maximum length** of the character string here, separated by a comma. The maximum length must not exceed 2048 characters.

## Video Additional fields
Here you can see the additional fields in action: 
Unfortunately only in German:
{{< youtube xt0l3_8gYgM >}}
