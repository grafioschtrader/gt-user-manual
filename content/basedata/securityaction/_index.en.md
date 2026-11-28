---
title: "Security action"
date: 2026-11-20T22:54:47+01:00
draft: false
weight: 65
archetype: "default"
---
The security action manages three use cases where GT automatically generates matching sell and buy transactions: **ISIN changes**, **spin-offs** and **security transfers**. All three require adjustments to holdings that would be error-prone and tedious by hand.

{{% callout style="info" title="Artificial transactions" %}}
In all three use cases, the system creates sell and buy transactions on behalf of the tenant. This is an exception to the principle that only the user creates transactions. The reports recognize these system bookings: for an ISIN change and a security transfer, the [German tax help]({{% ref "/reportportfolio/germantax" %}}) carries the purchases forward with their purchase date and cost without showing a gain, and for a spin-off it divides the cost instead of calculating a sale. In Switzerland these transactions have no effect on tax calculations.
{{% /callout %}}

{{% callout style="info" title="Transaction limit" %}}
Because the ISIN change, the spin-off and the security transfer create transactions, they are rejected when the client has already reached the [per-client transaction limit](../../transaction/). If the count is still below the limit, the action is carried out in full, even if this slightly exceeds the limit.
{{% /callout %}}

## Overview
Security actions are displayed as a **TreeTable** under **Base data** in the navigation tree. The tree has two root nodes:
+ **System actions**: System-wide actions created by an administrator. An **ISIN change** is recorded when a security receives a new ISIN, a **spin-off** when the shareholders of a company receive shares of a new company. All affected tenants can apply the action to their holdings.
+ **Client Transfers**: User-initiated transfers of a security from one securities account to another within the same tenant.

## TreeTable columns
| Column | Description |
|--------|-------------|
| **Name** | Name of the action or the affected security. |
| **Action type** | **ISIN change** or **Spin-off** (for system actions). |
| **Cost allocation** | Share of the cost basis that passes to the child security (for spin-offs). |
| **History basis** | Administrators only: whether the price source adjusted the parent's prices before the ex-date for the spin-off. **Undetermined** calls for a check, see [History basis]({{% ref "/basedata/securityaction/spinoff" %}}#HISTORYBASIS). |
| **Old ISIN** | The previous ISIN of the security; for a spin-off the ISIN of the parent. |
| **New ISIN** | The new ISIN of the security; for a spin-off the ISIN of the child. |
| **Action date** | Date on which the action takes effect; for a spin-off the ex-date. The closing price of this date is used for the generated transactions. |
| **From units** | Split factor numerator — indicates how many old units are converted (e.g. 1 for a pure ISIN change). For a spin-off the number of parent shares. |
| **To units** | Split factor denominator — indicates how many new units result (e.g. 1 for a pure ISIN change, 10 for a 1:10 split). For a spin-off the number of child shares allotted for them. |
| **Affected** | Number of affected tenants (for system actions). |
| **Applied** | Number of tenants that have already applied the action. |
| **Status** | Current status of the action for the logged-in user. |
| **Units** | Number of transferred units (for transfers). |
| **Price** | Price used for the generated transactions. |

## Status values
+ **Not applied**: The action has not yet been applied to the user's holdings.
+ **Applied**: The action has been applied and the corresponding transactions have been generated.
+ **Reversed**: The action was reversed after application; the generated transactions have been deleted.

## Detail pages
+ [ISIN change]({{% ref "/basedata/securityaction/securityisinrename" %}}): Describes the workflow when a security's ISIN is renamed.
+ [Spin-off]({{% ref "/basedata/securityaction/spinoff" %}}): Describes recording, applying and reversing a spin-off and how the cost basis is divided.
+ [Security transfer]({{% ref "/basedata/securityaction/securitytransfer" %}}): Describes transferring a security between securities accounts.
