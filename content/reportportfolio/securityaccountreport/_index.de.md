---
title: "Depots"
date: 2025-11-17T22:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
Der Depotbericht bietet eine umfassende Übersicht über alle Wertpapierpositionen eines Mandanten. Mithilfe dieses Reports können verschiedene Gruppierungsansichten zur Analyse der Vermögensstruktur erstellt werden. Zudem werden detaillierte Informationen zu Beständen, Kursen und Gewinnen für jedes Wertpapier angezeigt.

{{% notice style="info" title="Depotbericht auf verschiedenen Ebenen" %}}
Dieser Bericht ist auf verschiedenen Ebenen verfügbar. Klicken Sie im Navigationsbaum auf ein einzelnes Depot, um den Bericht für dieses Depot aufzurufen. Zudem gibt es den Bericht auf einer Ebene höher, dort heisst er „Depots”. Wenn nur ein Depot für ein Portfolio vorhanden ist, liefern diese Reports auf beiden Ebenen dieselben Ergebnisse.
{{% /notice %}}

## Funktionsweise
Der Report wertet alle Wertpapierpositionen des Mandanten aus. Dabei werden alle Transaktionen bis zum gewählten Datum berücksichtigt, um eine zeitpunktgenaue Darstellung der Bestände und Gewinne zu gewährleisten. Die Positionen werden gruppiert dargestellt, wobei für jede Gruppe Zwischensummen und am Ende eine Gesamtsumme ausgewiesen werden.

## Gruppierungsoptionen
Der Report bietet verschiedene Gruppierungsmöglichkeiten, die über das Dropdown-Menü ausgewählt werden können:

- **Gruppiert nach Währung**: Standardansicht. Die Positionen werden nach ihrer Handelswährung gruppiert. Für jede Währung wird der Wechselkurs zur Hauptwährung angezeigt. Dies ermöglicht eine klare Übersicht über Währungsexpositionen und erleichtert die Analyse von Währungsrisiken.
- **Gruppiert nach Anlageklasse**: Die Positionen werden nach ihrer Anlageklasse gruppiert, beispielsweise Aktien, Anleihen, Geldmarkt, Rohstoffe oder Immobilien. Dies ermöglicht eine Asset-Allokations-Analyse.
- **Finanzinstrument gruppiert**: Gruppierung nach spezialisiertem Investmentinstrument wie ETF, Investmentfonds, CFD, Optionen oder Futures. Diese Ansicht ist hilfreich zur Analyse der verwendeten Anlageinstrumente.
- **Unter Anlageklasse gruppiert**: Gruppierung nach Vermögensunterkategorie, was eine detailliertere Analyse innerhalb der Anlageklassen ermöglicht.

## Spalten im Report
Einige Spalten werden in der Währung des Wertpapiers angezeigt, andere in der Hauptwährung. Diese ist an dem Suffix der Spaltenüberschrift erkennbar. Der Report zeigt für jede Position folgende Spalten an:
- **Name**: Bezeichnung des Wertpapiers. Es wird der offizielle Name verwendet.
- **I**: Icon für das Finanzinstrument. Dieses Symbol visualisiert die Art des Instruments (z.B. Aktie, Anleihe, ETF, Währungspaar) und erleichtert die schnelle Zuordnung in der Tabelle.
- **Bestand**: Anzahl der gehaltenen Einheiten des Wertpapiers. Diese Spalte zeigt den aktuellen Bestand in der jeweiligen Handelswährung an. Bei Währungspaaren kann dieser Wert auch negativ sein.
- **Währung**: Die Handelswährung des Wertpapiers.
- **Zeit/Datum**: Zeitpunkt des letzten verfügbaren Kurses. Diese Information zeigt, auf welchem Stand die Bewertung basiert.
- **Kurs**: Der Kurs des Wertpapiers des oben angegebenen Datums. Falls kein Kurs für diesen Tag vorhanden ist, wird der Kurs eines vorherigen Datums angezeigt.
- **Gewinn Wertpapier**: Gewinn oder Verlust für dieses Wertpapier in der Handelswährung. Dieser Wert berücksichtigt sowohl realisierte als auch nicht realisierte Gewinne und Verluste inklusive Währungsgewinne. Die Spalte wird nur in der Gruppierung nach Währung angezeigt.
- **Wertpapier Risiko (Haputwährung)**: Das Risiko-Exposure der Position in der Hauptwährung. Diese Spalte wird bei Gruppierung nach Währung angezeigt und ist besonders relevant für Margin-Produkte und gehebelte Instrumente. Beispielsweise haben Sie eine Stückzahl von 10 eines zweifach inversen ETFs zum Kurs von 100. Damit ergibt sich ein negatives Wertpapierrisiko von 2.000. So können Sie das Risiko Ihrer Investitionen besser einschätzen.
- **Gehebelt Invers**: Es gibt gehebelte ETFs. Hier wird der Faktor angezeigt, falls dieser von einer 1 abweicht. Ein Minuszeichen bedeutet, dass es sich um einen Inverse-ETF handelt.
- **Gewinn Wertpapier (Hauptwährung)**: Der Gewinn oder Verlust in der Hauptwährung des Mandanten. Alle Werte werden automatisch in die Hauptwährung umgerechnet.
- **Konto relevant**: Der anrechenbare Wert der Position in der Handelswährung. Bei normalen Wertpapieren entspricht dies dem Marktwert der Position. Bei Margin-Produkten wird hier der Kontowert unter Berücksichtigung des Hebels ausgewiesen. Diese Spalte wird nur in der Gruppierung nach Währung angezeigt.
- **Konto relevant (Hauptwährung)**: Der anrechenbare Wert der Position in der Hauptwährung des Mandanten. 

