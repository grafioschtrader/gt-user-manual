---
title: "Risk-free rate mapping"
date: 2026-05-13T22:54:47+01:00
draft: false
weight: 65
archetype: "default"
---
The **risk-free rate** is the theoretical interest rate an investment would earn without any default risk. GT uses it as the reference for risk-adjusted metrics — today exclusively for the [Sharpe ratio in **Return and statistical data**]({{< relref "/watchlistinstrument/correlation/returnstatdata" >}}). Further evaluations that consume a risk-free rate are conceivable but not yet implemented.

In this mapping, every supported **ISO currency** is linked to a dedicated security whose closing prices represent the risk-free rate for that currency. Values are stored as decimal fractions — 4.32 % therefore corresponds to a closing price of 0.0432.

## Data model
A mapping row is the unique bridge between a currency and its risk-free security. The price history of that security is supplied by the **FRED** data provider and stored in the standard historical price table.

{{< mermaid >}}
erDiagram
    Currency ||--|| Mapping : "is key of"
    Mapping ||--|| Security : "points to"
    Security ||--o{ Historical-Quote : "supplies rate"
{{< /mermaid >}}

The security lives in the dedicated asset class **Risk-free rate** at the purpose-built stock exchange **Risk-Free Rate Sources**. It is not tradable — it acts solely as a container for the rate history.

## Pre-seeded mappings
After the first start of GT five major currencies are already mapped. Their closing prices are sourced from the following FRED series:

| Currency | Security                                | FRED series ID            |
|----------|-----------------------------------------|---------------------------|
| USD      | USD 3M Risk-Free Rate (FRED DGS3MO)     | `DGS3MO`                  |
| EUR      | EUR Risk-Free Rate (ESTR)               | `ECBESTRVOLWGTTRMDMNRT`   |
| GBP      | GBP Risk-Free Rate (SONIA)              | `IUDSOIA`                 |
| CHF      | CHF Risk-Free Rate (3M Interbank)       | `IR3TIB01CHM156N`         |
| JPY      | JPY Risk-Free Rate (Call Rate)          | `IRSTCI01JPM156N`         |

{{% notice style="warning" title="FRED API key required" %}}
For GT to actually fetch closing prices for these securities, an administrator must register a valid API key for the provider **FRED** under [Connector-Api-Key]({{< relref "/admindata/connectorapikey" >}}). Until that has happened, the price history stays empty and no Sharpe ratio can be computed. On the initial setup GT schedules the load jobs with a one-hour delay so the API key can be registered in time.
{{% /notice %}}

## Operation
The mapping is reached from the **navigation tree** under **Base data → Risk-free rate mapping**. The table has three columns:

- **Currency** — pick from the ISO currencies known to the system. A currency that is already used in another row no longer appears in the dropdown. The currency cannot be changed once the row has been saved — to correct a wrong assignment, delete the row and create it again.
- **Risk-free instrument** — pick from the existing risk-free securities. The list is restricted to securities whose own currency matches the chosen **Currency** column; an instrument already used in another row is hidden. When you switch the currency, this column is cleared automatically and has to be set again.
- **FRED series ID** — derived display of the series identifier of the chosen instrument; not directly editable.

A new row is added through the **plus icon** in the table header. An existing row is edited through the **pencil icon** at the end of the row, or deleted through the **trash icon**.

{{% notice style="info" title="Currency and instrument must match" %}}
Both the in-browser dropdown and an additional server-side check ensure that the row's currency matches the currency of the chosen instrument. A mismatched mapping is rejected by the server with an error message.
{{% /notice %}}

## User rights
Every authenticated user may read the table and create new rows. Existing rows may only be edited or deleted by the **creator**; in addition, users with the role **Administrator** or **without limits** have full access to all rows. Users with the role **Limits** are further capped at two changes per day — this cap is governed by the global parameter `gt.limit.day.RiskFreeRateMapping`.

## Adding a new currency
To add a risk-free rate for an additional currency, first create the **synthetic security** through normal security management with asset class **Risk-free rate** and stock exchange **Risk-Free Rate Sources**. Once historical prices are available for that security, it can be mapped to a currency in this table and will appear as a choice in the **Risk-free instrument** column.

{{% notice style="note" title="No inline create dialog" %}}
Creating a new risk-free security directly from this table is not implemented at this time. The security has to be created separately before it can be mapped.
{{% /notice %}}
