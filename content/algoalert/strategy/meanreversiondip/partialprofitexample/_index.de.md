---
title: "Beispiel mit Teilgewinnen"
date: 2026-09-26T12:00:00+02:00
draft: false
weight: 20
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="In Bearbeitung - Zielversion V0.38.0" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Diese Konfiguration setzt **Vorlage laden** im Dialog **Strategie Definition** ein. Sie enthält neben Einstieg, Gewinnziel und Stop-Loss einen ausgeschalteten Plan für Teilgewinne und die Einstellungen für das Aufstocken, die erst mit `loss_action: B_average_down` wirksam werden. Beides lässt sich so mit einer einzigen Änderung ausprobieren. Die Werte sind anpassbare Beispieleinstellungen und keine Anlageempfehlung. Eine kürzere Konfiguration ohne diese Zusätze zeigt das [einfache Beispiel](../simpleexample/).

```yaml
strategy_name: daily_dip_with_stop
universe:
  direction: long_only
cooldowns:
  after_buy_days: 2
  after_sell_days: 2
  max_trades_per_asset_per_30d: 10
entry:
  lookback_T: 10
  dip_reference:
    type: price_T_ago
  dip_threshold_pct: -0.12
  initial_buy_sizing:
    mode: pct_portfolio
    pct: 0.03
profit_management:
  # Partial profit taking. Switch scale_out_enabled on to give the position back in the tranches below;
  # each of them executes at most once per position, and the last one closes whatever is left.
  scale_out_enabled: false
  sell_fraction_basis: current_position
  scale_out_plan:
    - id: t1
      trigger:
        type: pct_gain
        value: 0.04
        reference: avg_cost
      sell_fraction: 0.33
    - id: t2
      trigger:
        type: pct_gain
        value: 0.07
        reference: avg_cost
      sell_remainder: true
  take_profit:
    mode: pct_gain
    pct: 0.10
    reference: avg_cost
downside_management:
  # Additional loss rule. A hard stop does not need it; indicator confirmation and averaging down do.
  trigger:
    down_reference: avg_cost
    down_threshold_pct: -0.10
    decision_basis: simple_threshold
  # The loss action alone selects the variant: A_sell_loss sells at the stop, B_average_down buys more instead.
  loss_action: A_sell_loss
  variant_A_sell_loss:
    stop_type: hard_stop
    stop_reference: avg_cost
    stop_threshold_pct: -0.10
  variant_B_average_down:
    add_sizing:
      mode: pct_portfolio
      pct: 0.01
    max_adds: 3
    add_step_rule:
      type: each_n_pct_drop
      reference: initial_entry_price
      drop_pct_step: 0.05
risk_controls:
  max_position_exposure_pct: 0.10
  max_position_drawdown_pct: 0.25
  force_exit_on_risk_breach: true
```

## So wie geliefert
Die Strategie steigt nach einem Rückgang von mindestens 12% über zehn Beobachtungen ein und setzt 3% des Nettovermögens ein. Liegt die Position 10% über dem gewichteten Durchschnittskurs, wird sie vollständig verkauft. Liegt sie 10% darunter, schliesst der Stop-Loss sie vollständig. Die zusätzliche Verlustbedingung unter `trigger` verwendet dieselbe Schwelle und ändert an diesem Verhalten nichts; sie ist die Grundlage für das Aufstocken weiter unten. Nach jeder Transaktion gilt eine Wartefrist von zwei Tagen für neue Einstiege, und innerhalb von 30 Tagen sind höchstens zehn Transaktionen des Instruments erlaubt. Die Position darf höchstens 10% des Nettovermögens ausmachen; bei einem Verlust von 25% oder beim Überschreiten einer Grenze wird sie geschlossen.

## Teilgewinne einschalten
Mit `scale_out_enabled: true` wird der Plan wirksam. Liegt die Position 4% über dem Durchschnittskurs, verkauft die Stufe `t1` einen Drittel der noch offenen Menge. Bei 7% verkauft die Stufe `t2` den ganzen Rest. Das Gewinnziel von 10% wird in diesem Fall nur noch erreicht, wenn ein einzelner Kurssprung die Stufe `t2` überspringt; es bleibt als Obergrenze bestehen. Wie Stufen, Bezugsgrössen und teilweise gebuchte Vorschläge zusammenwirken, beschreibt der Abschnitt [Teilgewinne mitnehmen](../#teilgewinne-mitnehmen).

## Aufstocken statt verkaufen
Mit `loss_action: B_average_down` verkauft die Strategie bei einem Verlust nicht, sondern stockt auf. Die Angaben unter `variant_A_sell_loss` werden dann nicht mehr ausgewertet. Gemessen am ersten Einstiegskurs wird bei 10%, 15% und 20% Verlust je 1% des Nettovermögens nachgekauft, höchstens dreimal. Die erste Schwelle stammt aus `trigger`, die folgenden ergeben sich aus dem Schritt von 5%. Erreicht der Verlust gegenüber dem Durchschnittskurs 25%, werden keine weiteren Aufstockungen vorgeschlagen, und der erzwungene Ausstieg schliesst die Position. Die Einzelheiten beschreibt der Abschnitt [Eine Verlustposition aufstocken](../#eine-verlustposition-aufstocken).
