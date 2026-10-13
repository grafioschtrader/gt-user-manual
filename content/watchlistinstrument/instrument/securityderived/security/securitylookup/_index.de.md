---
title: "Wertpapier bei Datenanbietern suchen"
date: 2026-10-11T12:00:00+02:00
draft: false
weight: 5
archetype: "default"
---
Ein neues Wertpapier verlangt bis zu vier Konnektoren mit ihren URL-Erweiterungen sowie Angaben wie Name, Ticker, Fälligkeit, Ausschüttungshäufigkeit und Stückelung. Die Suche bei den Datenanbietern ermittelt diese Angaben aus **ISIN**, **Währung**, **Handelsplatz** und **Anlageklasse**. Sie prüft jeden vorgeschlagenen Konnektor mit einer Abfrage echter Kursdaten, bevor Sie ihn übernehmen. Für die frei zugänglichen Datenanbieter braucht es weder GTNet noch ein kostenpflichtiges Abonnement.

Die Schaltfläche **Suchen** steht im Bearbeitungsdialog des Wertpapiers hinter dem Feld **Währung**. Sie wird aktiv, sobald **ISIN**, **Währung**, **Handelsplatz** und **Anlageklasse** ausgefüllt sind. Die Anlageklasse wird vorausgesetzt, weil sie bestimmt, welche Datenanbieter und Konnektoren in Frage kommen: Eine Anleihe wird bei anderen Quellen gesucht als eine Aktie, und eine Anleihe erhält weder einen Dividenden- noch einen Split-Konnektor. Ein Ticker, den Sie bereits erfasst haben, dient als zusätzlicher Suchbegriff. Die Suche speichert nichts; die gewählten Werte gelangen in den Bearbeitungsdialog, wo Sie sie prüfen und wie gewohnt speichern.

```mermaid
graph TD
    A[ISIN, Währung, Handelsplatz und Anlageklasse erfassen] --> B[Suchen]
    B --> C[Datenanbieter, die Handelsplatz und Anlageklasse abdecken, werden befragt]
    C --> D[Gleiche Listings verschiedener Anbieter werden zu einer Zeile zusammengeführt]
    D --> E[Je Konnektor wird die URL-Erweiterung abgeleitet und eine Probeabfrage ausgeführt]
    E --> F[Listing und je Feed einen Konnektor wählen]
    F --> G[Übernehmen füllt den Bearbeitungsdialog]
    G --> H[Prüfen und Speichern]
```

## Befragte Datenanbieter
Gesucht wird bei SIX, Deutsche Börse (Xetra und Frankfurt), Wiener Börse, Euronext, Yahoo, onvista, Boursorama, Consorsbank und OpenFIGI. Befragt wird nur ein Anbieter, der den gewählten Handelsplatz und die Art des Instruments abdeckt; die Börsen selbst liefern dabei die ausführlichsten Stammdaten, beispielsweise bei Anleihen die Fälligkeit, die Stückelung und die Zinstermine. EOD Historical Data wird zusätzlich befragt, wenn für diesen Konnektor ein [Verbindungs-API-Key]({{% relref "/admindata/connectorapikey" %}}) hinterlegt ist. Antwortet ein Anbieter nicht oder mit einem Fehler, erscheint über den Tabellen ein Hinweis mit seinem Namen; die Ergebnisse der übrigen Anbieter bleiben erhalten.

Indizes lassen sich bei den meisten Anbietern nicht über die ISIN finden, und ihr Symbol folgt keiner festen Regel (Yahoo führt den S&P 500 als `^GSPC`). Bei einem Index ermittelt GT deshalb zuerst das Indexkürzel, etwa bei Nasdaq Nordic oder in der Indexliste von EOD Historical Data, und fragt danach Yahoo und EOD Historical Data mit diesem Kürzel. Findet keine Quelle das Kürzel, tragen Sie es vor der Suche im Feld **Ticker/Symbol** ein, beispielsweise `OMXH25` für den OMX Helsinki 25.

## Tabelle "Basisdaten"
Jede Zeile ist ein Listing, also das Wertpapier an dem gewählten Handelsplatz in der gewählten Währung. Finden mehrere Anbieter dasselbe Listing, werden ihre Angaben zu einer Zeile zusammengeführt; die Spalte **Anbieter** nennt alle beteiligten Quellen. Die Angaben der Börse selbst haben Vorrang, die übrigen Anbieter ergänzen nur, was noch fehlt. Die Spalten **Anlageklasse** und **Finanzinstrument** zeigen Ihre Wahl. Meldet ein Anbieter einen anderen Typ, beispielsweise eine Anleihe statt einer Aktie, ist **Typ widerspricht** angekreuzt; prüfen Sie dann die Anlageklasse, bevor Sie übernehmen.

