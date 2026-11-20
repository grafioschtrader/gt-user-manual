---
title: "Dividende/Split Feed Ansicht"
date: 2026-11-18T22:54:47+01:00
draft: false
weight: 30
archetype: "default"
---
Diese Watchlist-Ansicht enthält Informationen über **Dividenden** und **Splits**.

## Funktionen
Diese **Watchlist-Ansicht** ermöglicht eine **Mehrfachselektion** von **Instrumenten**. Dazu wurde die Funktion **Selektierte Instrumente entfernen** implementiert, damit wird das Entfernen mehrere Instrumente vereinfacht.

## Expandierende Tabellenzeile
Ein vorhandenes **Expander-Symbol** beim Instrument ist ein Hinweis auf bestehenden **Dividende** oder **Splits**. Weitere informationen unter [Historische Daten](../../externaldata/historyquote). Die aufgeklappte Zeile zeigt links die Dividenden und rechts die Splits. Hat das Instrument Dividenden, folgen unter den Splits die **Dividendenkennzahlen**.

## Dividendenkennzahlen
Die Box **Dividendenkennzahlen** fasst die Dividendenhistorie des Instruments in Kennzahlen zusammen. Grundlage sind die Dividenden des Instruments, wie sie links in der Tabelle stehen, und nicht die Ausschüttungen, die Sie in Ihren Portfolios erhalten haben. Alle Beträge sind um Splits bereinigt und lauten auf die Währung des Instruments, die im Titel der Box steht. Eine Dividende in einer anderen Währung wird mit dem Wechselkurs am Ex-Tag umgerechnet. Jahresangaben beziehen sich auf Kalenderjahre. Eine Kennzahl, für die Angaben fehlen, wird weggelassen.

| Kennzahl | Bedeutung |
|---|---|
| **Letzter Ex-Tag**, **Letzte Dividende** | Die jüngste Dividende bis heute. |
| **Nächster Ex-Tag**, **Nächste Dividende** | Eine bereits bekannte künftige Dividende. Diese Zeilen erscheinen nur, wenn die Datenquelle eine solche Dividende geliefert hat. |
| **Dividenden der letzten 12 Monate** | Summe der Dividenden mit einem Ex-Tag in den letzten 365 Tagen. |
| **Kurs** | Der letzte Kurs, auf den sich die Renditen beziehen. |
| **Dividendenrendite TTM %** | Dividenden der letzten 12 Monate geteilt durch den Kurs. Sie entspricht der Funktion `DIVYIELD()` der [Indikatoren](../indicator/#vergleichsinstrument-und-dividenden). |
| **Zahlungen pro Jahr** | Die Ausschüttungsfrequenz des Instruments, sofern sie regelmässig ist. Andernfalls zählt GT die Dividenden der letzten 365 Tage. |
| **Forward-Dividendenrendite %** | Letzte Dividende mal Zahlungen pro Jahr, geteilt durch den Kurs. Es ist eine Hochrechnung und keine angekündigte Dividende. |
| **Dividendenwachstum 1J**, **Dividendenwachstum CAGR 3J**, **5J**, **10J %** | Durchschnittliches jährliches Wachstum der Jahresdividende bis zum letzten abgeschlossenen Jahr, wie die Funktion `DIVGROWTH()`. Das laufende Jahr zählt nicht mit. Liegt das Startjahr vor der ersten Dividende, fehlt der Wert. |
| **Jahre ohne Kürzung** | Abgeschlossene Jahre in Folge mit einer positiven Jahresdividende, die nicht tiefer als im Vorjahr ist. Die Regel entspricht `DIVYEARS()`, hat aber keine Obergrenze von 20 Jahren. |
| **Letzte Kürzung im Jahr**, **Letzte Kürzung %** | Das jüngste abgeschlossene Jahr mit einer tieferen Jahresdividende als im Vorjahr und deren Veränderung. Der Wert -100 % bedeutet eine ausgesetzte Dividende. |
| **Chowder-Wert (Rendite + CAGR 5J)** | Dividendenrendite TTM plus Dividendenwachstum über fünf Jahre. Eine verbreitete Faustregel betrachtet einen Wert ab 12 als attraktiv, bei Titeln mit hoher Rendite wie Versorgern ab 8. |
| **Ausschüttungsquote %** | Dividenden der letzten 12 Monate geteilt durch den Gewinn je Aktie der letzten 12 Monate. Sie erscheint nur bei direkten Aktien, für die GT einen geprüften Gewinn je Aktie aus den [Fundamentaldaten](../indicator/#fundamentaldaten) hat. |
| **Erster Ex-Tag**, **Anzahl Dividenden** | Beginn und Umfang der erfassten Dividendenhistorie. |

{{% notice style="note" %}}
Die Dividendentabelle kennt keine Unterscheidung zwischen ordentlichen und Sonderdividenden. Eine Sonderdividende erhöht deshalb die Rendite der letzten 12 Monate und das Wachstum, und im Folgejahr erscheint sie als Kürzung. Fehlt für eine Dividende in einer anderen Währung der Wechselkurs, bleiben die Kennzahlen leer, die dieses Jahr benötigen.
{{% /notice %}}

## Dividendenchart
Ein Klick auf die Box **Dividendenkennzahlen** aktiviert sie. Im Menü **Ansicht** stehen dann die Funktionen **Dividendenchart** und **Zum Dividendenchart hinzufügen**. Die beiden Symbole oben rechts in der Box führen dieselben Funktionen aus. Der Chart erscheint im unteren Bereich der Anwendung und bleibt bestehen, wenn Sie die Zeile wieder zuklappen.

### Ein Instrument
**Dividendenchart** zeigt den Chart für das Instrument der Box. Die Balken zeigen die Jahresdividende: Ein Jahr mit einer Kürzung ist rot, das laufende und damit noch unvollständige Jahr hell. Die Linie zeigt die Dividendenrendite TTM jeweils am Monatsende auf der rechten Achse. Splits erscheinen als gestrichelte senkrechte Linien, die nur mit ihrem Faktor beschriftet sind, zum Beispiel 1:4. Fährt man mit der Maus darüber, erscheint auch das Datum. Ein Klick auf den Legendeneintrag **Wertpapier-Split** blendet die Splits aus.

### Mehrere Instrumente vergleichen
Solange der Dividendenchart angezeigt wird, fügt **Zum Dividendenchart hinzufügen** das Instrument einer weiteren Box hinzu, bis zu zehn Instrumente. Ist noch kein Dividendenchart offen, beginnt die Funktion einen neuen. **Dividendenchart** beginnt immer wieder mit dem einen Instrument.

Ab zwei Instrumenten zeigt der obere Teil des Charts die Dividendenrendite TTM und der untere die Jahresdividende als Index. Der Index ist im Basisjahr 100. Das Basisjahr ist das erste abgeschlossene Jahr, in dem alle verglichenen Instrumente ein ganzes Jahr lang Dividenden gezahlt haben; das Jahr der ersten Dividende gilt als möglicherweise unvollständig. Beide Grössen haben keine Einheit, deshalb lassen sich Instrumente in unterschiedlichen Währungen ohne Umrechnung vergleichen. Ein Vergleich der Beträge selbst wäre wenig aussagekräftig, weil diese vom Kursniveau und von der Anzahl Aktien abhängen. Jedes Instrument hat einen Legendeneintrag, der seine beiden Linien gemeinsam ausblendet.
