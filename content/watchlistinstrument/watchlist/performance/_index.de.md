---
title: "Performanz Ansicht"
date: 2026-10-13T22:00:00+01:00
draft: false
weight: 10
archetype: "default"
---
Durch die Selektion einer Watchlist im **Navigationsbaum** wird diese Ansicht standardmässig im Hauptbereich angezeigt. Die **Watchlist-Ansicht** zeigt die Instrumente mit ihren offenen und gehandelten Positionen an. Zusätzlich ist es die einzige Möglichkeit einer **Innertag Kursaktualisierung** in GT.

## Innertag Kursaktualisierung
Die Selektion dieser Watchlist-Ansicht kann eine **Innertag Kursaktualisierung** bewirken, dabei wird der vorgegebene Zeitabstand seit der letzten Kursaktualisierung berücksichtigt. Dieser **Zeitabstand** wird auch bei jedem einzelnen Instrument für eine mögliche Kursaktualisierung herangezogen. Wann die letzte Aktualisierung der Innertag Kurse erfolgte, steht in der Titelzeile der Watchlist. Der Zeitabstand kann der Eigenschaft "gt.w.intra.update.timeout.seconds" der **Globale Einstellung** entnommen werden.

Erhält ein Instrument keine Innertag Kurse mehr, so wird es mit dem jüngsten historischen Schlusskurs bewertet. Die Spalten **Zeit/Datum** und **Kurs** zeigen mit einer Einfärbung an, wie belastbar der angezeigte Kurs ist, siehe [Aktualität der Kurse](../pricefreshness/).

## Funktionen
Diese **Performanz-Ansicht** hat nur die allgemeinen Funktionen auf dem Kontextmenü implementiert.

### Menü "Ansicht" der Menüleiste
**Zeitrahmen**: Die Einstellung über den Menüpunkt **Zeitrahmen** bestimmt die Periode für die Berechnung der Tabellenspalte "**Zeitrahmen %**". Falls die Zeitperiode gleich oder grösser ein Jahr ist, kommt die **geometrische Durchschnittsrendite** bei der Performanceberechnung zur Anwendung.

**Dividenden einbeziehen**: Ist dieser Menüpunkt markiert, enthalten die Spalten **Differenz Vortag**, **Seit Jahresbeginn**, **Zeitrahmen %** und **Jährliche Rendite** bei den Wertpapieren die Dividenden als [Gesamtrendite](../../../glossar/#gesamtrendite-total-return). Für jede Dividende, deren Ex-Tag zwischen dem Ausgangskurs und dem angezeigten Kurs liegt, wird der Ausgangskurs mit dem Faktor der Dividende verkleinert. Fällt der Ex-Tag auf den heutigen Handelstag, wird auch die Differenz zum Vortag entsprechend berichtigt. Die Einstellung wird wie der Zeitrahmen gespeichert. **Währungspaare**, **Indizes** und **abgeleitete Wertpapiere** werden nicht umgerechnet. Damit lässt sich ein Kursindex mit einem Performanceindex oder ein ausschüttender mit einem thesaurierenden ETF vergleichen, sofern beim Index der Performanceindex gewählt wird. Bei markiertem Menüpunkt erscheint zudem die Spalte **Gesamtrendite**. Sie zeigt das Warnzeichen ⚠ bei einem Wertpapier, das eine Ausschüttungshäufigkeit hat, aber im Zeitrahmen, mindestens jedoch im letzten Jahr, keine Dividende aufweist. Seine Werte entsprechen dann der reinen Kursentwicklung.

**Position in Kopfzeile anzeigen**: Ist dieser Menüpunkt markiert, zeigt die Titelzeile der Watchlist neben ihrem Namen in kleiner Schrift die offene Position der ausgewählten Tabellenzeile. In der Währung des Instruments sind dies **Bestand**, **Total Position**, **Kursgewinn** und **Kursgewinn %**, in der Hauptwährung **Total Position**, **Kursgewinn** und **Kursgewinn %**, wobei der Code der Hauptwährung der Bezeichnung angefügt ist. Der Kursgewinn ist der Gewinn oder Verlust, falls die offene Position zum aktuellen Kurs geschlossen würde, ohne Transaktionskosten. Die Titelzeile bleibt beim Blättern der Tabelle stehen, womit diese Werte auch bei einer langen Watchlist sichtbar bleiben. Ein Instrument ohne Bestand, beispielsweise ein Währungspaar, zeigt keine Werte. Bei einer Margin-Position ist die Total Position wie in der Tabelle ihr Nominalwert und der Kursgewinn der Gewinn oder Verlust ihrer offenen Positionen. Die Einstellung wird gespeichert.

## Werte in der Hauptwährung
Über den Menüpunkt **Spalten anzeigen** lassen sich zwei weitere Spalten einblenden, deren Überschrift den Code der Hauptwährung trägt, beispielsweise "Total Position CHF" und "Kursgewinn % CHF". Die **Total Position** in der Hauptwährung ist der Wert der offenen Position, zum aktuellen Devisenkurs umgerechnet. Der **Kursgewinn %** in der Hauptwährung enthält auch den [Währungsgewinn](../../../glossar/#währungsgewinn): Der Einstand wird zu den Devisenkursen umgerechnet, zu denen das Geld investiert wurde, der Wert hingegen zum aktuellen Kurs. Ein Wertpapier in der Hauptwährung zeigt deshalb in beiden Spalten des Kursgewinns denselben Prozentwert. Bei einer Margin-Position entspricht der Prozentwert demjenigen in der Währung des Instruments. Fehlt ein benötigter Devisenkurs, bleiben die Werte in der Hauptwährung leer.

## Expandierende Tabellenzeile
Ein vorhandenes **Expander-Symbol** beim Instrument ist ein Hinweis auf seine bestehenden **Transaktionen**. Es ist eine der vielen Orte in Grafioschtrader wo bestehende Transaktionen  bearbeitet werden können. Weitereführende Informationen unter  [Wertpapiertransaktionen](../../../transaction/security/).
