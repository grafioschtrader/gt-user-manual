---
title: "Mean-Reversion-Dip"
date: 2026-02-12T12:00:00+01:00
draft: false
weight: 40
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Die Mean-Reversion-Dip-Strategie ist eine komplexe Handelsstrategie, die auf dem Prinzip des Kaufens bei Kursrückgängen basiert. Sie wird über eine YAML-Konfiguration erstellt und bietet modulare Komponenten für den Einstieg, das Gewinnmanagement und das Verlustmanagement.

## Überblick
Die Strategie funktioniert nach folgendem Grundprinzip: Wenn der Kurs eines Wertpapiers um einen konfigurierten Schwellenwert gegenüber einem Referenzpunkt fällt, wird ein Kaufsignal generiert. Nachdem eine Position eröffnet wurde, verfolgt die Strategie zwei mögliche Pfade — einen Gewinnpfad mit stufenweisem Verkauf (Scale-Out) und einen Verlustpfad mit Verlustbegrenzung oder Nachkauf.

{{< mermaid >}}
graph TD
    A["Einstieg: Kursrückgang erkannt"] --> B{"Position eröffnet"}
    B --> C{"Kurs steigt?"}
    C -->|Ja| D["Gewinnmanagement:<br/>Stufenweiser Verkauf"]
    D --> E["Take-Profit:<br/>Position schliessen"]
    C -->|Nein| F{"Verlust-Schwelle<br/>erreicht?"}
    F -->|Nein| C
    F -->|Ja| G{"Variante?"}
    G -->|A| H["Stop-Loss:<br/>Position verkaufen"]
    G -->|B| I["Nachkauf:<br/>Position aufstocken"]
    I --> C
{{< /mermaid >}}

## YAML-Konfiguration
Die Mean-Reversion-Dip-Strategie wird über das Feld **Strategiekonfiguration (YAML)** konfiguriert. Die Konfiguration besteht aus mehreren Modulen, die jeweils einen Aspekt der Strategie definieren. Hier ein vereinfachtes Beispiel der Grundstruktur:

```yaml
strategy_name: "Mein Dip-Buying"
version: "1.0"
universe:
  mode: single_asset
  direction: long_only
data:
  price_field: close
execution:
  order_type: market
cooldowns:
  after_buy_days: 5
  after_sell_days: 3
entry:
  type: dip_buy
  lookback_T: 20
  dip_threshold_pct: 10.0
  dip_reference:
    type: price_T_ago
    period: 20
  initial_buy_sizing:
    mode: pct_portfolio
    pct: 2.5
profit_management:
  scale_out_enabled: true
  sell_fraction_basis: initial_position
  scale_out_plan:
    - id: "tranche_1"
      trigger:
        type: pct_gain
        value: 5.0
        reference: avg_cost
      sell_fraction: 0.25
    - id: "tranche_2"
      trigger:
        type: pct_gain
        value: 10.0
        reference: avg_cost
      sell_fraction: 0.50
      sell_remainder: true
  take_profit:
    mode: pct_gain
    pct: 15.0
    reference: avg_cost
downside_management:
  loss_action: A_sell_loss
  trigger:
    down_reference: avg_cost
    down_threshold_pct: 8.0
    decision_basis: simple_threshold
  variant_A_sell_loss:
    enabled: true
    stop_type: hard_stop
    stop_reference: avg_cost
    stop_threshold_pct: 10.0
    order_type: market
risk_controls:
  max_position_exposure_pct: 0.05
  max_position_drawdown_pct: 0.15
  force_exit_on_risk_breach: true
  block_entry_if_exposure_exceeded: true
outputs:
  emit_events: ["entry", "exit", "tranche"]
  track_metrics: ["return", "drawdown"]
```

## Konfigurationsmodule im Detail
Die folgenden Abschnitte beschreiben alle verfügbaren Schlüsselwörter, gruppiert nach Modulen.

### Stammebene
Auf der obersten Ebene der YAML-Konfiguration werden der Name und die Version der Strategie definiert.

