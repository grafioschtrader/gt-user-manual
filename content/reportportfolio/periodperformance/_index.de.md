---
title: "Periodenertrag"
date: 2026-10-05T22:54:47+01:00
draft: false
weight: 20
archetype: "default"
---

Der **Periodenertrag-Report** bietet eine detaillierte Analyse der Performance eines Portfolios oder Mandanten über einen definierten Zeitraum. Der Report ermöglicht es, die Wertentwicklung tages-, wochen- oder monatsweise zu verfolgen und die verschiedenen Einflussfaktoren auf die Gesamtperformance zu identifizieren.

## Aufruf des Reports

Der Periodenertrag-Report kann auf zwei Ebenen aufgerufen werden:

1. **Auf Mandantenebene**: Über das Hauptmenü → **Depots und Konten** → **Portfolios** → **Periodenertrag**
   - Analysiert die Performance aller Portfolios des Mandanten zusammen
   - Werte werden in der Mandantenwährung dargestellt

2. **Auf Portfolioebene**: Innerhalb eines einzelnen Portfolios → **Periodenertrag**
   - Analysiert nur das ausgewählte Portfolio
   - Werte werden in der Portfoliowährung dargestellt

## Eingabeparameter

Vor der Berechnung des Reports müssen folgende Parameter festgelegt werden:

### Datum von
Das Startdatum der Analyseperiode (inklusiv). Wichtig: Das Datum bezieht sich auf den Börsenschluss, d.h. der Ertrag des Starttages ist **nicht** in der Berechnung enthalten. Der Starttag dient somit als Vergleichsbasis, von der aus die Veränderung bis zum Enddatum gemessen wird.

Das Datum muss ein gültiger Handelstag sein, also weder ein Wochenende noch ein Feiertag oder ein Tag mit fehlenden Kursen. Zudem darf es nicht vor dem ältesten möglichen Datum liegen, das über dem Eingabeformular hinter dem Hinweis "Ältestes mögliche Datum:" angezeigt wird.

Dieses älteste mögliche Datum ist der letzte Handelstag **vor** dem ersten Wertpapierkauf. Diese Wahl ist bewusst so getroffen: An jenem Tag ist noch nichts investiert, weshalb eine Auswertung ab diesem Datum bei Gewinn, Wertpapieren, Saldo Kauf/Verkauf Wertpapiere, Dividenden, Gebühren und Kontozins bei null beginnt. Würde stattdessen der Tag des ersten Kaufs als Startdatum dienen, wäre dieser Kauf bereits in den Anfangswerten enthalten und sein Ergebnis ginge der Auswertung verloren, da der Ertrag des Starttages nicht mitgerechnet wird.

Das älteste mögliche Datum ist immer ein echter Handelstag. Fällt der Kalendertag vor dem ersten Wertpapierkauf auf ein Wochenende oder einen Feiertag, wird der davorliegende Handelstag angeboten, also beispielsweise der Freitag vor einem Kauf am Montag.

{{% notice note %}}
Beginnen die Konten erst am Tag des ersten Wertpapierkaufs oder später, existiert kein früherer Handelstag mit Kontodaten. In diesem Fall ist das älteste mögliche Datum der Tag des ersten Wertpapierkaufs selbst und die Anfangswerte beginnen nicht bei null.
{{% /notice %}}

### Datum bis
Das Enddatum der Analyseperiode (inklusiv). Das Datum bezieht sich auf den Börsenschluss.

**Einschränkungen:**
- Muss ein gültiger Handelstag sein
- Muss nach dem Startdatum liegen
- Darf nicht nach dem letzten verfügbaren Handelstag liegen

### Periodenaufteilung
Bestimmt, wie die Daten im Report aggregiert werden:

- **Woche**: Zeigt die Performance nach Wochentagen (Montag-Freitag) an
  - Nur verfügbar, wenn der Zeitraum maximal die konfigurierte Anzahl von Wochen umfasst
  - Ideal für detaillierte kurzfristige Analysen

- **Jahr/Monat**: Zeigt die Performance nach Monaten an
  - Nur verfügbar, wenn der Zeitraum mindestens die konfigurierte Anzahl von Monaten umfasst
  - Ideal für längerfristige Übersichten

Die Verfügbarkeit der Optionen wird dynamisch basierend auf dem gewählten Zeitraum angepasst.

## Berichtsinhalt

Der Periodenertrag-Report besteht aus mehreren Hauptbereichen:

### 1. Zusammenfassung (Periodenvergleich)

Dieser Bereich zeigt drei Spalten:

- **Erster Tag**: Alle Werte zum Startdatum der Periode
- **Letzter Tag**: Alle Werte zum Enddatum der Periode
- **Differenz**: Die Veränderung zwischen Start und Ende

**Dargestellte Metriken:**

Die ersten fünf Zeilen sind kumulierte Werte. Sie laufen seit der allerersten Transaktion auf und nicht erst seit dem gewählten Startdatum, weshalb allein die Spalte **Differenz** eine Aussage über den gewählten Zeitraum macht. Da der Starttag als Vergleichsbasis dient, ist eine Buchung, die auf den Starttag datiert ist, in dieser Differenz nicht enthalten.

