---
title: "Ertrag je Wertpapier"
date: 2026-11-24T18:00:00+01:00
draft: false
weight: 22
archetype: "default"
---
Der [Periodenertrag](../periodperformance/) beantwortet, wie sich ein Portfolio oder der ganze Mandant in einem Zeitraum entwickelt hat. Die Auswertung **Ertrag je Wertpapier** zerlegt denselben Zeitraum nach Wertpapieren. Für jedes Wertpapier, das im Zeitraum gehalten oder gehandelt wurde, zeigt sie zwei Zahlen: den **Gewinn Periode** in der Hauptwährung, also wo das Geld verdient wurde, und die **Zeitgewichtete Rendite** der Position in Prozent, also wie gut das Wertpapier lief, während Sie es hielten.

## Aufruf
Auf **Mandantenebene** steht die Auswertung als Reiter **Ertrag je Wertpapier** neben **Ertrag** im **Periodenertrag**; die Beträge sind in der Mandantenwährung. Innerhalb eines **Portfolios** folgt der Reiter **Ertrag je Wertpapier** direkt auf **Periodenertrag**; die Beträge sind in der Portfoliowährung. Auf Mandantenebene wird ein Wertpapier, das in mehreren Portfolios liegt, zu einer Zeile zusammengefasst.

## Zeitraum
**Datum von** und **Datum bis** gelten wie im Periodenertrag: Beide Daten beziehen sich auf den Börsenschluss, der Tag des Startdatums dient als Vergleichsbasis und gehört nicht zum Zeitraum. Wählbar sind nur Tage, an denen jedes gehaltene Wertpapier einen Kurs hat. Die beiden Ansichten teilen sich den Zeitraum: Ein in der einen Ansicht gewähltes Startdatum ist in der anderen bereits eingetragen. Endet der Zeitraum am letzten verfügbaren Handelstag, so rückt das Enddatum beim nächsten Öffnen auf einen neueren Handelstag nach. Die Berechnung startet erst mit **Übernehmen**.

## Die Tabelle
Jede Zeile ist ein Wertpapier. Die Tabelle ist nach **Gewinn Periode** absteigend sortiert; ein Klick auf eine Spaltenüberschrift sortiert nach dieser Spalte, etwa nach der Rendite. Über der Tabelle steht der berechnete Zeitraum, sodass erkennbar bleibt, wofür das Ergebnis gilt, auch wenn die Daten im Formular inzwischen geändert wurden. Beträge in der Hauptwährung tragen deren Bezeichnung in der Spaltenüberschrift. Einige Spalten sind zunächst ausgeblendet und lassen sich über die Spaltenauswahl der Tabelle einblenden.

| Spalte | Bedeutung |
|---|---|
| **Name**, **ISIN**, **Währung** | Das Wertpapier und die Währung, in der es gehandelt wird. |
| **Haltetage** | Kalendertage innerhalb des Zeitraums, an denen das Wertpapier bei Handelsschluss gehalten wurde. Wurde es während des ganzen Zeitraums gehalten, entspricht die Zahl der Länge des Zeitraums. Ein am selben Tag gekauftes und verkauftes Wertpapier hat null Haltetage. |
| **Stück am Anfang**, **Stück am Ende** | Die am Start- bzw. Enddatum gehaltene Stückzahl wie gehandelt. Ein Split ändert die Stückzahl, nicht den Wert. |
| **Wert am Anfang**, **Wert am Ende** | Wert der Position zum Schlusskurs des Start- bzw. Enddatums, umgerechnet mit dem Schlusskurs der Währung desselben Tages. |
| **Investiert** | Käufe des Zeitraums einschliesslich ihrer Kosten. |
| **Zurückgeflossen** | Verkäufe und Erträge des Zeitraums nach Kosten. |
| **Gewinn Periode in Wertpapierwährung** | Der Gewinn des Zeitraums in der Währung des Wertpapiers. |
| **Gewinn Periode** | Wert am Ende minus Wert am Anfang minus Investiert plus Zurückgeflossen. Er enthält Kursgewinn, Erträge, Transaktionskosten und das Währungsergebnis der Position. |
| **Zeitgewichtete Rendite in Wertpapierwährung** | Die Rendite der Position in der Währung des Wertpapiers, ohne Wechselkursbewegung. |
| **Zeitgewichtete Rendite** | Die Rendite der Position in der Hauptwährung über die Tage, an denen sie gehalten wurde, gemessen von Schlusskurs zu Schlusskurs. |
| **Bemerkung** | Der Grund, wenn eine Zahl der Zeile fehlt, siehe unten. |

Die Fusszeile **Gesamtsumme** fasst die Beträge aller Zeilen in der Hauptwährung zusammen, unabhängig von einem Filter der Tabelle. Fehlt mindestens einer Zeile der Gewinn, heisst sie **Teilsumme**, denn diese Zeilen sind darin nicht enthalten. Durch Aufklappen einer Zeile erscheinen die Transaktionen des Wertpapiers bis zum Enddatum.

