---
title: "Connector-Definition"
date: 2026-03-13T12:00:00+01:00
draft: false
weight: 10
archetype: "default"
---
Die Connector-Definition enthält die Grundeinstellungen des Datenanbieters.
## Connector-Einstellungen
- **Kurz-ID**: Eindeutige Kurzbezeichnung (2–32 Zeichen). Wird intern als Konnektor-Kennung verwendet.
- **Anzeigename**: Lesbarer Name, der in der Benutzeroberfläche angezeigt wird.
- **Beschreibung**: Optionale Beschreibung in Deutsch und Englisch.
- **Domain-URL**: Basis-URL des Datenanbieters (mit abschliessendem Schrägstrich).
- **API-Schlüssel benötigt**: Ob der Anbieter einen API-Schlüssel erfordert. Falls ja, muss dieser in der Konnektor-API-Schlüssel-Tabelle hinterlegt werden.
- **Unterstützt Wertpapier**: Ob Wertpapierkurse unterstützt werden.
- **Unterstützt Währung**: Ob Währungspaarkurse unterstützt werden.
## Ratenlimit-Einstellungen
Viele APIs beschränken die Anzahl Anfragen pro Zeiteinheit. Die Ratenlimit-Konfiguration stellt sicher, dass GT diese Grenzen respektiert.
- **Ratenlimit-Typ**: `Keine` (kein Limit), `Token-Bucket` (feste Anfragerate) oder `Semaphor` (Begrenzung gleichzeitiger Anfragen).
- **Ratenlimit-Anfragen**: Maximale Anzahl Anfragen pro Zeitraum (nur bei Token-Bucket).
- **Ratenlimit-Zeitraum (Sek.)**: Länge des Zeitfensters in Sekunden (nur bei Token-Bucket).
- **Max. gleichzeitig**: Maximale Anzahl gleichzeitiger Anfragen (nur bei Semaphor).
## Erweiterte Einstellungen
- **Intraday-Verzögerung (Sek.)**: Mindestabstand in Sekunden zwischen zwei Intraday-Abfragen für dasselbe Instrument (Standard: 900 = 15 Minuten).
- **URL-Muster (Regex)**: Optionaler regulärer Ausdruck zur Validierung der URL-Erweiterung bei Wertpapieren.
- **Lückenfüller**: Ob GT fehlende Handelstage in den historischen Daten automatisch mit dem Vortageswert füllen soll.
- **GBX-Teiler**: Ob Kurse automatisch durch 100 geteilt werden sollen — nötig für die London Stock Exchange, wo Kurse in Pence statt Pfund notiert sind.
- **Unterstützte Kategorien**: Mehrfachauswahl der Instrumentkategorien, die der Connector liefern kann. Die verfügbaren Kategorien sind: Währungspaar, Kryptowährung, Nicht investierbare Indizes, Aktien, Anleihen, ETF, Anlagefonds, Immobilienfonds und Emittentenrisikoprodukt. Wenn leer, werden alle Kategorien als unterstützt angenommen. Beispielsweise würde ein Kryptowährungsanbieter nur *Kryptowährung* auswählen, während ein breites Börsenportal *Aktien*, *Anleihen*, *ETF* und *Anlagefonds* auswählen könnte. Dieses Feld ist vorbereitend für künftige Connector-Filterung und wird zur Laufzeit noch nicht ausgewertet.
- **Geo-Einschränkungen**: Legt fest, welche Länder oder Börsen ein Connector abdeckt. Damit lässt sich der geografische Geltungsbereich modellieren — beispielsweise ein Connector, der nur Daten für US-Börsen liefert, oder einer, der die gesamte Schweiz abdeckt, ausser Anleihen an einer bestimmten Börse. Wenn leer, gilt der Connector als weltweit verfügbar. Dieses Feld ist vorbereitend und wird zur Laufzeit noch nicht ausgewertet.

  Im Bearbeitungsdialog wird dieses Feld in zwei Baumauswahl-Steuerelemente aufgeteilt:
  - **Geo-Einschlüsse**: Auswahl von Ländern (2-stelliger ISO-Code, z. B. `US`, `CH`, `DE`) oder einzelnen Börsen anhand ihres MIC-Codes (z. B. `XETR` für Xetra, `XSWX` für SIX Swiss Exchange, `XWAR` für Warschau). Der Baum ist als Land → Börse strukturiert, sodass ein ganzes Land oder nur bestimmte Börsen darin ausgewählt werden können.
  - **Geo-Ausschlüsse**: Optional können bestimmte Instrumentkategorien von einem bereits eingeschlossenen Land oder einer Börse ausgeschlossen werden. Jeder Ausschluss kombiniert einen geografischen Code mit einer Kategorie im Format `GEO:-KATEGORIE` — zum Beispiel bedeutet `XSWX:-FIXED_INCOME`, dass der Connector die SIX Swiss Exchange abdeckt, aber *nicht* für Anleihen. In der Baumauswahl stellen die übergeordneten Knoten die Börsen und die untergeordneten Knoten die auszuschliessenden Kategorien dar.

  **Beispiele:**
  - Ein Connector nur für US-Daten: **Geo-Einschlüsse** = `US`
  - Ein Connector für Xetra und Frankfurt: **Geo-Einschlüsse** = `XETR XFRA`
  - Ein Connector, der die Schweiz abdeckt, aber keine Anleihen von der SIX liefern kann: **Geo-Einschlüsse** = `CH`, **Geo-Ausschlüsse** = `XSWX:-FIXED_INCOME`
- **Token-Konfiguration (YAML)**: Optionaler YAML-Block für automatische Token-Beschaffung (SAML/SSO-Flows). Siehe [Token-Konfiguration (YAML)](../tokenconfiguration/) für Details. Wenn leer, verwendet der Connector den klassischen API-Schlüssel-Modus.
