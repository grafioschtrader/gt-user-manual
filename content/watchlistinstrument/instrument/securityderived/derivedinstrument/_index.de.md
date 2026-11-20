---
title: "Abgeleitetes Instrument"
date: 2026-08-13T22:54:47+01:00
draft: false
weight: 8
archetype: "default"
---
Ein abgeleitetes Instrument ermöglicht es, die Kursdaten eines oder mehrerer Wertpapiere durch eine **Formel** von mindestens einem anderen Instrument berechnen zu lassen. Ein abgeleitetes Instrument ist also immer von mindestens einem anderen Instrument abhängig. Ein abgeleitetes Instrument wird einem **Handelsplatz** und einer **Anlageklasse** zugeordnet und ist somit in GT handelbar.
## Kursberechnung
Ein abgeleitetes Instrument hat keine eigene Datenquelle, seine Kurse stammen aus den verknüpften Instrumenten. Die historischen Kurse werden dabei nicht bei jeder Abfrage neu gerechnet, sondern als gewöhnliche, berechnete Kurszeilen gespeichert. Grafiken, Auswertungen und Statistiken funktionieren deshalb wie bei jedem anderen Instrument. Von Hand bearbeiten oder importieren lassen sich diese Kurse hingegen nicht, die entsprechenden Menüpunkte fehlen bei einem abgeleiteten Instrument.
Ein Tag wird nur berechnet, wenn **jedes** verknüpfte Instrument für diesen Tag einen Kurs aufweist. Fehlt bei einem davon der Kurs, so fehlt er auch beim abgeleiteten Instrument; sobald er nachgeliefert wird, ergänzt GT den fehlenden Tag selbstständig. Wird umgekehrt ein Kurs eines zugrunde liegenden Instruments gelöscht, entfällt auch der abgeleitete Kurs dieses Tages. Da die abgeleiteten Kurse innerhalb eines Durchgangs immer nach den zugrunde liegenden berechnet werden, erhält ein neu erstelltes abgeleitetes Instrument seinen ersten Innertag-Kurs unter Umständen erst im nächsten Durchgang.
## Der Bearbeitungsdialog
Der Dialog wird in einer Watchlist über **Hinzufügen neues abgeleitetes Instrument** geöffnet. Die drei folgenden Eingabefelder unterscheiden ihn von einem gewöhnlichen Wertpapier, alle übrigen Felder sind unter [Wertpapier und abgeleitetes Instrument](../) beschrieben.
- **Basis Instrument (o)**: Dieses Feld ist zwingend. Das Instrument wird über die Schaltfläche neben dem Feld gewählt, worauf sich der [Suchdialog für Instrumente](../../searchdialog/) unter dem Titel **Instrument zuweisen** öffnet; die Wahl wird mit **Selektierte zuweisen** übernommen. Wählen Sie ein **Wertpapier**, so übernimmt GT dessen Name, Währung, Anlageklasse, Handelsplatz sowie die beiden Datumsfelder in das Formular, was sich anschliessend anpassen lässt. Bei einem **Währungspaar** wird lediglich die Währung gesetzt.
- **Preisberechungsformel**: Dieses Feld ist freiwillig und nimmt höchstens 255 Zeichen auf. Ohne Formel übernimmt das abgeleitete Instrument den Kurs des Basis Instruments unverändert.
- **Zusätzliches Instrumente**: Für jede Variable **p**, **q**, **r** oder **s**, die in der Formel vorkommt, erscheint ein solches Feld mit dem entsprechenden Buchstaben in Klammern. Wird die Variable aus der Formel entfernt, verschwindet das Feld wieder.
Felder für Datenquellen gibt es keine, ebenso wenig **ISIN** und **Ticker/Symbol**; ein abgeleitetes Instrument wird über seinen Namen angesprochen.
## Was als Baustein verwendet werden kann
Als Basis- und als zusätzliches Instrument können sowohl **Wertpapiere** als auch **Währungspaare** dienen. Ein weiteres abgeleitetes Instrument ist nicht zulässig, weshalb die Suche in diesem Dialog keine abgeleiteten Instrumente anbietet. Insgesamt sind höchstens fünf Instrumente beteiligt, das Basis Instrument und vier zusätzliche.
Zwei zusätzliche Variablen dürfen nicht auf dasselbe Instrument zeigen. Der Suchdialog hindert Sie nicht daran, dasselbe Instrument erneut zu wählen, siehe [Bereits enthaltene Instrumente](../../searchdialog/#bereits-enthaltene-instrumente); der Versuch scheitert erst beim Speichern.
Umgekehrt ist ein abgeleitetes Instrument überall sonst ein ganz gewöhnliches Instrument: es kann in Watchlisten und Korrelationssets aufgenommen und es kann gehandelt werden.
## Formel
Eine Formel besteht aus Zahlen, Variablen, mathematischen und boolesche Operatoren und möglichen Funktionen. GT verwendet das Framework [EvalEx - Java Expression Evaluator](https://github.com/uklimaschewski/EvalEx), daher siehe die Webseite für die möglichen Funktionen.
### Variable die Zuordnung zum einem Instrument 
Maximal können **5** Variablen mit den Buchstaben **o, p, q, r, s** in einer Formel benutzt werden. Die Variablen müssen einem Wertpapier zugeordnet werden. Diese Variablen sind die Platzhalter für den entsprechenden **historischen** oder  **Innertag-Kurs**.
Eine Variable muss dabei für sich allein stehen: `o * 2` und `(o + p) / 2` werden erkannt, `op` hingegen nicht.
### Prüfung beim Speichern
Wird eine Formel erfasst, so muss sie die Variable **o** enthalten sowie die Variable jedes zugewiesenen zusätzlichen Instruments. Beim Speichern liest GT die Formel und wertet sie einmal mit dem Wert 1 in allen Variablen aus. Eine fehlerhafte Formel wird deshalb sofort abgewiesen, beispielsweise mit der Meldung "Formel muss die Variable "o" enthalten!" oder "Variable "x" ist unbekannt!".
Dezimalzahlen dürfen Sie mit dem Dezimaltrennzeichen Ihrer Spracheinstellung erfassen; GT wandelt sie beim Speichern um und zeigt sie Ihnen später wieder in Ihrer eigenen Schreibweise an.
## Ein Währungspaar handelbar machen
Ein [Währungspaar](../../currencypair/) kann in GT nicht direkt gehandelt werden. Ein abgeleitetes Instrument, dessen Basis Instrument ein Währungspaar ist und das keine Formel aufweist, ergibt ein handelbares Instrument dafür. Die Auswahl der Anlageklasse beschränkt sich in diesem Fall auf den Eintrag für Währungspaare.
## Was ein abgeleitetes Instrument nicht kann
Auf einem abgeleiteten Instrument lassen sich weder **Splits** noch **Dividenden** erfassen, und es führt kein Handelsvolumen. Ein Split des zugrunde liegenden Instruments wirkt sich ohnehin über dessen Kurse aus und muss nicht nochmals erfasst werden.
Solange ein abgeleitetes Instrument auf ein Instrument verweist, kann dieses nicht gelöscht werden.
## Nach dem Speichern nicht mehr änderbar
Das **Basis Instrument** und die **Währung** stehen nach dem erstmaligen Speichern fest. Die **Formel** und die zusätzlichen Instrumente lassen sich weiterhin ändern, wobei GT nach jeder solchen Änderung die gesamte gespeicherte Kursgeschichte neu berechnet, siehe [Konnektorwechsel und erneutes Einlesen der Kursdaten](../#konnektorwechsel-und-erneutes-einlesen-der-kursdaten).
Diese Felder darf nur der Ersteller des Instruments oder ein Benutzer mit weitergehenden Rechten ändern; allen anderen werden sie angezeigt, aber gesperrt.
## Abgeleitetes Instrument in der Praxis
Ein Praxisbeispielt mit einem abgeleiteten Instrument. Von einer Feinunze Gold in USD zu 100 Gramm Gold in CHF. Dabei wird ein Wertpapier, Währungspaar und eine Formel für die Berechnung der historischen- und Innertag-Kurse genutzt.
Als **Basis Instrument (o)** dient das Goldinstrument, das in USD je Feinunze notiert, als **Zusätzliches Instrumente (p)** das Währungspaar USD/CHF. Die Formel lautet:
```
o * 3.2150746569 * p
```
Der Faktor 3.2150746569 ist 100 geteilt durch 31.1034768 und damit die Anzahl 100-Gramm-Einheiten in einer Feinunze; die Multiplikation mit **p** rechnet das Ergebnis von USD in CHF um.
{{< youtube iGJWAh55VkY >}}
Zusätzliche Informationen gibt es im Video von [Kursdaten](../../../).
