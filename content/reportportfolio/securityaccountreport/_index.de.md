---
title: "Depots"
date: 2026-10-05T22:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
Der Depotbericht bietet eine umfassende Übersicht über alle Wertpapierpositionen eines Mandanten. Mithilfe dieses Reports können verschiedene Gruppierungsansichten zur Analyse der Vermögensstruktur erstellt werden. Zudem werden detaillierte Informationen zu Beständen, Kursen und Gewinnen für jedes Wertpapier angezeigt.

{{% notice style="info" title="Depotbericht auf verschiedenen Ebenen" %}}
Dieser Bericht ist auf drei Ebenen verfügbar. Für den ganzen Mandanten finden Sie ihn im Register «Depots», das hier beschrieben wird. Für ein Portfolio klicken Sie im Navigationsbaum unterhalb des Portfolios auf «Depots», für ein einzelnes Depot auf das Depot selbst. Wenn nur ein Depot für ein Portfolio vorhanden ist, liefern die Berichte für das Portfolio und für das Depot dieselben Ergebnisse.
{{% /notice %}}

## Funktionsweise
Der Report wertet alle Wertpapierpositionen des Mandanten aus. Dabei werden alle Transaktionen bis zum gewählten Datum berücksichtigt, um eine zeitpunktgenaue Darstellung der Bestände und Gewinne zu gewährleisten. Die Positionen werden gruppiert dargestellt, wobei für jede Gruppe Zwischensummen und am Ende eine Gesamtsumme ausgewiesen werden.

## Gruppierungsoptionen
Der Report bietet verschiedene Gruppierungsmöglichkeiten, die über das Dropdown-Menü neben der Überschrift ausgewählt werden können:

- **Währung gruppiert**: Standardansicht. Die Positionen werden nach ihrer Handelswährung gruppiert. Für jede Währung wird der Wechselkurs zur Hauptwährung angezeigt. Dies ermöglicht eine klare Übersicht über Währungsexpositionen und erleichtert die Analyse von Währungsrisiken.
- **Anlageklassen gruppiert**: Die Positionen werden nach ihrer Anlageklasse gruppiert, beispielsweise Aktien, Anleihen, Geldmarkt, Rohstoffe oder Immobilien. Dies ermöglicht eine Asset-Allokations-Analyse.
- **Finanzinstrument gruppiert**: Gruppierung nach Finanzinstrument wie ETF, Investmentfonds, CFD, Forex oder Direktanlage. Diese Ansicht ist hilfreich zur Analyse der verwendeten Anlageinstrumente.
- **Unter Anlageklasse gruppiert**: Gruppierung nach der Unterkategorie der Anlageklasse, was eine detailliertere Analyse innerhalb der Anlageklassen ermöglicht.
- **Kombination gruppiert**: Jede Kombination aus Anlageklasse, Unterkategorie und Finanzinstrument bildet eine eigene Gruppe. Dies ist die feinste Gruppierung.

