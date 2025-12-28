---
title: "Wertpapier und abgeleitetes Instrument"
date: 2021-04-01T10:54:47+01:00
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
