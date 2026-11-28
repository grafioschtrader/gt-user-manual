---
title: "Realisierte Gewinne"
date: 2026-11-10T18:00:00+01:00
draft: false
weight: 55
archetype: "default"
---
Die Auswertung **Realisierte Gewinne** zeigt die Kursgewinne und -verluste aus Verkäufen, geordnet nach dem Jahr des Verkaufs. Sie steht auf **Mandantenebene** zur Verfügung, in jeder Hauptwährung und für jedes Land, und rechnet alle Beträge in die Hauptwährung um. Dividenden und Zinsen gehören nicht dazu, sie stehen in der Auswertung [Dividende und Zins](../dividends/). Die Auswertung ist keine Steuerberechnung.

## Methode wählen
Über dem Ergebnis steht die Auswahl der **Einstandswert-Methode**. Sie gilt nur für diese Auswertung und ist unabhängig von der Methode, die beim Klienten oder bei einem Portfolio eingestellt ist, siehe [Einstandswert-Methode](../costbasismethod/). So lässt sich derselbe Verkauf nach **Gleitender Durchschnitt**, **FIFO** oder **LIFO** vergleichen, ohne die anderen Ansichten zu verändern. Nach der Wahl berechnet die Schaltfläche **Berechnen** die Auswertung neu.

Die Option **Methode je Depot (Mandant und Portfolio)** rechnet jedes Depot nach der Methode, die für dieses Depot gilt. Verwenden Depots verschiedene Methoden, steht über der Tabelle **Gemischt, je Depot verschieden**. Welche Methode einen Verkauf berechnet hat, zeigt die Spalte **Einstandswert-Methode** der einzelnen Zeile.

Mit dem Menüpunkt **Auswertung über Depots** wählen Sie, von welchen Depots die Verkäufe aufgeführt werden. Die Auswahl ändert den Gewinn eines Verkaufs nicht: Beim gleitenden Durchschnitt bilden alle Depots mit dieser Methode für ein Wertpapier gemeinsam einen Topf, und auch ein Verkauf in einem einzelnen Depot rechnet mit diesem Topf. Es bleibt also bei jeder Depotauswahl derselbe Gewinn.

## Jahrestabelle
Die Tabelle enthält eine Zeile pro Kalenderjahr. Über der Tabelle stehen die Summen über alle Jahre. Alle Beträge sind in der Hauptwährung angegeben.
- **Jahr**: Das Jahr, in dem die Verkäufe stattfanden.
- **Gewinne**: Die Summe aller Verkäufe mit Gewinn.
- **Verluste**: Die Summe aller Verkäufe mit Verlust, als positiver Betrag.
- **Netto**: Gewinne minus Verluste.
- **Verkäufe**: Die Anzahl der Verkäufe. Ein Verkauf ohne Gewinn zählt mit, er ist weder Gewinn noch Verlust.
- **Systembuchungen, netto** und **Systembuchungen, Anzahl**: Gewinne minus Verluste der Systembuchungen und deren Anzahl, siehe unten.
- **Unvollständig**: Zeigt an, dass nicht jeder Verkauf des Jahres vollständig in die Summen einging, siehe unten.

Durch Aufklappen einer Jahreszeile erscheinen ihre Verkäufe. Bei FIFO und LIFO lässt sich ein Verkauf ein weiteres Mal aufklappen. Er zeigt dann die **von diesem Verkauf verbrauchten Käufe**.

## Verkäufe des Jahres
Jede Zeile ist ein Verkauf mit dem Datum, dem Wertpapier, der Währung und der **Anzahl**, wie sie in der Transaktion steht. **Erlös** ist der Verkaufserlös nach Abzug der Kosten des Verkaufs, **Einstand** sind die Kosten der verkauften Stücke nach der gewählten Methode, und **Gewinn/Verlust** ist die Differenz, jeweils in der Währung des Wertpapiers. **Gewinn/Verlust %** setzt den Gewinn ins Verhältnis zum Einstand.

Die Spalte **Gewinn/Verlust zum Kurs des Verkaufstags** rechnet den Gewinn in die Hauptwährung um, und zwar mit dem **Wechselkurs** des Verkaufstags. Ist das Wertpapier in der Hauptwährung notiert, ist der Wechselkurs 1. Die Spalte **Realisiert** der Depot-Auswertung rechnet dieselben Verkäufe dagegen zum Kurs des Berichtsdatums um. Bei einem Wertpapier in Fremdwährung weichen deshalb beide Beträge voneinander ab.

Bei einem Margin-Produkt wie einem CFD ist eine Zeile der Abschluss einer Position. Sie zeigt den Gewinn der geschlossenen Position und weder Erlös noch Einstand, und die Methode spielt für sie keine Rolle.

## Systembuchungen
Ein Depotübertrag und ein ISIN-Wechsel buchen GT als Verkauf und anschliessenden Kauf zum Schlusskurs. Der Verkauf ist eine **Systembuchung**: Am Markt wurde nichts verkauft, und trotzdem entsteht gegenüber dem bisherigen Einstand ein Gewinn oder Verlust. Diese Zeilen sind in der Tabelle gekennzeichnet, und ihre Beträge stehen nicht in den Spalten **Gewinne**, **Verluste** und **Netto**, sondern in eigenen Spalten.

Der Grund ist, dass eine Systembuchung Gewinn zwischen Depots und Jahren verschiebt. Der spätere Verkauf im Zieldepot gewinnt nur noch gegenüber dem Kurs des Übertrags. Erst die Summe aus den Verkäufen am Markt und den Systembuchungen über alle Jahre ergibt den Gewinn, den Sie wirtschaftlich erzielt haben. Ein einzelnes Jahr zeigt ihn nicht.

## Wenn Angaben fehlen
Reichen die Käufe nicht für alle verkauften Stücke, etwa weil ein Kauf nicht erfasst wurde, gilt der Gewinn für die fehlenden Stücke als 0. Die Zeile und das Jahr tragen dann die Kennzeichnung **Unvollständig**. Fehlt für den Verkaufstag der Wechselkurs der Wertschriftenwährung, bleibt der Betrag in der Hauptwährung leer, und der Verkauf zählt nicht in die Summen der Hauptwährung. Auch er macht das Jahr unvollständig.

{{% callout note %}}
Die Auswertung rechnet keine Steuern und nutzt keine Steuerregeln eines Landes. Sie zeigt, was die gewählte Methode als realisierten Kursgewinn ergibt. Für eine Steuererklärung massgebend sind die Bescheinigungen der Bank und die Vorschriften Ihres Landes.
{{% /callout %}}

## Steuerhilfe Deutschland
Für einen Klienten mit der Hauptwährung Euro und dem Land Deutschland erscheint unter der Jahrestabelle zusätzlich die **Steuerhilfe Deutschland**. Sie leitet für ein Steuerjahr die Ausgangswerte für Anlage KAP und Anlage KAP-INV her, siehe [Steuerhilfe Deutschland](../germantax/).

