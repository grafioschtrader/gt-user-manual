---
title: "Klient und Portfolio Reports"
date: 2026-08-10T22:54:47+01:00
draft: false
weight: 12
archetype: "default"
---
Auswertungen ist das, was der Benutzer von GT letztendlich interessiert. Die Auswertungen haben sehr unterschiedliche Schwerpunkte, daher gibt es unterschiedliche Reports.

## Performanceberechnung
...
{{% notice warning %}}
In **GT** können viele **Zertifikate** auch als Instrument nachgebildet werden. **GT** kann aber die **Risiken** für die meisten **Strukturierten Produkte** (Zertifikate) nicht bestimmen, da diese oftmals ein **asymmetrisches Auszahlungsprofil** haben und GT die Eigenschaften der einzelnen Produkte nicht bekannt sind.
{{% /notice %}}

## Warum keine Prozentangabe Vermögensänderung
In GT gibt es keine Angabe einer prozentualen Vermögensänderung. Auf diese wurde verzichtet, da die Aussagekraft bei einem nicht andauernden 100-prozentigen Investment gering ist.
{{% notice note %}}
**Benchmarking zukünftige GT-Version**\
Möglicherweise wird es in zukünftigen GT-Versionen eine Möglichkeit eines Benchmarking geben. Eine Idee einer Simulation: Wie wäre die Performance wenn die selben Beträge in den Benchmark investiert würden im Vergleich zu den Investitionen die real getätigt wurden.
{{% /notice %}}

## Abweichung zur Realität
Für die Berechnung der Performance werden hypothetische Transaktionen durchgeführt. Beispielsweise müssten Wertpapiere verkauft und die Fremdwährungen gegen die Hauptwährung gekauft werden. Dazu werden hypothetische Verkäufe und Währungstransaktionen durchgeführt. Bei den Fremdwährungskursen wird der Mittelkurs genommen. Dabei werden die Aufwendungen für Transaktionskosten und Steuern nicht berücksichtigt.
{{% notice note %}}
**Veräusserungskosten zukünftige GT-Version**\
In einer zukünftigen GT-Version werden die bisher vernachlässigten Veräusserungskosten berücksichtigt. Dabei werden aus der Historie von getätigten Transaktionen die **Veräusserungskosten** abgeleitet.
{{% /notice %}}

## Problematik Fremdwährung
Sobald eine Applikation Konten und Handel mit Fremdwährungen unterstützt, wird es Diskussionen über unterschiedliche Ansätze der Performance Berechnung geben. GT selbst is wissentlich nicht durchgehend konsistent was diese Berechnung betrifft. Beispielsweise werden im Report der **Portfolios** die Erträge und Aufwände für **Kontozins** bzw. **Konto- und Depotkosten** anders berechnet als im Report des Periodenertrags. Im ersteren werden die Erträge zum Transaktionsdatum in die Hauptwährung umgerechnet, beim Periodenertrag wird das Datum auf welche sich die Berechnung bezieht genommen. Vom **Währungsgewinn** gilt dies nicht, dieser wird im nächsten Abschnitt beschrieben und in allen Berichten gleich ermittelt.

## Währungsgewinn
Wer in einer Fremdwährung anlegt, erzielt sein Ergebnis aus zwei Quellen: aus dem Instrument selbst und aus der Bewegung des Wechselkurses. Der **Währungsgewinn** weist den zweiten Teil separat aus. Er beantwortet die Frage, wie viel des Ergebnisses allein daraus entstanden ist, dass sich der Kurs der Fremdwährung gegenüber Ihrer Hauptwährung verändert hat.

Das Prinzip ist einfach: Massgebend ist das Geld, das zum Stichdatum noch in der Fremdwährung steckt. Dieses wird zum Kurs des Stichdatums bewertet und mit dem Wert verglichen, den dasselbe Geld an den Tagen hatte, an denen es tatsächlich geflossen ist. Käufe, Verkäufe, Dividenden und Marchzinsen zählen dabei alle mit. Geld, das bereits wieder zurückgeflossen ist, etwa aus einem Verkauf oder einer Dividende, nimmt ab diesem Zeitpunkt nicht mehr an der Kursbewegung teil.

### Beispiel
Ein Beispiel macht den Zusammenhang deutlich. Die Hauptwährung ist CHF, gekauft werden 100 Einheiten eines Instruments zu USD 10.00 bei einem Kurs USD/CHF von 1.00. Bis zum Stichdatum steigt das Instrument auf USD 12.00, gleichzeitig fällt der Dollar auf einen Kurs von 0.80.

