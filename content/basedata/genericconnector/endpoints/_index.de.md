---
title: "Endpunkte"
date: 2026-02-27T12:00:00+01:00
draft: false
weight: 20
archetype: "default"
---
Jeder Endpunkt definiert, wie eine bestimmte Art von Preisdaten vom Anbieter abgerufen und interpretiert wird. Pro Connector können bis zu vier Endpunkte konfiguriert werden, wobei jede Kombination aus Feed-Typ und Instrumenttyp nur einmal vorkommen darf:
- **EOD-Verlauf — Wertpapier**: Historische Tagesschlusskurse für Wertpapiere
- **EOD-Verlauf — Währungspaar**: Historische Tagesschlusskurse für Währungspaare
- **Intraday — Wertpapier**: Aktuelle Kurse für Wertpapiere
- **Intraday — Währungspaar**: Aktuelle Kurse für Währungspaare
## Endpunkt-Einstellungen
- **Feed / Instrumenttyp**: Kombinierte Auswahl aus Feed-Typ und Instrumenttyp. Bereits verwendete Kombinationen werden automatisch ausgeblendet. Beim Bearbeiten eines bestehenden Endpunkts ist dieses Feld gesperrt.
- **URL-Vorlage**: Die URL zum Abrufen der Daten. Platzhalter werden automatisch ersetzt (siehe unten).
- **HTTP-Methode**: HTTP-Methode für den Abruf (Standard: `GET`).
- **Antwortformat**: `JSON`, `CSV` oder `HTML` — bestimmt, wie die API-Antwort geparst wird.
- **Zahlenformat**: Format für Dezimalzahlen: `US` (Punkt), `GERMAN` (Komma), `SWISS` (Apostroph) oder `PLAIN` (ohne Tausendertrennzeichen).
- **Datumsformat-Typ**: Wie Datumsangaben in der Antwort codiert sind: `Unix-Sekunden`, `Unix-Millisekunden`, `ISO_DATE`, `ISO_DATE_TIME` oder `PATTERN` (benutzerdefiniert).
- **Datumsformat-Muster**: Benutzerdefiniertes Datumsformat-Muster (nur bei Typ `PATTERN`), z.B. `yyyy-MM-dd`.
- **Endpunkt-Optionen**: Optionale Verarbeitungs-Flags für den Endpunkt (siehe [Endpunkt-Optionen](#endpunkt-optionen) weiter unten).
## Platzhalter in der URL-Vorlage
- **`{ticker}`**: Ticker-Symbol oder URL-Erweiterung des Instruments
- **`{fromDate}`**: Startdatum (formatiert gemäss Datumsformat-Einstellung)
- **`{toDate}`**: Enddatum (formatiert gemäss Datumsformat-Einstellung)
- **`{fromUnix}`**: Startdatum als Unix-Zeitstempel (Sekunden)
- **`{toUnix}`**: Enddatum als Unix-Zeitstempel (Sekunden)
- **`{fromUnixMs}`**: Startdatum als Unix-Zeitstempel (Millisekunden)
- **`{toUnixMs}`**: Enddatum als Unix-Zeitstempel (Millisekunden)
- **`{fromDay}`**: Tag des Startdatums (1–31)
- **`{fromMonth}`**: Monat des Startdatums (1–12)
- **`{fromYear}`**: Jahr des Startdatums (z.B. 2026)
- **`{toDay}`**: Tag des Enddatums (1–31)
- **`{toMonth}`**: Monat des Enddatums (1–12)
- **`{toYear}`**: Jahr des Enddatums (z.B. 2026)
- **`{fromCurrency}`**: ISO-Währungscode der Basiswährung (z.B. `USD`)
- **`{toCurrency}`**: ISO-Währungscode der Zielwährung (z.B. `CHF`)
- **`{limit}`**: Maximale Anzahl Datenpunkte pro Abfrage (aus der Endpunkt-Einstellung **Max. Datenpunkte**)
- **`{apiKey}`**: API-Schlüssel (aus der Konnektor-API-Schlüssel-Tabelle)
**Beispiel** einer URL-Vorlage für einen JSON-API-Anbieter:
```
https://api.example.com/v1/history?symbol={ticker}&from={fromDate}&to={toDate}&apikey={apiKey}
```
## Format-spezifische Einstellungen
Je nach gewähltem **Antwortformat** werden unterschiedliche Einstellungen relevant:
### JSON-Einstellungen
- **Datenstruktur**: `Array von Objekten` (häufigstes Format), `Parallele Arrays` oder `Einzelobjekt` (für Intraday).
- **Datenpfad**: Pfad zum Daten-Array in der JSON-Antwort (z.B. `results` oder `data.quotes`).
- **Statuspfad**: Optionaler Pfad zu einem Statusfeld für Fehlererkennung.
- **Status-OK-Wert**: Erwarteter Wert im Statusfeld bei erfolgreicher Antwort.
### CSV-Einstellungen
- **CSV-Trennzeichen**: Trennzeichen zwischen Spalten (Standard: Komma).
- **Kopfzeilen überspringen**: Anzahl Kopfzeilen, die beim Parsen übersprungen werden (Standard: 1).
### HTML-Einstellungen
- **CSS-Selektor**: JSoup-CSS-Selektor für das relevante HTML-Element.
- **Extraktionsmodus**: `Regex-Gruppen` (Werte über reguläre Ausdrücke extrahieren), `Aufteilungspositionen` (Text an Trennzeichen aufteilen) oder `Multi-Selektor` (je Feld ein eigener CSS-Selektor).
- **Textbereinigung**: Optionaler regulärer Ausdruck zum Bereinigen des extrahierten Textes.
- **Extraktions-Regex**: Regulärer Ausdruck mit Capture-Gruppen (nur bei Modus Regex-Gruppen).
- **Aufteilungs-Trennzeichen**: Trennzeichen zum Aufteilen des Textes (nur bei Modus Aufteilungspositionen).
## Ticker-Einstellungen
Manche Datenanbieter verwenden Ticker-Formate, die vom GT-Standard abweichen. Die Ticker-Einstellungen erlauben die Anpassung.
- **Ticker-Aufbaustrategie**: `URL_EXTEND` (das URL-Erweiterungsfeld des Wertpapiers wird verwendet) oder `CURRENCY_PAIR` (Währungspaar wird aus Basis- und Zielwährung zusammengesetzt).
- **Währungspaar-Trennzeichen**: Optionales Zeichen zwischen den beiden Währungscodes (z.B. `/` für `EUR/USD`).
- **Währungspaar-Suffix**: Optionales Suffix nach dem Währungspaar (z.B. `=X`).
- **Ticker Grossbuchstaben**: Ob der Ticker in Grossbuchstaben umgewandelt werden soll.
## Paginierungs-Einstellungen
- **Max. Datenpunkte**: Maximale Anzahl Datenpunkte pro Abfrage.
- **Paginierung aktiviert**: Ob Paginierung für grosse Datenmengen aktiviert ist.
## Endpunkt-Optionen
Endpunkt-Optionen sind Bitmask-Flags, die spezielle Verarbeitung für bestimmte Feed-Typen aktivieren.
- **`SKIP_WEEKEND_DATA`** (Historische Endpunkte): Filtert Datenpunkte heraus, die auf Samstag oder Sonntag fallen. Nützlich, wenn ein Datenanbieter fälschlicherweise Wochenend-Daten liefert.
- **`REMOVE_DUPLICATE_DATES`** (Historische Endpunkte): Entfernt doppelte Datumseinträge aus der Antwort. Nur der erste Eintrag pro Datum wird beibehalten. Nützlich, wenn ein Datenanbieter überlappende Daten über Paginierungsseiten hinweg oder doppelte Zeilen liefert.
