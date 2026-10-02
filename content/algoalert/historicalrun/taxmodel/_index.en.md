---
title: "Simulation tax model"
date: 2026-09-23T12:00:00+02:00
draft: false
weight: 20
archetype: "default"
aliases: ["/en/admindata/taxdata/simulationmodel/"]
---
A **simulation tax model** describes in YAML which transaction taxes and income withholding GT should estimate during a [historical replay]({{% ref "/algoalert/historicalrun" %}}). It belongs to a tax country and is edited only by users with **administrator** privileges. Personal income tax, capital-gains tax, wealth tax, refunds and treaty credits are outside this model. Existing manually booked or imported transactions remain authoritative.

{{% notice style="warning" title="Simulation approximation only" %}}
Rules do not assess personal taxes, refunds or treaty credits. The supplied country models are never applied automatically. A replay uses the models only when **Apply simulation tax models** is switched on.
{{% /notice %}}

The **Tax data** view is reached in the navigation area under **Administrative data**. On a country the context menu **Simulation tax model...** opens the editor. A country without tax years can still hold a model. The column **Tax model configured** shows whether YAML is stored.

The editor highlights YAML syntax, reports syntax errors, and suggests suitable properties and values. In conditions and calculations it completes the available variables, functions, and event kinds. Hovering over a property shows its meaning.

For the shared controls, see [Editing and validating YAML]({{% ref "/intro/userinterface#yaml-editor" %}}). **Validate** checks the document and its expressions. **Test unsaved model** validates and evaluates the current text without saving it. Only **Save** stores the model; if an error occurs, the text remains in the dialog for correction.

{{< mermaid >}}
graph TD
    A["Country model"] --> B{"Event kind"}
    B -->|"Buy or Sell"| C["Trade taxes"]
    B -->|"Interest/Dividends or Securities interest"| D["Income withholding"]
    C --> E["Period for the event date"]
    D --> E
    E --> F["First matching rule"]
    F --> G["Add country contributions"]
{{< /mermaid >}}

## YAML structure
The model has `version: 1` and at least one of the sections **Trade taxes** (`transactionTaxes`) or **Income withholding** (`incomeWithholding`). Each section contains either `rules` or `periods`, never both. A rule has a name, an optional EvalEx condition and an EvalEx expression for the tax amount. An omitted condition is the unconditional fallback.

**Buy** and **Sell** evaluate the trade-taxes section; **Interest/Dividends** and **Securities interest** evaluate income withholding. Inside a country the first matching rule of the selected period applies. Across countries, successful contributions are added in country-code order. An explicit zero rule documents that no tax applies and counts as complete coverage. A missing section, period or match leaves the estimate incomplete and GT shows a warning.

YAML is limited to 64 KiB of UTF-8 text. **Clear model** stores an empty model. **Validate model** checks schema, periods and expressions without saving.

### Flat rules
```yaml
version: 1
transactionTaxes:
  rules:
    - name: "Purchase tax"
      condition: 'eventKind == "BUY"'
      expression: "cleanValue * 0.001"
    - name: "Other trades"
      expression: "0"
incomeWithholding:
  rules:
    - name: "Dividend withholding"
      condition: 'eventKind == "DIVIDEND"'
      expression: "grossIncome * 0.20"
    - name: "Interest withholding"
      condition: 'eventKind == "SECURITY_INTEREST"'
      expression: "grossIncome * 0.10"
```

### Time-based periods
Each period has an inclusive **validFrom**, an optional inclusive **validTo** and its own rules. The **Date** of the test form, or the fill or payment date of the replay, selects the period. Overlapping ranges are rejected; adjacent dates are allowed.

```yaml
version: 1
transactionTaxes:
  periods:
    - validFrom: "2025-01-01"
      validTo: "2025-12-31"
      rules:
        - name: "Purchase 2025"
          condition: 'eventKind == "BUY"'
          expression: "cleanValue * 0.001"
        - name: "Other trades"
          expression: "0"
```

`requiredInputs` lists optional metadata without which the whole country contribution is omitted, for example `issuerCountry` or `dealerCountry`. Unknown values remain unknown; GT does not infer issuer country, dealer country or tax exemption from ISIN, venue or client country.

The countries and the exemption are entered at the following places:

