---
title: "Transaktionen"
date: 2025-11-19T22:54:47+01:00
draft: false
weight: 60
archetype: "default"
---
Die Auswertung **Transaktionen** zeigt alle finanziellen Bewegungen in einer tabellarischen Übersicht. Diese Ansicht ist auf **Mandantenebene** und auf der **Ebene der einzelnen Portfolios** verfügbar. Sie ermöglicht die Verwaltung und Analyse aller Wertpapier- und Kontotransaktionen.
## Aufruf der Auswertung
Auf **Mandantenebene** werden alle Transaktionen über sämtliche Portfolios und Konten hinweg angezeigt. Auf **Portfolioebene** beschränkt sich die Ansicht auf die Transaktionen des ausgewählten Portfolios. Diese fokussierte Ansicht erleichtert die Analyse einzelner Portfolio-Aktivitäten.
## Transaktionstypen
Die Auswertung zeigt verschiedene Arten von Transaktionen. Bei **Wertpapieren** werden Käufe, Verkäufe, Dividenden und Finanzierungskosten für Margin-Positionen erfasst. Bei **Kontos** handelt es sich um Einzahlungen, Auszahlungen, Kontozinsen und Gebühren. **Kontoüberträge** sind Überweisungen zwischen zwei Kontos innerhalb des Mandanten und werden als verbundene Transaktionen dargestellt.
## Tabellenspalten
Die wichtigsten Spalten der Auswertung sind **Datum** für den Zeitpunkt der Transaktion, **Bankkonto** für das betroffene Konto und **Transaktionstyp** für die Art der Bewegung. Bei Wertpapiertransaktionen werden zusätzlich **Wertpapier**, **Stückzahl** und **Kurs/Dividende** angezeigt. Die Spalten **Steuern** und **Gebühren** zeigen die entsprechenden Kosten. Der **Kassabetrag** gibt die Auswirkung auf den Kassenbestand an und wird in Rot dargestellt wenn er negativ ist.
{{% notice note %}}
Die **Stückzahl** ist bei Wertpapiertransaktionen immer positiv. Die Transaktionsart Kauf oder Verkauf bestimmt die Richtung der Bewegung.
{{% /notice %}}
## Filter- und Sortierfunktionen
Die Tabelle bietet umfangreiche Möglichkeiten zur Filterung und Sortierung. Jede Spalte kann einzeln gefiltert werden, dabei stehen je nach Datentyp unterschiedliche Filteroptionen zur Verfügung. Bei **Datumsspalten** können Sie ein Datum auswählen und festlegen ob Sie nach einem exakten Datum oder einem Datumsbereich suchen möchten. Für **Textspalten** wie Bankkonto, Transaktionstyp oder Wertpapier steht eine Auswahlliste zur Verfügung. Bei **Zahlenspalten** können Sie numerische Bereiche definieren. Die Sortierung erfolgt standardmässig nach Datum absteigend, kann aber durch Klicken auf die Spaltenüberschriften angepasst werden. Mit der **Strg-Taste** ist auch eine Mehrspaltensortierung möglich.
## Bearbeitungsfunktionen
Über das **Kontextmenü** oder das Menü **Bearbeiten** können Transaktionen bearbeitet oder gelöscht werden. Beim **Bearbeiten** können je nach Transaktionstyp Felder wie Datum, Stückzahl, Kurs, Steuern oder Gebühren angepasst werden. Das Konto und das Wertpapier können nach der Erstellung nicht mehr geändert werden. Beim **Löschen** einer Transaktion fordert das System eine Bestätigung, da dieser Vorgang endgültig ist. Bei Kontoüberträgen werden beide verbundenen Transaktionen gemeinsam gelöscht.
Eine besondere Funktion ist die **Umwandlung in Kontoübertrag**. Eine einzelne Einzahlung oder Auszahlung kann nachträglich in einen Kontoübertrag umgewandelt werden. Dies ist nützlich wenn eine Auszahlung ursprünglich als normale Auszahlung erfasst wurde aber tatsächlich ein interner Transfer war. Das System erstellt automatisch die verbundene Gegenbuchung auf dem Zielkonto.
## Besonderheiten
Bei **Margin-Positionen** werden separate Transaktionen für Finanzierungskosten erfasst die mit der Eröffnungstransaktion verbunden sind. Diese können positiv oder negativ sein. Bei **Dividenden** kann ein Ex-Datum angegeben werden wenn die Dividende nach dem Verkauf des Wertpapiers ausgezahlt wird. Die Wertpapier-Referenz bleibt dabei erhalten. Bei **Kontoüberträgen** mit unterschiedlichen Währungen wird der Wechselkurs für beide Transaktionen verwendet und der Betrag entsprechend konvertiert.