| Schlüssel | Typ | Pflicht | Beschreibung |
|---|---|---|---|
| `strategy_name` | Text | Ja | Name der Strategie (max. 100 Zeichen). |
| `version` | Text | Nein | Versionsnummer der Konfiguration (max. 10 Zeichen). |

### universe — Anlageuniversum
Definiert, auf welche Wertpapiere die Strategie angewendet wird und in welche Richtung gehandelt wird.

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `mode` | Auswahl | `single_asset` — Einzelnes Wertpapier. `watchlist` — Alle Wertpapiere einer Watchlist. |
| `assets` | Liste | Liste von Wertpapierbezeichnungen (nur bei `watchlist`-Modus relevant). |
| `direction` | Auswahl | `long_only` — Nur Kaufpositionen. `short_only` — Nur Leerverkäufe. `long_short` — Beide Richtungen. |

### data — Datenquelle
Konfiguriert die verwendeten Kursdaten.

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `price_field` | Auswahl | Welcher Kurswert verwendet wird: `close` (Schlusskurs), `last` (letzter Kurs), `mid` (Mittelwert). |
| `timeframe` | Text | Zeitrahmen der Kursdaten, z.B. `1d` (täglich). |

### execution — Auftragsausführung
Parameter für die Ausführung von Kauf- und Verkaufsaufträgen.

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `order_type` | Auswahl | `market` — Marktauftrag. `limit` — Limitauftrag. |
| `slippage_model` | Text | Modell für die Slippage-Berechnung (optional). |
| `fees_model` | Text | Modell für die Gebührenberechnung (optional). |

### cooldowns — Abkühlphasen
Zeitliche Beschränkungen zwischen aufeinanderfolgenden Transaktionen.

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `after_buy_days` | Ganzzahl | Mindestanzahl Tage Pause nach einem Kauf (min. 0). |
| `after_sell_days` | Ganzzahl | Mindestanzahl Tage Pause nach einem Verkauf (min. 0). |
| `max_trades_per_asset_per_30d` | Ganzzahl | Maximale Anzahl Transaktionen pro Wertpapier innerhalb von 30 Tagen (min. 1). |

### entry — Einstiegsmodul
Das Einstiegsmodul definiert die Bedingungen für den Kauf. Der zentrale Parameter ist der prozentuale Kursrückgang gegenüber einem Referenzpunkt.

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `type` | Auswahl | Art des Einstiegssignals: `dip_buy` (Kursrückgang), `breakout` (Ausbruch), `indicator_signal` (Indikatorsignal). |
| `lookback_T` | Ganzzahl | Rückblickzeitraum in Tagen für die Einstiegsberechnung. |
| `dip_threshold_pct` | Dezimalzahl | Prozentualer Kursrückgang, der den Einstieg auslöst. |
| `dip_reference` | Objekt | Konfiguration des Referenzpunkts für die Rückgangsberechnung (siehe unten). |
| `initial_buy_sizing` | Objekt | Konfiguration der Kaufgrösse (siehe Positionsgrösse). |

#### entry.dip_reference — Referenzpunkt für den Kursrückgang

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `type` | Auswahl | `price_T_ago` — Kurs vor T Tagen. `highest_in_window` — Höchster Kurs im Rückblickfenster. `moving_average` — Gleitender Durchschnitt. |
| `period` | Ganzzahl | Anzahl Tage für die Referenzberechnung. |
| `indicator` | Text | Name des Indikators (nur bei `moving_average`, z.B. `SMA` oder `EMA`). |

#### Positionsgrösse (sizing)
Dieses wiederverwendbare Objekt kommt in `entry.initial_buy_sizing` und `downside_management.variant_B_average_down.add_sizing` vor.

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `mode` | Auswahl | `pct_portfolio` — Prozent des Portfoliowerts. `absolute_amount` — Fester Betrag. |
| `pct` | Dezimalzahl | Prozentsatz des Portfolios (bei `pct_portfolio`). |
| `amount` | Dezimalzahl | Absoluter Betrag (bei `absolute_amount`). |

