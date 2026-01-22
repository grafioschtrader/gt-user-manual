---
title: "Handelsplatz"
date: 2021-03-29T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
An den Handelsplätzen werden die Instrumente gehandelt. In GT sind diese aus folgenden Gründen wichtig:
+ Mit dem Handelskalender eines Handelsplatzes lässt sich die **Vollständigkeit** der **historischen Kursdaten** evaluieren.
+ Die Zuordnung des Handelsplatzes zu einem **Instrument** bestimmt, ob es überhaupt öffentlich zugängliche Kursdaten für dieses gibt.
+ In einer zukünftigen GT-Version wird die Aktualisierung der **historischen Kursdaten** gestaffelt in den Zeiträumen der **geschlossenen Handelsplätze** durchgeführt.

Das **Land** und die **Zeitzone** können bei einem bestehenden Handelsplatz nicht mehr verändert werden, dies soll einer totalen Umschreibung eines Handelsplatzes entgegenwirken.

## Erstellen und bearbeiten Anlageklasse
Ein **Handelsplatz** wird im Hauptbereich in der **Ansicht Handelsplatz** erstellt, bearbeitet und gelöscht. Diese **Ansicht** erreichen Sie im **Navigationsbereich** auf dem statischen Element "Handelsplatz".
+ **Erstellen** eines **Handelsplatzes** über das **Kontextmenü**.
+ **Bearbeiten** eines **Handelsplatzes** über das **Kontextmenü** bei **selektiertem Handelsplatz**.
+ **Löschen** eines **Handelsplatzes** über das **Kontextmenü** bei **selektiertem Handelsplatz**.

## Eigenschaften und Tabellenspalten
- **MIC**: Mit der Auswahl des **MIC (Market Identifier Code)** erfolgt automatisch die Zuweisung des **Namens**, **Landes** und der **Webseite**. Bei den den **Hauptbörsen** wird zusätzlich noch die **Zeitzone** gesetzt. Der MIC kann in gewissen Kursdaten-Konnektoren verwendet werden.
- **Land**: Das **Land** wird durch die Auswahl des **MIC** gesteuert und kann nicht geändert werden.
- **Name**: Der **Name** des Handelsplatzes wird durch die Auswahl des **MIC** automatisch in Kleinbuchstaben vorgegeben. Dieser sollte entsprechend korrigiert werden. Durch Zusammenschlüsse von Börsenplätzen kann dieser im Laufe der Zeit verändern.
- **Sekundär Markt**: Einige Wertpapiere werden nicht auf dem Sekundärmarkt gehandelt, aber es gibt Kursdaten, die von einem Anbieter heruntergeladen werden können. Die meisten Wertpapiere werden jedoch auf dem Sekundärmarkt gehandelt.
- **Keine Kursdaten**: Falls es keine Kursdaten gibt, sollte dieses Markierungsfeld markiert werden. Dies aktiviert die Eingabemöglichkeit von **"Historischen Kurse für Periode"** beim Erfassen des **Instruments**.
- **Öffnungszeit, Zeit schluss und Zeitzone**: In einer zukünftigen GT-Version wird die Aktualisierung der historischen Kursdaten gestaffelt in den Zeiträumen der geschlossenen Handelsplätze durchgeführt. Diese Eigenschaften bestimmen den Zeitraum, im welchen die Aktualisierung stattfinden kann.

## Zusätzliche eigenschaften beim Editieren
- **Nur Hauptbörsen**: Die Auswahl des **MIC** wird entsprechend diesem Auswahlkasten auf die **Hauptbörsen** bzw. auf alle bekannten MIC eingeschränkt bzw. erweitert. Es ist zum Teil willkürlich, was in GT als **Hauptbörse** definiert ist, wir hoffen, es fühlt sich niemand negativ betroffen durch diese Auswahl.

### Index für Handelskalender
Der **Handelskalender** lässt sich automatisiert über einen an diesem Handelsplatz gehandelten **Index** aktualisieren. Dieser kann natürlich erst mit der zweiten Bearbeitung des **Handelsplatzes** gesetzt werden. Dass heisst:
1. Handelsplatz erfassen.
2. Optional eine Anlageklasse für das entsprechende Land und den Index erstellen.
3. Instrument für diesen Index erstellen.
4. Handelsplatz mit dem erstellten Index in Eingabefeld "**Index für Handelskalender**" ergänzen.

## Handelskalender
Der **Handelskalender** eines **Handelsplatzes** ist eine Voraussetzung für die **Vollständigkeitsprüfung** der historischen Kursdaten. Die "**automatisierte Markierung**" für geschlossene Tage über den Index kommt nur nach dem jüngsten durch den Benutzer markierten Tag zur Anwendung. Das heisst, das System markiert nie einen Tag der älter ist, als die jüngste **blaue** Markierung.
+ **Offene Tage**: **Grün** markierte Tage definieren sich aus dem **globalen Handelskalender**.
+ **Geschlossene Tage**: **Rot** markierte Tage wurden ursprünglich durch den **Index für Handelskalender** erstellt. **Blau** markierte Tage wurden durch den Benutzer verursacht. Durch die Kopierfunktionen ändern sich diese Merkmale nicht. 

### Funktionen
Es gibt zwei Kopierfunktionen um einen gesamten Handelskalender oder einen Jahreskalender auf den selektieren Jahreskalender zu kopieren. Dabei werden bestehende Kalender überschrieben.
