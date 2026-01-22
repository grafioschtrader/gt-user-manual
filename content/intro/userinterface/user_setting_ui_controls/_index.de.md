---
title: "Bedienelemte, Webspeicher und Eigenschaften"
date: 2025-11-19T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
## Bedienelemte
In GT setzt aus unterschiedlichen Bedienelementen zusammen. Gewisse **Bedienelemente** sind umfangreicher als andere und erlauben Einstellungen bezüglich dem Interaktionselement selbst.

### Tabelle
Die Steuerelemente **Tabelle**, **Baum** und **Hierarchietabelle** sind die Basis-Interaktionselemente von GT.

#### "Webspeicher" und Tabelle
Die Tabelle ist in GT eine oftmals benutztes **Interaktionselement** für die Präsentation von Daten. Dabei speichert der Webbrowser bei einigen Ansichten gewisse Einstellungen bezüglich der angezeigten Spalten in den **lokalen Speicher**. Dieser "Webspeicher" oder auch [Web Storage](//de.wikipedia.org/wiki/Web_Storage) genannt, wird auch beim Schliessen des Webbrowser nicht gelöscht und steht mit der nächsten Sitzung für die Konfiguration der Tabelle wieder bereit.

#### Expandierende Tabellenzeile
Bei gewissen Tabellen ist in der ersten Spalte ein **Expander-Symbol** ersichtlich. Durch ein Klicken expandiert die Tabellenzeile in vertikaler Richtung, dadurch werden unterschiedliche Detailangaben zu dieser Tabellenzeile sichtbar. Ein erneutes Klicken auf das **Expander-Symbol** reduziert die Tabellenzeile zu ihrem Ursprung.

#### Tooltip in Kopfzelle
Ein Tooltip bzw. Quickinfo ist auf einigen **Kopfzellen** des **Tabellenkopfes** implementiert. Dieses erscheint wenn der Mauszeiger eine kurze Zeit über der entsprechenden Kopfzelle verweilt.

### Sortierung der Spalten
In den meisten Tabellen können Sie die Daten nach einer oder mehreren Spalten sortieren. Durch Klicken auf die **Spaltenüberschrift** wird die Tabelle nach dieser Spalte sortiert. Ein erneutes Klicken kehrt die Sortierreihenfolge um. Viele Tabellen unterstützen eine **Mehrspaltensortierung**, dabei halten Sie die **Strg-Taste** gedrückt und klicken auf weitere Spaltenüberschriften. Die Sortierung berücksichtigt automatisch den jeweiligen Datentyp, beispielsweise werden Zahlen numerisch und Texte alphabetisch sortiert. Bei Spalten mit übersetzten Werten erfolgt die Sortierung nach den angezeigten Texten, nicht nach den internen Werten.

### Filterfunktion
Tabellen können mit einer **Filterzeile** ausgestattet sein, die sich direkt unter der Kopfzeile befindet. Diese ermöglicht es Ihnen, die angezeigten Daten nach verschiedenen Kriterien einzuschränken. Je nach Datentyp der Spalte stehen unterschiedliche Filtermöglichkeiten zur Verfügung. Bei **Textspalten** können Sie entweder einen Suchbegriff eingeben oder aus einer Liste von vorhandenen Werten auswählen. Für **Zahlen** steht ein Menü mit verschiedenen Vergleichsoperatoren zur Verfügung. Bei **Datumsspalten** öffnet sich ein Kalender, in dem Sie ein Datum auswählen können. Zusätzlich können Sie bei Datumsspalten festlegen, ob Sie nach einem exakten Datum suchen oder Daten vor oder nach einem bestimmten Datum anzeigen möchten. Die verfügbaren Optionen sind **Kein Filter**, **Gleich**, **Früher gleich als** und **Später gleich als**. Die Filter können in jeder Spalte separat gesetzt werden und wirken kombiniert, d.h. es werden nur Zeilen angezeigt, die alle gesetzten Filterbedingungen erfüllen.

### Tabellenspalten ein- und ausblenden
Bei vielen Tabellen können Sie die Sichtbarkeit einzelner Spalten individuell steuern. Diese Funktion erreichen Sie über das Menü **Ansicht** in der Menüleiste, sobald Sie die entsprechende Tabelle aktiviert haben. Im Untermenü finden Sie den Eintrag **Spalten anzeigen**, der eine Liste aller verfügbaren Spalten zeigt. Sichtbare Spalten sind mit einem ausgefüllten Quadrat markiert, ausgeblendete Spalten mit einem leeren Quadrat. Durch Klicken auf einen Spaltennamen wird die Sichtbarkeit dieser Spalte umgeschaltet. Ihre Einstellungen werden im lokalen Speicher des Webbrowsers gespeichert und bleiben auch nach dem Schliessen des Browsers erhalten. Wenn Sie GT das nächste Mal öffnen, werden Ihre bevorzugten Spalteneinstellungen automatisch wiederhergestellt.

## Eigenschaften
Einige Informationsklassen haben die allgemeine Eigenschaft **Bemerkung**, sie dient der Aufnahme einer Notiz. Im weiteren wird diese Eigenschaft in dieser Dokumentation nicht weiter erwähnt.

### Diagramm Grundfunktionalität
GT bietet verschiedene Möglichkeiten zur grafischen Darstellung von Daten. Diagramme werden im **Zusatzbereich** unterhalb des Hauptbereichs angezeigt. Die Anzeige eines Diagramms erfolgt meist über das **Kontextmenü** oder das Menü **Ansicht**. Häufig verwendete Diagrammtypen sind Liniendiagramme für Kursverläufe und Balkendiagramme für Vergleiche. Die **Legende** zeigt die verschiedenen Datenreihen und ermöglicht es Ihnen durch Klicken einzelne Reihen ein- oder auszublenden. Wenn Sie mit der Maus über das Diagramm fahren, werden Ihnen detaillierte Informationen zu den jeweiligen Datenpunkten angezeigt. Bei einigen Diagrammen können Sie durch Klicken auf einen Datenpunkt weitere Detailinformationen aufrufen. Der Inhalt des Zusatzbereichs bleibt erhalten, wenn Sie zu anderen Ansichten wechseln, sodass Sie Diagramme und Daten parallel betrachten können.
