---
title: "Connector-Definition"
date: 2026-02-26T12:00:00+01:00
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
- **Token-Konfiguration (YAML)**: Optionaler YAML-Block für automatische Token-Beschaffung (SAML/SSO-Flows). Siehe [Token-Konfiguration (YAML)](../tokenconfiguration/) für Details. Wenn leer, verwendet der Connector den klassischen API-Schlüssel-Modus.
