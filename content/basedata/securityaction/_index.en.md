---
title: "Security action"
date: 2026-03-08T22:54:47+01:00
draft: false
weight: 65
archetype: "default"
---
The security action manages two use cases where GT automatically generates matching sell and buy transactions: **ISIN changes** and **security transfers**. Both scenarios require manual adjustments to holdings that would be error-prone and tedious without this feature.

{{% notice style="info" title="Artificial transactions" %}}
In both use cases, the system creates sell and buy transactions on behalf of the tenant. This is an exception to the principle that only the user creates transactions, and is not an ideal solution. However, in Switzerland these artificial transactions have no effect on tax calculations. Since GT currently only supports tax calculations for Switzerland, this is not a problem in practice.
{{% /notice %}}

## Overview
Security actions are displayed as a **TreeTable** under **Base data** in the navigation tree. The tree has two root nodes:
+ **System Actions (ISIN Changes)**: System-wide actions created by an administrator when a security receives a new ISIN. All affected tenants can apply the action to their holdings.
+ **Client Transfers**: User-initiated transfers of a security from one securities account to another within the same tenant.

## TreeTable columns
| Column | Description |
|--------|-------------|
| **Name** | Name of the action or the affected security. |
| **Old ISIN** | The previous ISIN of the security (for ISIN changes). |
| **New ISIN** | The new ISIN of the security (for ISIN changes). |
| **Action date** | Date on which the action takes effect. The closing price of this date is used for the generated transactions. |
| **From units** | Split factor numerator — indicates how many old units are converted (e.g. 1 for a pure ISIN change). |
| **To units** | Split factor denominator — indicates how many new units result (e.g. 1 for a pure ISIN change, 10 for a 1:10 split). |
| **Affected** | Number of affected tenants (for ISIN changes). |
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
+ [Security transfer]({{% ref "/basedata/securityaction/securitytransfer" %}}): Describes transferring a security between securities accounts.
