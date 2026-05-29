---
title: "Archive of historical price data"
date: 2026-05-25T08:00:00+01:00
draft: false
weight: 10
archetype: "default"
---
When a security's **data provider** changes, or when a split triggers a complete reload of the historical price data, parts of the older history may no longer be available from the new provider. The **archive** preserves the displaced prices so that they can be merged back into the current price series. Existing price data is therefore not lost, even when the new provider only supplies a limited history.

## When the archive is filled
The archive is filled automatically only. There is intentionally no manual archive action. In two situations the current prices are first archived and the table is then rebuilt from the new data provider.

### Change of the data provider
If the security is delisted from the previous exchange or a different connector is configured for the historical price data, the new data provider may no longer be able to deliver prices for the older part of the history. Before the current data is discarded and refetched from the new provider, GT copies the existing prices into the archive.

### Reload after a split
When a split is applied to a security, the entire historical price data is reloaded from the active data provider. Some providers only supply data for the last 10 to 20 years. Without the archive, the older history would be lost on every split. Here too, the existing prices are moved into the archive before the reload begins.

## Workflow
The following diagram shows the workflow from triggering the background task to the renewed price series.

{{< mermaid >}}
flowchart TD
    A[Data provider changed or<br/>Job 5 triggered] --> B[Current prices copied to archive]
    B --> C[Current price table cleared]
    C --> D[Load prices from new data provider]
    D --> E{Does the new provider cover<br/>every archived date?}
    E -->|Yes| F[Archive is automatically cleared]
    E -->|No| G[Missing dates supplemented from the archive<br/>split factor is applied]
    G --> H[Archive is kept as a backup]
{{< /mermaid >}}

More on the underlying background task is available under [Background tasks]({{< relref "/admindata/taskdatachangemonitor/taskdescription" >}}) at Job 5.

## Opening the archive view
The context menu of the **Historical prices** view offers the entry **"Show archived historical price data"**. The number of currently archived daily prices for this security is shown in parentheses, for example **"Show archived historical price data (0)"** when nothing has been archived yet. The entry is also available when the archive is empty, so that an external backup can be brought in via the import.

Clicking the entry switches the display area to the archive view in place. The context menu entry **"Return to current historical price data"** in the archive view switches back.

## Editing and deleting individual archived prices
Individual archived daily prices can be edited and deleted, just like the current historical price data. On a selected row the context menu offers **"Edit Archived historical price data"** and **"Delete Archived historical price data"**. The same [user rights]({{< relref "/intro/userrights" >}}) apply as for the current prices: the owner of the security and members of the privileged or administrator group change and delete directly, while all other users can propose a change to an archived price through a [data change request]({{< relref "/basedata" >}}) that then has to be approved. Deleting requires the right to edit directly. When editing, the trading day and the transfer date stay unchangeable; only the price values can be adjusted. Individual prices cannot be created — the archive is filled automatically or through the import only.

## Further functions in the archive view
The context menu additionally offers the following functions for the whole archive.

- **Apply split to archived data** — Opens a dialog for entering a split that was discovered after the fact (split date and ratio). Only archived rows whose date is older than the chosen split date are adjusted. Use this when a split was known only to the old data provider and is not recorded under the current security configuration.
- **Delete all archived data** — Removes every archived row for this security after a confirmation prompt. Run this only once the current price series is considered complete.
- **Import historical data** — Loads a previously saved CSV file into the archive. Useful when the archived data is held outside of GT and needs to be restored.
- **Export table data CSV as file** — Downloads the complete archive for this security. The export format matches the import format.

## CSV format for export and import
The file uses a **semicolon** as the field separator. The first row contains the column headers and determines the order; the columns may appear in any order. The columns **date** (trading day) and **close** (closing price) are required; the other columns **open**, **high**, **low**, **volume** and **transferDate** are optional. If **transferDate** is missing from an imported row, today's date is used. Existing archived rows for the same trading day are not overwritten on re-import.

{{% notice style="info" title="Automatic life cycle" %}}
The archive is only cleared automatically when the new data provider supplies every day that was previously archived. If only a part is covered, the archive is kept as a backup. The final removal in that case is done manually via **"Delete all archived data"**.
{{% /notice %}}
