---
title: "Custom Field view"
date: 2024-06-23T20:54:47+01:00
draft: false
weight: 25
archetype: "default"
---
In this view, the **additional fields** for **currency pairs** and **instruments** are displayed in table form.

## Table view
Of course, the defined properties can also be shown and hidden in this table. All additional fields corresponding to the instrument are then still visible in the expandable table row.

### Website URL
If an additional field contains a website URL, this is represented by one of the following icons: {{< svg "link0.svg" svg-icon-size >}}, {{< svg "link1.svg" svg-icon-size >}}, {{< svg "link2.svg" svg-icon-size >}}, {{< svg "link3.svg" svg-icon-size >}}. 
{{% notice style="info" title="Determining which symbol" %}} 
The following information is of secondary importance for the user: GT uses a unique numerical key for each additional field internally. This is incremented by 1 when an additional field is created. The corresponding URL symbol is calculated from this key using Modulo. If there are several additional fields with a website URL, the symbols do not have to be evenly distributed. 
{{% /notice %}}

### Expanding table row
It may be that you have defined a large number of fields and only some of them are displayed in the table. All defined fields are displayed under the expandable table row.
