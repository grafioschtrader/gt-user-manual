---
title: "Klient und Portfolio Reports"
date: 2026-06-09T22:54:47+01:00
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
Sobald eine Applikation Konten und Handel mit Fremdwährungen unterstützt, wird es Diskussionen über unterschiedliche Ansätze der Performance Berechnung geben. GT selbst is wissentlich nicht durchgehend konsistent was diese Berechnung betrifft. Beispielsweise werden im Report der **Portfolios** die Erträge und Aufwände für **Kontozins** bzw. **Konto- und Depotkosten** anders berechnet als im Report des Periodenertrags. Im ersteren werden die Erträge zum Transaktionsdatum in die Hauptwährung umgerechnet, beim Periodenertrag wird das Datum auf welche sich die Berechnung bezieht genommen.

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
