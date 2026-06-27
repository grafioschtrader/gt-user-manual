---
title: "Security transfer"
date: 2026-03-09T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
A security transfer moves a security from one securities account to another within the same tenant. GT automatically generates a sell transaction in the source account and a buy transaction in the target account at the closing price of the transfer date. Since every securities transaction references a cash account, the sale proceeds are credited to a cash account in the source portfolio, while the purchase amount is debited from a cash account in the target portfolio. This feature is useful when you change brokers or want to redistribute holdings between securities accounts.

The following diagram shows the workflow of a security transfer and the accounts involved:

{{< mermaid >}}
sequenceDiagram
    participant U as User
    participant SA as Source account
    participant SC as Cash account<br/>(source portfolio)
    participant TA as Target account
    participant TC as Cash account<br/>(target portfolio)

    U->>SA: Right-click on security<br/>→ Transfer security
    Note over SA,TC: GT retrieves closing price on transfer date
    SA->>SC: Sell transaction<br/>(credit: units × price)
    TC->>TA: Buy transaction<br/>(debit: units × price)
    Note over SA,TA: Security moves from<br/>source to target account
{{< /mermaid >}}

{{% notice style="info" title="Cash account movement" %}}
Since the sell and buy transactions each reference a separate cash account in their respective portfolio, a credit arises in the source portfolio and a debit in the target portfolio. If desired, this balance can subsequently be settled manually via a cash account transfer between the two cash accounts.
{{% /notice %}}

## Creating a transfer
A security transfer is initiated directly from the securities table of a securities account. **Right-click** on the row of the desired security → **Transfer security**. For more information about the securities table, see [Securities accounts]({{% ref "/tenantportfolio/securityaccounts" %}}).

{{% notice style="info" title="Availability" %}}
The **Transfer security** menu item is only available when the security has open positions (units greater than 0) and is not a margin product.
{{% /notice %}}

### Dialog fields
| Field | Editable | Description |
|-------|----------|-------------|
| **Security** | No | Name of the security to be transferred (read-only). |
| **Source account** | No | The securities account from which the security is transferred (read-only). |
| **Target portfolio** | Yes | Dropdown with available portfolios. Only portfolios that contain at least one eligible target securities account are shown. |
| **Target account** | Yes | Dropdown with the securities accounts of the selected portfolio. The source account is excluded. The selection updates automatically when the target portfolio changes. |
| **Transfer date** | Yes | Date on which the transfer is carried out. The closing price of this date is used for the generated transactions. The transfer date must be **after** the last existing transaction on this security in the source account. If an earlier date is entered, GT rejects the transfer and shows the date of the last transaction. |
| **Note** | Yes | Optional note about the transfer. |

### What happens during the transfer
After saving, GT performs the following steps:
1. The closing price of the security on the transfer date is retrieved.
2. A **sell transaction** for all held units at the closing price is generated in the source account. The proceeds are credited to a cash account in the source portfolio.
3. A **buy transaction** for the same unit count and price is generated in the target account. The amount is debited from a cash account in the target portfolio. GT prefers a cash account in the security's currency.
4. Both transactions are linked to each other.
5. The transfer appears in the [Security action]({{% ref "/basedata/securityaction" %}}) TreeTable under the **Client Transfers** node.

{{% notice style="warning" title="Prerequisite" %}}
A closing price for the security must exist for the transfer date. If it is missing, the transfer cannot be carried out.
{{% /notice %}}

## Reversing a transfer
A completed transfer can be reversed in the [Security action]({{% ref "/basedata/securityaction" %}}) TreeTable. Navigate to the **Client Transfers** node, **right-click** on the corresponding transfer → **Reverse transfer**. This deletes both automatically generated transactions (sell and buy) as well as the transfer record itself.

A reversal is only possible if no transactions on the transferred security have been recorded in the target account after the transfer date. If such transactions exist (e.g. dividends or additional purchases), the **Reverse transfer** menu item is disabled. To reverse the transfer in this case, you must first delete the later transactions in the target account.