| Variable | Source in the replay |
|----------|----------------------|
| `issuerCountry` | **Issuer country** of the security |
| `exchangeCountry` | Country of the stock exchange of the security |
| `dealerCountry` | **Dealer country** in the [trading platform plan]({{% ref "/basedata/tradingplatformplan#dealer-country" %}}) of the securities account |
| `exemptInvestor` | **Exempt investor** in the [securities account]({{% ref "/tenantportfolio/securityaccounts" %}}) |

## Event kinds
Conditions use the YAML strings. The user interface translates the same values.

| YAML | Display |
|------|---------|
| `BUY` | **Buy** |
| `SELL` | **Sell** |
| `DIVIDEND` | **Interest/Dividends** |
| `SECURITY_INTEREST` | **Securities interest** |

## Variable reference
The following variables are available in conditions and expressions. All monetary amounts are in instrument currency. **Date** only selects the period and is not itself an EvalEx variable.

| Variable | Type | Meaning |
|----------|------|---------|
| `eventKind` | String | See event kinds above |
| `units` | Numeric | Absolute unit count; **Units** |
| `price` | Numeric | **Price** per unit |
| `cleanValue` | Numeric | **Clean consideration** |
| `accruedInterest` | Numeric | **Accrued interest** on a bond trade; zero for income |
| `tradeValue` | Numeric | `cleanValue + accruedInterest` |
| `grossIncome` | Numeric | **Gross income** before withholding; zero for trades |
| `currency` | String | Instrument **Currency**, ISO code |
| `instrument` | String | Instrument type, e.g. `"DIRECT_INVESTMENT"`, `"ETF"` |
| `assetclass` | String | Asset class, e.g. `"EQUITIES"`, `"FIXED_INCOME"`, `"CONVERTIBLE_BOND"` |
| `mic` | String | Exchange **Market identifier code**, e.g. `"XSWX"` |
| `issuerCountry` | String | **Issuer country**, ISO code |
| `dealerCountry` | String | **Dealer country**, ISO code |
| `exchangeCountry` | String | **Exchange country**, ISO code |
| `exemptInvestor` | Numeric | **Exempt investor**: `1`, `0` or absent |

Amounts inapplicable to the chosen event are bound to zero. Applicable amounts must be finite and nonnegative.

### EvalEx functions
Conditions and expressions support the standard **EvalEx** function library. Commonly used are:
+ `MAX(a, b)` and `MIN(a, b)`
+ `IF(condition, thenValue, elseValue)`
+ `ABS(value)`
+ `ROUND(value, decimals)`
+ Arithmetic: `+`, `-`, `*`, `/`
+ Comparisons: `==`, `!=`, `>`, `<`, `>=`, `<=`
+ String equality: `eventKind == "BUY"`

## Test unsaved model
Below the YAML editor the collapsible panel **Test unsaved model** evaluates the text currently in the editor without saving it.

| Field | Meaning |
|-------|---------|
| **Event kind** | Buy, Sell, Interest/Dividends or Securities interest |
| **Date** | Selects the period |
| **Currency** | Instrument currency |
| **Units** | Unit count |
| **Price** | Price per unit |
| **Clean consideration** | Trade consideration before accrued interest |
| **Accrued interest** | On a bond trade |
| **Gross income** | For Interest/Dividends and Securities interest |
| **Instrument**, **Asset class**, **Market identifier code** | Classification as in the fee model |
| **Issuer country**, **Dealer country**, **Exchange country** | Separate country inputs |
| **Exempt investor** | Unknown, no or yes |

The result shows whether the estimate is complete, the estimated amount, the matched rules and any warnings.

## Seeded country models
GT creates an initial model for large economies plus Switzerland, Austria and Poland when none is stored yet. The rates are current, non-treaty source withholding for an individual investor and apply without a time limit. They do not implement national tax law. An administrator can replace or clear any model.

| Country | Interest/Dividends | Securities interest | Trade taxes |
|---------|-------------------:|--------------------:|-------------|
| United States | 30 % | 30 % | none |
| China | 20 % | 20 % | none |
| Germany | 26.375 % | 0 %; 26.375 % for direct convertible bonds | none |
| Japan | 15.315 % | 15.315 % | none |
| India | 20 % | 20 % | none |
| United Kingdom | 0 % | 20 % | 0.5 % of `cleanValue` on purchases of direct UK equities |
| France | 12.8 % | 0 % | none |
| Italy | 26 % | 26 % | none |
| Canada | 25 % | 0 % for ordinary arm's-length portfolio debt | none |
| Brazil | 10 % | 15 % | none |
| Switzerland | 35 % | 35 % | with a Swiss dealer and a non-exempt investor, 0.075 % of `cleanValue` for Swiss securities and 0.15 % for foreign securities |
| Austria | 27.5 % | 27.5 % | none |
| Poland | 19 % | 20 % | none for exchange-traded securities |

