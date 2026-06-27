---
title: "Handelsplattform Plan"
date: 2026-02-18T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
Ein **Depot** hat einen **Handelsplattform Plan** aus folgenden Gründen:
+ Er steuert die korrekte Zuordnung der **Importvorlagen** zu einem Depot.
+ Es kann ein **Gebührenmodell** definiert werden, um Transaktionskosten für hypothetische Trades abzuschätzen. So kann GT bei der Simulation der **hypothetischen Glattstellung** von Positionen **realistische Ergebnisse** liefern.

Im folgenden Entity-Relationship-Diagramm sind die Beziehungen dargestellt:

{{< mermaid >}}
erDiagram
    Depot }o--|| Handelsplattform-Plan : hat
    Handelsplattform-Plan }o--o| Vorlagengruppe : hat
    Vorlagengruppe ||--o{ Importvorlage :  hat
{{< /mermaid >}}

## Gebührenmodell
Das Gebührenmodell ermöglicht es, **Transaktionskostenregeln** als YAML auf einem Handelsplattform Plan zu definieren. Jede Regel besteht aus einer **Bedingung** und einem **Gebührenausdruck**, beide in **EvalEx-Ausdrücken** formuliert. Das Gebührenmodell wird in einem **eigenen Dialog** bearbeitet, der über das Kontextmenü der Handelsplattform-Plan-Tabelle geöffnet wird: Rechtsklick auf einen Handelsplattform Plan → **Gebührenmodell bearbeiten...**. Der Dialog enthält einen **Monaco-Editor** mit schemabasierter Autovervollständigung und Hover-Dokumentation für das YAML.

{{% notice style="info" title="Depotspezifische Überschreibung" %}}
Einzelne [Depots]({{% ref "/tenantportfolio/securityaccounts" %}}) können ein eigenes Gebührenmodell-YAML definieren, das dieses Plan-Modell überschreibt. Der gleiche Gebührenmodell-Editor kann auch über das Kontextmenü des Depots im Navigationsbaum geöffnet werden → **Gebührenmodell bearbeiten...**. Wenn ein Depot ein eigenes Gebührenmodell hat, hat dieses Vorrang vor dem Modell dieses Handelsplattform Plans.
{{% /notice %}}

{{% notice style="warning" title="Aktueller Stand" %}}
Die Simulationsumgebung dient vorerst ausschliesslich zu Simulationszwecken. Sie wird derzeit noch nicht für die Berechnung offener Positionen verwendet — hypothetische Transaktionen berücksichtigen die Transaktionskosten also noch nicht.
{{% /notice %}}

Regeln werden **von oben nach unten** ausgewertet — die erste Regel, deren Bedingung `true` ergibt, bestimmt die Transaktionskosten. Verwenden Sie `"true"` als Bedingung für eine Auffangregel.

Es gibt zwei sich gegenseitig ausschliessende YAML-Formate auf der obersten Ebene:

### Flache Regeln (ohne Datumseinschränkung)
Verwenden Sie das `rules`-Array auf oberster Ebene, wenn sich das Gebührenmodell zeitlich nicht ändert. Beispiel für einen Schweizer Broker:

```yaml
rules:
  - name: "Schweizer ETF Pauschale"
    condition: 'instrument == "ETF" && mic == "XSWX"'
    expression: "9.0"
  - name: "US-Aktien"
    condition: 'assetclass == "EQUITIES" && currency == "USD"'
    expression: "MAX(15.0, tradeValue * 0.0025)"
  - name: "Standard"
    condition: "true"
    expression: "MAX(20.0, tradeValue * 0.003)"
```

### Zeitbasierte Perioden mit verschachtelten Regeln
Verwenden Sie das `periods`-Array auf oberster Ebene, wenn sich das Gebührenmodell im Laufe der Zeit ändert. Jede Periode hat ein **validFrom**-Datum, ein optionales **validTo**-Datum und ein eigenes `rules`-Array. Die erste Periode, die zum **Transaktionsdatum** passt, wird verwendet. Fehlt `validTo`, gilt die Periode unbefristet.

```yaml
periods:
  - validFrom: "2023-01-01"
    validTo: "2024-12-31"
    rules:
      - name: "Alte Pauschale"
        condition: "true"
        expression: "25.0"
  - validFrom: "2025-01-01"
    rules:
      - name: "Neue ETF-Gebühr"
        condition: 'instrument == "ETF"'
        expression: "5.0"
      - name: "Neue Standardgebühr"
        condition: "true"
        expression: "MAX(10.0, tradeValue * 0.002)"
```

{{% notice style="info" title="Gegenseitig ausschliessend" %}}
Ein Gebührenmodell-YAML muss entweder `rules` oder `periods` auf der obersten Ebene enthalten, nie beides.
{{% /notice %}}

### JSON-Schema
Das YAML muss dem folgenden JSON-Schema entsprechen. Es kann auch als Kontext für ein LLM verwendet werden, um gültiges Gebührenmodell-YAML zu generieren.

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

### Variablenreferenz
Die folgenden Variablen stehen in Bedingungen und Ausdrücken zur Verfügung:

| Variable | Typ | Beschreibung |
|----------|-----|--------------|
| `tradeValue` | Numerisch | Gesamtbetrag des Trades (Preis × Anzahl) |
| `units` | Numerisch | Anzahl der Anteile/Stücke |
| `instrument` | String | Finanzinstrumenttyp, z.B. `"DIRECT_INVESTMENT"`, `"ETF"`, `"MUTUAL_FUND"`, `"PENSION_FUNDS"`, `"CFD"`, `"FOREX"`, `"ISSUER_RISK_PRODUCT"` |
| `assetclass` | String | Anlageklasse, z.B. `"EQUITIES"`, `"FIXED_INCOME"`, `"MONEY_MARKET"`, `"COMMODITIES"`, `"REAL_ESTATE"`, `"MULTI_ASSET"`, `"CONVERTIBLE_BOND"`, `"CREDIT_DERIVATIVE"`, `"CURRENCY_PAIR"` |
| `mic` | String | MIC-Code der Börse, z.B. `"XSWX"`, `"XNYS"` |
| `currency` | String | ISO-Währungscode, z.B. `"CHF"`, `"USD"` |
| `fixedAssets` | Numerisch | Gesamtwert des Portfolios |
| `tradeDirection` | Numerisch | `0` = Kauf, `1` = Verkauf |
| `transactionDate` | Datum | Relevant für das Periodenformat — bestimmt, welche Periode gilt |

Zusätzlich stehen die numerischen Legacy-Variablen `specInvestInstrument` und `categoryType` als ordinale Byte-Werte der Instrument- bzw. Anlageklassen-Enums zur Verfügung.

### EvalEx-Funktionen
Bedingungen und Ausdrücke unterstützen die Standard-**EvalEx**-Funktionsbibliothek. Häufig verwendete Funktionen sind:
+ `MAX(a, b)` und `MIN(a, b)` — gibt den grösseren bzw. kleineren Wert zurück
+ `IF(bedingung, dannWert, sonstWert)` — bedingter Ausdruck
+ `ABS(wert)` — Absolutwert
+ `ROUND(wert, dezimalstellen)` — Rundung
+ Arithmetische Operatoren: `+`, `-`, `*`, `/`
+ Vergleiche: `==`, `!=`, `>`, `<`, `>=`, `<=`
+ String-Gleichheit: `instrument == "ETF"`

### Gebührenschätzung testen
Im Gebührenmodell-Dialog befindet sich unterhalb des YAML-Editors ein einklappbares Panel **Gebührenschätzung testen**. Damit können Sie überprüfen, ob Ihre Regeln die erwarteten Kosten liefern. Die Testparameter verwenden Dropdown-Auswahlfelder, wo immer möglich, so dass Sie z.B. eine Börse oder Währung bequem aus einer Liste wählen können.

| Feld | Beschreibung |
|------|-------------|
| **Handelswert** | Gesamtbetrag des Trades (Preis × Anzahl) |
| **Anzahl Position** | Anzahl der gehandelten Anteile/Stücke |
| **MIC** | Börse als Dropdown (z.B. «Euronext Lisbon - XLIS») |
| **Währung** | Handelswährung als Dropdown (z.B. CHF, USD) |
| **Depotwert** | Gesamtwert des Portfolios/Depots |
| **Finanzinstrument** | Instrumenttyp als Dropdown (z.B. ETF, Direktanlage, Investmentfonds) |
| **Anlageklasse** | Anlageklasse als Dropdown (z.B. Aktien, Obligationen, Rohstoffe) |
| **Handelsrichtung** | Dropdown mit Kaufen / Verkaufen |
| **Transaktionsdatum** | Datumswähler — relevant für das Periodenformat |

1. Füllen Sie die gewünschten Testparameter aus.
2. Klicken Sie auf **Gebührenschätzung testen**. Dabei wird das YAML zunächst automatisch gespeichert und danach die Schätzung gegen das gespeicherte Modell durchgeführt.
3. Das Ergebnis zeigt die **geschätzten Kosten** und den **Namen der zutreffenden Regel** an, oder eine Fehlermeldung, falls keine Regel zutraf.
4. Um das YAML ohne Test zu speichern, verwenden Sie die Schaltfläche **Speichern**.

{{% notice style="info" title="Vergleich mit realen Transaktionskosten" %}}
Während das Testpanel oben einzelne hypothetische Transaktionen validiert, vergleicht der [Gebührenmodellvergleich]({{% ref "/reportportfolio/transactioncosts" %}}) im Transaktionskosten-Report die geschätzten Kosten des Gebührenmodells mit den tatsächlich erfassten Kosten aller Kauf-/Verkaufstransaktionen eines Depots. Er liefert statistische Kennzahlen (mittlerer absoluter Fehler, mittlerer relativer Fehler, RMSE), um die Gesamtgenauigkeit des Gebührenmodells zu bewerten.
{{% /notice %}}
