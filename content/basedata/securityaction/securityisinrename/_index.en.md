---
title: "ISIN change"
date: 2026-03-10T22:54:47+01:00
lastmod: 2026-03-10T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
When a security receives a new ISIN — for example due to a corporate merger, restructuring, or regulatory change — all holdings must be transferred from the old to the new security. GT supports this process as a system-wide action: an administrator creates the ISIN change once, and each affected tenant can apply it to their own holdings.

## Workflow overview
The following diagram shows the typical workflow of an ISIN change — from discovery by the user to application on their own holdings:

{{< mermaid >}}
sequenceDiagram
    participant U as User
    participant S as System (GT)
    participant A as Administrator

    U->>S: Discovers ISIN change<br/>(e.g. via watchlist)
    U->>A: Reports ISIN change<br/>via internal mail
    A->>S: Creates ISIN change<br/>(System Actions)
    S->>S: References existing or creates<br/>new security, counts affected tenants
    S->>S: Copies connector settings and<br/>loads historical price data
    S->>U: Notification<br/>via internal mail
    U->>S: Applies ISIN change
    S->>S: Reassigns transactions from action<br/>date onwards to new security
    S->>S: Generates sell (old ISIN)<br/>and buy (new ISIN)
    S-->>U: Status: Applied
{{< /mermaid >}}

## Administrator workflow
The administrator creates an ISIN change in the [Security action]({{% ref "/basedata/securityaction" %}}) TreeTable by **right-clicking** on the root node **System Actions (ISIN Changes)** → **Create ISIN change**.

### Dialog fields
| Field | Required | Description |
|-------|----------|-------------|
| **Old security** | Yes | The existing security whose ISIN is changing. Selected via the search button. |
| **New ISIN** | Yes | The new ISIN that the security will receive. If a security with this ISIN already exists, it is referenced; otherwise GT automatically creates a new security. |
| **Action date** | Yes | Date on which the change takes effect. The closing price of this date is used for the generated transactions. |
| **From units** | No | Split factor numerator. Default value 1. For a pure ISIN change without a stock split, this remains at 1. |
| **To units** | No | Split factor denominator. Default value 1. For a simultaneous stock split of e.g. 1:10, enter 10 here. |
| **Note** | No | Optional note about the ISIN change. |

After saving, GT references the existing security or creates a new one with the specified ISIN, counts the affected tenants, and sends them an internal mail notification about the available ISIN change. When GT creates a new security, it automatically copies the data feed configuration from the old security and starts loading historical price data in the background.

{{% notice style="info" title="Copied and not copied properties" %}}
The following properties are **copied** from the old security to the new one: name (with " (new ISIN)" suffix), currency, asset class, stock exchange, ticker symbol, denomination, active-to date, distribution frequency, leverage factor, all three connectors (history, intraday, dividend) with their URL extensions, stock exchange link, and product link. The **active-from date** is set to the action date of the ISIN change.

**Not copied** are: note/description fields, historical price data (loaded automatically in the background), and any tenant-specific watchlist assignments.
{{% /notice %}}

{{% notice style="warning" title="Verify connector settings" %}}
The connector settings copied from the old security may not be correct for the new instrument. After the ISIN change is created, verify that the new security actually has historical price data and that the configured connectors (history, intraday, and dividend) deliver correct data. Adjust the connector settings on the new security if necessary.
{{% /notice %}}

{{% notice style="info" title="Performance watchlist" %}}
The new instrument is not automatically added to the watchlist designated as performance. To add it, open the corresponding watchlist and use the **Add existing instrument** search dialog with the **With active holding** checkbox activated.
{{% /notice %}}

### Deleting an ISIN change
The administrator can remove an ISIN change via **right-click** → **Delete ISIN change**. This is only possible as long as no tenant has applied the action.

## User workflow

### Applying an ISIN change
Once an administrator has created an ISIN change, every affected tenant sees the action in their TreeTable with the status **Not applied**. To apply the change: **right-click** on the corresponding row → **Apply ISIN change**.

GT then performs the following steps:
1. All existing transactions of the tenant on or after the action date that reference the old security are **reassigned to the new security**. This includes dividends, buy/sell transactions, and any other operations recorded after the ISIN change took effect.
2. The closing price of the old security on the action date is retrieved.
3. For each affected securities account, a **sell transaction** for the old security and a **buy transaction** for the new security are generated based on the remaining pre-action-date holdings.
4. If split factors are set (From units / To units not equal to 1:1), the unit count and price on the buy side are adjusted accordingly.
5. The status changes to **Applied**.

{{% notice style="warning" title="Prerequisite" %}}
A closing price for the old security must exist for the action date. If it is missing, the action cannot be applied.
{{% /notice %}}

### Reversing an ISIN change
An already applied ISIN change can be reversed via **right-click** → **Reverse ISIN change**. This deletes all automatically generated sell and buy transactions. The status changes to **Reversed**. The action can then be applied again.

## Practical examples
Inverse and leveraged ETFs lose value over time due to the daily reset mechanism and so-called volatility decay. When the price falls too far, issuers perform a reverse split to maintain minimum exchange listing prices. In the USA, a reverse split requires a new CUSIP number and thus also a new ISIN, as the security structure changes substantially. Such ISIN changes with a simultaneous split are a typical use case for the GT function.

### Examples: ProShares inverse ETFs
| Name | Reverse split | Effective date | Old ISIN | New ISIN |
|------|---------------|----------------|----------|----------|
| ProShares Short QQQ (PSQ) | 1:5 | 2024-04-10 | US74347B7148 | US74349Y8378 |
| ProShares Short S&P500 (SH) | 1:4 | 2024-11-07 | US74347B4251 | US74349Y7537 |

### Recording in GT
For the **ProShares Short QQQ** example, the administrator would record the ISIN change as follows: select the instrument with ISIN US74347B7148 as the **Old security**, enter US74349Y8378 as the **New ISIN**, and set the **Action date** to 2024-04-10. Since a reverse split of 1:5 occurred simultaneously, **From units** is set to 1 and **To units** to 5. When applied, GT then generates the corresponding sell and buy transactions with adjusted unit counts.

{{% notice style="tip" title="Act early" %}}
ISIN changes caused by reverse splits are often noticed late by users — for example, when historical price data is no longer updated or portfolio reports show implausible values. The longer the change goes unnoticed, the more transactions have been recorded under the old ISIN that would then need to be corrected manually. GT's ISIN change function automatically reassigns all affected transactions to the new security, saving a laborious manual correction process.
{{% /notice %}}
