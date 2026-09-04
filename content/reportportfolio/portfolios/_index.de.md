---
title: "Portfolios und Portfolio"
date: 2026-09-01T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
Die beiden Auswertungen **Portfolios** und **Portfolio** sind sehr ähnlich. Daher werden diese beiden zusammen dokumentiert. Bei beiden Auswertungen kann ein **Stichdatum** für den Tag der Auswertung gesetzt werden.

## Portfolios
Die Auswertung **Portfolios** ergibt eine Überblick über die Gesamtsumme aller Portfolios. Dabei werden die zwei Gruppierung **Währung gruppiert** und **Portfolio gruppiert** unterstützt. Falls es für eine Währung kein Konto gibt, wird die entsprechende Währung keinem Konto zugeordnet.

## Portfolio
Hierbei erfolgt die Auswertung gemäss den Kontos bzw. der Währung.

### Funktion Portfolios
Es gibt nur die Auswahl der Gruppierung.

### Funktion Portfolio
Die Erfassung einer [Kontotransaktion](../../account/transaction/) ist nur hier möglich. Zudem gibt es noch folgende Funktionalität: 
- Hier erfolgt die Bearbeitung der [Kontos](../../tenantportfolio/cashaccount/). 
- In der expandierende Tabellenzeile werden die Transaktionen des entsprechenden Kontos dargestellt. Diese können bearbeitet werden, siehe dazu [Transaktion](../../transaction/). 

## Berechnung der Auswertung
Bei beiden Auswertungen werden **sämtliche jemals erfassten Transaktionen** einbezogen. Die Berechnung beginnt bei der allerersten Transaktion und läuft chronologisch bis zum gewählten Stichdatum. Es werden dabei keine Zwischenstände gespeichert oder fortgeschrieben, sondern die Auswertung wird bei jedem Aufruf vollständig neu gerechnet.

Jede Transaktion in einer Fremdwährung wird mit dem Wechselkurs ihres eigenen Transaktionstages umgerechnet und nicht etwa mit dem Kurs des Stichdatums. Wertpapiere, die am Stichdatum noch im Bestand sind, werden mit dem Schlusskurs dieses Tages bewertet. Aktiensplits werden berücksichtigt, damit die Stückzahlen über den ganzen Zeitraum hinweg vergleichbar bleiben.

Daraus ergeben sich drei Punkte, die im Alltag von Bedeutung sind. Das Stichdatum kann frei gewählt werden, denn das Ergebnis bezieht sich immer auf die vollständige Historie bis zu diesem Tag. Wird eine weit zurückliegende Transaktion nachträglich korrigiert, ändern sich alle davon betroffenen Werte sofort. Mit zunehmender Anzahl an Transaktionen dauert die Auswertung entsprechend länger.

{{% notice tip %}}
Die Auswertung [Periodenertrag](../periodperformance/) ermittelt ihre Zahlen auf einem anderen Weg, weshalb sich die beiden Auswertungen gegenseitig als Plausibilitätskontrolle verwenden lassen. Einige Spalten dürfen dabei nicht auf den Rappen genau gleich sein. Welche das sind und weshalb, ist unter [Vergleich mit der Auswertung Portfolios]({{% relref "/reportportfolio/periodperformance" %}}#vergleich-mit-der-auswertung-portfolios) beschrieben.
{{% /notice %}}

## Tabellenspalten
Es werden nur die nicht selbsterklärenden Spalten beschrieben:
- **Konto und Depotkosten**: Separat verbuchte Gebühren des Kontos und des Depots. Handelskosten, die in einem Kauf oder Verkauf enthalten sind, gehören nicht hierher, sondern zum Ergebnis der Wertpapiere; die Finanzierungskosten von Margin-Positionen ebenso wenig, obwohl sie dem Konto belastet werden. Sie sind Kosten der Position und erscheinen in **Gewinn Wertpapiere**.
- **Kontozins**: Zinsen, die dem Bargeldkonto gutgeschrieben oder belastet wurden. Erträge aus Wertpapieren gehören nicht dazu.
- **Währungsgewinn Hauptwährung**: Der Anteil des Ergebnisses, der allein aus der Bewegung des Wechselkurses entstanden ist. Mit einem Kontoübertrag in eine Fremdwährung laufen ab der Transaktionszeit Währungsgewinne bzw. -verluste auf. Die Berechnung ist unter [Währungsgewinn]({{% ref "/reportportfolio#währungsgewinn" %}}) beschrieben.
- **Gewinn Wertpapiere Hauptwährung**: Der Gewinn Wertpapiere berechnet sich aus der Addition des Kursgewinnes und Dividenden. Die Steuern und Handelskosten werden entsprechend in Abzug gebracht.
- **Gewinn Wertpapiere**: Der hypothetische Gewinn der auf den Wertpapieren erzielt wird und wurde, damit ist gemeint, falls am **Stichdatum** alle Wertpapiere verkauft würden. Dieser Betrag enthält die Erträge aus Zinsen und Dividenden sowie auch die Aufwände der verbuchten Transaktions und Steuerkosten.

{{% notice note %}}
Die Werte **Wertpapier** und **Barsaldo** ergeben das **Total** in der entsprechenden Portfoliowährung.\
Auch die Werte **Externe Bargeld Ein/Auszahlung** - **Konto Transaktionskosten** - **Konto und Depotkosten** + **Kontozins** + **Währungsgewinn** + **Gewinn Wertpapiere** müssen das **Total** in der entsprechenden Portfoliowährung ergeben.
{{% /notice %}}

## Vergleich mit dem Periodenertrag

Vier Grössen dieser Auswertung erscheinen auch im [Periodenertrag](../periodperformance/), dort unter leicht anderen Bezeichnungen.

| Portfolios und Portfolio | Periodenertrag |
|---|---|
| **Konto und Depotkosten** | **Konto- und Deposten real** |
| **Kontozins** | **Kontozins real** |
| **Externe Bargeld Ein/Auszahlung** | **Externe Bargeld Ein-/Auszahlung** |
| **Barsaldo** | **Barsaldo** |

Es sind jeweils dieselben Buchungen, und trotzdem können die Beträge voneinander abweichen. Der Grund ist der Zeitpunkt der Währungsumrechnung: Diese Auswertung rechnet jede Buchung mit dem Wechselkurs ihres eigenen Buchungstages um, der Periodenertrag rechnet den aufgelaufenen Betrag einmal mit dem Kurs des Auswertungstages um. Bei einem Konto in der Hauptwährung spielt das keine Rolle, bei einem Fremdwährungskonto können mehrere Prozent Unterschied entstehen. Abweichungen von wenigen Rappen sind immer nur eine Rundung.

Ausführlich, mit Beispiel und weiteren Gründen, ist das unter [Vergleich mit der Auswertung Portfolios]({{% relref "/reportportfolio/periodperformance" %}}#vergleich-mit-der-auswertung-portfolios) beschrieben.
