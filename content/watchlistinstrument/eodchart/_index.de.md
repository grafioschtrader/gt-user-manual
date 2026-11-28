---
title: "Tagesendkurse als Linengrafik"
date: 2026-11-18T22:00:00+01:00
draft: false
weight: 30
archetype: "default"
---
In GT können die **Tagesendkurse** eines **Instrumentes** im **Zusatzbereich** als **Diagramm** dargestellt werden. Neben der klassischen **Liniengrafik** stehen auch **Kerzendiagramme** (Candlestick) und **OHLC-Diagramme** zur Verfügung, sofern die entsprechenden Kursdaten vorhanden sind. Daraus lässt sich die Performance der Kurse ab einem wählbaren Datum eines Instrumentes ablesen. Wobei sich die angebotene Funktionalität bezüglich dem **Wertpapier** einschliesslich **abgeleitetes Wertpapier** und dem **Währungspaar** leicht unterscheiden.

{{% callout info %}}
Die Liniengrafik basiert auf den historischen Kursdaten plus dem letzten Innertag Kurs falls dieser vorliegt.
{{% /callout %}}

{{% callout info %}}
Der Abruf der Kursdaten für dieses Diagramm zählt zum täglichen Kontingent unterschiedlicher Instrumente, siehe [Tageskontingent für den Abruf historischer Kurse](../externaldata/historyquote/pricedata/).
{{% /callout %}}

{{% callout warning %}}
### Kurschart oder Performancechart
Wie im [Glossar](../../glossar/) erwähnt, muss zwischen ETF bzw. Fonds **ausschüttend** oder **thesaurierend** wie auch zwischen **Kurs-** und **Performanceindex** unterschieden werden. Ein ähnlicher Unterschied besteht zwischen **Aktien** mit oder ohne **Dividende**.

GT bietet für das Problem der unterschiedlichen Währungen von Instrumenten eine wählbare Währung an. Ohne weitere Einstellung werden im Diagramm die täglichen **Schlusskurse** verwendet, diese können je nach Instrument die Erträge mit berücksichtigen oder auch nicht. Daher sollte sich der Benutzer immer bewusst sein, ob in diesem Diagramm **ausschüttende** oder **thesaurierende** ETFs bzw. Kurs- oder Performanceindizes miteinander verglichen werden.