{{% notice note %}}
Swiss transfer stamp tax and anticipatory tax are modelled separately. UK SDRT and the French financial-transaction tax are only coarse approximations in the seeded rules.
{{% /notice %}}

## JSON schema
The YAML must match the following JSON schema. It can also be used as context for an LLM to generate valid model YAML.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Simulation tax model",
  "description": "Country tax-estimation rules for trades and securities income. Rules are evaluated from top to bottom and the first matching rule contributes to the estimate.",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "version"
  ],
  "anyOf": [
    {
      "required": [
        "transactionTaxes"
      ]
    },
    {
      "required": [
        "incomeWithholding"
      ]
    }
  ],
  "properties": {
    "version": {
      "const": 1,
      "description": "Tax model format version. Version 1 is currently supported."
    },
    "transactionTaxes": {
      "$ref": "#/definitions/section",
      "description": "Taxes charged on BUY and SELL events."
    },
    "incomeWithholding": {
      "$ref": "#/definitions/section",
      "description": "Withholding deducted from DIVIDEND and SECURITY_INTEREST events."
    }
  },
  "definitions": {
    "rule": {
      "description": "One named tax rule. The first rule whose optional condition is true supplies the country contribution.",
      "type": "object",
      "additionalProperties": false,
      "required": [
        "name",
        "expression"
      ],
      "properties": {
        "name": {
          "type": "string",
          "minLength": 1,
          "description": "Human-readable rule name shown in estimate details."
        },
        "condition": {
          "type": "string",
          "minLength": 1,
          "description": "Optional EvalEx boolean expression. Omit it for an unconditional fallback rule."
        },
        "expression": {
          "type": "string",
          "minLength": 1,
          "description": "EvalEx numeric expression returning a finite, nonnegative tax amount in instrument currency."
        }
      }
    },
    "section": {
      "description": "A tax section with either undated rules or dated periods, never both.",
      "type": "object",
      "additionalProperties": false,
      "oneOf": [
        {
          "required": [
            "rules"
          ],
          "not": {
            "required": [
              "periods"
            ]
          }
        },
        {
          "required": [
            "periods"
          ],
          "not": {
            "required": [
              "rules"
            ]
          }
        }
      ],
      "properties": {
        "requiredInputs": {
          "type": "array",
          "uniqueItems": true,
          "description": "Optional metadata that must be known before this country's section can contribute.",
          "items": {
            "enum": [
              "instrument",
              "assetclass",
              "mic",
              "issuerCountry",
              "dealerCountry",
              "exchangeCountry",
              "exemptInvestor"
            ]
          }
        },
        "rules": {
          "type": "array",
          "minItems": 1,
          "description": "Undated rules evaluated in document order.",
          "items": {
            "$ref": "#/definitions/rule"
          }
        },
        "periods": {
          "type": "array",
          "minItems": 1,
          "description": "Non-overlapping date ranges. The event date selects one period before its rules are evaluated.",
          "items": {
            "type": "object",
            "description": "An inclusive validity period with its own ordered rules.",
            "additionalProperties": false,
            "required": [
              "validFrom",
              "rules"
            ],
            "properties": {
              "validFrom": {
                "type": "string",
                "pattern": "^[0-9]{4}-[0-9]{2}-[0-9]{2}$",
                "description": "First event date covered by the period, inclusive, in YYYY-MM-DD format."
              },
              "validTo": {
                "type": "string",
                "pattern": "^[0-9]{4}-[0-9]{2}-[0-9]{2}$",
                "description": "Last event date covered by the period, inclusive, in YYYY-MM-DD format. Omit for an open-ended period."
              },
              "rules": {
                "type": "array",
                "minItems": 1,
                "description": "Rules evaluated in document order for this period.",
                "items": {
                  "$ref": "#/definitions/rule"
                }
              }
            }
          }
        }
      }
    }
  }
}
```