### profit_management — Gewinnmanagement
Konfiguriert den stufenweisen Gewinnabbau und den finalen Take-Profit.

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `scale_out_enabled` | Boolean | Aktiviert den stufenweisen Teilverkauf. |
| `sell_fraction_basis` | Auswahl | Bezugsgrösse für den Verkaufsanteil: `initial_position` (ursprüngliche Positionsgrösse) oder `current_position` (aktuelle Positionsgrösse). |
| `scale_out_plan` | Liste | Liste von Tranchen für den stufenweisen Verkauf (siehe unten). |
| `take_profit` | Objekt | Konfiguration des finalen Take-Profit (siehe unten). |

#### profit_management.scale_out_plan[] — Tranchen
Jede Tranche definiert eine Verkaufsstufe.

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `id` | Text | Eindeutige Bezeichnung der Tranche (z.B. `"tranche_1"`). |
| `trigger` | Objekt | Auslösebedingung (siehe Trigger-Objekt). |
| `sell_fraction` | Dezimalzahl | Anteil der Position, der verkauft wird (0.0 bis 1.0, z.B. `0.25` für 25%). |
| `sell_remainder` | Boolean | Wenn `true`, wird die gesamte Restposition verkauft. |

#### Trigger-Objekt
Das Trigger-Objekt wird in Tranchen und im Take-Profit verwendet.

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `type` | Auswahl | `pct_gain` — Prozentualer Gewinn. `absolute_profit` — Absoluter Gewinnbetrag. `indicator` — Indikator-basiert. |
| `value` | Dezimalzahl | Schwellenwert für den Trigger. |
| `reference` | Auswahl | Bezugspreis: `avg_cost` (durchschnittlicher Einstandspreis) oder `initial_entry_price` (erster Einstiegspreis). |

#### profit_management.take_profit — Finaler Take-Profit

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `mode` | Auswahl | Trigger-Typ: `pct_gain`, `absolute_profit` oder `indicator`. |
| `pct` | Dezimalzahl | Gewinnziel in Prozent (bei `pct_gain`). |
| `profit_amount` | Dezimalzahl | Gewinnziel als absoluter Betrag (bei `absolute_profit`). |
| `reference` | Auswahl | Bezugspreis: `avg_cost` oder `initial_entry_price`. |
| `action` | Text | Aktion beim Erreichen (z.B. `"sell_all"`). |

### downside_management — Verlustmanagement
Konfiguriert das Verhalten bei fallenden Kursen nach dem Einstieg.

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `loss_action` | Auswahl | Gewählte Variante: `A_sell_loss` (Stop-Loss) oder `B_average_down` (Nachkauf). |
| `trigger` | Objekt | Bedingungen, die das Verlustmanagement aktivieren (siehe unten). |
| `variant_A_sell_loss` | Objekt | Konfiguration für Stop-Loss (Variante A). |
| `variant_B_average_down` | Objekt | Konfiguration für Nachkauf (Variante B). |

#### downside_management.trigger — Verlust-Auslöser

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `down_reference` | Auswahl | Bezugspreis: `avg_cost` oder `initial_entry_price`. |
| `down_threshold_pct` | Dezimalzahl | Prozentualer Verlust, der das Verlustmanagement aktiviert. |
| `decision_basis` | Auswahl | Entscheidungsgrundlage: `simple_threshold` (einfacher Schwellenwert), `indicator` (Indikator-basiert), `statistical` (statistisch) oder `hybrid` (kombiniert). |
| `indicator_rules` | Liste | Indikator-basierte Regeln (bei `indicator` oder `hybrid`). |
| `statistical_rules` | Liste | Statistische Regeln (bei `statistical` oder `hybrid`). |

