---
title: "Handelsplatz"
date: 2026-09-03T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
An den Handelsplätzen werden die Instrumente gehandelt. In GT sind diese aus folgenden Gründen wichtig:
+ Mit dem Handelskalender eines Handelsplatzes lässt sich die **Vollständigkeit** der **historischen Kursdaten** evaluieren.
+ Die Zuordnung des Handelsplatzes zu einem **Instrument** bestimmt, ob es überhaupt öffentlich zugängliche Kursdaten für dieses gibt.
+ In einer zukünftigen GT-Version wird die Aktualisierung der **historischen Kursdaten** gestaffelt in den Zeiträumen der **geschlossenen Handelsplätze** durchgeführt.

Das **Land** und die **Zeitzone** können bei einem bestehenden Handelsplatz nicht mehr verändert werden, dies soll einer totalen Umschreibung eines Handelsplatzes entgegenwirken. Die Einstellung **Keine Kursdaten** lässt sich nur so lange korrigieren, wie dem Handelsplatz noch kein Instrument zugeordnet ist.

## Erstellen und bearbeiten Anlageklasse
Ein **Handelsplatz** wird im Hauptbereich in der **Ansicht Handelsplatz** erstellt, bearbeitet und gelöscht. Diese **Ansicht** erreichen Sie im **Navigationsbereich** auf dem statischen Element "Handelsplatz". Die Ansicht ist in zwei Register aufgeteilt: den eigentlichen **Handelsplatz** und die [Handelskalender-Regeln](./tradingcalendarruleset/), die eine zweite, geteilte Quelle für den Handelskalender bieten.
+ **Erstellen** eines **Handelsplatzes** über das **Kontextmenü**.
+ **Bearbeiten** eines **Handelsplatzes** über das **Kontextmenü** bei **selektiertem Handelsplatz**.
+ **Löschen** eines **Handelsplatzes** über das **Kontextmenü** bei **selektiertem Handelsplatz**.

