---
title: "Mean-Reversion-Dip"
date: 2026-09-30T12:00:00+02:00
draft: false
weight: 40
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="In Bearbeitung - Zielversion V0.40.0" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Die Mean-Reversion-Dip-Strategie wertet abgeschlossene Tagesschlusskurse aus und schlägt nach einer konfigurierten Kursbewegung einen Einstieg vor. Sie unterstützt den vollständigen Ausstieg bei Stop-Loss, Gewinnmitnahme und einer erzwungenen Risikobegrenzung sowie das schrittweise Mitnehmen von Teilgewinnen und das Aufstocken einer Verlustposition. Im Hauptportfolio bucht eine Empfehlung oder Benachrichtigung keine Transaktion.

Zwei Beispiele zeigen die Konfiguration im Ganzen: Das [einfache Beispiel](./simpleexample/) beschränkt sich auf Einstieg, Gewinnziel, Stop-Loss und Risikogrenzen. Das [Beispiel mit Teilgewinnen](./partialprofitexample/) entspricht der Vorlage im Dialog und enthält zusätzlich einen Plan für Teilgewinne und die Einstellungen für das Aufstocken.

## Strategie einrichten
Der Mean-Reversion-Dip wird unter **Regelbasierter Handel** einem Wertpapier des Baums zugewiesen; auf der Portfoliobasierten Strategie und auf einer Anlageklasse wird er nicht angeboten. Die Portfoliobasierte Strategie muss mit einer Watchlist verknüpft sein, und das Wertpapier muss in dieser Watchlist enthalten sein. Ordnen Sie jedes Instrument genau einer Anlageklasse mit einer Gewichtung zu. Die Gewichtungen der Anlageklassen sowie jene der Wertpapiere innerhalb jeder Anlageklasse müssen jeweils 100% ergeben. Die Anlageklasse des Wertpapiers wird dadurch taktisch: Ihre Gewichtung ist nur noch eine Obergrenze, und eine Portfolio-Neugewichtung kauft in ihr nicht mehr nach, auch nicht bei den übrigen Wertpapieren. Legen Sie Wertpapiere mit Mean-Reversion-Dip deshalb am besten in eine eigene Anlageklasse, siehe [Neugewichtung und Mean-Reversion-Dip gemeinsam](../../#neugewichtung-und-mean-reversion-dip-gemeinsam).

Wählen Sie im Dialog **Strategie Definition** unter **Strategiename** den **Mean-Reversion-Dip**. Der YAML-Editor ist mit dem [Beispiel mit Teilgewinnen](./partialprofitexample/) vorbelegt, **Vorlage laden** stellt es wieder her. Lassen Sie **Aktiv** für eine ausführbare Strategie eingeschaltet. Schalten Sie es aus, um einen Entwurf zu speichern, der noch nicht ausführbare Einstellungen enthält. Gespeichert wird mit **Übernehmen**. Eine aktive Strategie wird zurückgewiesen, wenn eine Pflichtangabe fehlt oder eine Einstellung gewählt ist, die noch nicht ausgeführt werden kann. Die Meldung nennt die betroffene Einstellung.

## Wirksame Eigenschaften
Die folgenden Tabellen beschreiben alle Eigenschaften, die eine Entscheidung der Strategie beeinflussen. Anteile werden als Dezimalzahl geschrieben: `0.03` bedeutet 3%, `-0.12` bedeutet einen Rückgang um 12%. Pflichtangaben sind mit einem Stern markiert.

Das Instrument wird nicht im YAML gewählt, es ist immer das Wertpapier, dem die Strategie zugewiesen ist. Angaben zur Datenquelle, zur Ausführungsart und zum Auswahlmodus sind fest vorgegeben: Die Strategie arbeitet mit Tagesschlusskursen und Marktausführung ohne eigenes Gebühren- oder Slippage-Modell. Solche Angaben können Sie weglassen. Ältere Konfigurationen, die sie noch enthalten, bleiben gültig.

### Name und Richtung
| Eigenschaft | Bedeutung | Werte |
|---|---|---|
| `strategy_name`* | Name der Strategie. Er erscheint beim Erfassen einer Transaktion als Auswahl in der **Strategiezuordnung**. | Text, höchstens 100 Zeichen |
| `universe.direction`* | Erlaubte Handelsrichtung. | `long_only`, `short_only`, `long_short` |

### Wartefristen
Die Wartefristen beschränken nur neue Einstiege, Ausstiege werden nie verschoben.

| Eigenschaft | Bedeutung | Werte |
|---|---|---|
| `cooldowns.after_buy_days`* | Kalendertage nach der letzten Eröffnung, bevor ein neuer Einstieg möglich ist. | ganze Zahl ab 0 |
| `cooldowns.after_sell_days`* | Kalendertage nach der letzten Reduktion, bevor ein neuer Einstieg möglich ist. | ganze Zahl ab 0 |
| `cooldowns.max_trades_per_asset_per_30d`* | Höchstzahl gebuchter Transaktionen des Instruments innerhalb von 30 Tagen, einschliesslich Teilausführungen. | ganze Zahl ab 1 |

### Einstieg
| Eigenschaft | Bedeutung | Werte |
|---|---|---|
| `entry.lookback_T`* | Anzahl Beobachtungen, über welche die Kursbewegung gemessen wird. | 1 bis 999 |
| `entry.dip_reference.type`* | Vergleichskurs: der Kurs vor T Beobachtungen, der höchste (bei Short der tiefste) vorherige Schlusskurs eines Zeitfensters oder ein gleitender Durchschnitt. | `price_T_ago`, `highest_in_window`, `moving_average` |
| `entry.dip_reference.period` | Länge des Zeitfensters beziehungsweise des gleitenden Durchschnitts. Nicht nötig bei `price_T_ago`. | 1 bis 999 |
| `entry.dip_reference.indicator` | Art des gleitenden Durchschnitts, nur bei `moving_average`. | `SMA`, `EMA` |
| `entry.dip_threshold_pct`* | Mindestbewegung gegenüber dem Vergleichskurs. Bei Long ein Rückgang, bei Short ein entsprechender Anstieg. | zwischen -1 und 0, z.B. `-0.08` |
| `entry.initial_buy_sizing.mode`* | Grösse des Einstiegs als Anteil am Nettovermögen oder als fester Betrag in der Mandantenwährung. | `pct_portfolio`, `absolute_amount` |
| `entry.initial_buy_sizing.pct` | Anteil am Nettovermögen bei `pct_portfolio`. | grösser 0 bis 1 |
| `entry.initial_buy_sizing.amount` | Betrag bei `absolute_amount`. | grösser 0 |

### Gewinnmitnahme
Der ganze Bereich `profit_management` ist optional. Ohne ihn nimmt die Strategie keinen Gewinn mit und schliesst eine Position nur über Verlust- oder Risikoregeln.

| Eigenschaft | Bedeutung | Werte |
|---|---|---|
| `take_profit.mode` | Gewinnziel als prozentualer Gewinn oder als Gewinnbetrag in der Mandantenwährung. Beim Erreichen wird die ganze Restposition geschlossen. | `pct_gain`, `absolute_profit` |
| `take_profit.pct` / `take_profit.profit_amount` | Höhe des Gewinnziels passend zum Modus. | grösser 0 |
| `take_profit.reference` | Bezugskurs: gewichteter Durchschnittskurs oder erster Einstiegskurs. | `avg_cost`, `initial_entry_price` |
| `scale_out_enabled` | Schaltet den Plan für Teilgewinne ein. | `true`, `false` |
| `sell_fraction_basis` | Bezug der Anteile, siehe [Teilgewinne mitnehmen](#teilgewinne-mitnehmen). Pflicht, wenn der Plan eingeschaltet ist. | `current_position`, `initial_position` |
| `scale_out_plan` | Liste der Stufen. Jede Stufe hat eine Kurzbezeichnung `id`, einen Auslöser `trigger` und entweder einen Anteil `sell_fraction` oder `sell_remainder: true` für die letzte Stufe. | höchstens 20 Stufen |

### Verlustbehandlung
| Eigenschaft | Bedeutung | Werte |
|---|---|---|
| `downside_management.loss_action`* | Wählt, was bei einem Verlust geschieht: verkaufen oder aufstocken. Diese Angabe allein bestimmt die Variante. | `A_sell_loss`, `B_average_down` |
| `variant_A_sell_loss.stop_type`* | Fester Stop bei einer Kursschwelle oder ein Stop, der durch Indikatorregeln bestätigt wird. Pflicht bei `A_sell_loss`. | `hard_stop`, `indicator_stop` |
| `variant_A_sell_loss.stop_reference`* | Bezugskurs des Stops. Pflicht bei `A_sell_loss`. | `avg_cost`, `initial_entry_price` |
| `variant_A_sell_loss.stop_threshold_pct`* | Nachteilige Bewegung, bei der die ganze Position geschlossen wird. Pflicht bei `A_sell_loss`. | zwischen -1 und 0, z.B. `-0.10` |
| `trigger.down_reference`, `trigger.down_threshold_pct` | Zusätzliche Verlustbedingung. Beim festen Stop kann sie fehlen; für einen Indikator-Stop und für das Aufstocken ist sie Pflicht. | wie beim Stop |
| `trigger.decision_basis` | Wie die Verlustbedingung entschieden wird: nur Kursschwelle, Indikatorregeln, Z-Score oder Kursschwelle mit beiden Regelgruppen. | `simple_threshold`, `indicator`, `statistical`, `hybrid` |
| `trigger.indicator_rules`, `trigger.statistical_rules` | Regeln für die Entscheidungsgrundlagen `indicator`, `statistical` und `hybrid`. | `rsi`, `sma`, `ema`, `zscore` |
| `variant_B_average_down.*` | Einstellungen für das Aufstocken, siehe [Eine Verlustposition aufstocken](#eine-verlustposition-aufstocken). Pflicht bei `B_average_down`. | |

### Risikogrenzen
| Eigenschaft | Bedeutung | Werte |
|---|---|---|
| `risk_controls.max_position_exposure_pct`* | Höchster Anteil der Position am Nettovermögen. Ein grösserer Einstieg oder eine grössere Aufstockung wird blockiert. Diese Grenze gilt immer, keine Einstellung hebt sie auf. | grösser 0 bis 1 |
| `risk_controls.max_position_drawdown_pct`* | Höchster Verlust der Position gegenüber dem Durchschnittskurs. | grösser 0 bis 1 |
| `risk_controls.force_exit_on_risk_breach`* | Schliesst die Position vollständig, sobald eine Risikogrenze überschritten ist. | `true`, `false` |

## Tägliche Entscheidungen
Die Strategie verwendet den letzten abgeschlossenen Handelstag vor dem aktuellen UTC-Tag. Wochenenden und erfasste Börsenfeiertage werden übersprungen; für Kryptowährungen gelten Kalendertage. Fehlt der erwartete Schlusskurs oder ausreichend Indikatorhistorie, ist die Entscheidung nicht verfügbar. Es wird kein aktueller Kurs als Ersatz verwendet.

Ein Long-Einstieg vergleicht den Schlusskurs mit dem Kurs vor T Beobachtungen, dem höchsten vorherigen Schlusskurs eines Zeitfensters oder einem vorherigen SMA/EMA. Ein Short-Einstieg spiegelt die Kursbewegung und verwendet beim Zeitfenster den tiefsten vorherigen Schlusskurs. Der aktuelle Schlusskurs gehört nicht zu diesen Einstiegsreferenzen. Short-Positionen benötigen ein Margin-Instrument. Eine Margin-Empfehlung benötigt ausserdem eine bestehende Transaktion mit einheitlichem Wert pro Punkt; unterschiedliche Kontraktdefinitionen desselben Instruments verhindern die Auswertung.

Die Obergrenze der Hierarchie, die Bereichs- und Instrumentallokation sowie das Positionslimit gelten gemeinsam; eine Hierarchiegewichtung von `50` bedeutet 50%. Einstiegsvorschläge reservieren Kapazität für die übrigen Strategien derselben Auswertung. Leerverkaufserlöse erhöhen das Nettovermögen nicht. Ein Ausstiegsvorschlag gibt erst nach der Buchung Kapazität frei. Ein zu grosser Einstieg wird blockiert und nicht automatisch verkleinert.

## Tatsächliche Transaktionen zuordnen
Wählen Sie beim Erfassen eines Kaufs oder Verkaufs die **Strategiezuordnung**. Ordnen Sie Eröffnungen und Ausstiege einheitlich zu, auch bei bereits offenen Positionen. Mehrere Strategien können dasselbe Instrument mit getrennten Zuordnungen halten. Abgeschlossene historische Transaktionen ohne Zuordnung blockieren die Auswertung nicht; offene Mengen ohne Zuordnung dagegen schon. Ein Margin-Abschluss übernimmt die Zuordnung seiner Eröffnung. Eine Änderung dieser Eröffnung aktualisiert auch ihre Abschlüsse.

Menge, gewichteter durchschnittlicher Einstiegskurs und Wartefristen werden aus den zugeordneten Transaktionen rekonstruiert. Dabei werden Splits berücksichtigt. Änderungen und Löschungen von Transaktionen beeinflussen deshalb die nächste Entscheidung. Beim Löschen einer Strategie bleiben die Portfoliotransaktionen erhalten und verlieren ihre Zuordnung. Eine Benachrichtigung verändert den Ausführungsstand nie.

## Ausstiege und Wartefristen
Ist eine Risikogrenze überschritten und der erzwungene Ausstieg eingeschaltet, wird zuerst die vollständige Schliessung vorgeschlagen. Danach wird der feste Stop ausgewertet, anschliessend die zusätzliche Verlustbedingung, sofern sie vorhanden ist. Indikator- und Statistikregeln verwenden ihre konfigurierten Vergleiche; alle Regeln einer Gruppe müssen erfüllt sein. Eine hybride Verlustbedingung verlangt die Kursschwelle und beide Regelgruppen. Bei konstanten Kursen kann kein Z-Score berechnet werden; die Entscheidung ist dann nicht verfügbar.

Die Basisausstiege schliessen die gesamte Restmenge der Strategie und haben Vorrang vor einer Teilgewinnmitnahme. Ein Ausstieg unterdrückt in derselben Auswertung einen widersprechenden Einstieg desselben Instruments. Einstiegswartefristen zählen Kalendertage seit der letzten Eröffnungs- beziehungsweise Reduktionstransaktion. Das Limit über 30 Tage zählt tatsächlich gebuchte Transaktionen einschliesslich Teilausführungen.

## Teilgewinne mitnehmen
Statt eine Position erst beim Gewinnziel vollständig zu schliessen, können Sie sie in mehreren Schritten abbauen. Dazu schalten Sie bei der Gewinnmitnahme die Teilverkäufe ein und beschreiben einen Plan aus einzelnen Stufen. Jede Stufe erhält eine eigene Kurzbezeichnung, eine Bedingung und den Anteil, der bei deren Erfüllung abgegeben wird. Die letzte Stufe darf statt eines Anteils die gesamte Restmenge schliessen. Bei einer Long-Position wird verkauft, bei einer Short-Position eingedeckt.

Sie wählen, worauf sich die Anteile beziehen. Beim Bezug auf die Ausgangsgrösse ist jeder Anteil ein Teil der Menge, mit der die Position eröffnet wurde; die Anteile dürfen zusammen die ganze Position nicht überschreiten. Beim Bezug auf die aktuelle Grösse ist jeder Anteil ein Teil dessen, was zum Zeitpunkt der Stufe noch offen ist. Zwei Stufen zu je 30% geben so zuerst 30% und danach 30% des verbleibenden Rests ab.

Eine Stufe wird durch einen prozentualen Gewinn gegenüber dem ersten oder dem gewichteten durchschnittlichen Einstiegskurs, durch einen Gewinnbetrag in der Währung des Mandanten oder durch Indikatorregeln ausgelöst. Die Stufen werden der Reihe nach abgearbeitet und nie übersprungen. Erreicht ein einzelner Kurssprung mehrere Ziele gleichzeitig, werden die betroffenen Stufen in derselben Auswertung zusammengefasst.

Jede Stufe wird pro Position höchstens einmal ausgeführt. Massgebend ist allein, wie viel der Position bereits zurückgegeben wurde; eine Benachrichtigung verändert den Ausführungsstand nie. Buchen Sie einen Vorschlag nur teilweise, schlägt die nächste Auswertung genau die fehlende Menge erneut vor, und ein Neustart des Programms ändert daran nichts. Vorgeschlagen wird nie mehr, als tatsächlich offen ist. Wird die Position geschlossen und später erneut eröffnet, beginnt der Plan von vorne. Ausgeführte Zielmengen mit derselben Stufenbezeichnung bleiben festgehalten; der übrige Fortschritt folgt dem konfigurierten Plan und den gebuchten Reduktionen.

## Eine Verlustposition aufstocken
Setzen Sie `loss_action` auf `B_average_down`. Diese Angabe allein wählt das Aufstocken; die Einstellungen der Stop-Loss-Variante werden dann nicht ausgewertet. Die Aufstockung benötigt die zusätzliche Verlustbedingung unter `trigger`. Long-Positionen werden zu tieferen Kursen aufgestockt, Short-Positionen zu höheren. Jede Aufstockung verlangt die nachteilige Kursschwelle. Indikator- oder Statistikregeln bestätigen sie zusätzlich; im hybriden Modus müssen beide Gruppen erfüllt sein. Fehlende Kursdaten lösen keine Aufstockung aus.

Unter `variant_B_average_down` legen Sie die Grösse jeder Aufstockung (`add_sizing`, wie beim Einstieg), die Höchstzahl der Aufstockungen (`max_adds`) und die Schrittregel (`add_step_rule`) fest. Beim Bezug auf den ersten Einstiegskurs erlauben eine Verlustschwelle von 10% und ein Schritt von 5% Aufstockungen bei 10%, 15% und 20% nachteiliger Bewegung. Erst ausgeführte Aufstockungen schalten die nächste Stufe frei. Beim Bezug auf den Durchschnittskurs verlangt jede Aufstockung den eingestellten Schritt gegenüber dem aktuellen gewichteten Durchschnitt und zusätzlich die Verlustbedingung. Indikatorbasierte Schritte (`indicator_based`) verwenden die Indikatorbestätigung ohne zusätzliche prozentuale Stufenfolge und setzen die Entscheidungsgrundlage `indicator` oder `hybrid` voraus. Benutzerdefinierte Schritte lassen sich nicht aktivieren. Der Durchschnittskurs wird nach jeder Aufstockung neu berechnet.

Übergeordnete Allokationen, Positionslimiten, Wartefristen und das rollierende Transaktionslimit gelten weiterhin. Zu grosse Aufstockungen werden blockiert. Eine überschrittene Verlustlimite verhindert Aufstockungen auch ohne erzwungenen Ausstieg; ist dieser eingeschaltet, wird die vollständige Schliessung vorgeschlagen. Gewinnmitnahmen und Ausstiege haben Vorrang, auch bei widersprechenden Empfehlungen einer anderen Strategie für dasselbe Instrument.

Die erste Teilmenge eines Aufstockungssignals zählt als eine Aufstockung. Weitere Teilmengen desselben Signals erhöhen den Zähler nicht; unausgeführte Vorschläge zählen nicht. Jede manuelle Erhöhung ohne Signalverknüpfung zählt einzeln. Teilausführungen des ursprünglichen Einstiegs bleiben der Eröffnung zugeordnet. Nach Schliessung und erneuter Eröffnung beginnt der Zähler von vorne.

Bei Gewinnstufen mit Bezug auf die aktuelle Menge werden zusätzliche Stücke beim Auslösen einer neuen Stufe berücksichtigt. Mit der ersten Ausführung wird die Zielmenge festgehalten. Weitere Aufstockungen vergrössern diese begonnene Stufe nicht und wiederholen keine abgeschlossene Stufe. Manuelle Reduktionen verbrauchen ausstehende Stufen der Reihe nach, ausgehend von der Menge vor der ersten Reduktion. Stufen mit Bezug auf die Ausgangsmenge behalten diese Grundlage.

## Ergebnis prüfen
Öffnen Sie **Alarmdiagnose** in der Alarmansicht des Portfolios und wählen Sie das Register **Handelsentscheidungen**. Welche Spalten es zeigt, was die Aktionen **Kauf**, **Verkauf**, **Halten** und **Blockiert** bedeuten und wann die Tabelle leer bleibt, beschreibt [Alarmdiagnose](../../alert/#handelsentscheidungen). **Aktualisieren** liest nur Ergebnisse, **Jetzt auswerten** berechnet sie neu. Nur **Kauf** und **Verkauf** führen zu einer Benachrichtigung; deren Zustellung zeigt das Register **Benachrichtigungen**.
Die Spalte **Begründung** erklärt jeden Entscheid. Die folgende Tabelle führt alle Begründungen mit der Aktion auf, unter der sie erscheinen. Bei **Blockiert** wartet ein Teil der Begründungen von selbst ab, etwa eine Wartefrist; die übrigen verlangen, dass Sie die Konfiguration oder die Daten ergänzen.

| Begründung | Aktion | Bedeutung und Abhilfe |
|---|---|---|
| **Einstiegsbedingung nach Kursrückgang erfüllt** | Kauf oder Verkauf | Die konfigurierte Kursbewegung ist eingetreten. Vorgeschlagen wird ein Einstieg in der Grösse des ersten Kaufs, bei einer Short-Position als Verkauf. |
| **Bestehende Position aufstocken** | Kauf oder Verkauf | Die Verlustposition soll aufgestockt werden, siehe [Eine Verlustposition aufstocken](#eine-verlustposition-aufstocken). |
| **Gewinn mitnehmen: Position schliessen** | Kauf oder Verkauf | Das Gewinnziel ist erreicht, die ganze Position soll geschlossen werden. |
| **Teilgewinn mitnehmen: Position reduzieren** | Kauf oder Verkauf | Eine Gewinnstufe ist erreicht, siehe [Teilgewinne mitnehmen](#teilgewinne-mitnehmen). |
| **Teilgewinn mitnehmen: Restposition schliessen** | Kauf oder Verkauf | Die letzte Gewinnstufe schliesst die verbleibende Position. |
| **Verlustbegrenzung: Position schliessen** | Kauf oder Verkauf | Der Stop-Loss ist erreicht. |
| **Ausstiegsbedingung bei Verlust: Position schliessen** | Kauf oder Verkauf | Die Ausstiegsbedingung der Verlustbehandlung ist erfüllt, siehe [Verlustbehandlung](#verlustbehandlung). |
| **Risikolimite überschritten: Position schliessen** | Kauf oder Verkauf | Eine Risikogrenze ist überschritten und der erzwungene Ausstieg eingeschaltet, siehe [Risikogrenzen](#risikogrenzen). |
| **Position halten** | Halten | Die Position ist offen, und weder eine Gewinn- noch eine Verlustbedingung ist erfüllt. |
| **Einstiegsbedingung nicht erfüllt** | Halten | Es besteht keine Position, und die Kursbewegung reicht für einen Einstieg noch nicht aus. |
| **Kursbewegung oder Bestätigung noch nicht ausgelöst** | Halten | Beim Aufstocken fehlt noch die nachteilige Kursschwelle oder die Bestätigung durch einen Indikator. |
| **Warten auf den nächsten nachteiligen Kursschritt** | Halten | Seit der letzten Aufstockung hat sich der Kurs noch nicht um den nächsten Schritt bewegt. |
| **Einstiegssperrfrist oder Handelslimite erreicht** | Blockiert | Eine Wartefrist nach einem Kauf oder Verkauf läuft noch, oder die Höchstzahl der Transaktionen innerhalb von 30 Tagen ist erreicht, siehe [Wartefristen](#wartefristen). Die Sperre endet von selbst. |
| **Ungenügender Investitionsspielraum** | Blockiert | Der Einstieg würde die Gewichtung der Portfoliobasierten Strategie, der Anlageklasse oder des Wertpapiers oder den höchsten Anteil der Position am Nettovermögen überschreiten. Erhöhen Sie die Gewichtung oder verkleinern Sie den ersten Kauf. |
| **Beide Einstiegsrichtungen ausgelöst** | Blockiert | Long- und Short-Einstieg sind gleichzeitig erfüllt. GT schlägt deshalb keinen der beiden vor. |
| **Für dieses Instrument steht ein Ausstieg an** | Blockiert | Eine andere Strategie schlägt am selben Tag einen Ausstieg aus diesem Wertpapier vor. Ein gleichzeitiger Einstieg würde diesen Ausstieg wieder aufheben. |
| **Risikokontrollen verhindern eine Aufstockung** | Blockiert | Die Aufstockung würde eine Risikogrenze verletzen. |
| **Maximale Anzahl ausgeführter Aufstockungen erreicht** | Blockiert | Die eingestellte Zahl an Aufstockungen ist ausgeschöpft. |
| **Bestehende Buchungen zuerst ihren Strategien zuordnen** | Blockiert | Eine offene Menge des Wertpapiers hat keine **Strategiezuordnung**, siehe [Tatsächliche Transaktionen zuordnen](#tatsächliche-transaktionen-zuordnen). |
| **Instrument ist nicht in der Watchlist der Hierarchie** | Blockiert | Nehmen Sie das Wertpapier in die Watchlist auf, mit der die Portfoliobasierte Strategie verknüpft ist. |
| **Instrument ist mehreren Allokationen zugeordnet** | Blockiert | Das Wertpapier steht in mehr als einer Anlageklasse. Ordnen Sie es genau einer zu. |
| **Aktive Allokation und ihre Gewichtungen vervollständigen** | Blockiert | Das Wertpapier ist keiner Anlageklasse zugeordnet, eine Gewichtung fehlt, oder die Gewichtungen ergeben nicht je 100%, siehe [Strategie einrichten](#strategie-einrichten). |
| **Einstandspreis der Position fehlt** | Blockiert | Für die offene Position lässt sich kein Einstandspreis bestimmen, deshalb sind Gewinn und Verlust unbekannt. |
| **Leerverkäufe benötigen ein Margin-Instrument** | Blockiert | Ein Short-Einstieg ist nur bei einem Margin-Instrument möglich. |
| **Eine einheitliche Margin-Kontraktdefinition ist erforderlich** | Blockiert | Für das Margin-Instrument fehlt eine bestehende Transaktion, oder die Transaktionen verwenden unterschiedliche Werte pro Punkt. |
| **Abgeschlossener Tagesschlusskurs fehlt** | Blockiert | Für den letzten abgeschlossenen Handelstag liegt kein Schlusskurs vor. Prüfen Sie die historischen Kurse des Wertpapiers. |
| **Ungenügende Kurshistorie** | Blockiert | Für die Einstiegsreferenz oder einen Indikator sind zu wenige Schlusskurse vorhanden. |
| **Ungültige oder zukünftige Marktbeobachtung** | Blockiert | Ein historischer Kurs ist ungültig oder liegt in der Zukunft. |
| **Statistische Regel nicht verfügbar: konstante Kurse** | Blockiert | Die Kurse des Zeitfensters sind alle gleich, eine statistische Regel lässt sich deshalb nicht berechnen. |
| **Indikatordaten sind nicht verfügbar** | Blockiert | Ein in der Konfiguration verwendeter Indikator lässt sich mit den vorhandenen Kursen nicht berechnen. |
| **Historischer Wechselkurs fehlt** | Blockiert | Für die Umrechnung in die Währung des Mandanten fehlt ein Wechselkurs. |

Steht in der Spalte ein technischer Text, konnte die Strategie aus einem anderen Grund nicht ausgewertet werden; die Meldung wird unverändert angezeigt.

Wie die Strategie in der Vergangenheit entschieden hätte, zeigt eine [Historische Wiederholung](../../historicalrun/) in einer Simulationsumgebung. Dort gelten die Gebührenmodelle der Depots. Aufstockungen verwenden dieselbe Auswertung von Tagesschlusskursen und Ausführungen.