### Gruppensummen und Gesamtsummen
Für jede Gruppe werden Zwischensummen angezeigt. Bei der Gruppierung nach Währung wird zusätzlich der Wechselkurs zur Hauptwährung ausgewiesen. Am Ende des Reports erscheint die Gesamtsumme über alle Gruppen hinweg.

## Filteroptionen
Der Report bietet verschiedene Filtermöglichkeiten zur Anpassung der Ansicht:
- **Bis Datum**: Der Report kann auf einen bestimmten Stichtag eingestellt werden, wodurch die Portfoliosicht zu einem historischen Zeitpunkt dargestellt wird. Dies ist besonders nützlich für Vergleiche oder Jahresabschlüsse. Durch Klick auf das Wiederholungs-Icon neben dem Datumsfeld wird das Datum auf heute zurückgesetzt.
- **Geschlossene Positionen einbeziehen**: Über das Kontextmenü kann gewählt werden, ob auch bereits geschlossene Positionen im Report angezeigt werden sollen. Standardmässig werden nur aktive Positionen angezeigt.

## Kontextmenü auf selektierten Instrument
Es sind die meisten Funktionen, die auch in der Watchlist verfügbar sind. Siehe hierzu [Funktionen selektiertes Instrument](../../watchlistinstrument/watchlist/#funktionen-markierte-watchlist-ansicht)

## Expandierbare Zeilen
Jede Position kann durch Klick auf das Expander-Symbol expandiert werden. In der expandierten Ansicht werden alle **Transaktionen** für dieses Wertpapier angezeigt. Dies ermöglicht eine detaillierte Nachverfolgung aller Käufe, Verkäufe, Dividenden und anderen Transaktionen, die zu der aktuellen Position geführt haben.

### Kontextmenü auf Transaktionen
Auch an dieser Stelle können die einzelnen bestehenden Transkation bearbeitet werden. Weitereführende Informationen unter  [Wertpapiertransaktionen](../../../transaction/security/).

## Diagrammansicht
Über die Funktion «Zeige Diagramm» im Kontextmenü wird eine grafische Darstellung der Portfolioallokation generiert. Das Kreisdiagramm visualisiert die prozentuale Verteilung entsprechend der gewählten Gruppierung. Dies ermöglicht eine schnelle Einschätzung der Diversifikation und zeigt auf einen Blick, wie das Vermögen verteilt ist.

## Besonderheiten
- **Wertpapierorientierte Sicht**: Im Gegensatz zur Kontoübersicht, die Konten in den Vordergrund stellt, fokussiert dieser Report auf die Wertpapiere und deren Performance.
- **Mehrwährungsunterstützung**: Der Report unterstützt Positionen in verschiedenen Währungen. Alle Werte werden automatisch in die Hauptwährung des Mandanten umgerechnet.
- **Margin-Produkte**: Für gehebelte Instrumente wird der anrechenbare Kontowert unter Berücksichtigung des Hebels berechnet. Der Hebelfaktor wird in einer separaten Spalte ausgewiesen.
- **Währungsgewinne**: Bei der Gruppierung nach Währung werden Währungsgewinne und -verluste separat ausgewiesen, was eine präzise Analyse der Währungseffekte ermöglicht.
- **Transaktionsdetails**: Durch Expandieren der Zeilen können alle zugrundeliegenden Transaktionen eingesehen werden, was eine lückenlose Nachverfolgung ermöglicht.