## Diagramme
Über **Ansicht** → **Zeige Diagramm** erscheinen die Diagramme des berechneten Zeitraums im unteren Bereich. Der **Diagrammtyp** wählt zwischen **Rendite gegen Gewinn** und **Treemap der Gewinne und Verluste**; die Wahl bleibt gespeichert. Nach einer neuen Berechnung zeigt das Diagramm deren Ergebnis. Wechseln Sie zu einem anderen Reiter, bleibt das Diagramm erhalten; kehren Sie zurück, zeigt auch die Tabelle die letzte Berechnung wieder. Erst die Auswertung eines anderen Portfolios oder des Mandanten leert es. Ein Klick auf ein Wertpapier im Diagramm klappt dessen Zeile in der Tabelle auf.

### Rendite gegen Gewinn
Jedes Wertpapier ist ein Punkt: waagrecht die **Zeitgewichtete Rendite**, senkrecht der **Gewinn Periode** in der Hauptwährung. Die beiden Nulllinien teilen das Diagramm in vier Felder:
- **oben rechts**: gut gelaufen und Geld verdient;
- **unten rechts**: gute Rendite, aber wenig verdient, meist eine kleine oder kurz gehaltene Position;
- **oben links**: schwache Rendite, aber viel verdient, weil viel Kapital eingesetzt war;
- **unten links**: Verlust.

Ein Punkt ist umso grösser, je mehr Haltetage das Wertpapier hat. Ein kleiner Punkt mit hoher Rendite ist damit als kurz gehaltene Position erkennbar. Ein Verlust ist rot. Fährt die Maus über einen Punkt, erscheinen Name, Gewinn, Rendite und Haltetage. Wertpapiere ohne Rendite oder Gewinn, etwa Margin-Instrumente, fehlen in diesem Diagramm; ihre Anzahl steht unter dem Titel.

### Treemap der Gewinne und Verluste
Die Treemap zeigt, woher der Gewinn stammt und wo Geld verloren ging. Sie hat zwei Äste, **Gewinne** in Grün und **Verluste** in Rot. Die Fläche einer Kachel ist der Gewinn oder Verlust des Wertpapiers in der Hauptwährung, unabhängig von der Rendite. Die beiden Äste stehen im Verhältnis ihrer Summen zueinander: Ein kleiner Gesamtverlust neben einem grossen Gesamtgewinn ist auch klein. Ein Klick auf einen Ast vergrössert ihn, sodass auch kleine Kacheln lesbar werden; die Leiste darüber führt zurück. Unter dem Titel stehen die Summe der Gewinne, die Summe der Verluste und der Saldo, der der Gesamtsumme der Tabelle entspricht. Margin-Instrumente sind enthalten. Wertpapiere ohne Gewinn oder mit einem Gewinn von null haben keine Fläche; ihre Anzahl steht unter dem Titel.

## Gewinn und Rendite
Ein Beispiel: Ein Wertpapier war am Startdatum 10'000 wert. Während des Zeitraums wurden für 5'000 weitere Stücke gekauft, für 3'000 Stücke verkauft und 200 Dividende gutgeschrieben. Am Enddatum ist die Position 13'000 wert. Der **Gewinn Periode** beträgt 13'000 − 10'000 − 5'000 + 3'200 = 1'200.

Die **Zeitgewichtete Rendite** misst dagegen, wie sich das Wertpapier selbst entwickelt hat, unabhängig davon, wie viel davon wann gehalten wurde. Sie verkettet die Renditen der einzelnen Tage. Ein Kauf zählt ab dem Schlusskurs des Vortages, ein Verkauf und ein Ertrag zum Schlusskurs des Tages. Abschnitte, in denen das Wertpapier nicht gehalten wurde, zählen nicht. Wurde eine Position verkauft und später wieder gekauft, verkettet die Rendite beide Haltephasen.

Die Rendite in der Hauptwährung enthält die Bewegung des Wechselkurses, jene in der Wertpapierwährung nicht. Der Unterschied der beiden Spalten zeigt also, wie stark die Währung die Rendite verändert hat.