## Spalten im Report
Einige Spalten werden in der Währung des Wertpapiers angezeigt, andere in der Hauptwährung. Spalten in der Hauptwährung tragen deren Kürzel als Suffix in der Spaltenüberschrift, beispielsweise «Gewinn Wertpapier CHF». In der folgenden Liste steht dafür «(Hauptwährung)». Die Spalten sind in allen Gruppierungen dieselben:
- **Name**: Bezeichnung des Wertpapiers. Es wird der offizielle Name verwendet.
- **I**: Icon für das Finanzinstrument. Dieses Symbol visualisiert die Art des Instruments (z.B. Aktie, Anleihe, ETF, Währungspaar) und erleichtert die schnelle Zuordnung in der Tabelle.
- **Bestand**: Anzahl der gehaltenen Einheiten des Wertpapiers. Bei Währungspaaren und Short-Positionen kann dieser Wert auch negativ sein.
- **Währung**: Die Handelswährung des Wertpapiers.
- **Zeit/Datum**: Zeitpunkt des letzten verfügbaren Kurses. Diese Information zeigt, auf welchem Stand die Bewertung basiert.
- **Kurs**: Der Kurs des Wertpapiers des oben angegebenen Datums. Falls kein Kurs für diesen Tag vorhanden ist, wird der Kurs eines vorherigen Datums angezeigt. Ein gelb hinterlegter Kurs wurde nie gehandelt, sondern beim Schliessen von Lücken in den historischen Kursdaten errechnet, siehe [Aktualität der Kurse](../../watchlistinstrument/watchlist/pricefreshness/).
- **Gewinn Wertpapier**: Gewinn oder Verlust für dieses Wertpapier in der Handelswährung. Dieser Wert berücksichtigt sowohl realisierte als auch nicht realisierte Gewinne und Verluste. Da er in der Handelswährung des Instruments ausgewiesen wird, enthält er keinen Währungseffekt, dieser steht in der Spalte **Währungsgewinn**. Eine Zwischensumme gibt es nur in der Gruppierung nach Währung, weil sich Beträge in unterschiedlichen Währungen nicht addieren lassen.
- **Wertpapier Risiko (Hauptwährung)**: Das Risiko-Exposure der Position in der Hauptwährung, also der Marktwert multipliziert mit dem Hebelfaktor. Diese Spalte ist besonders relevant für Margin-Produkte und gehebelte Instrumente, denn bei einem CFD steht hier der volle Marktwert der Position und nicht nur deren Gewinn oder Verlust. Beispielsweise haben Sie eine Stückzahl von 10 eines zweifach inversen ETFs zum Kurs von 100. Damit ergibt sich ein negatives Wertpapierrisiko von 2'000. So können Sie das Risiko Ihrer Investitionen besser einschätzen.
- **Gehebelt Inverse**: Es gibt gehebelte ETFs. Hier wird der Faktor angezeigt, falls dieser von einer 1 abweicht. Ein Minuszeichen bedeutet, dass es sich um einen Inverse-ETF handelt.
- **Gewinn Wertpapier (Hauptwährung)**: Der Gewinn oder Verlust in der Hauptwährung des Mandanten. Alle Werte werden automatisch in die Hauptwährung umgerechnet.
- **Währungsgewinn (Hauptwährung)**: Der Anteil des Ergebnisses, der allein aus der Bewegung des Wechselkurses gegenüber der Hauptwährung entstanden ist. Zusammen mit **Gewinn Wertpapier (Hauptwährung)** ergibt sich das gesamte Ergebnis der Position in der Hauptwährung. Der Wert kann negativ sein, obwohl das Instrument zugelegt hat. Siehe [Währungsgewinn]({{% ref "/reportportfolio#währungsgewinn" %}}).
- **Konto relevant**: Der Betrag, der bei einem Verkauf zum Stichtag dem Konto gutgeschrieben würde, in der Handelswährung. Bei normalen Wertpapieren entspricht dies dem Marktwert der Position. Bei CFD und Forex ist es der noch nicht realisierte Gewinn oder Verlust der offenen Positionen, denn nur dieser Betrag würde beim Schliessen gebucht. Eine Zwischensumme gibt es nur in der Gruppierung nach Währung.
- **Konto relevant (Hauptwährung)**: Derselbe Betrag in der Hauptwährung des Mandanten. Die Gesamtsumme dieser Spalte ist das Total des Reports.
- **Anteil %**: Der Anteil der Position am Total des Reports, siehe den folgenden Abschnitt.
- **Veräusserungskosten (Hauptwährung)**: Nur sichtbar, wenn die Schätzung der Veräusserungskosten eingeschaltet ist. Geschätzte Kommission, Transaktionssteuer und Aufschlag bei der Währungsumrechnung eines Verkaufs der ganzen Position zum Stichtag. Eine gelb hinterlegte Zelle bedeutet, dass ein Teil nicht geschätzt werden konnte; der Tooltip nennt den Grund. Siehe [Veräusserungskosten]({{% ref "/reportportfolio#veräusserungskosten" %}}).
- **Wert nach Veräusserung (Hauptwährung)**: **Konto relevant (Hauptwährung)** abzüglich der geschätzten Veräusserungskosten. Ebenfalls nur bei eingeschalteter Schätzung sichtbar.

