---
title: "Simulationssteuermodell"
date: 2026-09-23T12:00:00+02:00
draft: false
weight: 20
archetype: "default"
aliases: ["/de/admindata/taxdata/simulationmodel/"]
---
Ein **Simulationssteuermodell** beschreibt in YAML, welche Transaktionssteuern und Ertragssteuerabzüge GT in einer [historischen Wiederholung]({{% ref "/algoalert/historicalrun" %}}) schätzen soll. Es gehört zu einem Steuerland und wird ausschliesslich von Benutzern mit **Administrator**-Benutzerrechten bearbeitet. Persönliche Einkommenssteuer, Kapitalgewinnsteuer, Vermögenssteuer, Rückerstattungen und Abkommensgutschriften sind nicht Gegenstand dieses Modells. Bestehende manuell erfasste oder importierte Transaktionen bleiben massgebend.

{{% notice style="warning" title="Nur eine Simulationsnäherung" %}}
Regeln berechnen keine persönlichen Steuern, Rückerstattungen oder Abkommensgutschriften. Die mitgelieferten Ländermodelle werden nie automatisch angewendet. Damit eine Wiederholung die Modelle verwendet, muss dort **Simulationssteuermodelle anwenden** eingeschaltet sein.
{{% /notice %}}

Die Ansicht **Steuerdaten** erreichen Sie im Navigationsbereich unter **Administrative Daten**. Auf einem Land öffnet das Kontextmenü **Simulationssteuermodell...** den Editor. Ein Land ohne Steuerjahre kann trotzdem ein Modell tragen. Die Spalte **Steuermodell hinterlegt** zeigt, ob YAML gespeichert ist.

Der Editor hebt die YAML-Syntax farblich hervor, meldet Syntaxfehler und schlägt passende Eigenschaften und Werte vor. Bei Bedingungen und Berechnungen ergänzt er die verfügbaren Variablen, Funktionen und Ereignisarten. Wenn Sie den Mauszeiger über eine Eigenschaft halten, erscheint deren Bedeutung.

Zur gemeinsamen Bedienung siehe [YAML bearbeiten und prüfen]({{% ref "/intro/userinterface#yaml-editor" %}}). **Prüfen** kontrolliert das Dokument und seine Ausdrücke. **Ungespeichertes Modell testen** prüft und berechnet den aktuellen Text, ohne ihn zu speichern. Erst **Speichern** übernimmt das Modell; bei einem Fehler bleibt der Text zur Korrektur im Dialog.

{{< mermaid >}}
graph TD
    A["Ländermodell"] --> B{"Ereignisart"}
    B -->|"Kaufen oder Verkaufen"| C["Transaktionssteuern"]
    B -->|"Zins/Dividende oder Wertpapierzinsen"| D["Ertragssteuerabzug"]
    C --> E["Zeitraum zum Ereignisdatum"]
    D --> E
    E --> F["Erste zutreffende Regel"]
    F --> G["Beiträge der Länder addieren"]
{{< /mermaid >}}

## YAML-Aufbau
Das Modell hat `version: 1` und mindestens einen der Abschnitte **Transaktionssteuern** (`transactionTaxes`) oder **Ertragssteuerabzug** (`incomeWithholding`). Jeder Abschnitt enthält entweder `rules` oder `periods`, nie beides. Eine Regel hat einen Namen, einen optionalen EvalEx-Ausdruck als Bedingung und einen EvalEx-Ausdruck für den Steuerbetrag. Fehlt die Bedingung, gilt die Regel als Auffangregel.

Bei **Kaufen** und **Verkaufen** wertet GT den Abschnitt Transaktionssteuern aus, bei **Zins/Dividende** und **Wertpapierzinsen** den Abschnitt Ertragssteuerabzug. Innerhalb eines Landes gilt die erste zutreffende Regel des gewählten Zeitraums. Über mehrere Länder werden erfolgreiche Beiträge in der Reihenfolge der Ländercodes addiert. Eine ausdrückliche Nullregel dokumentiert, dass keine Steuer anfällt, und gilt als vollständige Abdeckung. Fehlt der Abschnitt, der Zeitraum oder eine zutreffende Regel, bleibt die Schätzung unvollständig und GT zeigt einen Hinweis.

