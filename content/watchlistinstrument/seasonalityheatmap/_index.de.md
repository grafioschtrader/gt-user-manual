---
title: "Saisonalitäts-Heatmap"
date: 2026-06-24T10:00:00+02:00
draft: false
weight: 35
archetype: "default"
---
Die **Saisonalitäts-Heatmap** stellt die Renditen eines **Instruments** über mehrere Jahre als farbige Tabelle dar. Jede Zeile steht für ein Kalenderjahr, jede Spalte für einen Monat oder ein Quartal. Eine Zelle ist umso grüner, je positiver die Rendite jener Periode war, und umso roter, je negativer sie ausfiel. So lassen sich wiederkehrende Muster im Jahresverlauf erkennen, etwa eine traditionell schwache Sommerphase oder eine Jahresendrally. Die Ansicht steht für **Wertpapiere**, **abgeleitete Wertpapiere** und **Währungspaare** zur Verfügung, sofern eine genügend lange Kurshistorie vorhanden ist.

{{% notice info %}}
Die Berechnung beruht auf den **Schlusskursen** der historischen Kursdaten. Für jeden Monat wird der letzte verfügbare Handelstag verwendet; ein Quartals- oder Jahreswert leitet sich aus dem letzten Monatswert der jeweiligen Periode ab.
{{% /notice %}}

## Aktivieren dieser Ansicht
Die Saisonalitäts-Heatmap erscheint im **Zusatzbereich**, also am gleichen Ort wie das [Tagesendkurse-Diagramm](../eodchart/) und die Tagesendkurse als Tabelle. Sie wird über das **Kontextmenü** eines Instruments mit dem Eintrag **Saisonalität** geöffnet und ersetzt dabei die bisher im Zusatzbereich angezeigte Ansicht. Wie beim Diagramm steht der Eintrag überall dort zur Verfügung, wo eine Tabelle eine Zeile pro Instrument enthält, in der Praxis vor allem in der Watchlist.

## Einstellungen
Oberhalb der Tabelle stehen drei Eingaben bereit, welche die Heatmap bei jeder Änderung neu berechnen.

- **Periode**: Wahl zwischen **Monatlich** und **Quartalsweise**. Bei der monatlichen Darstellung enthält die Tabelle zwölf Spalten, bei der quartalsweisen vier.
- **Dividenden einbeziehen**: Ist diese Option gewählt, fliessen Dividenden und Zinsen in die Rendite ein. Eine Ausschüttung wird dem Monat beziehungsweise Quartal ihres Ex-Tags zugerechnet. Die Option ist nur auswählbar, wenn das Instrument überhaupt Dividenden oder Zinsen ausschüttet.
- **In Hauptwährung**: Damit werden die Renditen in die **Hauptwährung des Mandanten** umgerechnet. Die Option ist nicht verfügbar bei Währungspaaren sowie dann, wenn das Instrument bereits in der Hauptwährung geführt wird.

## Jahresrendite und Fusszeile
Als letzte Spalte zeigt die Tabelle die **Jahresrendite** des jeweiligen Jahres. In der Fusszeile werden je Spalte drei Kennzahlen über alle Jahre ausgewiesen: der **Durchschnitt**, der **Median** und **Positiv %**, also der Anteil der Jahre, in denen die betreffende Periode positiv abgeschlossen hat. Gerade **Positiv %** ist eine der aussagekräftigsten Grössen einer Saisonalitätsbetrachtung, da sie zeigt, wie verlässlich sich ein Muster über die Jahre wiederholt hat.

{{% notice note "Farbgebung ist instrumentbezogen" %}}
Die Einfärbung richtet sich nach der Verteilung der Renditen **innerhalb des betrachteten Instruments**: Der stärkste Verlust wird voll rot, der stärkste Gewinn voll grün dargestellt, die übrigen Werte liegen dazwischen. Die Jahresrendite-Spalte wird dabei eigenständig eingefärbt, da Jahreswerte naturgemäss grösser ausfallen als Monats- oder Quartalswerte. Aus diesem Grund sind die Farben **nicht zwischen verschiedenen Instrumenten vergleichbar** – dieselbe Farbe bedeutet bei einem ruhigen und bei einem sehr schwankungsfreudigen Instrument unterschiedliche Prozentwerte.
{{% /notice %}}

{{% notice warning %}}
### Kurs- oder Gesamtrendite
Wie im [Glossar](../../glossar/) erwähnt, ist zwischen **ausschüttenden** und **thesaurierenden** ETFs beziehungsweise zwischen **Kurs-** und **Performanceindizes** zu unterscheiden; Ähnliches gilt für **Aktien** mit oder ohne **Dividende**. Standardmässig beruht die Heatmap auf den reinen **Schlusskursen**. Über die Option **Dividenden einbeziehen** lässt sich die Rendite um die Ausschüttungen ergänzen, soweit für das Instrument entsprechende Daten vorliegen. Der Benutzer sollte sich daher stets bewusst sein, welche Art von Rendite er gerade betrachtet.
{{% /notice %}}

{{% notice info %}}
Das Beschaffen der Kursdaten für diese Ansicht zählt zum Tageskontingent unterschiedlicher Instrumente, siehe [Tageskontingent für das Beziehen historischer Kursdaten](../externaldata/historyquote/pricedata/).
{{% /notice %}}