### Anteil %
Die Spalte **Anteil %** zeigt, welchen Teil des Totals eine Position, eine Gruppe oder ein Barguthaben ausmacht. Gerechnet wird mit der Spalte **Konto relevant (Hauptwährung)**: Der Wert der Zeile wird durch die Gesamtsumme dieser Spalte geteilt. Die Anteile aller Positionen ergeben deshalb zusammen immer 100%, und die Zeile der Gesamtsumme zeigt 100.

Eine Position mit negativem Wert hat einen negativen Anteil. Das betrifft vor allem CFD und Forex im Verlust, aber auch eine Short-Position oder ein überzogenes Barguthaben. Die übrigen Positionen ergeben dann zusammen mehr als 100%. Ein Beispiel: Sie halten ein Wertpapier im Wert von 10'000 und einen CFD, der mit 5'000 im Minus steht. Das Total beträgt 5'000, das Wertpapier hat einen Anteil von 200% und der CFD einen von −100%. Das Wertpapier ist also doppelt so viel wert wie das Nettovermögen, weil der Verlust des CFD dieses schmälert.

Die Spalte zeigt den Anteil am Vermögen und nicht das Risiko. Ein CFD mit einem Marktwert von 100'000 und einem Gewinn von 100 hat nur einen kleinen Anteil, obwohl er das Vermögen stark bewegen kann. Das Risiko zeigt die Spalte **Wertpapier Risiko**. Ist das Total null oder negativ, bleibt die Spalte leer, denn Anteile an einem negativen Total hätten umgekehrte Vorzeichen und wären nicht aussagekräftig.

{{% notice style="tip" title="Total des Depotberichts" %}}
Der Depotbericht enthält nur Wertpapiere, deshalb ist **Anteil %** hier der Anteil an den Wertpapieren. Den Anteil am gesamten Vermögen einschliesslich der Barguthaben zeigt der Bericht [Anlageklassen mit Cash](../securitycashaccountreport/).
{{% /notice %}}

### Gruppensummen und Gesamtsummen
Für jede Gruppe werden Zwischensummen angezeigt. Bei der Gruppierung nach Währung steht in der Gruppenzeile zusätzlich der Wechselkurs zur Hauptwährung. Am Ende des Reports erscheint die Gesamtsumme über alle Gruppen hinweg. Negative Beträge erscheinen in allen Zahlenspalten rot.

## Filteroptionen
Der Report bietet verschiedene Filtermöglichkeiten zur Anpassung der Ansicht:
- **Bis Datum**: Der Report kann auf einen bestimmten Stichtag eingestellt werden, wodurch die Portfoliosicht zu einem historischen Zeitpunkt dargestellt wird. Dies ist besonders nützlich für Vergleiche oder Jahresabschlüsse. Durch Klick auf das Wiederholungs-Icon neben dem Datumsfeld wird das Datum auf heute zurückgesetzt.
- **Zeige geschlossene Positionen**: Über das Kontextmenü kann gewählt werden, ob auch bereits geschlossene Positionen im Report angezeigt werden sollen. Ein Häkchen zeigt an, dass die Option aktiv ist. Standardmässig werden nur offene Positionen angezeigt.

## Kontextmenü
Das Kontextmenü enthält immer die Einträge **Spalten anzeigen...**, **Zeige Diagramm**, **Zeige geschlossene Positionen** und **PDF-Bericht...**. Mit **Spalten anzeigen...** lassen sich einzelne Spalten ein- oder ausblenden; die Auswahl bleibt gespeichert.

