---
title: "ISIN change"
date: 2026-03-08T22:54:47+01:00
lastmod: 2026-03-08T22:54:47+01:00
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
    S->>U: Notification<br/>via internal mail
    U->>S: Applies ISIN change
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

After saving, GT references the existing security or creates a new one with the specified ISIN, counts the affected tenants, and sends them an internal mail notification about the available ISIN change.

### Deleting an ISIN change
The administrator can remove an ISIN change via **right-click** → **Delete ISIN change**. This is only possible as long as no tenant has applied the action.

## User workflow

### Applying an ISIN change
Once an administrator has created an ISIN change, every affected tenant sees the action in their TreeTable with the status **Not applied**. To apply the change: **right-click** on the corresponding row → **Apply ISIN change**.

GT then performs the following steps:
1. All transactions of the tenant that reference the old security are identified.
2. The closing price of the old security on the action date is retrieved.
3. For each affected securities account, a **sell transaction** for the old security and a **buy transaction** for the new security are generated.
4. If split factors are set (From units / To units not equal to 1:1), the unit count and price on the buy side are adjusted accordingly.
5. The status changes to **Applied**.

{{% notice style="warning" title="Prerequisite" %}}
A closing price for the old security must exist for the action date. If it is missing, the action cannot be applied.
{{% /notice %}}

### Reversing an ISIN change
An already applied ISIN change can be reversed via **right-click** → **Reverse ISIN change**. This deletes all automatically generated sell and buy transactions. The status changes to **Reversed**. The action can then be applied again.
