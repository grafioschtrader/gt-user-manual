---
title: "Trading platform plan"
date: 2026-09-24T12:00:00+02:00
draft: false
weight: 15
archetype: "default"
---
A **securities account** has a **trading platform plan** for the following reasons:
+ It controls the correct assignment of **import templates** to a depot.
+ A **fee model** can be defined to estimate transaction costs for hypothetical trades. This allows GT to deliver **realistic results** when simulating the **hypothetical closing out** of positions.
+ It supplies the **dealer country** for the tax estimate of the [historical replay]({{% ref "/algoalert/historicalrun" %}}).

The relationships are shown in the following entity relationship diagram:
{{< mermaid >}}
erDiagram
  "Securities account" }o--|| "Trading platform plan" : has
  "Trading platform plan" }o--o| "Template group" : has
  "Template group" ||--o{ "Import template" : has
{{< /mermaid >}}

## Dealer country
The **dealer country** is the country of the securities dealer through which the securities accounts of this plan trade. It is not the investor's residence: an investor living in Switzerland who trades with a foreign broker selects the broker's country. The field is only shown when rule-based trading is enabled on this GT instance. It has an effect only in a historical replay with **Apply simulation tax models** switched on; real transactions are not affected.

In reality, the Swiss transfer stamp duty is only due when a Swiss securities dealer takes part in the trade. The supplied Swiss [simulation tax model]({{% ref "/algoalert/historicalrun/taxmodel" %}}) reflects this: when the dealer country is Switzerland, the replay charges the client share of the duty on every buy and sell, namely 0.075 % for Swiss and 0.15 % for foreign securities. With a foreign dealer there is no stamp duty. Withholding taxes on dividends and interest, on the other hand, depend on the issuer country and not on the dealer; a foreign broker does not change them.

With a Swiss dealer, **Exempt investor** of the securities account must also be set to no or yes, otherwise the model cannot calculate the duty. If the dealer country is left empty, the replay estimates no stamp duty and marks every trade as incompletely estimated. The investor's residence is not part of the estimate. The exemption of foreign bonds for a counterparty resident abroad is therefore missing.

## Fee model
The fee model lets you define **transaction cost rules** as YAML on a trading platform plan. Each rule consists of a **condition** and a **fee expression**, both written using **EvalEx expressions**. The fee model is edited in a **separate dialog** opened via the context menu of the trading platform plan table: right-click on a trading platform plan → **Edit fee model...**. The dialog contains a **Monaco editor** with schema-assisted autocompletion and hover documentation for the YAML.

See [Editing and validating YAML]({{% ref "/intro/userinterface#yaml-editor" %}}) for the shared controls. **Validate** checks the document and its expressions without saving it. For custody fees, suggestions match the calculation field: a fee amount uses different variables from individual position valuations or the selection of eligible trades.

{{% notice style="info" title="Per-account override" %}}
Individual [securities accounts]({{% ref "/tenantportfolio/securityaccounts" %}}) can define their own fee model YAML that overrides this plan-level model. The same fee model editor can also be opened from the securities account's context menu in the navigation tree → **Edit fee model...**. When a securities account has its own fee model, it takes priority over this trading platform plan's model. Commissions and custody are replaced together, while the [currency conversion markups](#currency-conversion-markups) are inherited separately.
{{% /notice %}}

[Historical replay]({{% ref "/algoalert/historicalrun/fees" %}}) applies the configured trading commissions, custody models and currency conversion markups. The commission test below evaluates one trade only.

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
A fee model YAML must contain either `rules` or `periods` at the top level, never both. A trading platform plan always needs one of them. Only a securities account's own document may consist of an `fx` section alone; it then keeps the plan's commissions and custody.
{{% /notice %}}

### Trade allowances and currency conversion
Some brokers waive the commission for a limited number of trades, for example the first trade of each quarter or the first order per ETF and month. Rules can refer to the number of **earlier trades** of the same securities account: `tradesInMonth`, `tradesInQuarter` and `tradesInYear` count the earlier buy and sell trades within the calendar month, quarter or year of the trade, and `securityTradesInMonth` counts only those of the same security in the calendar month. The trade being priced is never included. The counting is limited to one securities account; an allowance a broker grants across several accounts of the same banking relationship cannot be modelled.

A currency conversion fee is usually only charged when the trade is settled from a cash account in another currency. `settlementCurrency` contains the currency of the cash account the trade settles in, so it can be compared with the instrument currency `currency`.

```yaml
rules:
  - name: "One free trade per quarter up to 5000"
    condition: "tradesInQuarter == 0 && tradeValue <= 5000"
    expression: "0"
  - name: "First order per ETF and month from 1000"
    condition: 'instrument == "ETF" && securityTradesInMonth == 0 && tradeValue >= 1000'
    expression: "0"
  - name: "Commission plus conversion fee"
    condition: "true"
    expression: "tradeValue * (0.005 + IF(settlementCurrency != currency, 0.0095, 0))"
```

The fee model comparison and the historical simulation determine these values from the booked transactions of the securities account. In the simulation, the trades before the simulation start in the same year also count, and only trades that are actually booked use up an allowance.

A conversion fee in a commission rule is an amount in the instrument currency. A percentage margin in the exchange rate belongs in the separate [currency conversion markups](#currency-conversion-markups) instead. Do not model the same charge in both places: if a commission rule already charges the conversion through `settlementCurrency`, add an explicit zero FX tariff for these conversions.

### Custody fees and trading credits
A fee model can also describe recurring custody charges. Add the top-level `custody` section alongside the existing commission `rules` or `periods`; do not replace the commission rules with the example below. Custody periods have their own dates because a broker can change its custody tariff without changing its trading commissions. Both dates are inclusive; omit `validTo` for an open-ended period. Overlapping custody periods are rejected.

The historical simulation uses the model belonging to each securities account. An account-specific document with commission rules replaces the commissions and custody fees of the platform plan together. Copy both sections when creating an override. The `fx` section is not part of this: without its own `fx` section, the account keeps the plan's currency conversion markups. Saving a model does not create charges in a real account.

#### Verified terms and explicit assumptions
Use `VERIFIED` for supported contractual terms, `ASSUMED` for a deliberately chosen simulation convention, and `UNRESOLVED` for incomplete terms. Always supply `source`; assumed and unresolved periods also require an explanatory `note`. An unresolved period, a missing historical period or unavailable valuation data stops the simulation. A missing custody section means **unmodelled custody**, not confirmed free custody. To model free custody, use a dated, executable period with `amount: "0"` and no minimum.

The supplied broker templates distinguish today's published terms from historical evidence. Saxo's retail custody exemption starts on 1 February 2025; it must not be applied retrospectively to Strateo. PostFinance's supplied executable tariff starts on 1 January 2026. Older charges alone do not establish all earlier contractual conditions. Migros's June 2025 brochure specified annual billing; the current brochure specifies quarterly billing with monthly calculation. Its exact observation date needs confirmation. Swissquote and Cornèrtrader templates retain unresolved details instead of silently selecting valuation or billing dates. Migros pension-fund exemptions require checking which investments qualify.

#### Advance charge with expiring trading credits
This example assumes eligible online securities trades only. Offline-only instruments must be excluded explicitly, for example by ISIN in the eligibility expression. The quarterly CHF 18 debit remains a custody charge. The CHF 18 credit reduces later eligible commissions in that quarter and never pays transaction taxes. Unused credits expire; they are not cash and do not increase portfolio value. Corporate-action settlements do not use these credits.

```yaml
custody:
  periods:
    - validFrom: "2026-01-01"
      status: ASSUMED
      source: "https://www.postfinance.ch/content/dam/pfch/doc/480_499/482_22_en.pdf"
      note: "This account uses eligible online securities trades only."
      currency: CHF
      valuation: NONE
      billingMonths: 3
      collection: START
      billingDay: 0
      dayCount: ACTUAL_ACTUAL
      amount: "18"
      creditAmount: 18
      creditEligibility: 'instrument != "FOREX" && instrument != "CFD"'
```

#### Valuation and billing are separate
`valuation` selects no asset valuation (`NONE`), daily closing values (`DAILY`), one monthly observation (`MONTHLY`), or a billing-period-end observation (`PERIOD_END`). For a monthly schedule, `observationDay` is required: 0 means month end, otherwise use a day from 1 to 31; a shorter month uses its last day. Values include securities in that account, converted into the fee currency, and exclude cash. Only historical quotes on or before the observation date are used.

`amount` is the fee per bill for `NONE`. For the other modes it is an **annual** fee expression: monthly observations contribute one twelfth, period-end observations contribute the corresponding fraction of a year, and daily observations use `dayCount`. Supported day counts are actual days divided by 365, 360 or the actual number of days in the year. `assetValue` is the chargeable value; `accountValue` is the value of all securities in the account. Optional first-matching `valueRules` calculate each chargeable position value from `positionValue`, instrument category, asset class, ISIN, currency and stock exchange (`mic`), allowing exemptions or discounts.

`billingMonths` is 1, 3, 6 or 12; the billing periods always follow the calendar year, so a semi-annual schedule is billed at the end of June and December. `collection` is `START` or `END`; advance collection requires `NONE`. `billingDay: 0` uses the calendar boundary. A different day selects that day in the first or last month of the billing period. This is an explicit convention and must agree with the broker's terms. Trading credits require advance collection at the calendar boundary. A charged tariff change inside a billing cycle must be represented by an explicit account model using suitable billing periods.

`minimum` and `maximum` are amounts **per bill**; `annualCap` caps the calendar year's billed amount before VAT. `vatRate` is a fraction, for example 0.081. Never copy an annual minimum directly into a quarterly minimum. The simulation retains accrued unpaid fees as a liability. Settlement reduces cash and clears that liability, so performance is not charged twice. A simulation ending before the next invoice still includes the accrued liability. Closing an account settles accrued fees for an end-billed model; advance fees are not refunded.

A calendar-boundary simulation opening can start with zero accruals. Otherwise, enter the opening state described under [Fees in the replay]({{% ref "/algoalert/historicalrun/fees" %}}). The simulation freezes models and opening balances at submission. The audit trail shows billed fees, VAT, source period and credits used. Accounts without a custody section remain explicitly unmodelled. The **Test fee estimation** panel estimates **commissions only**; it does not run a custody schedule or consume credits. Currency conversion markups have their own **FX conversion** panel.

#### Fees per stock exchange
Some brokers charge a yearly fee for every stock exchange on which positions are held or trades are made. In the `amount` expression of a valued schedule, `exchangeCount` contains the number of different stock exchanges of the billing period: exchanges of positions carried into the period, of positions held at an observation, and of trades booked during the period. `positionCount` contains the number of positions of the observation. The optional `exchangeCondition` decides which exchanges are counted, for example to exclude exchanges for which the broker charges nothing; it can use `mic`, instrument category, asset class, ISIN and currency. Without it, every exchange counts.

```yaml
custody:
  periods:
    - validFrom: "2026-01-01"
      status: ASSUMED
      source: "Broker fee schedule"
      note: "Fee per exchange and calendar year, at most 0.25 percent of the account value."
      currency: EUR
      valuation: PERIOD_END
      billingMonths: 12
      collection: END
      billingDay: 0
      dayCount: ACTUAL_ACTUAL
      amount: "MIN(2.5 * exchangeCount, 0.0025 * accountValue)"
      exchangeCondition: 'mic != "XAMS"'
```

The simulation charges this fee at the end of the billing period, not at the first trade on an exchange. A cap relative to the account value uses the value at the observation, not the highest value of the year.

### Currency conversion markups
Many brokers do not charge a currency conversion as a separate fee but convert at a rate that is somewhat worse than the market rate. The top-level `fx` section describes this margin as a **percentage markup** on the end-of-day mid rate of the currency pair. Whoever buys the pair's first currency pays the markup on top; whoever sells it receives correspondingly less. Add the `fx` section alongside the commission rules; a securities account's own document may also contain the `fx` section alone.

The markup is used in three places: the [historical replay]({{% ref "/algoalert/historicalrun/fees#currency-conversion-markups" %}}) converts with it, the [observed FX deviation]({{% ref "/reportportfolio/transactioncosts#observed-fx-deviation-from-the-eod-close" %}}) in the Transaction Costs report compares it with the recorded exchange rates, and the **FX conversion** test panel below calculates it for a single conversion. Real transactions are never changed by it.

Like the commissions, the `fx` section contains either flat `rules` or dated `periods`, never both. Every period needs `validFrom`, `status` and `source`; `validTo` is optional and inclusive. `status` is `VERIFIED` for a documented tariff or `ASSUMED` for a deliberately chosen assumption, which also requires an explanatory `note`. Periods must not overlap. Within the valid period, the first rule whose condition is true determines the markup. Its expression returns the markup **in percent**: `1.5` means 1.5 %. Only values from zero up to, but excluding, 5 % are allowed, and a larger or negative result counts as an invalid tariff. Fixed conversion commissions and minimum charges cannot be expressed with this percentage model.

The optional `currencyClasses` group currencies under names of your choice, for example `MAJOR` or `EMERGING`. Each currency may belong to one class only. A currency that belongs to no class has an empty class. The optional `amountCurrency` is the currency in which amount tiers are graded; it is required as soon as a rule uses `tierAmount`.

| Variable | Type | Description |
|----------|------|-------------|
| `payCurrency` | String | ISO code of the currency given up |
| `receiveCurrency` | String | ISO code of the currency received |
| `kind` | String | `"TRADE"` for a buy or sell, `"INCOME"` for a dividend or interest payment, `"TRANSFER"` for a cash transfer |
| `amount` | Numeric | Amount given up in the pay currency, valued at mid before the markup; a purchase includes commission, taxes and accrued interest, a sale or income payment uses the net proceeds |
| `tierAmount` | Numeric | `amount` converted into `amountCurrency` |
| `payClass` / `receiveClass` | String | Class of the pay or receive currency from `currencyClasses` |
| `mic` | String | MIC code of the security's stock exchange; empty for a cash transfer |

Example with currency classes:
```yaml
fx:
  currencyClasses:
    MAJOR: [USD, EUR, JPY, GBP, CHF, AUD, CAD, NZD]
    MINOR: [NOK, SEK, DKK, SGD, HUF, PLN, CZK, CNH]
  periods:
    - validFrom: "1900-01-01"
      status: ASSUMED
      source: "Currency exchange page of the broker"
      note: "Class matrix observed 2026-09, applied to the whole history."
      rules:
        - name: "Major/Major"
          condition: "payClass == 'MAJOR' && receiveClass == 'MAJOR'"
          expression: "0.95"
        - name: "All other conversions"
          condition: "true"
          expression: "1.5"
```
Example with amount tiers in Swiss francs, independent of the currencies actually converted:
```yaml
fx:
  amountCurrency: CHF
  rules:
    - name: "Up to CHF 50,000"
      condition: "tierAmount <= 50000"
      expression: "1.5"
    - name: "Above CHF 50,000"
      condition: "true"
      expression: "1.0"
```
An explicit zero-percent rule means that a conversion is free. That is different from a conversion no rule covers. GT therefore distinguishes the following cases: **No FX tariff configured**, **No FX tariff covers this date**, **No FX rule covers this conversion**, **Historical rate for the tariff currency is missing** (a tier in a third currency needs its closing rate on the day) and **Invalid FX tariff or request**. An invalid tariff is never treated as zero markup.

The supplied trading platform plans for Swissquote, Migros Bank, PostFinance and Saxo contain an FX tariff, marked as an assumption with its source. It was only added where the plan's fee model was still unchanged; a plan you have edited yourself keeps its document.

#### FX conversion test panel
Below **Test fee estimation**, the fee model dialog contains the collapsible **FX conversion** panel. It calculates the markup of a single conversion from the current, unsaved YAML text. Enter **Pay currency**, **Receive currency**, **Conversion kind** (Trade, Income or Cash transfer), **Amount** in the pay currency, **Date** and optionally the **Market identifier code**, then click **Test FX markup**. The result shows which of the cases above applies. For a matching rule it also shows **Markup (%)**, **Valid from** of the period, **Matched rule** and **Status** (Verified or Assumed). In the dialog of a securities account the test uses the unsaved account document together with the plan's FX tariff, exactly as it would be inherited. Only **Save** stores the document.

### JSON Schema
The YAML must conform to the following JSON Schema. It can also be used as context for an LLM to generate valid fee model YAML.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Fee Model Configuration",
  "description": "Trading commissions with optional independently dated custody charges, expiring trading credits and percentage FX conversion markups.",
  "type": "object",
  "oneOf": [
    {
      "required": [
        "rules"
      ],
      "properties": {
        "rules": {
          "type": "array",
          "description": "Fee rules (no date restriction). Evaluated top-to-bottom; first matching condition wins.",
          "minItems": 1,
          "items": {
            "$ref": "#/$defs/FeeRule"
          }
        },
        "custody": {
          "$ref": "#/$defs/Custody"
        },
        "fx": {
          "$ref": "#/$defs/Fx"
        }
      },
      "additionalProperties": false
    },
    {
      "required": [
        "periods"
      ],
      "properties": {
        "periods": {
          "type": "array",
          "description": "Time-based fee periods. Each period has a date range and its own rules array. First period matching the transaction date is used.",
          "minItems": 1,
          "items": {
            "$ref": "#/$defs/FeeModelPeriod"
          }
        },
        "custody": {
          "$ref": "#/$defs/Custody"
        },
        "fx": {
          "$ref": "#/$defs/Fx"
        }
      },
      "additionalProperties": false
    },
    {
      "required": [
        "fx"
      ],
      "properties": {
        "fx": {
          "$ref": "#/$defs/Fx"
        }
      },
      "additionalProperties": false
    }
  ],
  "$defs": {
    "FeeRule": {
      "type": "object",
      "required": [
        "name",
        "condition",
        "expression"
      ],
      "properties": {
        "name": {
          "type": "string",
          "description": "Human-readable rule name (e.g., 'Swiss stocks - Premium tier')"
        },
        "condition": {
          "type": "string",
          "description": "EvalEx boolean expression. Variables: tradeValue (trade amount), units (share count), instrument (string: DIRECT_INVESTMENT, ETF, MUTUAL_FUND, PENSION_FUNDS, CFD, FOREX, ISSUER_RISK_PRODUCT, NON_INVESTABLE_INDICES), assetclass (string: EQUITIES, FIXED_INCOME, MONEY_MARKET, COMMODITIES, REAL_ESTATE, MULTI_ASSET, CONVERTIBLE_BOND, CREDIT_DERIVATIVE, CURRENCY_PAIR), mic (MIC code, e.g. XSWX, XNYS), currency (ISO code), fixedAssets (portfolio value), tradeDirection (0=buy, 1=sell), settlementCurrency (ISO code of the settling cash account), tradesInMonth / tradesInQuarter / tradesInYear (earlier trades of the account in the calendar period), securityTradesInMonth (earlier trades of this security in the calendar month). Legacy numeric aliases: specInvestInstrument, categoryType. Use 'true' for a catch-all default rule."
        },
        "expression": {
          "type": "string",
          "description": "EvalEx numeric expression for fee calculation. Supports MAX(), MIN(), IF(), ABS(), ROUND(), arithmetic, comparisons. Example: 'MAX(9.0, tradeValue * 0.001)'"
        }
      }
    },
    "FeeModelPeriod": {
      "type": "object",
      "required": [
        "validFrom",
        "rules"
      ],
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
          "items": {
            "$ref": "#/$defs/FeeRule"
          }
        }
      }
    },
    "CustodyPeriod": {
      "type": "object",
      "required": [
        "validFrom",
        "status",
        "source"
      ],
      "properties": {
        "validFrom": {
          "type": "string",
          "format": "date"
        },
        "validTo": {
          "type": "string",
          "format": "date"
        },
        "status": {
          "enum": [
            "VERIFIED",
            "ASSUMED",
            "UNRESOLVED"
          ]
        },
        "source": {
          "type": "string",
          "minLength": 1
        },
        "note": {
          "type": "string"
        },
        "currency": {
          "type": "string",
          "pattern": "^[A-Z]{3}$"
        },
        "valuation": {
          "enum": [
            "NONE",
            "DAILY",
            "MONTHLY",
            "PERIOD_END"
          ]
        },
        "observationDay": {
          "type": "integer",
          "minimum": 0,
          "maximum": 31,
          "description": "Monthly valuation day; 0 means month end."
        },
        "billingMonths": {
          "enum": [
            1,
            3,
            6,
            12
          ],
          "description": "Billing period length in months, aligned to the calendar year."
        },
        "collection": {
          "enum": [
            "START",
            "END"
          ]
        },
        "billingDay": {
          "type": "integer",
          "minimum": 0,
          "maximum": 31
        },
        "dayCount": {
          "enum": [
            "ACTUAL_365",
            "ACTUAL_360",
            "ACTUAL_ACTUAL"
          ]
        },
        "closingPolicy": {
          "enum": [
            "ACCRUED",
            "FULL_PERIOD",
            "PRORATED"
          ],
          "description": "Required for an arrears account closing mid-period. ACCRUED keeps daily/monthly observations; FULL_PERIOD assesses fixed/period-end charges in full; PRORATED scales fixed/period-end fees and bill limits by elapsed calendar days."
        },
        "amount": {
          "type": "string",
          "description": "EvalEx annual fee, or amount per bill for NONE. Variables: assetValue (weighted custody value), accountValue (all securities), exchangeCount (distinct exchanges held or traded in the billing period, filtered by exchangeCondition), positionCount (observed positions). No cash. Example per-exchange fee: 'MIN(2.5 * exchangeCount, 0.0025 * accountValue)'."
        },
        "valueRules": {
          "type": "array",
          "minItems": 1,
          "items": {
            "$ref": "#/$defs/FeeRule"
          },
          "description": "First match returns chargeable position value. Variables: positionValue, accountValue, instrument, assetclass, isin, currency, mic."
        },
        "exchangeCondition": {
          "type": "string",
          "description": "EvalEx boolean deciding which exchanges exchangeCount counts. Variables: mic, instrument, assetclass, isin, currency. Omitted means every exchange counts. Example: 'mic != \"XETR\" && mic != \"XAMS\"'."
        },
        "creditEligibility": {
          "type": "string",
          "description": "Eligible online commission: instrument, assetclass, isin, currency, mic, tradeDirection. Excludes taxes and corporate actions."
        },
        "minimum": {
          "type": "number",
          "minimum": 0
        },
        "maximum": {
          "type": "number",
          "minimum": 0
        },
        "annualCap": {
          "type": "number",
          "minimum": 0
        },
        "vatRate": {
          "type": "number",
          "minimum": 0
        },
        "creditAmount": {
          "type": "number",
          "minimum": 0
        }
      },
      "additionalProperties": false,
      "allOf": [
        {
          "if": {
            "properties": {
              "status": {
                "enum": [
                  "VERIFIED",
                  "ASSUMED"
                ]
              }
            }
          },
          "then": {
            "required": [
              "currency",
              "valuation",
              "billingMonths",
              "collection",
              "billingDay",
              "dayCount",
              "amount"
            ]
          }
        }
      ]
    },
    "Custody": {
      "type": "object",
      "required": [
        "periods"
      ],
      "properties": {
        "periods": {
          "type": "array",
          "minItems": 1,
          "maxItems": 100,
          "items": {
            "$ref": "#/$defs/CustodyPeriod"
          }
        }
      },
      "additionalProperties": false
    },
    "Fx": {
      "type": "object",
      "properties": {
        "amountCurrency": {
          "type": "string",
          "pattern": "^[A-Z]{3}$"
        },
        "currencyClasses": {
          "type": "object",
          "additionalProperties": {
            "type": "array",
            "items": {
              "type": "string",
              "pattern": "^[A-Z]{3}$"
            }
          }
        },
        "rules": {
          "type": "array",
          "minItems": 1,
          "items": {
            "$ref": "#/$defs/FeeRule"
          }
        },
        "periods": {
          "type": "array",
          "minItems": 1,
          "items": {
            "$ref": "#/$defs/FxPeriod"
          }
        }
      },
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
      "additionalProperties": false
    },
    "FxPeriod": {
      "type": "object",
      "required": [
        "validFrom",
        "status",
        "source",
        "rules"
      ],
      "properties": {
        "validFrom": {
          "type": "string",
          "format": "date"
        },
        "validTo": {
          "type": "string",
          "format": "date"
        },
        "status": {
          "enum": [
            "VERIFIED",
            "ASSUMED"
          ]
        },
        "source": {
          "type": "string",
          "minLength": 1
        },
        "note": {
          "type": "string"
        },
        "rules": {
          "type": "array",
          "minItems": 1,
          "items": {
            "$ref": "#/$defs/FeeRule"
          }
        }
      },
      "additionalProperties": false
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
| `settlementCurrency` | String | ISO currency code of the cash account the trade settles in; empty when unknown |
| `tradesInMonth` | Numeric | Earlier trades of the securities account in the calendar month of the trade |
| `tradesInQuarter` | Numeric | Earlier trades of the securities account in the calendar quarter of the trade |
| `tradesInYear` | Numeric | Earlier trades of the securities account in the calendar year of the trade |
| `securityTradesInMonth` | Numeric | Earlier trades of the same security in the calendar month of the trade |
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
Inside the fee model dialog, below the YAML editor, there is a collapsible **Test fee estimation** panel. You can use it to verify that your rules produce the expected costs. The test parameters use dropdown selection fields wherever possible, so you can conveniently pick a stock exchange or currency from a list.

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
| **Settlement currency** | Currency of the settling cash account as dropdown; leave empty if the model does not use it |
| **Earlier trades in the month** | Number of earlier trades in the calendar month; empty counts as 0 |
| **Earlier trades in the quarter** | Number of earlier trades in the calendar quarter; empty counts as 0 |
| **Earlier trades in the year** | Number of earlier trades in the calendar year; empty counts as 0 |
| **Earlier trades of this security in the month** | Number of earlier trades of the same security in the calendar month; empty counts as 0 |
| **Transaction date** | Date picker — relevant for the periods format |

1. Fill in the desired test parameters.
2. Click **Test fee estimation**. GT validates the current YAML text and uses it to calculate the estimate without saving the model. This applies to both trading platform plans and account-specific models.
3. The result shows the **estimated cost** and the **name of the matched rule**, or an error message if no rule matched.
4. Only **Save** stores your changes permanently, whether or not you tested an estimate first. Closing the dialog without saving leaves the previously saved model in place.

{{% notice style="info" title="Comparison with actual transaction costs" %}}
While the test panel above validates single hypothetical transactions, the [Fee model comparison]({{% ref "/reportportfolio/transactioncosts" %}}) in the Transaction Costs report compares the fee model's estimates against actual recorded costs for all buy/sell transactions of a securities account. It provides statistical metrics (mean absolute error, mean relative error, RMSE) to evaluate the overall accuracy of the fee model. The comparison walks the transactions in booking order, counts the earlier trades for the allowance variables and takes the settlement currency from each transaction's cash account.
{{% /notice %}}
