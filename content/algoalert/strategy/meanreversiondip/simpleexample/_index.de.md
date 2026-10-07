---
title: "Einfaches Beispiel"
date: 2026-09-26T12:00:00+02:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="In Bearbeitung - Zielversion V0.40.0" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Dieses Beispiel enthält nur, was ein [Mean-Reversion-Dip](../) zwingend braucht: eine Einstiegsregel, ein Gewinnziel, einen Stop-Loss und die Risikogrenzen. Teilgewinne und das Aufstocken fehlen bewusst. Es eignet sich als Ausgangspunkt für eigene Versuche, etwa in einer [Historischen Wiederholung](../../../historicalrun/). Die Werte sind anpassbare Beispieleinstellungen und keine Anlageempfehlung.

Ersetzen Sie im Dialog **Strategie Definition** den Inhalt des YAML-Editors durch die folgende Konfiguration und speichern Sie mit **Übernehmen**.

```yaml
strategy_name: einfacher_dip
universe:
  direction: long_only
cooldowns:
  after_buy_days: 5
  after_sell_days: 5
  max_trades_per_asset_per_30d: 4
entry:
  lookback_T: 20
  dip_reference:
    type: price_T_ago
  dip_threshold_pct: -0.08
  initial_buy_sizing:
    mode: pct_portfolio
    pct: 0.02
profit_management:
  take_profit:
    mode: pct_gain
    pct: 0.06
    reference: avg_cost
downside_management:
  loss_action: A_sell_loss
  variant_A_sell_loss:
    stop_type: hard_stop
    stop_reference: avg_cost
    stop_threshold_pct: -0.08
risk_controls:
  max_position_exposure_pct: 0.05
  max_position_drawdown_pct: 0.15
  force_exit_on_risk_breach: true
```

## Was die Einstellungen bewirken
Die Strategie kauft nur (`long_only`). Sie vergleicht jeden Tagesschlusskurs mit dem Schlusskurs 20 Beobachtungen, also rund vier Handelswochen, davor. Liegt der Kurs mindestens 8% tiefer, schlägt sie einen Kauf über 2% des Nettovermögens vor.

Eine offene Position wird vollständig verkauft, sobald sie 6% über dem gewichteten Durchschnittskurs liegt. Fällt sie 8% unter diesen Kurs, wird sie mit dem Stop-Loss ebenfalls vollständig verkauft. Eine zusätzliche Verlustbedingung unter `trigger` ist hier nicht nötig, weil der feste Stop allein über den Ausstieg entscheidet.

Nach einem Kauf oder Verkauf wartet die Strategie fünf Kalendertage, bevor sie einen neuen Einstieg vorschlägt. Innerhalb von 30 Tagen sind höchstens vier gebuchte Transaktionen dieses Instruments erlaubt. Die Risikogrenzen dienen als Sicherheitsnetz: Die Position darf höchstens 5% des Nettovermögens ausmachen, und bei einem Verlust von 15% oder beim Überschreiten einer Grenze wird sie sofort geschlossen.

## Ein Ablauf in Zahlen
Das Nettovermögen beträgt 100'000, der Kurs des Wertpapiers lag vor 20 Beobachtungen bei 100.

1. Der Kurs schliesst bei 92, also 8% tiefer. Die Strategie schlägt einen Kauf über 2'000 vor, das sind rund 21.7 Stück.
2. Sie buchen den Kauf zu 92 und wählen dabei in der **Strategiezuordnung** den Namen `einfacher_dip`. Der Durchschnittskurs ist nun 92.
3. Steigt der Kurs auf 97.52 oder höher (92 plus 6%), schlägt die Strategie den Verkauf der ganzen Position vor.
4. Fällt der Kurs stattdessen auf 84.64 oder tiefer (92 minus 8%), schlägt der Stop-Loss den Verkauf der ganzen Position vor.
5. Nach dem gebuchten Verkauf beginnt die Wartefrist von fünf Tagen; danach ist wieder ein Einstieg möglich.

Die Grenze von 15% wird in diesem Ablauf nicht erreicht, weil der Stop bei 8% früher greift. Sie wirkt erst, wenn ein einzelner Kurssprung beide Schwellen auf einmal überschreitet. Das Ergebnis ist dann dasselbe, nämlich der vollständige Verkauf.

## Erweitern
Die fehlenden Möglichkeiten zeigt das [Beispiel mit Teilgewinnen](../partialprofitexample/). Alle Eigenschaften sind unter [Wirksame Eigenschaften](../#wirksame-eigenschaften) beschrieben.
