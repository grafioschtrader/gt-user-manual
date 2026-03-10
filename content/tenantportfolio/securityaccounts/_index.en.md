---
title: "Security accounts"
date: 2026-02-26T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
The securities account is a "container" for the instruments you hold; no **transactions** with instruments can be carried out without it.
+ A **portfolio** can contain several **securities accounts** - as a rule, it is usually only one.
+ A portfolio can be evaluated on its own or across all **portfolios** in a **portfolio**.
+ There is a limit to the total number of securities accounts.
+ A securities account cannot be deleted as long as it is still referenced by a transaction.

## Creating and editing securities accounts
A **securities account** is created, edited and deleted via the **navigation area**.
+ **Create** a **securities account** via the **context menu** on the **static element** Securities accounts.
+ **Edit** a **securities account** via the **context menu** on the corresponding **element** of the securities account.
+ **Delete** a **securities account** via the **context menu** on the corresponding **element** of the securities account.

### Properties
All properties of the portfolio can be changed completely at any time.
+ **Name securities account**: A name for your securities account is displayed in various places in GT.
+ **Trading platform plan**: Selection of the trading platform plan for the corresponding securities account. Further information under [Trading platform plan]({{% ref "/basedata/tradingplatformplan" %}}).
+ **Trading periods** (table below the form): Defines which instrument types are allowed in this securities account and optionally within which date range. See [Trading periods](#trading-periods) below.
+ **Cheapest transaction costs**: This input is currently not used. Will be used in a future GT version for the implementation of the problem solution ["Deviation from reality"]({{% ref "/reportportfolio" %}}).

### Fee model
A securities account can optionally define its own fee model that overrides the one inherited from its trading platform plan. This is useful when you have negotiated special rates or conditions with your broker that differ from the plan's standard fee schedule.

When a securities account has its own fee model, it takes priority over the trading platform plan's fee model for cost estimation and comparison. If no account-level fee model is defined, the plan's fee model is used as before.

The fee model editor is opened via the context menu on the securities account node in the navigation tree: right-click on a securities account → **Edit fee model...**. The editor uses the same YAML format and Monaco editor as the trading platform plan fee model. For details on YAML syntax, available variables, and EvalEx functions, see [Trading platform plan — Fee model]({{% ref "/basedata/tradingplatformplan" %}}).
The correctness of the configured fee model can be verified using the [Fee model comparison]({{% ref "/reportportfolio/transactioncosts" %}}) in the Transaction Costs report. It shows the deviation between actual recorded transaction costs and the costs estimated by the fee model for all buy/sell transactions of the securities account.

### Trading periods
Trading periods restrict which instrument types can be traded in a securities account. Each row in the trading periods table represents one permitted combination of instrument type and asset class category within a date range.

**If no trading periods are defined, all trading is allowed** — this ensures backward compatibility for existing securities accounts.

#### Table columns
| Column | Required | Description |
|--------|----------|-------------|
| **Instrument** | Yes | The special investment instrument type (e.g., Direct Investment, ETF, CFD, etc.). |
| **Asset class** | No | The asset class category (e.g., Equities, Bonds). If empty, all categories are permitted for the selected instrument type. |
| **Date from** | Yes | Start date from which trading is permitted. Defaults to 2000-01-01. |
| **Date to** | No | End date until which trading is permitted. If empty, trading is permitted indefinitely. |

#### Default values
When a new securities account is created, two trading periods are automatically added:
- **Equities / Direct Investment** (from 2000-01-01, no end date)
- **Equities / ETF** (from 2000-01-01, no end date)

These defaults can be modified or removed as needed.

#### Editing rules
- For **existing rows** (already saved), only the **Date to** column can be changed. To change the instrument type or asset class category, delete the row and create a new one.
- For **new rows**, all columns can be edited.

#### Validation rules
- **Overlap check**: Two trading periods with the same instrument type and the same asset class category cannot have overlapping date ranges.
- **Transaction conflict on delete**: A trading period cannot be deleted if transactions already exist for that instrument type in this securities account.
- **Transaction conflict on Date to**: The **Date to** cannot be set to a date earlier than the latest existing transaction for that instrument type.

When creating a [securities transaction]({{% ref "/transaction/security" %}}), the instrument type of the security is checked against the trading periods of the target securities account. If no matching trading period covers the transaction date, the transaction is rejected.

## Transfer security
From the securities table of a securities account, a security can be transferred to another securities account within the same tenant. **Right-click** on the row of the desired security → **Transfer security**. The menu item is only available when the security has open positions (units greater than 0) and is not a margin product.

GT automatically generates a sell transaction in the source account and a buy transaction in the target account at the closing price of the selected transfer date. For full documentation on dialog fields, workflow, and reversing, see [Security transfer]({{% ref "/basedata/securityaction/securitytransfer" %}}).