Das YAML darf höchstens 64 KiB UTF-8-Text umfassen. **Modell entfernen** speichert ein leeres Modell. **Modell prüfen** prüft Schema, Zeiträume und Ausdrücke, ohne zu speichern.

### Flache Regeln
```yaml
version: 1
transactionTaxes:
  rules:
    - name: "Kaufsteuer"
      condition: 'eventKind == "BUY"'
      expression: "cleanValue * 0.001"
    - name: "Übrige Trades"
      expression: "0"
incomeWithholding:
  rules:
    - name: "Dividendenabzug"
      condition: 'eventKind == "DIVIDEND"'
      expression: "grossIncome * 0.20"
    - name: "Zinsabzug"
      condition: 'eventKind == "SECURITY_INTEREST"'
      expression: "grossIncome * 0.10"
```

### Zeitbasierte Perioden
Jede Periode hat ein einschliessliches **validFrom**, ein optionales einschliessliches **validTo** und eigene Regeln. Das **Datum** des Testformulars bzw. das Ausführungs- oder Zahlungdatum der Wiederholung wählt die Periode. Überlappende Zeiträume werden abgelehnt, aneinandergrenzende sind zulässig.

```yaml
version: 1
transactionTaxes:
  periods:
    - validFrom: "2025-01-01"
      validTo: "2025-12-31"
      rules:
        - name: "Kauf 2025"
          condition: 'eventKind == "BUY"'
          expression: "cleanValue * 0.001"
        - name: "Übrige Trades"
          expression: "0"
```

`requiredInputs` listet optionale Angaben, ohne die der ganze Länderbeitrag entfällt, zum Beispiel `issuerCountry` oder `dealerCountry`. Unbekannte Werte bleiben unbekannt; GT leitet Emittentenland, Händlerland oder Steuerbefreiung nicht aus ISIN, Börse oder Klientenland ab.

Die Länderangaben und die Steuerbefreiung erfasst der Benutzer an folgenden Stellen:

| Variable | Herkunft in der Wiederholung |
|----------|------------------------------|
| `issuerCountry` | **Emittentenland** des Wertpapiers |
| `exchangeCountry` | Land der Börse des Wertpapiers |
| `dealerCountry` | **Händlerland** im [Handelsplattform Plan]({{% ref "/basedata/tradingplatformplan#händlerland" %}}) des Depots |
| `exemptInvestor` | **Steuerbefreiter Anleger** im [Depot]({{% ref "/tenantportfolio/securityaccounts" %}}) |

## Ereignisarten
In Bedingungen verwenden Sie die YAML-Zeichenketten. Die Oberfläche übersetzt dieselben Werte.

| YAML | Anzeige |
|------|---------|
| `BUY` | **Kaufen** |
| `SELL` | **Verkaufen** |
| `DIVIDEND` | **Zins/Dividende** |
| `SECURITY_INTEREST` | **Wertpapierzinsen** |

## Variablenreferenz
Die folgenden Variablen stehen in Bedingungen und Ausdrücken zur Verfügung. Alle Geldbeträge sind in der Instrumentwährung. Das **Datum** wählt nur den Zeitraum und ist selbst keine EvalEx-Variable.

| Variable | Typ | Beschreibung |
|----------|-----|--------------|
| `eventKind` | String | Siehe Ereignisarten oben |
| `units` | Numerisch | Absolute Stückzahl; **Anzahl** |
| `price` | Numerisch | **Kurs** je Einheit |
| `cleanValue` | Numerisch | **Handelswert ohne Marchzinsen** |
| `accruedInterest` | Numerisch | **Aufgelaufener Zins** beim Bondhandel; bei Ertrag null |
| `tradeValue` | Numerisch | `cleanValue + accruedInterest` |
| `grossIncome` | Numerisch | **Bruttoertrag** vor Steuerabzug; bei Trades null |
| `currency` | String | **Währung** des Instruments, ISO-Code |
| `instrument` | String | Finanzinstrument, z.B. `"DIRECT_INVESTMENT"`, `"ETF"` |
| `assetclass` | String | Anlageklasse, z.B. `"EQUITIES"`, `"FIXED_INCOME"`, `"CONVERTIBLE_BOND"` |
| `mic` | String | **MIC** der Börse, z.B. `"XSWX"` |
| `issuerCountry` | String | **Emittentenland**, ISO-Code |
| `dealerCountry` | String | **Händlerland**, ISO-Code |
| `exchangeCountry` | String | **Börsenland**, ISO-Code |
| `exemptInvestor` | Numerisch | **Steuerbefreiter Anleger**: `1`, `0` oder abwesend |