| Zeile | Beschreibung |
|--------|-------------|
| **Zins/Dividende real** | Vereinnahmte Dividenden und Zinsen aus Wertpapieren, so wie sie dem Konto gutgeschrieben wurden. |
| **Konto- und Deposten real** | Separat verbuchte Gebühren des Kontos und des Depots. Handelskosten, die in einem Kauf oder Verkauf enthalten sind, gehören nicht hierher, sondern zu **Saldo Kauf/Verkauf Wertpapiere**; die Finanzierungskosten von Margin-Positionen ebenfalls nicht, sie gehören zum Ergebnis der Wertpapiere. Der Aufwand wird als positiver Betrag ausgewiesen. |
| **Kontozins real** | Zinsen, die einem Bargeldkonto gutgeschrieben oder belastet wurden. Erträge aus Wertpapieren gehören nicht dazu. |
| **Externe Bargeld Ein-/Auszahlung** | Einzahlungen, die von externen Konten kamen. Damit sind Kontoübertragungen ausgeschlossen. |
| **Saldo Kauf/Verkauf Wertpapiere** | Nettobetrag, der durch Käufe und Verkäufe von Wertpapieren vom Konto abgeflossen oder ihm zugeflossen ist, einschliesslich der in diesen Transaktionen enthaltenen Handels- und Steuerkosten. |
| **Wertpapiere + inkl. Gewinn Margin Pos.** | Wert der gehaltenen Wertpapiere am jeweiligen Tag, zuzüglich des Gewinns der offenen Margin-Positionen. |
| **Wertpapier Risiko** | Marktwert aller gehaltenen Positionen zum vollen Gegenwert, also einschliesslich der über Margin gehaltenen, jeweils multipliziert mit dem Hebelfaktor wie in den Depots, siehe [Hebelfaktor und Wertpapier Risiko]({{% relref "/reportportfolio" %}}#hebelfaktor-und-wertpapier-risiko). |
| **Barsaldo** | Bestand aller Bargeldkonten am jeweiligen Tag. |
| **Wertpapiere + Saldo** | Barsaldo, Wertpapiere und Gewinn der offenen Margin-Positionen zusammen. |
| **Gewinn** | Barsaldo zuzüglich Wertpapiere, abzüglich der externen Ein- und Auszahlungen. Die Margin-Positionen sind hier nicht enthalten. |
| **Gewinn offene Margin Position** | Gewinn oder Verlust, der anfiele, wenn die offenen Margin-Positionen zum Kurs des jeweiligen Tages geschlossen würden. |
| **Total Gewinn** | **Gewinn** zuzüglich **Gewinn offene Margin Position**. |

Das Datum der jeweiligen Spalte steht in deren Titelzeile und ist keine eigene Zeile der Tabelle.

**Wichtig**: Alle Werte werden in der Hauptwährung dargestellt - entweder Mandanten- oder Portfoliowährung, je nach Aufrufkontext. Die Hauptwährung wird in der Bezeichnung der jeweiligen Zeile mit ausgegeben.

Unter der Zusammenfassung folgen die relativen Kennzahlen des Zeitraums, beschrieben im Abschnitt [Rendite und Risiko](#rendite-und-risiko).

### 2. Detaillierte Periodenfenster (Tabellenansicht)

Dieser Bereich zeigt die täglichen Veränderungen strukturiert nach Perioden (Wochen oder Monate):

Bei wöchentlicher Aufteilung steht jede Zeile für eine Woche, die Spalten zeigen die Tage Montag bis Freitag. Bei monatlicher Aufteilung steht jede Zeile für ein Jahr und die Spalten zeigen die zwölf Monate. In beiden Fällen folgt am Ende die Spalte **Total** mit der Summe der Periode, und die Fusszeile **Gesamtsumme** fasst jede Spalte über alle Perioden zusammen.

Alle Beträge dieser Tabelle enthalten den Gewinn der offenen Margin-Positionen, auch die Spalte **Total** und die Fusszeile. Die Zellen einer Zeile ergeben deshalb zusammen den Wert der Spalte **Total**, und die Gesamtsumme dieser Spalte entspricht dem **Total Gewinn** in der Spalte **Differenz** der Zusammenfassung.

Rechts neben **Total** steht die Spalte **Zeitgewichtete Rendite %**. Sie zeigt die Rendite der ganzen Woche bzw. des ganzen Jahres und ist schon im zugeklappten Zustand sichtbar, sodass sich die Perioden direkt miteinander vergleichen lassen. Die Rendite und der Gewinn einer Periode können verschiedene Vorzeichen haben: Floss während eines Kursrückgangs viel Geld zu, ist die Rendite negativ, der Gewinn wegen der Einzahlung aber positiv. Jede Zahl wird nach ihrem eigenen Vorzeichen rot oder grün eingefärbt. In der Fusszeile steht dort die zeitgewichtete Rendite des gesamten Zeitraums. Sie ist die Verkettung der Periodenrenditen und nicht deren Summe. Hat eine Periode keinen einzigen bewerteten Tag, etwa weil sie nur aus Feiertagen besteht, bleibt ihre Zelle leer.

Jede Periode lässt sich aufklappen und zeigt dann bis zu fünf Zeilen, deren Bezeichnung in der ersten Spalte steht:

| Zeile | Beschreibung |
|------|-------------|
| Wochenbereich bzw. Jahr | **Total Gewinn**: Veränderung des Gesamtgewinns gegenüber dem Vortag. Nur diese Zeile führt in der Spalte **Total** den Wert der ganzen Periode. |
| **Zeitgewichtete Rendite %** | Rendite jedes einzelnen Tages bzw. Monats in Prozent. Der Tooltip einer Zelle nennt die Zeitspanne, über die gemessen wurde. Fehlt die letzte Bewertung eines Monats, beginnt die Messung des Folgemonats bereits im Vormonat, was im Tooltip sichtbar wird. |
| **Barsaldo** | Veränderung des Kassenbestands gegenüber dem Vortag |
| **Wertpapier** | Veränderung des Wertpapierwerts gegenüber dem Vortag |
| **Gewinn offene Margin Position** | Veränderung des Gewinns aus Margin-Positionen gegenüber dem Vortag |

Ein Tooltip auf einer Zelle zeigt zusätzlich deren vollständigen Wert an.

**Farbcodierung in der Tabelle:**
- 🟩 **Grün**: Regulärer Handelstag mit Daten
- 🟨 **Gelb**: Feiertag (keine Handelsaktivität)
- 🟥 **Rot**: Handelstag mit fehlenden historischen Kursdaten
- ⬜ **Grau**: Wochenende oder nicht relevanter Tag

### 3. Spaltensummen

Am Ende jeder Spalte (Wochentag oder Monat) wird die Summe der Gewinne für alle entsprechenden Tage/Monate angezeigt. Dies ermöglicht es, Muster zu erkennen (z.B. "Ist Montag ein schlechter Tag für mein Portfolio?").

### 4. Diagrammansicht
Nach einer Berechnung öffnet **Zeige Diagramm** im Menü **Ansicht** ein Liniendiagramm im [Zusatzbereich]({{% relref "/intro/userinterface" %}}); vor der ersten Berechnung ist der Menüpunkt nicht anwählbar. Das Diagramm zeigt für jeden Tag des berechneten Zeitraums fünf Linien in der Hauptwährung. **Wertpapiere + Saldo** ist das Gesamtvermögen am jeweiligen Tag. Die übrigen vier Linien zeigen die Veränderung seit dem ersten Tag des Zeitraums und beginnen deshalb bei null: **Externe Bargeld Ein-/Auszahlung** die seither ein- oder ausbezahlten Beträge, **Gewinn** den seither erzielten Gewinn oder Verlust, **Barsaldo** die Veränderung der Bargeldbestände und **Wertpapiere** die Veränderung des Wertpapierbestands. So lässt sich unterscheiden, ob eine Zunahme des Vermögens aus Einzahlungen oder aus Gewinnen stammt.
Unter dem Diagramm befindet sich ein verkleinertes Abbild der ganzen Zeitachse. Durch Ziehen seiner beiden Ränder grenzen Sie den angezeigten Zeitraum ein, ohne die Berechnung zu wiederholen. Die Legende steht unter dem Diagramm; ein Klick auf einen Eintrag blendet die Linie aus oder wieder ein. Beim Überfahren mit der Maus erscheint der Wert des nächstgelegenen Tages. Wird der Report mit einem anderen Zeitraum neu berechnet, übernimmt ein geöffnetes Diagramm die neuen Werte.

### 5. PDF-Bericht

Über den Menüpunkt **PDF-Bericht...** lässt sich die berechnete Auswertung als PDF-Dokument ausgeben, etwa für die Übergabe an einen Klienten. Zeitraum und Periodenaufteilung werden aus der angezeigten Berechnung übernommen. Welche Abschnitte das Dokument enthält und wie es aufgebaut ist, beschreibt das Kapitel [PDF-Bericht]({{% relref "/reportportfolio/periodperformance/pdfreport" %}}).

## Rendite und Risiko

Die Beträge der Zusammenfassung hängen von der Höhe des eingesetzten Kapitals ab. Ein Gewinn von 1'000 ist bei einem Portfolio von 10'000 viel und bei einem von 1'000'000 wenig, und sobald Geld ein- oder ausbezahlt wurde, sagt auch der Gewinn geteilt durch den Anfangsbestand nichts mehr über die Rendite aus. Zwischen der Zusammenfassung und der Tabelle zeigt der Report deshalb relative Kennzahlen, mit denen sich Zeiträume und Portfolios unterschiedlicher Grösse vergleichen lassen. Sie werden bei jedem Aufruf aus den täglichen Werten des Reports, den datierten Ein- und Auszahlungen und den Gebührenbuchungen des Zeitraums berechnet.

Prozentwerte werden als Prozent ausgegeben, 12.34 bedeutet also 12.34 %. Ein leeres Feld bedeutet, dass sich die Kennzahl für diesen Zeitraum nicht bestimmen lässt; den Grund nennt die Gruppe **Datengrundlage**. Die Kennzahlen sind in vier Gruppen aufgeteilt.

### Rendite

| Kennzahl | Bedeutung |
|------|-------------|
| **Zeitgewichtete Rendite** | Rendite der Anlagen selbst, unabhängig davon, wann und wie viel Geld ein- oder ausbezahlt wurde. Der Report misst die Rendite jedes einzelnen Tages und verkettet diese. Diese Kennzahl eignet sich für den Vergleich von Zeiträumen und Portfolios. |
| **Zeitgewichtete Rendite p.a.** | Die zeitgewichtete Rendite auf ein Jahr umgerechnet. Sie wird erst ab einem Zeitraum von 360 Kalendertagen ausgewiesen, damit nicht die Rendite weniger Monate auf ein Jahr hochgerechnet wird. |
| **Geldgewichtete Rendite (IZF)** | Interner Zinsfuss Ihres eingesetzten Geldes. Sie berücksichtigt, wann wie viel Geld investiert war, und beantwortet damit die Frage, wie sich Ihr Geld tatsächlich verzinst hat. |
| **Geldgewichtete Rendite p.a.** | Die geldgewichtete Rendite auf ein Jahr umgerechnet, ebenfalls erst ab 360 Kalendertagen. |
| **Maximaler Rückgang** | Grösster Rückgang des zeitgewichteten Werts gegenüber einem vorherigen Höchststand innerhalb des Zeitraums. Ein- und Auszahlungen verändern diesen Wert nicht. |

### Risiko

| Kennzahl | Bedeutung |
|------|-------------|
| **Höchststand vor dem Rückgang** | Datum des Höchststands, von dem aus der maximale Rückgang gemessen wurde. |
| **Tiefpunkt des Rückgangs** | Datum des tiefsten Punkts dieses Rückgangs. |
| **Höchststand wieder erreicht** | Datum, an dem der vorherige Höchststand wieder erreicht wurde. Das Feld bleibt leer, solange dies bis zum Ende des Zeitraums nicht der Fall war. |
| **Aktueller Rückgang** | Abstand am letzten Tag des Zeitraums zum höchsten zeitgewichteten Wert des Zeitraums. Null bedeutet, dass der Zeitraum auf einem Höchststand endet. |
| **Volatilität p.a.** | Annualisierte Schwankungsbreite (Standardabweichung) der Tagesrenditen. Sie wird ab 20 Tagesrenditen ausgewiesen, unabhängig von der Länge des Zeitraums. |
| **Bester Tag bzw. Monat** und **Datum des besten Tags bzw. Monats** | Höchste Rendite eines einzelnen Tages (Aufteilung nach Woche) bzw. eines Monats (Aufteilung nach Jahr) mit dessen letztem Tag. |
| **Schlechtester Tag bzw. Monat** und **Datum des schlechtesten Tags bzw. Monats** | Tiefste Rendite eines einzelnen Tages bzw. Monats mit dessen letztem Tag. |

Beim besten und schlechtesten Tag bzw. Monat werden nur vollständige Schritte berücksichtigt. Ein Tag, dem ein Handelstag ohne vollständige Kurse vorausgeht, umfasst in Wirklichkeit mehrere Tage, und ein Monat, dessen letzte Bewertung fehlt oder dessen Vormonat unvollständig endet, ist kein ganzer Monat. Solche Schritte erscheinen zwar in der Tabelle, werden aber nicht in diese Rangliste aufgenommen. Auch der erste und der letzte Monat eines Zeitraums zählen nur dann, wenn der Zeitraum genau am Monatsende beginnt bzw. endet.

### Kosten

| Kennzahl | Bedeutung |
|------|-------------|
| **Durchschnittlich eingesetztes Kapital** | Das Kapital zu Beginn jedes Tages, gewichtet mit den Kalendertagen. Eine Einzahlung zählt ab dem Tag ihrer Buchung, eine Auszahlung bis zu diesem Tag. |
| **Konto- und Depotgebühren** | Die separat gebuchten Gebühren des Zeitraums, umgerechnet wie in der Auswertung **Portfolios**. Transaktionskosten, die in einem Kauf oder Verkauf enthalten sind, sowie Finanzierungskosten von Margin-Positionen gehören nicht dazu. Eine Rückvergütung verkleinert den Betrag. Fehlt einer Gebühr der Wechselkurs, enthält der Betrag nur die umrechenbaren Gebühren; wie viele fehlen, zeigt **Gebühren ohne Wechselkurs**. |
| **Konto- und Depotgebührenquote** | Die Konto- und Depotgebühren in Prozent des durchschnittlich eingesetzten Kapitals. Die Quote bleibt leer, sobald einer Gebühr des Zeitraums der Wechselkurs fehlt, weil sie sonst die Kosten zu tief auswiese. |
| **Konto- und Depotgebührenquote p.a.** | Die Quote linear auf 365 Tage hochgerechnet, ab einem Zeitraum von 360 Kalendertagen. |

Die Konto- und Depotgebühren dieser Gruppe sind nicht dasselbe wie die Differenz der Zeile **Konto- und Deposten real** der Zusammenfassung. Jene Zeile rechnet alle bisher aufgelaufenen Gebühren eines Fremdwährungskontos mit dem Kurs des jeweiligen Tages um und verändert sich deshalb schon durch eine Kursbewegung, ohne dass eine Gebühr gebucht wurde. Hier zählen nur die Gebühren, die im gewählten Zeitraum gebucht wurden.

### Datengrundlage

| Kennzahl | Bedeutung |
|------|-------------|
| **Kalendertage** | Anzahl Kalendertage vom Startdatum bis zum Enddatum. Unter 360 Tagen bleiben die p.a.-Werte der Renditen und der Gebührenquote leer. |
| **Bewertete Handelstage** | Anzahl Tage im Zeitraum mit vollständigen Kursen. |
| **Erwartete Handelstage** | Werktage im Zeitraum, die kein Feiertag einer Börse der gehaltenen Instrumente sind. Ist diese Zahl grösser als die der bewerteten Handelstage, fehlen an einzelnen Tagen Kurse. |
| **Renditen über Lücken** | Anzahl Renditen, die mindestens einen Handelstag ohne vollständige Kurse überspannen. |
| **Gebühren ohne Wechselkurs** | Anzahl Gebührenbuchungen des Zeitraums, die mangels Wechselkurs nicht in die Hauptwährung umgerechnet werden konnten. Ist sie grösser als null, fehlen diese Gebühren in den Konto- und Depotgebühren, und die Gebührenquoten bleiben leer. |
| **Tagesrenditen für die Volatilität** | Anzahl Tagesrenditen, aus denen die Volatilität berechnet wurde. Renditen über Lücken zählen nicht dazu. |
| **Geldgewichtete Rendite** | Ergebnis der Berechnung des internen Zinsfusses: «berechnet», «Totalverlust», «keine Lösung zwischen −100 % und einer sehr hohen Rendite», «nicht eindeutig wegen abwechselnder Ein- und Auszahlungen» oder «zu wenige Zahlungen». Nur bei den ersten beiden Ergebnissen wird eine geldgewichtete Rendite ausgewiesen. |

### Zeitgewichtete oder geldgewichtete Rendite?

Ohne Ein- und Auszahlungen sind beide Renditen gleich. Unterschiedlich werden sie erst, wenn während des Zeitraums Geld fliesst. Dazu ein Beispiel über ein Jahr: Ein Portfolio startet mit 10'000 und gewinnt im ersten Halbjahr 10 % auf 11'000. Nun werden weitere 10'000 eingezahlt, der Bestand beträgt 21'000. Im zweiten Halbjahr verlieren die Anlagen 10 %, am Ende stehen 18'900.

- Die **zeitgewichtete Rendite** verkettet die beiden Halbjahre: 1.10 × 0.90 − 1 = −1 %. Sie beschreibt, wie gut die Anlagen selbst waren, und ist unabhängig davon, dass die Einzahlung zu einem ungünstigen Zeitpunkt kam.
- Die **geldgewichtete Rendite** beträgt rund −7.3 %. Sie berücksichtigt, dass im schlechten zweiten Halbjahr doppelt so viel Geld investiert war wie im guten ersten, und beschreibt damit, wie es Ihrem Geld tatsächlich ergangen ist.
- Der Gewinn geteilt durch den Anfangsbestand ergibt dagegen (18'900 − 20'000) / 10'000 = −11 %. Diese Zahl ist keine Rendite, weil die Einzahlung von 10'000 nicht im Anfangsbestand enthalten ist.

Für den Vergleich zweier Zeiträume oder zweier Portfolios eignet sich die zeitgewichtete Rendite. Die geldgewichtete Rendite zeigt, wie sich die Entscheidungen, wann Geld investiert oder abgezogen wurde, ausgewirkt haben.

### Grenzen der Kennzahlen

- Einzahlungen gelten als zu Beginn des Tages, Auszahlungen als am Ende des Tages geflossen. Fallen eine Einzahlung und ein Kursgewinn auf denselben Tag, verteilt sich der Gewinn auf das grössere Kapital.
- Tage ohne vollständige Kurse sind keine Bewertung. Die zeitgewichtete Rendite über eine solche Lücke behandelt alle Einzahlungen der Lücke als an ihrem Anfang und alle Auszahlungen als an ihrem Ende geflossen, und ein Tiefpunkt an einem solchen Tag fehlt im Rückgang. Die geldgewichtete Rendite verwendet dagegen die tatsächlichen Buchungstage. Die Volatilität lässt Renditen über eine Lücke aus; die Gruppe **Datengrundlage** zeigt, wie viele es sind.
- Ein Instrument, dessen Kurse periodenweise konstant erfasst werden, gilt als vollständig bewertet. Volatilität und Rückgang fallen dadurch tiefer aus, als sie tatsächlich waren.
- Alle Kennzahlen sind in der Hauptwährung gerechnet und enthalten deshalb auch Währungsgewinne und -verluste. Dividenden, Zinsen und gebuchte Gebühren sind im Vermögen enthalten, Steuern soweit sie gebucht wurden. Die Renditen sind somit weder «brutto» noch «nach allen Kosten».
- Die täglichen Werte sind auf zwei Nachkommastellen gerundet. Bei einem sehr kleinen Vermögen kann diese Rundung die Prozentwerte sichtbar verändern.
- Die Konto- und Depotgebührenquote umfasst nur separat gebuchte Konto- und Depotgebühren, keine Transaktions-, Finanzierungs- oder in Fonds enthaltenen Kosten. Sie ist deshalb nicht der Renditeverlust durch sämtliche Kosten. Die p.a.-Quote ist linear hochgerechnet, sodass eine jährliche Depotgebühr in einem Zeitraum von 13 Monaten zweimal enthalten sein kann.
- Bei abwechselnden Ein- und Auszahlungen kann die geldgewichtete Rendite mehrdeutig sein. Der Report zeigt dann keinen Wert und nennt den Grund.
- Ein Vermögen von null oder darunter, das etwa mit Margin-Positionen oder Leerverkäufen entstehen kann, zählt als Totalverlust. Spätere Perioden zeigen in der Tabelle wieder ihre eigene Rendite, der gesamte Zeitraum bleibt jedoch bei −100 %.
- Die p.a.-Werte werden auf 365 Tage gerechnet. Ein Schaltjahr hat von Datum zu Datum 366 Tage, der p.a.-Wert liegt dann knapp unter der Rendite des Zeitraums.
- Alle Angaben zum Rückgang beziehen sich auf den gewählten Zeitraum. Ein Höchststand vor dem Startdatum wird nicht berücksichtigt.

## Vergleich mit der Auswertung Portfolios

Einige Grössen erscheinen sowohl im **Periodenertrag** als auch in der Auswertung [Portfolios und Portfolio](../portfolios/). Dass sich die Beträge unterscheiden können, hat System und bedeutet nicht, dass eine der beiden Auswertungen falsch rechnet. Dieser Abschnitt erklärt, woher die Unterschiede kommen, wie gross sie sein dürfen und wann die Werte exakt übereinstimmen müssen.

### Zwei verschiedene Konstruktionen

Die Auswertung **Portfolios** ist eine Momentaufnahme auf ein Stichdatum. Sie rechnet bei jedem Aufruf die gesamte Geschichte neu durch, von der allerersten Transaktion bis zum Stichdatum, und behält dabei nichts zurück.

Der **Periodenertrag** ist eine Zeitreihe über Handelstage. Er liest laufend nachgeführte Tagessalden, die bei jeder Buchung mitgeschrieben werden, und zeigt daraus den Verlauf sowie die Differenz zwischen zwei Tagen.

Die beiden Berichte rechnen also nicht denselben Weg zweimal nach. Es sind zwei verschiedene Konstruktionen, die einige Grössen gemeinsam haben.

### Welche Werte sich entsprechen

| Periodenertrag | Portfolios und Portfolio | Übereinstimmung |
|---|---|---|
| **Konto- und Deposten real** | **Konto- und Depotkosten** | Dieselben Buchungen. Bei einem Fremdwährungskonto verschieden umgerechnet, siehe unten. |
| **Kontozins real** | **Kontozins** | Dieselben Buchungen. Bei einem Fremdwährungskonto verschieden umgerechnet, siehe unten. |
| **Externe Bargeld Ein-/Auszahlung** | **Externe Bargeld Ein-/Auszahlung** | Gleiche Umrechnung, deshalb bis auf Rundungen gleich. |
| **Barsaldo** | **Barsaldo** | Beide auf den Stichtag bewertet, deshalb bis auf Rundungen gleich. |

### Der Wechselkurs: der wichtigste Grund für eine Abweichung

Die Auswertung **Portfolios** rechnet jede einzelne Buchung mit dem Wechselkurs ihres eigenen Buchungstages um. Der **Periodenertrag** führt den aufgelaufenen Betrag in der Währung des Kontos und rechnet ihn einmal um, mit dem Wechselkurs des Auswertungstages.

```mermaid
graph LR
    T[Buchung in Fremdwährung] --> A[Kurs des Buchungstages]
    A --> P[Portfolios: Konto- und Depotkosten, Kontozins]
    T --> S[Laufender Saldo in Kontowährung]
    S --> B[Kurs des Auswertungstages]
    B --> R[Periodenertrag: Konto- und Deposten real, Kontozins real]
```

Ein Beispiel macht es anschaulich. Auf einem Euro-Konto wurden Ende 2008 EUR 218.01 Zins gutgeschrieben. Bei einem Kurs von 1.4655 waren das damals CHF 319.49, und mit diesem Betrag erscheint die Gutschrift in der Auswertung **Portfolios**. Der **Periodenertrag** rechnet dieselben EUR 218.01 mit dem Kurs des Auswertungstages um, und bei einem Kurs von 0.9386 sind das CHF 204.62.

Keiner der beiden Werte ist falsch. Der eine sagt, was der Zins zum Zeitpunkt der Gutschrift wert war, der andere, was derselbe Betrag am Auswertungstag wert ist. Der Unterschied wächst mit dem Alter der Buchungen und mit der Bewegung der Währung: Bei einem Konto, das seit Jahren in einer Währung geführt wird, die gegenüber der Hauptwährung deutlich verloren oder gewonnen hat, können die beiden Beträge um mehrere Prozent auseinanderliegen.

Wer das nicht will, kann die Auswertung **Portfolios** auf den Weg des Periodenertrags umstellen. Mit der Klienteneinstellung **Kosten und Zinsen zum Stichdatum umrechnen** rechnet auch sie diese beiden Grössen mit dem Kurs des Stichdatums um, siehe [Klient](../../tenantportfolio/client/); danach stimmen die vier Zeilen dieser Tabelle bis auf die Rundung überein. Der Preis dafür ist, dass die Kursbewegung dieser Buchungen in der Auswertung **Portfolios** nicht mehr gesondert im **Währungsgewinn** erscheint.

### Rundung

Die beiden Auswertungen runden an verschiedenen Stellen des Rechenwegs. Eine Abweichung von wenigen Rappen ist deshalb auch dort zu erwarten, wo sonst alles übereinstimmt, und sagt nichts über die Datenqualität aus.

### Fehlende Kurse wirken sich verschieden aus

Der **Periodenertrag** lässt einen ganzen Tag aus, sobald an diesem Tag der Kurs eines gehaltenen Wertpapiers oder ein benötigter Wechselkurs fehlt. Die Auswertung **Portfolios** kennt das nicht. Sie lässt stattdessen dasjenige Konto weg, das sie nicht umrechnen kann, und weist die betroffenen Währungen gesondert aus.

Eine Abweichung kann deshalb auch daher rühren, dass die beiden Berichte gar nicht auf demselben Datenbestand aufsetzen. Sind Kurse lückenhaft, lohnt es sich, diese zuerst zu ergänzen und die Auswertungen danach erneut zu vergleichen.

### Wann die Werte exakt übereinstimmen

Bei einem Konto, das in der Hauptwährung geführt wird, ist keine Umrechnung nötig, und beide Auswertungen liefern denselben Betrag. Dasselbe gilt für die Grössen, die auf beiden Seiten gleich umgerechnet werden: **Externe Bargeld Ein-/Auszahlung** und **Barsaldo**, und für alle vier Grössen, sobald die Klienteneinstellung **Kosten und Zinsen zum Stichdatum umrechnen** gesetzt ist. Übrig bleiben in allen Fällen die Rundungsdifferenzen von wenigen Rappen.

{{% notice info %}}
So lässt sich eine Abweichung einordnen: Wenige Rappen sind eine Rundung. Prozente bei einem Fremdwährungskonto sind der oben beschriebene Wechselkurseffekt und kein Fehler in den Daten. Ein unerwarteter Sprung, für den beides nicht in Frage kommt, deutet auf fehlende Kursdaten hin; siehe [Fehlende Tagesendkurse](#fehlende-tagesendkurse).
{{% /notice %}}

### Finanzierungskosten von Margin-Positionen

Diese fallen bei Margin-Produkten wie Forex und CFD laufend an und werden dem Bargeldkonto belastet. Trotzdem erscheinen sie in **keiner** der beiden Kostenspalten: weder in **Konto- und Deposten real** noch in **Konto- und Depotkosten**. Sie sind Kosten der Position und nicht des Bankkontos und werden deshalb über das Ergebnis der Wertpapiere ausgewiesen.

Im Periodenertrag haben sie keine eigene Zeile. Sie mindern den **Barsaldo** und damit den **Gewinn**, werden aber nirgends gesondert ausgewiesen.

## Fehlende Tagesendkurse

Ein wichtiger Aspekt des Periodenertrag-Reports ist die Behandlung fehlender Kursdaten. Fehlende Kursdaten entstehen, wenn an einem Handelstag keine historischen Kurse für gehaltene Wertpapiere verfügbar sind. Dies kann verschiedene Ursachen haben: Der Kursanbieter hat keine Daten geliefert, es sind technische Probleme beim Datenabruf aufgetreten, oder das Wertpapier wurde an diesem Tag tatsächlich nicht gehandelt.

Die Auswirkungen auf den Report sind erheblich. Fehlende Tage werden im Periodenkalender rot markiert, und die Performance kann an diesen Tagen nicht berechnet werden. Genau gleich behandelt wird ein fehlender **Wechselkurs**: Kann ein Wertpapier oder ein Kontosaldo an einem Tag mangels Kurs des Währungspaars nicht in die Hauptwährung umgerechnet werden, so fällt dieser Tag ebenfalls aus dem Report. Das ist beabsichtigt, denn andernfalls würde ein Fremdwährungsbetrag unumgerechnet in die Auswertung einfliessen und einen Gewinn oder Verlust vortäuschen, den es nie gab. Zur Entstehung solcher Lücken siehe [Kurse für jeden Kalendertag]({{% relref "/watchlistinstrument/instrument/currencypair" %}}#kurse-für-jeden-kalendertag). Das System zählt die aufeinanderfolgenden fehlenden Tage und zeigt diese im Feld "Fehlende Tage" an. Bei der Datumsauswahl werden Tage mit fehlenden Kursen automatisch als ungültige Handelstage markiert, sodass diese nicht als Start- oder Enddatum gewählt werden können.

{{% notice warning %}}
Ohne vollständige historische Kursdaten ist eine genaue Performance-Berechnung nicht möglich. Es wird dringend empfohlen, fehlende Kurse vor der Analyse zu beheben, um aussagekräftige Ergebnisse zu erhalten.
{{% /notice %}}

### Übersicht und Interaktion mit fehlenden Kursen

Der Report bietet eine separate Ansicht zur Analyse und Behebung fehlender Kursdaten. Diese kann über das Menü unter "Fehlende Tagesendkurse" aufgerufen werden und zeigt zwei miteinander verbundene Bereiche: Einen Jahreskalender im oberen Bereich und eine Wertpapier-Tabelle im unteren Bereich.

Der **Jahreskalender** zeigt alle Handelstage des ausgewählten Jahres in einer übersichtlichen Darstellung. Die Farbcodierung macht sofort ersichtlich, wo Probleme vorliegen: Grün markierte Tage signalisieren, dass alle Kurse verfügbar sind. Rot markierte Tage zeigen an, dass mindestens ein Wertpapier fehlende Kurse aufweist. Gelb markierte Tage sind Tage mit fehlenden Kursen, die vom Benutzer selektiert wurden. Wenn Sie auf einen rot markierten Tag klicken, reagiert die Wertpapier-Tabelle sofort und zeigt nur noch die Wertpapiere an, die an diesem spezifischen Tag fehlende Kurse aufweisen.

Die **Wertpapier-Tabelle** listet alle Wertpapiere auf, die im gewählten Zeitraum fehlende Kurse haben. Neben dem Namen des Wertpapiers wird auch die Anzahl der fehlenden Tage angezeigt. Auch hier funktioniert die Interaktion bidirektional: Wenn Sie in der Tabelle auf ein Wertpapier klicken, werden im Kalender automatisch alle Tage gelb markiert, an denen dieses spezifische Wertpapier fehlende Kurse hat. Diese bidirektionale Interaktion ermöglicht es, systematische Datenlücken schnell zu identifizieren und gezielt zu beheben.

In derselben Tabelle erscheinen auch die **Währungspaare**, deren Wechselkurs an einzelnen Tagen fehlt. Sie stehen dort unter ihrer Bezeichnung wie «USD/CHF», während ISIN sowie «Aktiv von» und «Aktiv bis oder Fälligkeitsdatum» bei ihnen leer bleiben, da ein Währungspaar diese Angaben nicht kennt. Im Kalender werden ihre Tage genauso rot markiert wie jene der Wertpapiere, und die Auswahl wirkt in beide Richtungen wie bei einem Wertpapier.

Fehlt einem benötigten Währungspaar der Kurs nicht nur an einzelnen Tagen, sondern gänzlich, so bleibt für den Periodenertrag kein einziger auswertbarer Tag übrig. In diesem Fall bricht der Report mit einer Meldung ab, welche die betroffenen Währungspaare nennt, damit Sie die Ursache nicht im Wertpapierbestand suchen. Wie es zu einem Währungspaar ganz ohne Kursgeschichte kommt, ist unter [Wenn ein Währungspaar ohne Kurse bleibt]({{% relref "/watchlistinstrument/instrument/currencypair" %}}#wenn-ein-währungspaar-ohne-kurse-bleibt) beschrieben.

{{% notice tip %}}
Die Kombination aus Kalender und Tabelle erlaubt zwei Analyseansätze: Sie können entweder von einem problematischen Tag ausgehen und sehen, welche Wertpapiere betroffen sind, oder Sie können ein problematisches Wertpapier auswählen und sehen, an welchen Tagen Kurse fehlen.
{{% /notice %}}

### Lösung des Problems fehlender Kursdaten

Für die Behebung fehlender historischer Kursdaten stehen mehrere Möglichkeiten zur Verfügung. Die einfachste Methode ist das **manuelle Nachladen** der Kursdaten. Über die Wertpapier-Verwaltung können historische Kurse erneut vom Datenanbieter abgerufen werden. Wählen Sie das betroffene Wertpapier aus und triggern Sie eine Aktualisierung der historischen Daten.

Eine besonders elegante Lösung für einzelne fehlende Tage zwischen vorhandenen Kursen ist das **lineare Befüllen fehlender Kursdaten**. Das System kann Kurslücken durch lineare Interpolation füllen, wobei der fehlende Kurs aus den benachbarten vorhandenen Kursen berechnet wird. Diese Methode eignet sich besonders für einzelne fehlende Tage in ansonsten vollständigen Kursreihen, beispielsweise wenn der Datenanbieter an einem einzelnen Tag einen Ausfall hatte.

{{% notice style="info" title="Lineares Befüllen von Kurslücken" %}}
Für eine detaillierte Anleitung zum linearen Befüllen fehlender Kursdaten siehe [Lineares Befüllen fehlender Kursdaten](/gt-user-manual/de/watchlistinstrument/externaldata/historyquote/pricedata/). Diese Funktion interpoliert fehlende Werte basierend auf den umgebenden Kursen und eignet sich ideal für einzelne Lücken in der Zeitreihe.
{{% /notice %}}

Wenn ein Datenanbieter systematisch Lücken aufweist, kann es sinnvoll sein, zu einem **alternativen Datenanbieter** zu wechseln. GT unterstützt verschiedene Datenanbieter, und oft bietet ein anderer Anbieter eine bessere Abdeckung für bestimmte Märkte oder Wertpapiere. Die Umstellung des Datenanbieters erfolgt in den Einstellungen des jeweiligen Wertpapiers.

Für wenige fehlende Tage, insbesondere bei exotischen Wertpapieren mit schlechter Datenabdeckung, besteht auch die Möglichkeit der **manuellen Kurseingabe**. Diese Option ist zwar zeitaufwändig, kann aber in Einzelfällen die einzige Möglichkeit sein, eine vollständige Kurshistorie zu erreichen.

## Technische Details

### Handelstage und Feiertage

Das System berücksichtigt automatisch:
- **Globale Feiertage**: Weltweit gültige Nicht-Handelstage
- **Börsenspezifische Feiertage**: Feiertage der Börsen, an denen die gehaltenen Wertpapiere gehandelt werden
- **Wochenenden**: Samstag und Sonntag sind immer ausgeschlossen

### Währungskonvertierung

Alle Wertpapiere und Konten werden automatisch in die Hauptwährung umgerechnet, bei einer Mandantenauswertung in die Mandantenwährung und bei einer Portfolioauswertung in die Portfoliowährung.

Massgebend ist dabei der Wechselkurs des jeweils ausgewerteten Tages. Das gilt auch für die kumulierten Zeilen **Zins/Dividende real**, **Konto- und Deposten real**, **Kontozins real**, **Saldo Kauf/Verkauf Wertpapiere** und **Barsaldo**: Der über Jahre aufgelaufene Betrag wird in der Währung des Kontos geführt und erst zum Schluss mit dem Kurs des Auswertungstages umgerechnet. Eine Ausnahme bildet **Externe Bargeld Ein-/Auszahlung**, wo jede Ein- und Auszahlung mit dem Kurs ihres eigenen Buchungstages umgerechnet wird.

Für ein Konto in der Hauptwährung spielt das keine Rolle. Bei einem Fremdwährungskonto führt es dazu, dass sich diese Zeilen von den entsprechenden Spalten der Auswertung **Portfolios** unterscheiden können, siehe [Vergleich mit der Auswertung Portfolios](#vergleich-mit-der-auswertung-portfolios).

### Performance-Optimierung

- Das System verwendet einen Cache mit 2-minütiger Gültigkeit für Handelstag-Metadaten
- Ergebnisse werden für wiederholte Anfragen innerhalb der Cache-Periode wiederverwendet

## Typische Anwendungsfälle

1. **Performance-Analyse**: Wie hat sich mein Portfolio über das letzte Quartal entwickelt?
2. **Einflussanalyse**: Welche Faktoren (Gewinne, Einzahlungen, Kursentwicklung) haben die Performance beeinflusst?
3. **Muster-Erkennung**: Gibt es bestimmte Wochentage oder Monate mit besonders guter/schlechter Performance?
4. **Datenqualität**: Gibt es systematische Lücken in meinen historischen Kursdaten?
5. **Vergleich**: Wie unterscheidet sich die Performance verschiedener Portfolios?

## Einschränkungen und Hinweise

- Der Report erfordert mindestens einen Wertpapierbestand. Wurden bisher nur Konten geführt und nie ein Wertpapier gehalten, bleibt das Eingabeformular gesperrt.
- Für eine aussagekräftige Analyse sollte der Zeitraum mindestens einige Handelstage umfassen
- Fehlende Kursdaten können die Genauigkeit der Berechnungen beeinträchtigen
- Die Auswahl der Periodenaufteilung wird automatisch basierend auf dem Zeitraum eingeschränkt
- Bei sehr grossen Zeiträumen (mehrere Jahre) kann die Berechnung einige Sekunden dauern
