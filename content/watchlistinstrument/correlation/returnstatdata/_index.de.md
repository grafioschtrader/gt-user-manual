---
title: "Rendite und statistische Daten"
date: 2026-05-13T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
Diese Auswertung beinhaltet die **jährliche Rendite** und mehrere **statistische Kennzahlen** für ein **einzelnes Instrument**. Sie wird durch Aufklappen einer Zeile in der [Korrelationsmatrix]({{< relref "/watchlistinstrument/correlation" >}}) sichtbar gemacht.

- **Zeitraum**: Die **Rendite** wird über den gesamten Zeitraum der vorliegenden Tagesendkurse ermittelt. Der Zeitraum für die Berechnung der **statistischen Kennzahlen** lässt sich über die Datumseingaben der Korrelationsmatrix beschränken.
- **Währung Klient und Instrument**: Die Währung des Instruments kann von der Währung des Klienten (Hauptwährung) abweichen. Daher werden die berechneten Resultate in zwei Spalten dargestellt — einmal in der Instrumentenwährung und einmal in der Hauptwährung. Für Währungspaare und Forex wird die Berechnung in der Hauptwährung nicht durchgeführt.

## Rendite
Die Berechnungen basieren auf den letzten verfügbaren Schlusskursen innerhalb eines Jahres — bestenfalls vom 31.12. des vorhergehenden Jahres bis zum 31.12. des auszuwertenden Jahres. Falls **Dividenden** für das entsprechende Instrument vorliegen, werden diese zusätzlich hinzugerechnet, wobei der **Ex-Tag** die Jahreszuordnung bestimmt. Bei abweichender Instrumentenwährung verwendet GT die historischen Wechselkurse, um die zweite Spalte in der Hauptwährung zu liefern.

#### Rendite pro Kalenderjahr
Für jedes vollständig vorliegende Kalenderjahr sowie für das aktuelle Jahr wird die Rendite berechnet. Das Resultat ist eine prozentuale Gesamtperformance, die sowohl Kursveränderung als auch Dividenden berücksichtigt.

#### Annualisierte Rendite
Die annualisierte Rendite verdichtet die einzelnen Jahresrenditen mittels **geometrischem Mittel** (Compound Annual Growth Rate). GT berechnet sie für die Standard-Horizonte **1, 3, 5 und 10 Jahre** sowie zusätzlich für den **gesamten verfügbaren Zeitraum**, sofern dieser länger als 10 Jahre ist. Das laufende Jahr wird ausgeklammert, damit ein unvollständiges Jahr die mehrjährigen Werte nicht verzerrt.

## Statistische Kennzahlen
Die Hierarchietabelle gliedert die statistischen Kennzahlen nach **Stichprobenzeitraum** (täglich, monatlich, jährlich). Pro Stichprobenzeitraum werden eine oder mehrere Eigenschaften ausgewiesen, jeweils in Instrumentenwährung und Hauptwährung.

- **Standardabweichung** — Streuung der prozentualen Veränderungen. Sie ist die Basis für eine Volatilitätseinschätzung und ergibt sich aus den Tages-, Monats- beziehungsweise Jahreswerten.
- **Minimum** und **Maximum** — kleinste und grösste prozentuale Tagesveränderung im Beobachtungszeitraum. Diese beiden Werte werden nur auf täglicher Basis ausgegeben.

{{% notice style="info" title="Jahres-Standardabweichung" %}}
Die jährliche Standardabweichung wird nicht aus Jahresrenditen direkt berechnet, sondern aus der monatlichen Standardabweichung über den Faktor √12 hochgerechnet. Damit lässt sich die Volatilität auch dann sinnvoll annualisieren, wenn nur wenige vollständige Jahre vorliegen.
{{% /notice %}}

{{% notice style="note" title="Wirkung der Datumseinschränkung" %}}
Die Datumsfelder der Korrelationsmatrix wirken sich auf alle statistischen Kennzahlen — und damit auch auf die im nächsten Abschnitt beschriebene Sharpe-Ratio — aus. Die Renditeberechnung weiter oben verwendet hingegen weiterhin den gesamten Zeitraum der vorliegenden Schlusskurse.
{{% /notice %}}

## Sharpe-Ratio
Die **Sharpe-Ratio** ist eine risikoadjustierte Kennzahl, die das Verhältnis zwischen **Überrendite** und **Schwankung** misst. Eine höhere Sharpe-Ratio bedeutet, dass das Instrument für jede Einheit Risiko mehr Rendite oberhalb des risikofreien Zinssatzes geliefert hat.

```
Sharpe = (annualisierte Rendite − ⌀ risikofreier Zinssatz) ÷ annualisierte Standardabweichung
```

Die Kennzahl wird pro Horizont in der Spalte **Annualisierte Rendite** der Hierarchietabelle ausgewiesen — sowohl in Instrumentenwährung als auch in Hauptwährung. Bei gleicher Währung sind beide Werte identisch. Bei abweichender Instrumentenwährung verwendet die Hauptwährungsvariante den risikofreien Zinssatz der **Klient-Hauptwährung**, während die Instrumentenwährungsvariante den Zinssatz der **Instrumentenwährung** verwendet.

Der durchschnittliche risikofreie Zinssatz pro Horizont ist das arithmetische Mittel der täglichen Zinssatzwerte im Fenster `[heute − Anzahl Jahre, heute]`. Liegt im Fenster kein einziger Datenpunkt vor, fällt GT auf den **zuletzt bekannten** Zinssatz vor dem Endedatum zurück (Fortschreibung).

{{% notice style="warning" title="Voraussetzung: Risikofreier-Zinssatz-Zuordnung" %}}
Die Berechnung benötigt für jede Währung eine gültige Zeile in der [Risikofreier-Zinssatz-Zuordnung]({{< relref "/basedata/riskfreeratemapping" >}}). Fehlt die Zuordnung — oder enthält das verknüpfte Wertpapier noch keine historischen Schlusskurse — bleibt die Sharpe-Ratio in der entsprechenden Spalte leer.
{{% /notice %}}

{{% notice style="info" title="Aktuelle Vereinfachung" %}}
Alle Horizonte teilen sich derzeit dieselbe annualisierte Standardabweichung — nämlich den Wert, der eine Zeile höher in der Statistiktabelle steht. Eine horizontspezifische Standardabweichung würde pro Horizont eine zusätzliche Datenbankabfrage erfordern und ist als künftige Verfeinerung vorgesehen.
{{% /notice %}}

Die Sharpe-Ratio bleibt zudem leer, wenn die annualisierte Standardabweichung null oder nicht endlich ist — etwa weil die Schlusskurse konstant sind oder zu wenige Datenpunkte für eine sinnvolle Streuungsberechnung vorliegen.

{{% notice style="note" title="Heute nur eine Verwendung" %}}
Die Sharpe-Ratio wird im aktuellen Stand ausschliesslich an dieser Stelle angezeigt. Weitere Auswertungen, die den risikofreien Zinssatz konsumieren, sind möglich, aber noch nicht umgesetzt.
{{% /notice %}}