| | USD | CHF |
|---|---:|---:|
| Einsatz beim Kauf | 1'000.00 | 1'000.00 |
| Wert am Stichdatum | 1'200.00 | 960.00 |
| **Gewinn Wertpapier** | **+200.00** | **+160.00** |
| **Währungsgewinn** | | **-200.00** |
| **Total** | | **-40.00** |

Das Instrument hat um 20 Prozent zugelegt, und trotzdem steht die Position in CHF mit 40 Franken im Minus. Der Kursgewinn von USD 200.00 ist zum Stichdatum noch CHF 160.00 wert, dem steht ein Währungsverlust von CHF 200.00 auf dem eingesetzten Kapital gegenüber. Ein negativer Währungsgewinn bei steigendem Instrument ist also kein Widerspruch, sondern der Normalfall bei einer schwächer werdenden Fremdwährung.

{{% notice note %}}
**Die beiden Spalten ergeben zusammen das Ergebnis**\
**Gewinn Wertpapier** in der Hauptwährung und **Währungsgewinn** ergeben zusammen immer genau das Ergebnis der Position in Ihrer Hauptwährung. Weichen die Zahlen von Ihrer Erwartung ab, fehlen in aller Regel Kursdaten des Instruments oder des Währungspaares.
{{% /notice %}}

### Wo der Währungsgewinn erscheint
Der Währungsgewinn wird für Konten und für Wertpapiere nach demselben Verfahren berechnet, weshalb sich die Berichte gegenseitig als Kontrolle verwenden lassen. Er erscheint in [Portfolios und Portfolio]({{% ref "/reportportfolio/portfolios" %}}) für die Fremdwährungskonten, in [Depots]({{% ref "/reportportfolio/securityaccountreport" %}}) und [Anlageklassen mit Cash]({{% ref "/reportportfolio/securitycashaccountreport" %}}) für die einzelnen Wertpapiere sowie in der Transaktionsliste einer [Wertpapiertransaktion]({{% ref "/transaction/security" %}}) für jede einzelne Transaktion.

In der Spaltenüberschrift steht hinter der Bezeichnung die Hauptwährung, also beispielsweise «Währungsgewinn CHF».

## Allgemeine Darstellungen in Berichten
Bestimmte Darstellungformen in den Berichten werden allgemein angewandt und sind hier beschrieben.

### Farbliche Darstellung
Zur besseren Lesbarkeit und schnelleren Erfassung von Gewinnen und Verlusten verwendet der Report eine konsistente Farbcodierung:
- **Positive Werte** (Gewinne): Werden in der Standardtextfarbe oder in Grün dargestellt
- **Negative Werte** (Verluste): Werden in Rot dargestellt

Diese Farbcodierung gilt für die Gewinn-Spalten, wodurch auf einen Blick erkennbar ist, welche Anlageklassen und Wertpapiere positive Performance zeigen und welche Verluste aufweisen.

### Nachkommastellen je Währung
Beträge werden mit der für die jeweilige Währung sinnvollen Anzahl Nachkommastellen dargestellt. Für die meisten Währungen sind dies zwei Nachkommastellen. Eine Einheit des japanischen Yen (JPY) hat jedoch einen geringen Wert, daher werden Beträge in JPY ohne Nachkommastellen angezeigt. Umgekehrt hat eine Einheit der Kryptowährung Bitcoin (BTC) einen sehr hohen Wert, weshalb Beträge in BTC mit acht Nachkommastellen dargestellt werden. Diese Darstellung gilt für Geldbeträge wie Saldo, Kosten, Steuern oder Gewinn in den Berichten. Kurse und Wechselkurse sind davon ausgenommen, da diese eine feinere Auflösung mit bis zu acht Nachkommastellen benötigen.
Die Eingabe von Beträgen verhält sich entsprechend: In den Dialogen für Transaktionen und Daueraufträge ist die Anzahl der Nachkommastellen durch die Währung des jeweiligen Kontos bzw. Instruments begrenzt.
{{% notice note %}}
**Globale Einstellung**\
Der Administrator legt die Nachkommastellen pro Währung über den globalen Parameter «gt.currency.precision» fest. Währungen ohne Eintrag verwenden zwei Nachkommastellen, ein Eintrag hat die Form «BTC=8,ETH=7,JPY=0». Eine Änderung wird für den Benutzer nach einer erneuten Anmeldung wirksam.
{{% /notice %}}