Für das gewählte Ereignis unzutreffende Beträge bindet GT auf null. Anwendbare Beträge müssen endlich und nicht negativ sein.

### EvalEx-Funktionen
Bedingungen und Ausdrücke unterstützen die Standard-**EvalEx**-Funktionsbibliothek. Häufig verwendet werden:
+ `MAX(a, b)` und `MIN(a, b)`
+ `IF(bedingung, dannWert, sonstWert)`
+ `ABS(wert)`
+ `ROUND(wert, dezimalstellen)`
+ Arithmetik: `+`, `-`, `*`, `/`
+ Vergleiche: `==`, `!=`, `>`, `<`, `>=`, `<=`
+ String-Gleichheit: `eventKind == "BUY"`

## Ungespeichertes Modell testen
Unterhalb des YAML-Editors liegt das einklappbare Panel **Ungespeichertes Modell testen**. Es wertet den gerade im Editor stehenden Text aus, ohne ihn zu speichern.

| Feld | Bedeutung |
|------|-----------|
| **Ereignisart** | Kaufen, Verkaufen, Zins/Dividende oder Wertpapierzinsen |
| **Datum** | Wählt den Zeitraum |
| **Währung** | Instrumentwährung |
| **Anzahl** | Stückzahl |
| **Kurs** | Preis je Einheit |
| **Handelswert ohne Marchzinsen** | Reiner Handelswert |
| **Aufgelaufener Zins** | Beim Bondhandel |
| **Bruttoertrag** | Bei Zins/Dividende und Wertpapierzinsen |
| **Instrument**, **Anlageklasse**, **MIC** | Klassifikation wie im Gebührenmodell |
| **Emittentenland**, **Händlerland**, **Börsenland** | Getrennte Länderangaben |
| **Steuerbefreiter Anleger** | Unbekannt, nein oder ja |

Das Ergebnis zeigt, ob die Schätzung vollständig ist, den geschätzten Betrag, die zutreffenden Regeln und allfällige Hinweise.

## Mitgelieferte Ländermodelle
GT legt für grosse Volkswirtschaften und die Schweiz, Österreich und Polen ein erstes Modell an, sofern noch keines gespeichert ist. Die Sätze sind aktuelle Quellensteuer ohne Abkommen für eine natürliche Person und gelten zeitlos. Sie ersetzen kein nationales Steuerrecht. Ein Administrator kann jedes Modell ersetzen oder entfernen.

| Land | Zins/Dividende | Wertpapierzinsen | Transaktionssteuern |
|------|---------------:|-----------------:|---------------------|
| USA | 30 % | 30 % | keine |
| China | 20 % | 20 % | keine |
| Deutschland | 26,375 % | 0 %; 26,375 % für direkte Wandelanleihen | keine |
| Japan | 15,315 % | 15,315 % | keine |
| Indien | 20 % | 20 % | keine |
| Vereinigtes Königreich | 0 % | 20 % | 0,5 % von `cleanValue` beim Kauf direkter britischer Aktien |
| Frankreich | 12,8 % | 0 % | keine |
| Italien | 26 % | 26 % | keine |
| Kanada | 25 % | 0 % für gewöhnliche fremdfinanzierte Portfolioschulden | keine |
| Brasilien | 10 % | 15 % | keine |
| Schweiz | 35 % | 35 % | mit Schweizer Händler und nicht befreitem Anleger 0,075 % von `cleanValue` für schweizerische und 0,15 % für ausländische Wertpapiere |
| Österreich | 27,5 % | 27,5 % | keine |
| Polen | 19 % | 20 % | keine für börsengehandelte Wertpapiere |

{{% notice note %}}
Die schweizerische Umsatzabgabe und die Verrechnungssteuer sind getrennt modelliert. Die britische SDRT und die französische Finanztransaktionssteuer sind in den mitgelieferten Regeln nur grob angenähert.
{{% /notice %}}

## JSON-Schema
Das YAML muss dem folgenden JSON-Schema entsprechen. Es kann auch als Kontext für ein LLM verwendet werden, um gültiges Modell-YAML zu erzeugen.

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