#### Indikator- und Statistik-Regeln
Die Objekte in `indicator_rules` und `statistical_rules` haben folgende Struktur:

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `id` | Text | Eindeutige Bezeichnung der Regel. |
| `type` | Auswahl | Indikatortyp: `rsi`, `zscore`, `sma` oder `ema` (bei Indikatorregeln). Text bei statistischen Regeln. |
| `params.length` | Ganzzahl | Berechnungsperiode (min. 1). |
| `params.condition` | Text | Vergleichsbedingung (max. 10 Zeichen, z.B. `"<"`, `">"`, `"<="`, `">="`) |
| `params.value` | Dezimalzahl | Schwellenwert für die Bedingung. |
| `params.lookback` | Ganzzahl | Rückblickzeitraum (min. 1). |

#### downside_management.variant_A_sell_loss — Stop-Loss

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `enabled` | Boolean | Aktiviert die Stop-Loss-Variante. |
| `stop_type` | Auswahl | `hard_stop` — Fester Stop bei Schwellenwert. `indicator_stop` — Indikator-basierter Stop. |
| `stop_reference` | Auswahl | Bezugspreis: `avg_cost` oder `initial_entry_price`. |
| `stop_threshold_pct` | Dezimalzahl | Prozentualer Verlust, der den Stop auslöst. |
| `order_type` | Auswahl | `market` oder `limit`. |
| `action` | Text | Aktion (z.B. `"sell_all"`). |

#### downside_management.variant_B_average_down — Nachkauf

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `enabled` | Boolean | Aktiviert die Nachkauf-Variante. |
| `add_sizing` | Objekt | Positionsgrösse für jeden Nachkauf (siehe Positionsgrösse oben). |
| `max_adds` | Ganzzahl | Maximale Anzahl von Nachkäufen (min. 1). |
| `add_step_rule` | Objekt | Regel für die Nachkauf-Stufen (siehe unten). |
| `recalculate_avg_cost` | Boolean | Wenn `true`, wird nach jedem Nachkauf der durchschnittliche Einstandspreis neu berechnet. |

#### add_step_rule — Nachkauf-Schritte

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `type` | Auswahl | `each_n_pct_drop` — Nachkauf bei jedem weiteren N%-Rückgang. `indicator_based` — Nachkauf bei Indikatorsignal. `custom` — Benutzerdefinierte Regel. |
| `drop_pct_step` | Dezimalzahl | Prozentualer Kursrückgang pro Nachkauf-Stufe (bei `each_n_pct_drop`). |
| `reference` | Auswahl | Bezugspreis: `avg_cost` oder `initial_entry_price`. |

### risk_controls — Risikokontrolle
Übergeordnete Grenzen zum Schutz vor übermässigem Risiko.

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `max_position_exposure_pct` | Dezimalzahl | Maximale Exposition pro Position in Prozent des Portfolios (0.0 bis 1.0, z.B. `0.05` für 5%). |
| `max_position_drawdown_pct` | Dezimalzahl | Maximaler erlaubter Drawdown pro Position (0.0 bis 1.0, z.B. `0.15` für 15%). |
| `force_exit_on_risk_breach` | Boolean | Wenn `true`, wird die Position bei Überschreitung der Risikogrenzen sofort geschlossen. |
| `block_entry_if_exposure_exceeded` | Boolean | Wenn `true`, werden keine neuen Positionen eröffnet, wenn die Expositionsgrenze erreicht ist. |
| `block_add_if_exposure_exceeded` | Boolean | Wenn `true`, werden keine Nachkäufe ausgeführt, wenn die Expositionsgrenze erreicht ist. |

### outputs — Ausgaben
Konfiguriert, welche Ereignisse und Metriken die Strategie erzeugt.

| Schlüssel | Typ | Beschreibung |
|---|---|---|
| `emit_events` | Liste | Liste von Ereignistypen, die protokolliert werden (z.B. `["entry", "exit", "tranche", "stop_loss"]`). |
| `track_metrics` | Liste | Liste von Metriken, die erfasst werden (z.B. `["return", "drawdown", "sharpe"]`). |

{{% notice style="info" title="Hinweis" %}}
Diese Strategie wird über YAML konfiguriert. In zukünftigen Versionen wird ein visueller Editor mit Syntaxhervorhebung zur Verfügung stehen, der die Konfiguration vereinfacht.
{{% /notice %}}
