---
title: "Portfolios und Portfolio"
date: 2021-05-20T22:54:47+01:00
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

## Tabellenspalten
Es werden nur die nicht selbsterklärenden Spalten beschrieben:
- **Währungsgewinn Hauptwährung**: Der Währungsgewinn berechnet sich als hätte die Transaktion in der Hauptwährung stattgefunden und nicht in der Fremdwährung.
  - Mit einem Kontoübertrag in eine Fremdwährung laufen ab der Transaktionszeit Währungsgewinne bzw. -verluste auf. 
- **Gewinn Wertpapiere Hauptwährung**: Der Gewinn Wertpapiere berechnet sich aus der Addition des Kursgewinnes und Dividenden. Die Steuern und Handelskosten werden entsprechend in Abzug gebracht.
- **Gewinn Wertpapiere**: Der hypothetische Gewinn der auf den Wertpapieren erzielt wird und wurde, damit ist gemeint, falls am **Stichdatum** alle Wertpapiere verkauft würden. Dieser Betrag enthält die Erträge aus Zinsen und Dividenden sowie auch die Aufwände der verbuchten Transaktions und Steuerkosten.

{{% notice note %}}
Die Werte **Wertpapier** und **Barsaldo** ergeben das **Total** in der entsprechenden Portfoliowährung.\
Auch die Werte **Externe Bargeld Ein/Auszahlung** - **Konto Transaktionskosten** - **Konto und Depotkosten** + **Kontozins** + **Währungsgewinn** müssen das **Total** in der entsprechenden Portfoliowährung ergeben.
{{% /notice %}}