## Vergleich mit der Benchmark
Das Feld **Vergleichsinstrument** zeigt die gespeicherte [Benchmark]({{% relref "/reportportfolio/periodperformance" %}}#vergleich-mit-einer-benchmark) des Portfolios oder des Mandanten. Die Schaltfläche im Feld wählt eine andere Benchmark nur für die nächste Berechnung, und der Knopf **Als Standard speichern** dahinter speichert sie wie im Periodenertrag als Standard des Portfolios oder des Mandanten. **Benchmark für diesen Bericht ausblenden** im Menü **Ansicht** rechnet ohne Vergleich.

Mit einer Benchmark kommen die Spalten **Benchmark-Rendite** und **Differenz zur Benchmark (Pp.)** hinzu. Die Benchmark wird über genau die Tage verkettet, an denen die Position Kapital hielt. Wurde eine Position im März verkauft und im Oktober wieder gekauft, zählt die Bewegung der Benchmark dazwischen also nicht. Deckt der Vergleichszeitraum der Benchmark einen Haltetag nicht ab, bleiben beide Spalten der Zeile leer, und der Kurzhinweis der Zelle nennt den Grund. Im Diagramm **Rendite gegen Gewinn** zeigt der Kurzhinweis eines Punktes die Benchmark-Rendite ebenfalls.

## Margin-Instrumente
Bei CFD und Forex ist der Wert der Position der offene Gewinn oder Verlust und kein eingesetztes Kapital. Eine Rendite darauf wäre ohne Aussage, deshalb zeigen diese Zeilen nur den Gewinn. **Investiert** enthält bei ihnen die realisierten Verluste und Kosten, **Zurückgeflossen** die realisierten Gewinne. Der Gewinn Periode ist die Veränderung des offenen Ergebnisses zuzüglich dessen, was im Zeitraum abgerechnet wurde.

## Bemerkungen
Eine Zahl, die sich nicht bestimmen lässt, bleibt leer und wird nie als null ausgegeben. Die Spalte **Bemerkung** nennt den Grund:
- **Kein Kurs am Anfang oder Ende der Periode**: Das Wertpapier wurde am Start- oder Enddatum gehalten, es fehlt aber der Kurs oder der Wechselkurs. Werte, Gewinn und Rendite bleiben leer.
- **Wechselkurs für eine Buchung fehlt**: Für einen Kauf, Verkauf oder Ertrag des Zeitraums fehlt der Wechselkurs in die Hauptwährung. Die Beträge in der Hauptwährung und die Rendite in der Hauptwährung bleiben leer; Gewinn und Rendite in der Wertpapierwährung werden trotzdem ausgewiesen.
- **Negative Stückzahl, keine Rendite**: An einem Tag des Zeitraums war die Stückzahl negativ. Der Gewinn wird ausgewiesen.
- **An keinem Handelsschluss gehalten, keine Rendite**: Es gab im Zeitraum kein eingesetztes Kapital, etwa weil nur eine Dividende nach dem Verkauf gutgeschrieben wurde. Diese Dividende zählt zum Gewinn.
- **Margin-Instrument, nur Gewinn**: siehe oben.

## Einschränkungen
- Start und Ende werden mit Schlusskursen und Schlusskursen der Währung bewertet. Endet der Zeitraum am letzten Handelstag, kann der Gewinn deshalb von der Auswertung [Depots](../securityaccountreport/) abweichen, die den jüngsten Tageskurs verwendet.
- Käufe, Verkäufe und Erträge in Fremdwährung werden mit dem Schlusskurs der Währung ihres Tages umgerechnet, nicht mit dem Kurs der Buchung. Der Gewinn Periode in der Hauptwährung ist deshalb nicht die Differenz der Spalten Gewinn und Währungsgewinn der Auswertung **Depots** an den beiden Daten, denn jene rechnet den Gewinn über die ganze Haltedauer mit dem Kurs des Stichdatums um und weist den Rest als Währungsgewinn aus.
- Die Rendite misst das Wertpapier über die Tage, an denen es gehalten wurde. Eine Position, die nur drei Tage gehalten wurde, kann deshalb eine hohe Rendite zeigen; die Haltetage stehen daneben.
- Da ein Kauf ab dem Schlusskurs des Vortages zählt, verschiebt ein grosser Kauf an einem Tag mit starker Kursbewegung die Rendite dieses Tages spürbar. Dasselbe gilt für einen Abschnitt über einen Tag ohne Kurs.
- Die Rendite vergrössert widersprüchliche Daten: Ein Geschäft, das weit vom Schlusskurs seines Tages entfernt gebucht ist, oder ein Split, den die Kurshistorie nicht abbildet, ergibt eine unplausible Rendite, während der Gewinn dieselbe Abweichung zeigt wie die Auswertung **Depots**.
- Gewinn und Rendite verstehen sich nach Transaktionskosten und, bei Dividenden, vor oder nach Quellensteuer gemäss der Einstellung des Klienten. Konto- und Depotgebühren werden keinem Wertpapier zugeordnet.
- Eine Dividende zählt an ihrem Buchungstag, nicht am Ex-Tag. Kursrückgang und Ertrag können deshalb auf verschiedene Tage fallen.
- Ein Ertrag, der eingeht, während das Wertpapier nicht gehalten wird, etwa eine Dividende nach dem Verkauf, zählt zum Gewinn, aber nicht zur Rendite.
- Finanzierungskosten einer CFD- oder Forex-Position zählen ebenfalls an ihrem Buchungstag. Werden sie beim Schliessen für die ganze Haltedauer gebucht, fällt auch der Anteil früherer Zeiträume in den Zeitraum dieser Buchung.
- Ein ISIN-Wechsel, eine Abspaltung oder ein Bezugsrecht, die als Verkauf und Kauf gebucht sind, erscheinen als zwei Wertpapiere.
- Die Gewinne der Zeilen ergeben zusammen nicht das Ergebnis des Periodenertrags, denn dieses enthält zusätzlich Zinsen, Gebühren und das Währungsergebnis der Bargeldkonten.
- Ein Portfolio, dessen Geschäfte alle am selben Tag eröffnet und geschlossen wurden, hat keinen Bestand und bietet deshalb keinen wählbaren Zeitraum.
