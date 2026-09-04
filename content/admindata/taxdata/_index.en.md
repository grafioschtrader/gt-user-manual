---
title: "Tax data"
date: 2026-08-24T22:54:47+01:00
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
        C --> J[Take over exchange rates<br/>of the tax year]
        J -.-> K[Correct a rate<br/>if required]
        D -.-> E[Re-import after<br/>new securities]
    end
    subgraph User["User"]
        F[Open Dividends and interest]
        F --> G[Compare ICTax columns]
        G --> H[Exclude securities from<br/>tax statement]
        H --> I[Export tax statement<br/>eCH-0196 ZIP]
    end
    D --> F
    J --> F
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
The imported ICTax data appears in the [Dividends and interest](../../reportportfolio/dividends/) report as additional columns for comparison and can be used there for tax statement export. These columns only appear when the client's [country](../../tenantportfolio/client/#properties) is set to **Switzerland**.
{{% /notice %}}

## Exchange rates of the tax authority
A price list contains not only the tax values of the securities but also the official exchange rates. GT takes these over automatically during the upload and assigns them to the tax year. As a rule they are the closing prices of the last trading day in December, which count as the tax value as at 31 December. Because these rates come from a different source and are taken at a different moment than the rates of GT's data providers, they usually deviate slightly from the rates GT has loaded for the same currency pair. For the tax return the rate of the tax authority is decisive, which is why GT gives it preference.
To see the rates of a tax year, expand the row of the tax year with the arrow at the beginning of the row. A table then appears below it with one row per currency and the columns **Currency**, **Denomination**, **Year-end rate**, **Annual mean rate**, **Year-end rate correction** and **Annual mean rate correction**. The **Denomination** states how many units of the foreign currency a rate refers to; for currencies with a small value such as the Japanese yen or the Danish krone this is usually 100. If a tax year contains no exchange rates, its row cannot be expanded.
The two rates of the tax authority cannot be changed. An administrator can, however, enter an own rate in the two correction columns, for example when the published rate is rounded too coarsely for the own calculation. An entered correction replaces the published rate; if the field stays empty, the published rate continues to apply. A correction is also retained when the price list of the same tax year is uploaded or re-imported later on.

{{% notice note %}}
The **Year-end rate** is used in the [Dividends and interest](../../reportportfolio/dividends/) report and in the tax statement for valuing positions in a foreign currency as at the end of the year, provided the client's [country](../../tenantportfolio/client/#properties) is set to **Switzerland** and the main currency to **CHF**. The effect is most noticeable on cash accounts in a foreign currency, because the price list holds no tax value of its own for those. If a rate is missing for a currency, GT uses its own year-end rate of the currency pair as before. The **Annual mean rate** is currently only imported and displayed, but not yet used for calculations.
{{% /notice %}}

{{% notice note %}}
A price list that contains only the changes compared with an earlier edition carries no exchange rates. The complete price list should therefore be uploaded at least once for a tax year.
{{% /notice %}}