Klappen Sie eine Zeile auf, sehen Sie die weiteren Stammdaten wie **Emittentenland**, **Handel ab**, **Aktiv bis Datum**, **Ausschüttungshäufigkeit**, **Stückelung**, **Produkt Hyperlink** und **Hyperlink Handelsplatz**, jeweils mit dem Anbieter, von dem der Wert stammt. Unter **Weitere Daten des Anbieters** stehen Angaben, für die GT kein eigenes Feld hat, etwa der Kupon einer Anleihe.
- Die **Ausschüttungshäufigkeit** stammt vom Anbieter oder wird aus den bisherigen Ausschüttungen abgeleitet. Für einen ausschüttenden Fonds oder ein Listing, das weniger als ein Jahr gehandelt wird, schlägt GT nie **keine** vor.
- Der **Hyperlink Handelsplatz** stammt von der Börse selbst oder wird für viele weitere Börsen aus dem Ticker oder der ISIN gebildet, beispielsweise für NASDAQ, NYSE, London, Tokio, Hongkong, Südkorea, Toronto, Sydney, Mailand, Madrid, Stuttgart und München. Für die Bolsa Institucional de Valores (BIVA) in Mexiko fragt GT die Börse nach der Seite des Emittenten, für Stockholm und Kopenhagen die Suche von Nasdaq Nordic nach der Seite des Wertpapiers.
- Der **Produkt Hyperlink** wird nur aus Seiten von Emittenten gebildet, derzeit für Fonds und ETF von UBS. Seiten von Vergleichsportalen werden nicht vorgeschlagen.

## Tabellen der Konnektoren
Haben Sie ein Listing gewählt, erscheinen darunter die Tabellen **Historischer Konnektor**, **Intraday Konnektor**, **Dividenden Konnektor** und **Split Konnektor**. Angeboten wird jeder aktivierte Konnektor, der den Handelsplatz und die Anlageklasse unterstützt und dessen URL-Erweiterung sich aus den gefundenen Daten ableiten lässt oder der keine braucht. Konnektoren, die einen API-Key verlangen, erscheinen nur, wenn dieser hinterlegt ist.

Für jeden Konnektor führt GT eine **Probeabfrage** durch, wie sie auch beim Speichern des Wertpapiers erfolgt, und zeigt einen **Beispielwert** mit dessen Datum sowie die Anzahl gelieferter **Datensätze**. Die Spalte **Hinweis** erklärt eine fehlgeschlagene Probeabfrage. Wählbar ist nur ein Konnektor, dessen Probeabfrage **OK** ergab. Bei Dividenden und Splits ist auch **Keine Daten** wählbar, denn viele Wertpapiere hatten im geprüften Zeitraum schlicht kein solches Ereignis. Ist in einer Tabelle genau ein Konnektor wählbar, ist er bereits ausgewählt.

Bei einer Anlageklasse, die keine Dividenden oder keine Splits haben kann, etwa bei einer direkt gehaltenen Anleihe, erscheint die entsprechende Tabelle nicht, wie auch im Bearbeitungsdialog. Schüttet das Instrument nichts aus, ist die Tabelle der Dividenden-Konnektoren gesperrt und es wird kein Dividenden-Konnektor übernommen.

## Übernehmen
**Übernehmen** schliesst den Dialog und überträgt die Angaben des gewählten Listings sowie die gewählten Konnektoren mit ihren URL-Erweiterungen in den Bearbeitungsdialog. Für einen Feed ohne gewählten Konnektor bleibt die bisherige Einstellung unverändert. Die **Ausschüttungshäufigkeit** stammt vom gewählten Dividenden-Konnektor; lässt dieser sie offen, etwa weil das Wertpapier erst im laufenden Jahr zum ersten Mal ausgeschüttet hat, übernimmt GT die Angabe, in der sich die übrigen Dividenden-Konnektoren einig sind. Prüfen Sie die übernommenen Werte und speichern Sie das Wertpapier wie gewohnt; dabei prüft GT die Konnektoren noch einmal.

{{% callout style="info" title="Grenzen der Suche" %}}
Die Suche gilt nur für Wertpapiere, nicht für Währungspaare. Selbst definierte [generische Konnektoren]({{% relref "/basedata/genericconnector" %}}) werden nicht angeboten; ihre URL-Erweiterung muss weiterhin von Hand erfasst werden. Die Probeabfrage ist eine Momentaufnahme: Ein Konnektor, der heute Daten liefert, kann später ausfallen. Für Handelsplätze, die keiner der Datenanbieter führt, findet die Suche kein Listing. Für Hamburg, Berlin, Shanghai, Shenzhen, Jakarta, B3 und Kairo wird kein Hyperlink Handelsplatz vorgeschlagen.
{{% /callout %}}

Neben **Suchen** kann die Schaltfläche **GTNet-Suche** erscheinen. Sie befragt andere GT-Instanzen im GTNet und ist unabhängig von der hier beschriebenen Suche.
