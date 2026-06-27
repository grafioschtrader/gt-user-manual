---
title: "Connector-Definition"
date: 2026-05-27T12:00:00+01:00
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
- **Unterstützte Kategorien**: Mehrfachauswahl der Instrumentkategorien, die der Connector liefern kann. Die verfügbaren Einträge sind dieselben, die auch im Dropdown erscheinen: Währungspaar, Kryptowährung, Nicht investierbare Indizes, Aktien, Anleihen, ETF, Anlagefonds, Immobilienfonds, Emittentenrisikoprodukt, CFD-Derivat und Pensionsfonds. Wenn leer, werden alle Kategorien als unterstützt angenommen. Beispielsweise würde ein Kryptowährungsanbieter nur *Kryptowährung* anhaken, während ein breites Börsenportal *Aktien*, *Anleihen*, *ETF* und *Anlagefonds* anhaken könnte. Dieses Feld ist vorbereitend für künftige Connector-Filterung und wird zur Laufzeit noch nicht ausgewertet.
- **Geo-Einschlüsse**: Baumauswahl mit den in GT konfigurierten Ländern und Börsen. Länder erscheinen als übergeordnete Knoten (mit lokalisiertem Namen und Flagge — zum Beispiel *Schweiz*, *Deutschland*, *Vereinigte Staaten*); jedes Land lässt sich aufklappen und zeigt darunter die zugehörigen Börsen (zum Beispiel *SIX Swiss Exchange*, *Xetra*, *Frankfurt Stock Exchange*). Hier werden die Einträge angehakt, die dieser Connector abdeckt. Die Auswahl eines Landes und die Auswahl einer seiner Börsen sind voneinander unabhängig: Wer *Schweiz* anhakt, hakt damit *SIX Swiss Exchange* nicht automatisch mit an — und umgekehrt. Wenn leer, gilt der Connector als weltweit verfügbar. Dieses Feld ist vorbereitend und wird zur Laufzeit noch nicht ausgewertet.
- **Geo-Ausschlüsse**: Baumauswahl, mit der Instrumentkategorien markiert werden, die dieser Connector für eine bestimmte Geo-Auswahl *nicht* liefern kann. Der Baum zeigt nur Länder und Börsen, die in **Geo-Einschlüsse** angehakt sind (er wird automatisch neu aufgebaut, sobald sich die Einschlussauswahl ändert). Den gewünschten Eintrag aufklappen und die auszuschliessenden Kategorien — *Aktien*, *Anleihen*, *ETF*, *Anlagefonds* usw., dieselben Bezeichnungen wie in **Unterstützte Kategorien** — anhaken. Wenn keine Ausschlüsse nötig sind, bleibt der Baum leer.

  **Beispiele:**
  - Ein Connector nur für US-Daten: In **Geo-Einschlüsse** *Vereinigte Staaten* anhaken.
  - Ein Connector für die deutschen Börsen Xetra und Frankfurt: In **Geo-Einschlüsse** *Deutschland* aufklappen und *Xetra* sowie *Frankfurt Stock Exchange* anhaken (den Knoten *Deutschland* selbst nicht anhaken).
  - Ein Connector, der die Schweiz allgemein abdeckt, aber keine Anleihen von der SIX liefern kann: In **Geo-Einschlüsse** *Schweiz* und zusätzlich *SIX Swiss Exchange* anhaken; anschliessend in **Geo-Ausschlüsse** den Knoten *SIX Swiss Exchange* aufklappen und *Anleihen* anhaken.
- **Token-Konfiguration (YAML)**: Optionaler YAML-Block für automatische Token-Beschaffung (SAML/SSO-Flows). Siehe [Token-Konfiguration (YAML)](../tokenconfiguration/) für Details. Wenn leer, verwendet der Connector den klassischen API-Schlüssel-Modus.