## Eigenschaften und Tabellenspalten
- **MIC**: Mit der Auswahl des **MIC (Market Identifier Code)** erfolgt automatisch die Zuweisung des **Namens**, **Landes** und der **Webseite**. Bei den den **Hauptbörsen** wird zusätzlich noch die **Zeitzone** gesetzt. Der MIC kann in gewissen Kursdaten-Konnektoren verwendet werden. Das MIC-Dropdown enthält ein **Suchfeld**, mit dem MICs schnell nach Name oder Code gefiltert werden können.
- **Land**: Das **Land** wird durch die Auswahl des **MIC** gesteuert und kann nicht geändert werden.
- **Name**: Der **Name** des Handelsplatzes wird durch die Auswahl des **MIC** automatisch in Kleinbuchstaben vorgegeben. Dieser sollte entsprechend korrigiert werden. Durch Zusammenschlüsse von Börsenplätzen kann dieser im Laufe der Zeit verändern.
- **Sekundär Markt**: Einige Wertpapiere werden nicht auf dem Sekundärmarkt gehandelt, aber es gibt Kursdaten, die von einem Anbieter heruntergeladen werden können. Die meisten Wertpapiere werden jedoch auf dem Sekundärmarkt gehandelt.
- **Keine Kursdaten**: Falls es für die Instrumente dieses Handelsplatzes keine öffentlich verfügbaren Kursdaten gibt, sollte dieses Markierungsfeld markiert werden. Dies aktiviert die Eingabemöglichkeit von **"Historischen Kurse für Periode"** beim Erfassen des **Instruments**; ein Kursdaten-Konnektor steht für ein solches Instrument nicht zur Verfügung. Solange dem Handelsplatz noch kein Instrument zugeordnet ist, lässt sich die Einstellung korrigieren. Sobald das erste Instrument zugeordnet ist, wird das Markierungsfeld grau dargestellt und die Einstellung kann nicht mehr geändert werden, da die bereits erfassten Kurse dadurch unbrauchbar würden. Ein Wechsel ist dann nur möglich, indem ein neuer Handelsplatz mit der gewünschten Einstellung erstellt und die Instrumente diesem zugeordnet werden. Umgehängt werden können dabei nur Instrumente, für die noch keine Kurse vorliegen und die noch nicht gehandelt wurden, siehe [Handelsplatz](../../../watchlistinstrument/instrument/securityderived/#handelsplatz).
- **Öffnungszeit, Zeit schluss und Zeitzone**: In einer zukünftigen GT-Version wird die Aktualisierung der historischen Kursdaten gestaffelt in den Zeiträumen der geschlossenen Handelsplätze durchgeführt. Diese Eigenschaften bestimmen den Zeitraum, im welchen die Aktualisierung stattfinden kann.

## Zusätzliche eigenschaften beim Editieren
- **Nur Hauptbörsen**: Die Auswahl des **MIC** wird entsprechend diesem Auswahlkasten auf die **Hauptbörsen** bzw. auf alle bekannten MIC eingeschränkt bzw. erweitert. Es ist zum Teil willkürlich, was in GT als **Hauptbörse** definiert ist, wir hoffen, es fühlt sich niemand negativ betroffen durch diese Auswahl.

### Kalenderquelle: Index oder Regelsatz
Der **Handelskalender** eines Handelsplatzes lässt sich automatisiert aus einer von zwei Quellen nachführen, und die beiden schliessen sich **gegenseitig aus** — ein Handelsplatz verwendet entweder einen Index oder einen Regelsatz, nie beides. Die Auswahl der einen Quelle deaktiviert das Eingabefeld der anderen.

Die erste Quelle ist ein an diesem Handelsplatz gehandelter **Index**, der im Feld "**Index für Handelskalender**" gesetzt wird. Das Dropdown zeigt nur **nicht investierbare Indizes aus dem Land** des Handelsplatzes an; die Liste wird dynamisch geladen, sobald ein Land ausgewählt wird. Jeder mögliche Handelstag, an dem der Index keinen Kurs geliefert hat, wird zu einem geschlossenen Tag. Falls das benötigte Index-Instrument **bereits existiert**, kann es direkt beim Erstellen des Handelsplatzes ausgewählt werden. Andernfalls ist der Ablauf:
1. Handelsplatz erstellen.
2. Optional eine Anlageklasse für das entsprechende Land und den Index erstellen.
3. Instrument für diesen Index erstellen.
4. Handelsplatz bearbeiten und den erstellten Index im Eingabefeld "**Index für Handelskalender**" setzen.

Die zweite Quelle ist ein "**Handelskalender-Regelsatz**", der im gleichnamigen Dropdown gewählt wird. Wird ein MIC gewählt, für den ein passender Regelsatz existiert, ist dieser Regelsatz bereits vorausgewählt. Im Gegensatz zum Index leitet ein Regelsatz die geschlossenen Tage aus Feiertagsregeln ab und deckt damit auch zukünftige Jahre ohne Kursdaten ab. Siehe [Handelskalender-Regeln](./tradingcalendarruleset/) dazu, wie diese Regelsätze erstellt und gepflegt werden.

## Handelskalender
Der **Handelskalender** eines **Handelsplatzes** ist eine Voraussetzung für die **Vollständigkeitsprüfung** der historischen Kursdaten. Die "**automatisierte Markierung**" für geschlossene Tage kommt nur nach dem jüngsten durch den Benutzer markierten Tag zur Anwendung. Das heisst, das System markiert nie einen Tag der älter ist, als die jüngste **blaue** Markierung.
+ **Offene Tage**: **Grün** markierte Tage definieren sich aus dem **globalen Handelskalender**.
+ **Geschlossene Tage**: **Rot** markierte Tage wurden vom System aus der gewählten Kalenderquelle erstellt — dem **Index** oder dem **Regelsatz**. **Blau** markierte Tage wurden durch den Benutzer verursacht. Durch die Kopierfunktionen ändern sich diese Merkmale nicht. 

### Funktionen
Es gibt zwei Kopierfunktionen um einen gesamten Handelskalender oder einen Jahreskalender auf den selektieren Jahreskalender zu kopieren. Dabei werden bestehende Kalender überschrieben.
