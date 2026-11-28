---
title: "Positionschart"
date: 2026-11-18T22:00:00+01:00
draft: false
weight: 31
archetype: "default"
---
Die [Liniengrafik der Tagesendkurse](../eodchart/) zeigt, wie sich ein **Instrument** entwickelt. Der **Positionschart** zeigt dagegen, wie sich die **eigene Position** in einem **Wertpapier** entwickelt: wie viel Geld darin steckt, was es wert ist, was es bisher eingebracht hat und wie tief es zwischenzeitlich gefallen ist. Er wird im **Zusatzbereich** angezeigt und steht für Wertpapiere einschliesslich CFD und Forex zur Verfügung, nicht aber für Währungspaare. Mit einem Auswahlfeld wird zwischen vier Diagrammtypen gewechselt: **Ergebnis**, **Wertentwicklung**, **Rendite** und **Rückgang (Drawdown)**.

{{% callout info %}}
Sämtliche Zahlen berechnet GT aus den eigenen Transaktionen und den Tagesendkursen des Wertpapiers. Bewertet wird mit dem **Schlusskurs**; der heutige Tag erscheint deshalb erst, wenn sein Schlusskurs vorliegt. Die Kopfzeile nennt das Datum des letzten bewerteten Tages.
{{% /callout %}}

## Aktivierung dieser Ansicht
Der Positionschart wird über den Eintrag **Positionschart** im Kontextmenü geöffnet, der in allen Ansichten mit einem Wertpapier pro Tabellenzeile direkt nach **Hinzufügen als Liniengrafik** steht, beispielsweise in der Watchlist, in einem Portfolio oder in einem Depot. Derselbe Eintrag findet sich im Kontextmenü der Liniengrafik, sofern dort genau ein Wertpapier angezeigt wird. Umgekehrt führt im Positionschart der Eintrag **Tagesendkurse als Linengrafik** zurück zur Liniengrafik desselben Wertpapiers.

### Bereich der Auswertung
Der Positionschart wertet genau den Bereich aus, aus dem er geöffnet wurde. Wird er aus einem **Depot** aufgerufen, zählen nur die Transaktionen und der Bestand dieses Depots. Aus einem **Portfolio** aufgerufen, zählen die Depots dieses Portfolios, und aus der **Watchlist** oder einer Ansicht des Klienten zählen alle Depots. Dasselbe Wertpapier kann deshalb je nach Aufruf unterschiedliche Kurven zeigen. Die Kopfzeile über dem Diagramm nennt deshalb das Wertpapier, den Bereich, die Haltephase und das Datum, bis zu dem bewertet ist.

## Haltephase
Eine **Haltephase** beginnt mit dem ersten Tag, an dessen Ende Stück im Bestand sind, nachdem zuvor keine mehr vorhanden waren. Sie endet am nächsten Tag, an dessen Ende der Bestand wieder null ist. Massgebend ist jeweils der Bestand am Ende des Buchungstages. Ein vollständiger Verkauf mit Wiederkauf am selben Tag bleibt deshalb eine einzige Phase, während ein Kauf und Verkauf aller Stück am selben Tag keine Phase bildet. Eine Dividende, die erst nach dem letzten Verkauf ausbezahlt wird, gehört noch zur beendeten Phase.

Der Positionschart zeigt immer genau eine Haltephase, weil Einstand, Rendite und Rückgang mit jeder Phase neu beginnen. Vorgewählt ist die jüngste Phase, also die offene oder, falls keine offen ist, die zuletzt abgeschlossene. Gibt es mehrere Phasen, erscheint das Auswahlfeld **Haltephase**, in dem jede frühere Phase einzeln geöffnet werden kann.

{{% callout note "Übertrag zwischen Depots" %}}
GT bucht einen Übertrag zwischen zwei Depots als Verkauf im einen und als Kauf im anderen Depot zum Kurs des Übertrags. Wird der Positionschart aus einem einzelnen Depot geöffnet, endet die Phase im abgebenden Depot und beginnt im empfangenden neu. Liegen beide Depots im ausgewerteten Bereich, bleibt es eine Phase. Investiertes Kapital, Summe aller Käufe und realisiertes Ergebnis beginnen dann aber zum Wert des Übertrags neu, so wie der Übertrag gebucht ist. Der Rückgang bleibt davon unberührt, und unter dem Diagramm erscheint ein Hinweis.
{{% /callout %}}

