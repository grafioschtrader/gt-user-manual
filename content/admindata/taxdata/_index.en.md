---
title: "Tax data"
date: 2026-03-12T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
GT supports importing **ICTax price lists** from the Swiss Federal Tax Administration (SFTA). The imported data enables a direct comparison of your own dividend and interest income with the official tax values and the export of an **eCH-0196 tax statement**. Management of tax data (import, re-import and deletion) is available exclusively to users with **administrator** privileges. Usage of the imported data (comparison and export) is available to every user in the [Dividends and interest](../../reportportfolio/dividends/) report. The **Tax data** view can be reached in the navigation area via the static sub-element **Tax data** under **Administrative data**.

{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The tax data functionality has not been finally checked and may still change.
{{% /notice %}}

{{< mermaid >}}
graph TD
    subgraph Admin["Administrator"]
        A[Create tax country] --> B[Create tax year]
        B --> C[Upload ICTax price list ZIP]
        C --> D[Import processes only<br/>held ISINs]
        D -.-> E[Re-import after<br/>new securities]
    end
    subgraph User["User"]
        F[Open Dividends and interest]
        F --> G[Compare ICTax columns]
        G --> H[Exclude securities from<br/>tax statement]
        H --> I[Export tax statement<br/>eCH-0196 ZIP]
    end
    D --> F
{{< /mermaid >}}

## Hierarchy table structure
Tax data is displayed in a three-level tree table:
- **Country**: Localized country name (e.g. "Switzerland")
- **Tax year**: Year within a country
- **File**: Uploaded price list file within a tax year

The table shows the columns **Name**, **Upload date** and **Records**. The upload date and records columns are only populated at file level.

## Creating and managing tax data
Operations are performed via the context menu and depend on the selected level:
- **No selection**: "Create tax country..." opens a dialog with a country dropdown for selecting the tax country.
- **Country selected**: "Create tax year..." creates a new tax year for the selected country. "Delete..." removes the country with all associated tax years and files.
- **Tax year selected**: "Upload tax data..." opens a dialog for uploading a ZIP file containing the ICTax price list in XML format. "Delete..." removes the tax year with all associated files.
- **File selected**: "Re-import tax data..." reprocesses the existing file against the tenant's current ISINs. "Delete..." removes the file and all associated tax data.

{{% notice note %}}
The import only creates entries for ISINs that the tenant currently holds. If new securities are added after the initial import, a **re-import** can be performed to capture the newly matching ISINs.
{{% /notice %}}

{{% notice note %}}
The imported ICTax data appears in the [Dividends and interest](../../reportportfolio/dividends/) report as additional columns for comparison and can be used there for tax statement export.
{{% /notice %}}