### GT und diese Problematik
Mit dem Kontrollkästchen **Dividenden einbeziehen** im Kopfteil zeigt GT die Wertpapiere als [Gesamtrendite](../../glossar/#gesamtrendite-total-return). Die Dividenden werden dabei rechnerisch am Ex-Tag wieder angelegt, womit ein ausschüttendes und ein thesaurierendes Instrument vergleichbar werden. Für einen Index wird dagegen nichts umgerechnet, hier wählen Sie den Performanceindex.
{{% /callout %}}

## Aktivierung dieser Ansicht
Die **Liniengrafik** im **Zusatzbereich** kann von Unterschiedlichen Ansichten des **Hauptbereiches** aktiviert werden. In der Praxis wird sie wahrscheinlich meistens durch die Funktionalität der Watchlist genutzt. Jedoch gibt es in GT auch andere Ansichten mit einem Instrument pro Tabellenzeile, wo die folgende Funktionalität über das Kontextmenü angeboten wird.
+ **Tagesenddaten als Liniengrafik**: Erstellt eine **neue Liniengrafik** mit dem selektierten **Instrument**.
+ **Hinzufügen als Liniengrafik**: Das selektierte **Instrument** wird der **bestehenden Liniengrafik** hinzugefügt. Damit kann die Performance unterschiedliche Instrumente verglichen werden.
+ **Positionschart**: Zeigt für ein **Wertpapier** die Entwicklung der **eigenen Position** mit Ergebnis, Wertentwicklung, Rendite und Rückgang, siehe [Positionschart](../positionchart/). Derselbe Eintrag steht im Kontextmenü der Liniengrafik, sofern sie genau ein Wertpapier zeigt.

## Diagrammtypen
GT unterstützt drei verschiedene Diagrammtypen für die Darstellung von Kursdaten. Die Auswahl des **Diagrammtyps** ist nur verfügbar, wenn ein **einzelnes Instrument** angezeigt wird und die erforderlichen OHLC-Daten (Eröffnungs-, Höchst-, Tiefst- und Schlusskurse) vorhanden sind.
- **Liniengrafik**: Der klassische Diagrammtyp, der die Schlusskurse als durchgehende Linie darstellt. Dieser Typ ist immer verfügbar und wird standardmässig verwendet.
- **Kerze** (Candlestick): Zeigt für jeden Handelstag einen Kerzenkörper, der die Spanne zwischen Eröffnungs- und Schlusskurs darstellt, sowie Dochte für Höchst- und Tiefstkurs. Steigende Kurse werden typischerweise grün, fallende rot dargestellt.
- **OHLC**: Ähnlich wie das Kerzendiagramm, jedoch werden die Kursdaten als horizontale Striche dargestellt. Der linke Strich markiert den Eröffnungskurs, der rechte den Schlusskurs.

Bei der Anzeige **mehrerer Instrumente** gleichzeitig wird automatisch die **Liniengrafik** verwendet, da Kerzen- und OHLC-Diagramme für den Vergleich verschiedener Instrumente nicht geeignet sind.

## Transaktionen in der Liniengrafik
Bei der Abbildung der Transaktionen wird unterschieden zwischen einem **Wertpapier** bzw. **abgeleiteten Wertpapier** und **Währungspaar**.
- **Wertpapier und abgeleiteten Wertpapier**: Die **Transaktionen** wie **Kaufen**, **Verkaufen** und **Zins/Dividende** werden als gleich grosse Punkte in der Liniengrafik dargestellt. Ist **Dividenden einbeziehen** markiert, wird der tatsächliche Kauf- bzw. Verkaufskurs gleich wie die Kurslinie umgerechnet, damit der Punkt auf der Linie bleibt.
- **Währungspaar**: Der Durchmesser des Punktes ist ein Abbild der Gesamtsumme der Transaktion. Ausserdem werden auch die Transaktion des **wechselseitigen Währungspaars** abgebildet. So enthält beispielsweise die Liniengrafik des Währungspaar EUR/USD auch die Transaktionen des Währungspaar USD/EUR. Die Grösse des Punkts wird vereinheitlicht sobald der Auswahlkasten **Prozent** markiert ist und/oder mehrere Instrumente bzw. Währungspaare angezeigt werden. Wertpapiertransaktionen mit dem involvierten Währungspaar wird entsprechend in **Kaufen** bzw. **Verkaufen** der betreffenden Währung transformiert.

## Bestand
Für ein **einzelnes Instrument** kann der **Bestand** über den gewählten Zeitraum betrachtet werden. Die Skalierung der **Y-Achse** des **Bestandes** wird auf der **rechten Seite** dargestellt. Auf der linken Seite befindet sich wie immer die Skalierung der Datenreihe der Kurse mit dem Preis- oder der Prozentangabe. Natürlich können auch negative Bestände dargestellt werden. Die Anzeige des Bestands kann im **Kopfteil** des Diagramms **ein- bzw. ausgeschaltet** werden.

## Funktionen
Allgemein verfügt ein [Diagramm über eine gewisse Grundfunktionalität](../../intro/userinterface/user_setting_ui_controls/) die hier nicht weiter beschrieben wird. Zudem besitzt dieses Diagramm über einen **interaktiven Kopfteil** sowie ein **Kontextmenü** für die rudimentäre technische Analyse und die Ausgabe als PDF.

### Interaktionen im Kopfteil
Dieses Diagramm bietet im **Kopfteil** zusätzliche Interaktionsmöglichkeiten:
- **Datum von**: Ab diesem Datum werden die Kursdaten angezeigt. Für eine schnelle Einstellung stehen oberhalb mehrere **Knöpfe für unterschiedliche Zeiträume** zur Verfügung. Die Knöpfe **5D**, **1M** und **3M** zeigen die Kurse der letzten fünf Tage, des letzten Monats bzw. der letzten drei Monate, während **1Y**, **3Y**, **5Y** und **10Y** den Zeitraum auf ein, drei, fünf bzw. zehn Jahre zurück festlegen. Fällt das so berechnete Datum auf ein Wochenende, wird automatisch der nächste Handelstag verwendet. Der Knopf **Seit Jahresbeginn** (YTD) setzt das Datum auf den Jahresanfang. Mit dem Knopf **SEH** (Tooltip «Seit dem ersten Handel») wird das Datum auf die **älteste eigene Transaktion** des Instrumentes gesetzt, sodass die gesamte Kursentwicklung seit dem ersten Kauf bzw. Verkauf sichtbar wird. Dieser Knopf ist nur dann aktiv, wenn für das Instrument überhaupt **Transaktionen** vorliegen, und bleibt bei einem reinen Beobachtungsinstrument ohne Transaktionen deaktiviert. Über das **Zurücksetzen-Symbol** (Pfeil) links neben den Zeitraum-Knöpfen wird das Datum auf den **ältesten verfügbaren Kurs** gesetzt und damit die gesamte vorhandene Kurshistorie dargestellt.
- **Diagrammtyp**: Ermöglicht die Auswahl zwischen Liniengrafik, Kerzendiagramm und OHLC-Diagramm. Diese Option erscheint nur bei der Anzeige eines einzelnen Instruments mit verfügbaren OHLC-Daten.
- **Prozent**: Sobald zwei oder mehr Instrumente als Liniengrafik angezeigt werden, ist die Anzeige der Grafik mit absoluten Werten nur in wenigen Fällen sinnvoll. Daher gibt es dieses Kontrollkästchen, es wird automatisch markiert, falls die Funktion "**Hinzufügen als Liniengrafik**" erstmalig ausgeführt wird. Wobei das System den vom Benutzer eingestellte Zustand dieses Auswahlkasten nicht übersteuert. Die prozentuale Anzeige der unterschiedlichen Instrumente ab einem bestimmten Datum ermöglicht einen besseren Vergleich der Kursänderungen.
- **Verbinde Lücken**: Ist dieses Kontrollkästchen markiert, werden die vorhandenen Kursdaten mit einer Linie verbunden. Bei nicht markierten Auswahlkasten werden die möglichen Lücken von fehlenden Kursdaten sichtbar. Fehlende Kursdaten werden anhand des Handelskalender des entsprechenden Instrumentes ermittelt.
- **Dividenden einbeziehen**: Dieses Kontrollkästchen erscheint, sobald die Grafik mindestens ein Wertpapier enthält, und wirkt auf alle Wertpapiere der Grafik. Ist es markiert, zeigt die Grafik die [Gesamtrendite](../../glossar/#gesamtrendite-total-return): Die Bruttodividende wird zum Schlusskurs des Ex-Tags wieder angelegt, und alle älteren Kurse werden entsprechend kleiner. Der jüngste Kurs bleibt unverändert, der Kursrückgang am Ex-Tag verschwindet. Eröffnungs-, Höchst- und Tiefstkurs werden gleich wie der Schlusskurs umgerechnet, das Volumen bleibt unverändert. Auch die **technischen Indikatoren** werden auf der umgerechneten Kurslinie berechnet. **Währungspaare**, **Indizes** und **abgeleitete Wertpapiere** werden nicht umgerechnet. Hat ein Wertpapier eine Ausschüttungshäufigkeit, aber in seinem Kursverlauf keine Dividende, erscheint in der Legende hinter seinem Namen das Warnzeichen ⚠, da seine Linie dann der reinen Kursentwicklung entspricht.
- **Währung**: GT unterstützt die Instrumente in unterschiedlichen Währungen. Bei der Vergleichbarkeit der Performance von Instrumenten muss die Währung daher mitberücksichtigt werden. Ausgehend von der **Währung des Klient** kann eine beliebige Währung der angezeigten Instrumente ausgewählt werden. Die Wahl der Währung hat keine Auswirkung auf die Liniengrafik der **Währungspaare**.

{{% callout note "Zeitraum-Knöpfe ohne ausreichende Kursdaten" %}}
Mit Ausnahme des Knopfes **SEH** sind die Knöpfe für die Zeiträume bewusst **nicht deaktiviert**. Reicht der gewählte Zeitraum weiter zurück als die vorhandenen Kursdaten, wird die Grafik lediglich ab dem **ältesten verfügbaren Kurs** dargestellt. Wird bereits der gesamte verfügbare Zeitraum angezeigt, bleibt die Grafik unverändert, sodass das Betätigen eines solchen Knopfes **wirkungslos erscheinen** kann, obwohl bereits der grösstmögliche Zeitraum dargestellt wird.
{{% /callout %}}

{{% callout note "Währungen und nicht vorhandene Währungspaare" %}}
Es wird oder werden Währungspaare benötigt, damit die Funktion von **Währung** funktioniert. Falls ein verlangtes Währungspaar dem System nicht vorliegt, wird dieses vom System erstellt und die **historischen Kursdaten** direkt **heruntergeladen**. Dies kann die Erstellung der Liniengrafik um einige Sekunden verzögern.
{{% /callout %}}


### Kontextmenü für technische Indikatoren
GT unterstützt verschiedene technische Indikatoren, die über das Kontextmenü aktiviert und deaktiviert werden können. Die Parameter der Indikatoren sind in einem Dialog einstellbar.
#### Gleitende Durchschnitte (SMA/EMA)
Der **einfache gleitende Durchschnitt** (SMA) und der **exponentiell gleitende Durchschnitt** (EMA) werden direkt in der Kursgrafik als Overlay angezeigt. Standardmässig können drei Perioden konfiguriert werden.
#### Relative Strength Index (RSI)
Der **RSI** ist ein Momentum-Oszillator, der die Geschwindigkeit und das Ausmass von Kursänderungen misst. Er oszilliert zwischen 0 und 100, wobei Werte über 70 typischerweise auf überkaufte Bedingungen und Werte unter 30 auf überverkaufte Bedingungen hinweisen. Der RSI wird in einem separaten Bereich unterhalb der Kursgrafik dargestellt und ist nur bei der Anzeige eines einzelnen Instruments verfügbar. Standardmässig können zwei Perioden konfiguriert werden.
{{% callout info %}}
GT wird kein Werkzeug für die **technische Analyse** sein, trotzdem werden zukünftig noch einige **technische Indikatoren** implementiert werden.
{{% /callout %}}
### Zeichenwerkzeuge
Die Liniengrafik bietet interaktive Zeichenwerkzeuge für die manuelle Chartanalyse. Über die Werkzeugleiste oberhalb des Diagramms können verschiedene Formen gezeichnet werden: Linien, offene und geschlossene Pfade, Kreise und Rechtecke. Ein Radiergummi-Werkzeug ermöglicht das Entfernen einzelner gezeichneter Elemente. Zusätzlich stehen in der Werkzeugleiste drei weitere Schaltflächen zur Verfügung:
- **Rückgängig**: Macht die letzte Zeichenänderung rückgängig. Mehrfaches Klicken ermöglicht das schrittweise Zurücknehmen mehrerer Änderungen.
- **Wiederherstellen**: Stellt eine zuvor rückgängig gemachte Zeichenänderung wieder her.
- **Alle Zeichnungen löschen**: Entfernt sämtliche gezeichneten Formen auf einmal. Vor dem Löschen wird ein Bestätigungsdialog angezeigt. Diese Aktion kann ebenfalls über die Rückgängig-Funktion wiederhergestellt werden.

Die gezeichneten Formen werden pro Benutzer und Instrument auf dem Server gespeichert. Dadurch bleiben die Zeichnungen auch nach dem Schliessen des Browsers erhalten und sind auf allen Geräten verfügbar, auf denen sich der Benutzer anmeldet.

### Diagramm als PDF ausgeben
Über den Eintrag **Diagramm als PDF...** im Kontextmenü, durch eine Trennlinie von den technischen Indikatoren abgesetzt, entsteht aus dem angezeigten Diagramm ein PDF-Dokument zum Ausdrucken, Ablegen oder Weitergeben. Das Diagramm wird so ausgegeben, wie es auf dem Bildschirm zu sehen ist: mit dem gewählten Zeitraum und Ausschnitt, dem **Bestand**, den eingeschalteten Indikatoren und den eigenen Zeichnungen. Das Diagramm kann deshalb zuerst angepasst werden, beispielsweise mit oder ohne **Bestand**, und erst danach als PDF ausgegeben werden. Auf dem Bildschirm ändert sich dabei nichts. Der Eintrag steht nur für ein **einzelnes Wertpapier** zur Verfügung. Bei einem Vergleich mehrerer Instrumente und bei einem Währungspaar bleibt er deaktiviert.

Im Dialog werden das **Papierformat** (A4 oder Letter), die **Berichtssprache** und das **Zahlen- und Datumsformat** gewählt. Das Kontrollkästchen **Tabelle der nummerierten Transaktionen** bestimmt, ob dem Diagramm eine Tabelle folgt. Ohne dieses Kontrollkästchen enthält das Dokument nur das Diagramm, und die Punkte der Transaktionen tragen keine Nummern. Mit **PDF erstellen** wird das Dokument auf dem Server erzeugt und vom Browser gespeichert.

Das Dokument nennt oben den Namen des Wertpapiers, den Bereich, aus dem die Transaktionen stammen (alle Portfolios, ein Portfolio oder ein Depot), und die Währung der Kurse. Darunter stehen **ISIN** und **Ticker**, sofern das Wertpapier sie hat, und das Diagramm. Es wird als Vektorgrafik eingebettet: Linien bleiben bei jeder Vergrösserung scharf, und die Beschriftungen lassen sich im PDF markieren und durchsuchen. Für den Druck hat das Diagramm einen weissen Hintergrund, der Zeitschieber unterhalb der Kurslinie fehlt, und die Punkte der Transaktionen sind nicht durchscheinend.

#### Nummerierte Transaktionen
Jeder Punkt einer Transaktion im angezeigten Ausschnitt trägt im PDF eine Nummer. Die Nummern folgen der zeitlichen Reihenfolge der Buchungen. Dieselbe Nummer steht in der ersten Spalte der Tabelle **Transaktionen im Diagramm**, sodass sich jede Zeile dem Punkt im Diagramm zuordnen lässt. Transaktionen ausserhalb des angezeigten Zeitraums erhalten weder eine Nummer noch eine Zeile. Die Tabelle zeigt die Transaktionen des Bereichs, für den das Diagramm geöffnet wurde: bei einem Aufruf aus einem Depot dessen Transaktionen, bei einem Aufruf aus einem Portfolio diejenigen des Portfolios und sonst alle des Klienten. Es sind dieselben Transaktionen, die als Punkte im Diagramm erscheinen. Reichen die Zeilen nicht auf eine Seite, wird die Tabelle auf der nächsten Seite mit wiederholter Kopfzeile fortgesetzt.

Die Tabelle hat dieselben Spalten wie die Transaktionstabelle des Instrumentes: **Nr.**, **Buchungsdatum**, **Transaktionstyp**, **Anzahl**, **Preis**, **Betrag in Kontowährung**, **Gewinn** und **Gewinn %**.
- **Anzahl** und **Preis** sind die gebuchten Werte. Die Punkte im Diagramm liegen dagegen auf den um Aktiensplits bereinigten Kursen. Unterscheiden sich beide, weist eine Fussnote darauf hin.
- **Gewinn** und **Gewinn %** erscheinen bei Zeilen, für die GT einen Gewinn berechnet, vor allem bei **Verkaufen**. Der Gewinn ist in der Währung des Wertpapiers angegeben. Bei einem Kauf bleiben beide Spalten leer. Wie der Gewinn eines Verkaufs berechnet wird, hängt von der [Methode der Kostenbasis]({{% relref "/reportportfolio/costbasismethod" %}}) ab. Decken die Käufe des Depots einen Verkauf nicht vollständig, ist der Gewinn mit einem Stern markiert, und eine Fussnote erklärt, dass für die fehlenden Stück kein Gewinn ausgewiesen wird.

{{% callout note "Dicht beieinanderliegende Transaktionen" %}}
Liegen viele Transaktionen zeitlich dicht beieinander, können sich ihre Nummern im Diagramm teilweise überdecken. Ein kürzerer Zeitraum im Diagramm schafft Abhilfe, weil dann weniger Punkte und damit weniger Nummern erscheinen.
{{% /callout %}}

#### Verkauf hypothetisch
Hält der Klient im gewählten Bereich noch Stück des Wertpapiers, schliesst die Tabelle mit der Zeile **Verkauf hypothetisch** ab, durch eine Linie abgesetzt und in Fettschrift. Sie bewertet die noch gehaltenen Stück zum letzten Kurs des Wertpapiers und zeigt den noch nicht realisierten **Gewinn** und **Gewinn %**, als würde die Position heute verkauft. Die Zeile trägt keine Nummer, weil es dazu keinen Punkt im Diagramm gibt. Sie erscheint auch dann, wenn im angezeigten Zeitraum keine Transaktion zu sehen ist. Ist beim Klienten **Veräusserungskosten schätzen** gesetzt, sind die [Veräusserungskosten]({{% ref "/reportportfolio#veräusserungskosten" %}}) im Betrag und im Gewinn dieser Zeile berücksichtigt. Fehlt dafür ein Gebührenmodell, ein Steuermodell oder eine passende Regel, steht unter der Tabelle ein entsprechender Hinweis.

## Tagesendkurse als Linengrafik in der Praxis
{{< youtube qNaBNDIAnZ0 >}}