## Auswahl im Kopfteil
Im Kopfteil stehen vier Auswahlfelder in zwei Zeilen:
- **Diagrammtyp** wählt das Diagramm. GT merkt sich die zuletzt gewählte Darstellung; beim ersten Aufruf ist es **Ergebnis**.
- **Bezugsgrösse** erscheint bei **Ergebnis** und **Rendite** und bestimmt, ob nur die **Offene Position** oder das Ergebnis **Gesamt seit Einstieg** gezeigt wird.
- **Haltephase** erscheint, sobald es mehr als eine Haltephase gibt.
- **Währung** erscheint, wenn sich die Währung des Wertpapiers von der Hauptwährung des Bereichs unterscheidet, und wechselt zwischen **Hauptwährung** und **Wertpapierwährung**.

## Diagrammtypen
### Ergebnis
Das **Ergebnis** zeigt den Gewinn oder Verlust der Position in Geld als Fläche um die Nulllinie, grün im Gewinn und rot im Verlust. Mit der Bezugsgrösse **Offene Position** ist es der Wert abzüglich des investierten Kapitals, also der unrealisierte Gewinn oder Verlust. Mit **Gesamt seit Einstieg** kommen die Erträge und das realisierte Ergebnis aus Verkäufen dazu, womit die Kurve alles zeigt, was die Phase bisher eingebracht hat.

Weil das eingesetzte Geld herausgerechnet ist, erzeugen Käufe und Verkäufe keine Sprünge. Eine Kursbewegung von drei Prozent zeigt sich unabhängig von der Grösse der Position als entsprechender Betrag und füllt die ganze Höhe des Diagramms. Darum eignet sich dieser Diagrammtyp besonders für gestaffelte Käufe und Sparpläne.

### Wertentwicklung
Die **Wertentwicklung** stellt den **Wert** der Position dem **Investierten Kapital** gegenüber. Das investierte Kapital ist der Einstand der gehaltenen Stück einschliesslich Transaktionskosten und Steuern der Käufe und wird als Treppe gezeichnet. Die Fläche zwischen beiden Linien ist der unrealisierte Gewinn oder Verlust. Am rechten Rand stehen der letzte Wert und das letzte investierte Kapital als Marken. In der Legende lassen sich zusätzlich die Linien **Wert inkl. Erträge und realisiertes Ergebnis** und **Summe aller Käufe** einblenden.

Jeder Kauf hebt diese Kurve um den vollen Betrag. Bei gestaffelten Käufen steigt sie deshalb treppenartig, und die eigentliche Kursbewegung ist im Verhältnis klein. Für die Beurteilung des Ertrags ist der Diagrammtyp **Ergebnis** besser geeignet.

### Rendite
Die **Rendite** zeigt dasselbe wie das Ergebnis, jedoch in Prozent und ebenfalls als grüne beziehungsweise rote Fläche. Die Bezugsgrösse bestimmt, worauf sich die Prozentzahl bezieht:
- **Offene Position**: (Wert − investiertes Kapital) / investiertes Kapital. Die Prozentzahl zeigt, wie der am jeweiligen Tag noch gehaltene Bestand gegenüber seinem Einstand dasteht. Eine gepunktete Linie zeigt zum Vergleich die **Kursveränderung** seit Beginn der Phase.
- **Gesamt seit Einstieg**: (Wert − investiertes Kapital + Erträge + realisiertes Ergebnis) / Summe aller Käufe der Phase. Diese Zahl hängt nicht von der [Methode der Kostenbasis]({{% relref "/reportportfolio/costbasismethod" %}}) ab.

Am Tag eines vollständigen Verkaufs hat die offene Position keinen Einstand mehr. Dieser Tag erscheint deshalb nur mit der Bezugsgrösse **Gesamt seit Einstieg**.

