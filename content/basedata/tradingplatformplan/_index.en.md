---
title: "Trading platform plan"
date: 2026-02-18T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
A **securities account** has a **trading platform plan** for the following reasons:
+ It controls the correct assignment of **import templates** to a depot.
+ A **fee model** can be defined to estimate transaction costs for hypothetical trades. This allows GT to deliver **realistic results** when simulating the **hypothetical closing out** of positions.

The relationships are shown in the following entity relationship diagram:
{{< mermaid >}}
erDiagram
  "Securities account" }o--|| "Trading platform plan" : has
  "Trading platform plan" }o--o| "Template group" : has
  "Template group" ||--o{ "Import template" : has
{{< /mermaid >}}

## Fee model
The fee model lets you define **transaction cost rules** as YAML on a trading platform plan. Each rule consists of a **condition** and a **fee expression**, both written using **EvalEx expressions**. The fee model is edited in a **separate dialog** opened via the context menu of the trading platform plan table: right-click on a trading platform plan → **Edit fee model...**. The dialog contains a **Monaco editor** with schema-assisted autocompletion and hover documentation for the YAML.

{{% notice style="info" title="Per-account override" %}}
Individual [securities accounts]({{% ref "/tenantportfolio/securityaccounts" %}}) can define their own fee model YAML that overrides this plan-level model. The same fee model editor can also be opened from the securities account's context menu in the navigation tree → **Edit fee model...**. When a securities account has its own fee model, it takes priority over this trading platform plan's model.
{{% /notice %}}

{{% notice style="warning" title="Current status" %}}
The simulation environment is currently intended for simulation purposes only. It is not yet used to calculate open positions — hypothetical transactions do not yet take transaction costs into account.
{{% /notice %}}

Rules are evaluated **top-to-bottom** — the first rule whose condition evaluates to `true` determines the transaction cost. Use `"true"` as condition for a catch-all default rule.

There are two mutually exclusive YAML formats at the top level:

### Flat rules (no date restriction)
Use the top-level `rules` array when the fee schedule does not change over time. Example for a Swiss broker:

```yaml
rules:
  - name: "Swiss ETF flat fee"
    condition: 'instrument == "ETF" && mic == "XSWX"'
    expression: "9.0"
  - name: "US equities"
    condition: 'assetclass == "EQUITIES" && currency == "USD"'
    expression: "MAX(15.0, tradeValue * 0.0025)"
  - name: "Default"
    condition: "true"
    expression: "MAX(20.0, tradeValue * 0.003)"
```

### Time-based periods with nested rules
Use the top-level `periods` array when the fee schedule changes over time. Each period has a **validFrom** date, an optional **validTo** date, and its own `rules` array. The first period matching the **transaction date** is used. Omitting `validTo` means the period is open-ended.

```yaml
periods:
  - validFrom: "2023-01-01"
    validTo: "2024-12-31"
    rules:
      - name: "Old flat fee"
        condition: "true"
        expression: "25.0"
  - validFrom: "2025-01-01"
    rules:
      - name: "New ETF fee"
        condition: 'instrument == "ETF"'
        expression: "5.0"
      - name: "New default fee"
        condition: "true"
        expression: "MAX(10.0, tradeValue * 0.002)"
```

{{% notice style="info" title="Mutually exclusive" %}}
A fee model YAML must contain either `rules` or `periods` at the top level, never both.
{{% /notice %}}

### JSON Schema
The YAML must conform to the following JSON Schema. It can also be used as context for an LLM to generate valid fee model YAML.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Fee Model Configuration",
  "description": "Rule-based transaction fee model. Supports flat rules (always applicable) or time-based periods with nested rules.",
  "type": "object",
  "oneOf": [
    {
      "required": ["rules"],
      "properties": {
        "rules": {
          "type": "array",
          "description": "Fee rules (no date restriction). Evaluated top-to-bottom; first matching condition wins.",
          "minItems": 1,
          "items": { "$ref": "#/$defs/FeeRule" }
        }
      },
      "additionalProperties": false
    },
    {
      "required": ["periods"],
      "properties": {
        "periods": {
          "type": "array",
          "description": "Time-based fee periods. Each period has a date range and its own rules array. First period matching the transaction date is used.",
          "minItems": 1,
          "items": { "$ref": "#/$defs/FeeModelPeriod" }
        }
      },
      "additionalProperties": false
    }
  ],
  "$defs": {
    "FeeRule": {
      "type": "object",
      "required": ["name", "condition", "expression"],
      "properties": {
        "name": {
          "type": "string",
          "description": "Human-readable rule name (e.g., 'Swiss stocks - Premium tier')"
        },
        "condition": {
          "type": "string",
          "description": "EvalEx boolean expression. Variables: tradeValue (trade amount), units (share count), instrument (string: DIRECT_INVESTMENT, ETF, MUTUAL_FUND, PENSION_FUNDS, CFD, FOREX, ISSUER_RISK_PRODUCT, NON_INVESTABLE_INDICES), assetclass (string: EQUITIES, FIXED_INCOME, MONEY_MARKET, COMMODITIES, REAL_ESTATE, MULTI_ASSET, CONVERTIBLE_BOND, CREDIT_DERIVATIVE, CURRENCY_PAIR), mic (MIC code, e.g. XSWX, XNYS), currency (ISO code), fixedAssets (portfolio value), tradeDirection (0=buy, 1=sell). Legacy numeric aliases: specInvestInstrument, categoryType. Use 'true' for a catch-all default rule."
        },
        "expression": {
          "type": "string",
          "description": "EvalEx numeric expression for fee calculation. Supports MAX(), MIN(), IF(), ABS(), ROUND(), arithmetic, comparisons. Example: 'MAX(9.0, tradeValue * 0.001)'"
        }
      }
    },
    "FeeModelPeriod": {
      "type": "object",
      "required": ["validFrom", "rules"],
      "properties": {
        "validFrom": {
          "type": "string",
          "format": "date",
          "description": "Start date (inclusive) in YYYY-MM-DD format"
        },
        "validTo": {
          "type": "string",
          "format": "date",
          "description": "End date (inclusive) in YYYY-MM-DD format. Omit for open-ended (until further notice)."
        },
        "rules": {
          "type": "array",
          "description": "Fee rules for this period",
          "minItems": 1,
          "items": { "$ref": "#/$defs/FeeRule" }
        }
      }
    }
  }
}
```

### Variable reference
The following variables are available in conditions and expressions:

| Variable | Type | Description |
|----------|------|-------------|
| `tradeValue` | Numeric | Total trade amount (price × units) |
| `units` | Numeric | Number of shares/units |
| `instrument` | String | Financial instrument type, e.g. `"DIRECT_INVESTMENT"`, `"ETF"`, `"MUTUAL_FUND"`, `"PENSION_FUNDS"`, `"CFD"`, `"FOREX"`, `"ISSUER_RISK_PRODUCT"` |
| `assetclass` | String | Asset class, e.g. `"EQUITIES"`, `"FIXED_INCOME"`, `"MONEY_MARKET"`, `"COMMODITIES"`, `"REAL_ESTATE"`, `"MULTI_ASSET"`, `"CONVERTIBLE_BOND"`, `"CREDIT_DERIVATIVE"`, `"CURRENCY_PAIR"` |
| `mic` | String | MIC code of the stock exchange, e.g. `"XSWX"`, `"XNYS"` |
| `currency` | String | ISO currency code, e.g. `"CHF"`, `"USD"` |
| `fixedAssets` | Numeric | Total portfolio value |
| `tradeDirection` | Numeric | `0` = buy, `1` = sell |
| `transactionDate` | Date | Relevant for the periods format — determines which period applies |

Additionally, the legacy numeric variables `specInvestInstrument` and `categoryType` are available as the ordinal byte values of the instrument and asset class enums respectively.

### EvalEx functions
Conditions and expressions support the standard **EvalEx** function library. Commonly used functions include:
+ `MAX(a, b)` and `MIN(a, b)` — return the larger or smaller value
+ `IF(condition, thenValue, elseValue)` — conditional expression
+ `ABS(value)` — absolute value
+ `ROUND(value, scale)` — rounding
+ Arithmetic operators: `+`, `-`, `*`, `/`
+ Comparisons: `==`, `!=`, `>`, `<`, `>=`, `<=`
+ String equality: `instrument == "ETF"`

### Test fee estimation
Inside the fee model dialog, below the YAML editor, there is a collapsible **Test Fee Estimation** panel. You can use it to verify that your rules produce the expected costs. The test parameters use dropdown selection fields wherever possible, so you can conveniently pick a stock exchange or currency from a list.

| Field | Description |
|-------|-------------|
| **Trade value** | Total trade amount (price × units) |
| **Open units** | Number of shares/units traded |
| **MIC** | Stock exchange as dropdown (e.g. "Euronext Lisbon - XLIS") |
| **Currency** | Trade currency as dropdown (e.g. CHF, USD) |
| **Portfolio value** | Total portfolio/account value |
| **Instrument** | Instrument type as dropdown (e.g. ETF, direct investment, mutual fund) |
| **Asset class** | Asset class as dropdown (e.g. equities, fixed income, commodities) |
| **Trade direction** | Dropdown with Buy / Sell |
| **Transaction date** | Date picker — relevant for the periods format |

1. Fill in the desired test parameters.
2. Click **Test Fee Estimation**. This automatically saves the YAML first and then runs the estimation against the persisted model.
3. The result shows the **estimated cost** and the **name of the matched rule**, or an error message if no rule matched.
4. To save the YAML without testing, use the **Save** button.

{{% notice style="info" title="Comparison with actual transaction costs" %}}
While the test panel above validates single hypothetical transactions, the [Fee model comparison]({{% ref "/reportportfolio/transactioncosts" %}}) in the Transaction Costs report compares the fee model's estimates against actual recorded costs for all buy/sell transactions of a securities account. It provides statistical metrics (mean absolute error, mean relative error, RMSE) to evaluate the overall accuracy of the fee model.
{{% /notice %}}
