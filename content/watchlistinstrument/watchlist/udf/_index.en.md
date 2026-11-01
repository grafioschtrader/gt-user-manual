---
title: "Custom Field view"
date: 2026-10-15T12:00:00+02:00
draft: false
weight: 25
archetype: "default"
---
In this view, the **additional fields** for **currency pairs** and **instruments** are displayed in table form.

## Table view
Of course, the defined properties can also be shown and hidden in this table. All additional fields corresponding to the instrument are then still visible in the expandable table row.

### Website URL
If an additional field contains a website URL, this is represented by one of the following icons: {{< svg "link0.svg" svg-icon-size >}}, {{< svg "link1.svg" svg-icon-size >}}, {{< svg "link2.svg" svg-icon-size >}}, {{< svg "link3.svg" svg-icon-size >}}. 
{{% callout style="info" title="Determining which symbol" %}} 
The following information is of secondary importance for the user: GT uses a unique numerical key for each additional field internally. This is incremented by 1 when an additional field is created. The corresponding URL symbol is calculated from this key using Modulo. If there are several additional fields with a website URL, the symbols do not have to be evenly distributed. 
{{% /callout %}}

### Expanding table row
It may be that you have defined a large number of fields and only some of them are displayed in the table. All defined fields are displayed under the expandable table row.

## Automatically filled values
Additional fields for instruments can [fill their values automatically]({{% relref "/basedata/udfmetadata/instruments" %}}). When this view is opened, fields with a template are filled at once, except in a simulation environment or when accessing another client. An icon in front of the value shows where it comes from:
- A magic wand marks an automatically filled value. The tooltip gives the fill source, the time of the last check and, where available, the source.
- A lock marks a cell you have taken over manually.
- A light bulb with an italic value marks a proposal of a language model that has not been accepted yet. The tooltip shows the excerpt from the source.

A value without an icon was entered manually. For a security with automatically filled fields, the context menu contains the entry **Automatic fill**. Below it you find, per field, **Take over manually** or **Fill automatically again** and, for a proposal, **Accept proposal** and **Reject proposal**. GT no longer changes a cell taken over manually, even when it is empty. **Fill automatically again** recomputes a cell with a template at once.