### Rückgang (Drawdown)
Der **Rückgang** zeigt, wie weit das Geld in der Position unter seinem eigenen bisherigen Höchststand liegt. Ein Kauf ist neues Geld und hebt diesen Höchststand um den vollen Betrag. Ein Verkauf oder eine Ausschüttung nimmt einen Teil der Position heraus und senkt den Höchststand im gleichen Verhältnis, sodass ein Verkauf zum aktuellen Kurs den Rückgang nicht verändert. Ein vollständiger Verkauf endet mit seinem Ergebnis gegenüber dem Höchststand und nicht mit −100 %. Eine gepunktete Linie zeigt zum Vergleich den **Kursrückgang** vom höchsten Schlusskurs der Phase. Der Abstand zwischen beiden Linien ist die Wirkung der eigenen Käufe.

{{% callout note "Im Gewinn und trotzdem im Rückgang" %}}
Rendite und Rückgang messen Unterschiedliches. Die Rendite misst den Abstand zum **Einstand**, der Rückgang den Abstand zum bisherigen **Höchststand** der Position. Eine Position kann deshalb gleichzeitig im Gewinn und unter ihrem Höchststand sein. Bei einem Wertpapier, das seitwärts läuft, ist der Rückgang fast immer rot, weil nur ein neuer Höchststand ihn auf null zurückführt.
{{% /callout %}}

Unter dem Diagramm fasst GT den Rückgang zusammen. Der **Maximale Rückgang** steht zusammen mit dem **Höchststand vor dem Rückgang**, dem **Tiefpunkt des Rückgangs**, dem Tag, an dem der **Höchststand wieder erreicht** wurde, und der **Dauer (Tage)** dieser Episode. Die längste Zeit unter dem Höchststand ist oft eine andere Episode als der tiefste Rückgang, denn ein flacher Rückgang kann viel länger dauern. Ist das der Fall, erscheint zusätzlich die **Maximale Rückgangsdauer (Tage)** mit ihren eigenen Daten. Zuletzt folgt der **Aktuelle Rückgang** oder, bei einer abgeschlossenen Phase, der **Rückgang beim Verkauf**.

## Ereignisse und Tooltip
Käufe, Verkäufe, Überträge, Erträge und Finanzierungskosten erscheinen in allen Diagrammtypen als kleine Markierungen am unteren Rand, beschriftet mit einem Buchstaben: **K** für Kauf, **V** für Verkauf, **Ü** für Übertrag, **E** für Ertrag und **F** für Finanzierungskosten. Mehrere Ereignisse derselben Art an einem Tag teilen sich eine Markierung. Jede Art kann in der Legende ausgeblendet werden, was bei Sparplänen mit vielen Käufen die Kurve ruhiger macht. Der Tooltip einer Markierung nennt Stückzahl und Betrag.

Der Tooltip der Kurve zeigt für das gewählte Datum den **Wert**, das **Investierte Kapital**, das **Offene Ergebnis**, das **Realisierte Ergebnis**, die **Erträge**, das **Gesamtergebnis**, die Prozentzahl der gewählten Bezugsgrösse und den Rückgang. Damit lässt sich beispielsweise ein Sprung am Tag eines Teilverkaufs nachrechnen.

## Ausschnitt und Skalierung
Das Diagramm zeigt immer die ganze Haltephase. Mit dem Zeitschieber unterhalb des Diagramms oder durch Aufziehen mit der Maus lässt sich ein Ausschnitt vergrössern. Die Y-Achse passt sich dabei dem sichtbaren Ausschnitt an, sodass auch ein kurzer Zeitraum die ganze Höhe nutzt. Bei Ergebnis, Rendite und Rückgang bleibt die Nulllinie sichtbar. Ein Doppelklick in das Diagramm setzt den Ausschnitt zurück.

Der Ausschnitt verändert keine Zahlen. Prozentzahlen und die Zusammenfassung des Rückgangs beziehen sich immer auf die ganze Phase, weshalb die Achse der Rendite mit **% seit Phasenbeginn** beschriftet ist.

## Dividenden, Erträge und Kosten
Zu den **Erträgen** zählen Dividenden und Zinsen sowie bezahlte und erhaltene Stückzinsen von Anleihen. Ob sie brutto oder netto der Quellensteuer gezählt werden, richtet sich nach der Einstellung des Klienten, wie in den übrigen Auswertungen. Erträge und Ergebnis werden am **Zahltag** gebucht.