### Kontextmenü auf selektiertem Instrument
Ist ein Wertpapier selektiert, kommen die meisten Funktionen hinzu, die auch in der Watchlist verfügbar sind, siehe hierzu [Funktionen selektiertes Instrument](../../watchlistinstrument/watchlist/#funktionen-markierte-watchlist-ansicht). Zudem lassen sich mit **Kaufen**, **Verkaufen** und **Zins/Dividende...** direkt Transaktionen für dieses Wertpapier erfassen. **Verkaufen** ist nur bei einem vorhandenen Bestand möglich, und **Zins/Dividende...** fehlt bei CFD und Forex.

## Expandierbare Zeilen
Jede Position kann durch Klick auf das Expander-Symbol expandiert werden. In der expandierten Ansicht werden alle **Transaktionen** für dieses Wertpapier angezeigt. Dies ermöglicht eine detaillierte Nachverfolgung aller Käufe, Verkäufe, Dividenden und anderen Transaktionen, die zu der aktuellen Position geführt haben. Bei CFD und Forex erscheinen die Transaktionen als Baum: Unter jeder Eröffnung stehen die Transaktionen, mit denen sie ganz oder teilweise geschlossen wurde.

### Kontextmenü auf Transaktionen
Auch an dieser Stelle können die einzelnen bestehenden Transaktionen bearbeitet werden. Weiterführende Informationen unter [Wertpapiertransaktionen](../../../transaction/security/).

## Diagrammansicht
Über **Zeige Diagramm** im Menü **Ansicht** oder im Kontextmenü erscheint eine grafische Darstellung der Vermögensaufteilung im [Zusatzbereich]({{% relref "/intro/userinterface" %}}). In der Gruppierung nach Währung zeigt ein Kreisdiagramm den Anteil jeder Währung am Total. In allen übrigen Gruppierungen steht links zusätzlich ein Balkendiagramm mit dem **Wertpapier Risiko** jeder Gruppe, rechts das Kreisdiagramm mit dem Anteil jeder Gruppe am Total. Dies ermöglicht eine schnelle Einschätzung der Diversifikation und zeigt auf einen Blick, wie das Vermögen verteilt ist.

{{% notice style="note" title="Negative Werte im Kreisdiagramm" %}}
Ein Kreisdiagramm kann keine negativen Anteile darstellen. Eine Gruppe mit negativem Wert, etwa CFD im Verlust, fehlt deshalb im Kreis, und die Prozentangaben im Kreis beziehen sich nur auf die Gruppen mit positivem Wert. Massgebend sind dann die Werte der Spalte **Anteil %**.
{{% /notice %}}

## Besonderheiten
- **Wertpapierorientierte Sicht**: Im Gegensatz zur Kontoübersicht, die Konten in den Vordergrund stellt, fokussiert dieser Report auf die Wertpapiere und deren Performance.
- **Mehrwährungsunterstützung**: Der Report unterstützt Positionen in verschiedenen Währungen. Alle Werte werden automatisch in die Hauptwährung des Mandanten umgerechnet.
- **Margin-Produkte**: Bei CFD und Forex zählt für das Vermögen nur der nicht realisierte Gewinn oder Verlust, für das Risiko dagegen der volle Marktwert. Deshalb unterscheiden sich bei diesen Produkten die Spalten **Konto relevant** und **Wertpapier Risiko** deutlich.
- **Währungsgewinne**: Währungsgewinne und -verluste werden in jeder Gruppierung als eigene Spalte ausgewiesen, was eine präzise Analyse der Währungseffekte ermöglicht. Für Instrumente, die bereits in der Hauptwährung notieren, ist der Wert null.
- **Transaktionsdetails**: Durch Expandieren der Zeilen können alle zugrundeliegenden Transaktionen eingesehen werden, was eine lückenlose Nachverfolgung ermöglicht.

## PDF-Bericht
Über den Eintrag **PDF-Bericht...** im Kontextmenü entsteht ein Vermögensauszug auf das eingestellte Datum. Er umfasst den ganzen Mandanten bzw. das ganze Portfolio mit allen Bargeldkonten, nicht nur die angezeigten Depots. Siehe [PDF-Bericht]({{% relref "/reportportfolio/periodperformance/pdfreport" %}}).
