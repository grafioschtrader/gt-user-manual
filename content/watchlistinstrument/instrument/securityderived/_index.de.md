---
title: "Wertpapier und abgeleitetes Instrument"
date: 2026-01-20T10:54:47+01:00
draft: false
weight: 8
archetype: "default"
---
Ein **Wertpapier** und ein **abgeleitetes Wertpapier** haben viele Gemeinsamkeiten, daher werden diese hier zusammen dokumentiert.

## Wertpapier-Split
Wird ein Wertpapiersplit erfasst, dessen Datum vor dem Datum des "Volle Datenladung" liegt, so werden die historischen Daten dieses Wertpapiers neu eingelesen.

## Erstellen und bearbeiten Wertpapier oder abgeleitetes Instrument
Ein Wertpapier bzw. Instrument wird im Hauptbereich in einer der **Wachlisten-Ansichten** erstellt, bearbeitet und gelöscht. Diese Ansichten können in Navigationsbereich über den Namen der **Watchlist** erreicht werden..
+ Erstellen eines Wertpapiers über das Kontextmenü mit der Wahl "**Hinzufügen neues Wertpapier**".
+ Erstellen eines abgeleitetes Instrument mit der Wahl "**Hinzufügen neues abgeleitetes Instrument**".
+ Bearbeiten eines Wertpapiers bzw. Instrument über das Kontextmenü mit Menüpunkt "**Bearbeiten Instrument**" bei selektierten Instrument bzw. Wertpapier.
+ Löschen eines Wertpapiers bzw. Instrument über das Kontextmenü mit Menüpunkt "**Instrument entfernen und löschen**" auf dem selektierten Instrument.

## Eigenschaften
Die meisten gemeinsamen Eigenschaften von **Wertpapier** und **abgeleitetes Wertpapier** sind selbsterklärend und werden nicht weiter dokumentiert.
- **ISIN** und **Ticker/Symbol**: Die Eigenschaft **ISIN** kann nur beim Erstellen eines Instrumentes gesetzt werden. Wenn die **ISIN** angezeigt wird, muss ein Wert angegeben werden. Zusammen mit der Währung wird ein Wertpapier in GT identifiziert. Der **Ticker/Symbol** sollte wenn möglich erfasst werden, diese werden ggf. in den **benutzerdefinierten Zusatzfeldern** zur **Identifikation** bei einem **Datenlieferanten** verwendet.
- **Privates Wertpapier**: Ein Instrument kann als private definiert werden. Damit ist dieses für andere Benutzer nicht sichtbar. Ein **privates Wertpapier** kann keine **ISIN** und **Ticker/Symbol** aufweisen. 
- **Währung**: Die Währung des Instruments kann nach dem erstmaligen Speichern nicht mehr geändert werden.
- **Handel ab**: Wird für ein bestehendes **aktives** Wertpapier dieses Datum auf einen früheren Zeitpunkt gesetzt, werden die historischen Kursdaten neu eingelesen.
- **Hyperlink Handelsplatz**: Dieser Hyperlink ist für den Aufruf eines Haupthandelsplatzes des Instruments gedacht. Diese Funktionalität wird in unterschiedlichen Ansichten über das Kontextmenü angeboten.
- **Produkt Hyperlink**: Hiermit kann ein Hyperlink für das Produkt eingetragen werden. Dieser Hyperlink wird in unterschiedlichen Ansichten über das Kontextmenü angeboten. Dabei wird dieser Hyperlink je nach Finanzinstrument unterschiedliche Ausprägungen haben. Bei einem ETF oder Anlagefonds wird meistens der Link auf das Produkt des entsprechenden Anbieters eingetragen. Bei einer Aktie könnte der Link auf die **Investor Relations** der entsprechenden Firma verweisen.

## GTNet Wertpapiersuche
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung der GTNet Wertpapiersuche ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}

Beim Erstellen eines neuen Wertpapiers kann die **GTNet Wertpapiersuche** verwendet werden, um Wertpapierdaten von anderen GTNet-Instanzen zu übernehmen. Dies erleichtert die Erfassung neuer Wertpapiere erheblich, da Metadaten und Konnektoreinstellungen automatisch vorausgefüllt werden.

### Funktionsweise
Im Dialog zur Erstellung eines neuen Wertpapiers steht die Schaltfläche **GTNet-Suche** zur Verfügung. Nach Eingabe eines Suchbegriffs (z.B. Name, ISIN oder Tickersymbol) wird eine Anfrage an alle verbundenen GTNet-Instanzen gesendet.

### Suchergebnisse
Die Suchergebnisse werden in einer Tabelle mit folgenden Informationen angezeigt:
- **Name**: Name des Wertpapiers
- **ISIN**: Internationale Wertpapierkennnummer
- **Währung**: Handelswährung
- **Ticker/Symbol**: Börsenkürzel
- **Handelsplatz**: Name und MIC-Code der Börse
- **Anlageklasse**: Kategorie des Wertpapiers (z.B. Aktie, ETF)
- **Finanzinstrument**: Art des Instruments
- **Quelldomäne**: GTNet-Instanz, von der die Daten stammen

### Konnektorkonfiguration
Jede Tabellenzeile kann aufgeklappt werden, um die übereinstimmenden Konnektoreinstellungen anzuzeigen. Dabei werden vier Kategorien unterschieden:
- **Historische Kurse**: Konnektor und URL-Erweiterung für historische Preisdaten
- **Intraday-Kurse**: Konnektor und URL-Erweiterung für Tageskurse
- **Dividenden**: Konnektor und URL-Erweiterung für Dividendendaten
- **Splits**: Konnektor und URL-Erweiterung für Splitdaten

Die Konnektoren werden nur angezeigt, wenn ein passender Konnektor in der lokalen GT-Instanz verfügbar ist. Dies ermöglicht eine automatische Übernahme der Einstellungen.

### Übernahme der Daten
Nach Auswahl eines Suchergebnisses und Klick auf **Ausgewähltes anwenden** werden die Wertpapierdaten in das Erstellungsformular übernommen. Der Benutzer kann die Daten vor dem Speichern noch anpassen.

Folgende Daten werden bei der Übernahme berücksichtigt:
- **Stammdaten**: ISIN, Name, Währung, Ticker/Symbol
- **Klassifikation**: Anlageklasse und Finanzinstrument
- **Börseninformationen**: Name, MIC-Code und Hyperlink des Handelsplatzes
- **Zeitraum**: Handel ab und Handel bis (falls vorhanden)
- **Spezifische Eigenschaften**: Denominierung, Verteilungsfrequenz und Hebelfaktor (je nach Instrumenttyp)
- **Hyperlinks**: Produkt Hyperlink (falls vorhanden)
- **Konnektoren**: Datenquellen für historische Kurse, Intraday-Kurse, Dividenden und Splits