| Diagrammtyp | Offene Position | Gesamt seit Einstieg |
|---|---|---|
| Ergebnis | ohne Erträge | mit Erträgen und realisiertem Ergebnis |
| Rendite | ohne Erträge | mit Erträgen und realisiertem Ergebnis, bezogen auf alle Käufe der Phase |
| Wertentwicklung | Die Linie des Werts enthält keine Erträge, die einblendbare Linie **Wert inkl. Erträge und realisiertes Ergebnis** schon. | |

Beim Rückgang ist eine Dividende ein Abfluss aus der Position und senkt den Höchststand im gleichen Verhältnis. Ist für die Dividende ein **Ex-Datum** erfasst, wirkt dieser Abfluss am Ex-Tag, an dem auch der Kurs um die Dividende fällt; der Kursabschlag zählt dadurch nicht als Rückgang. Fehlt das Ex-Datum, wirkt der Abfluss erst am Zahltag, und zwischen Ex-Tag und Zahltag zeigt der Rückgang einen vorübergehenden Knick in der Höhe der Dividende. Wird die Quellensteuer abgezogen, fällt der Kurs um die Bruttodividende, als Abfluss zählt aber nur der Nettobetrag. Die einbehaltene Steuer bleibt deshalb als kleiner Rückgang stehen.

## CFD und Forex
Bei CFD und Forex ist das **investierte Kapital** der **Einstiegswert** der offenen Positionen, also der Wert der noch offenen Stück zum Eröffnungskurs ohne Kosten, und der **Wert** ist Einstiegswert plus offenes Ergebnis. Bei einer Long-Position entspricht der Wert damit Stückzahl mal Wert pro Punkt mal Kurs, bei einer Short-Position steigt er bei fallendem Kurs. Long- und Short-Positionen, die gleichzeitig im selben Depot offen sind, werden einzeln bewertet. Die Kosten von Eröffnung und Schliessung zählen zum realisierten Ergebnis, die Finanzierungskosten als negative Erträge.

Der Diagrammtyp **Wertentwicklung** wird für CFD und Forex nicht angeboten. Der Einstiegswert ist das ganze Exposure, und die Ausschläge des Ergebnisses wären daneben nicht ablesbar. Die Rendite bezieht sich auf dieses Exposure und nicht auf die beim Broker hinterlegte Margin, die GT nicht kennt. Finanzierungskosten fliessen in die Position, ohne ihren Wert zu erhöhen, und vertiefen deshalb den Rückgang.

## Hauptwährung
In der Hauptwährung wird der Wert zum Schlusskurs der Währung jedes Tages umgerechnet, das investierte Kapital dagegen zum Kurs des jeweiligen Kaufs. Ist das Geldkonto in der Hauptwährung, gilt der gebuchte Kurs, sonst der Schlusskurs des Buchungstages. Das realisierte Ergebnis eines Verkaufs ist der Erlös zum Kurs des Verkaufs abzüglich des Einstands zu den Kursen der Käufe. Die Kurven in der Hauptwährung enthalten deshalb den Währungserfolg und können auseinanderlaufen, auch wenn der Kurs des Wertpapiers stillsteht.

## Hinweise unter dem Diagramm
Fehlende Kurse oder Wechselkurse werden nie als null gezeichnet. Stattdessen erscheint unter dem Diagramm ein Hinweis:
- Wurde das Wertpapier im ausgewerteten Bereich nie über Nacht gehalten, ersetzt ein Hinweis das Diagramm.
- Fehlt an einem Handelstag der Börse des Wertpapiers der Kurs, wird dieser Tag ausgelassen. Ein Feiertag der Börse gilt dagegen als erwartete Pause und erzeugt keinen Hinweis.
- Fehlen Wechselkurse, bleiben die davon abhängigen Werte in der Hauptwährung leer, während die Werte in der Wertpapierwährung vollständig sind.
- Tage mit negativem Bestand haben keinen Wert und werden ausgelassen.
- Ein Übertrag zwischen Depots des ausgewerteten Bereichs wird wie oben beschrieben angezeigt.

## Einschränkungen
Der Positionschart zeigt eine Haltephase nach der anderen; mehrere Phasen in derselben Kurve gibt es nicht. Ein Vergleich mit einem Index, eine Monatsübersicht und ein interner Zinsfuss der Phase sind nicht enthalten. Prozentzahlen beziehen sich immer auf die ganze Phase, ein Zeitfenster mit eigener Basis wie seit Jahresbeginn wird nicht berechnet.
