---
title: "Alarme"
date: 2026-02-12T12:00:00+01:00
draft: false
weight: 30
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Das Alarmsystem in GT informiert den Benutzer, wenn konfigurierte Bedingungen auf Wertpapieren erfüllt werden. Alarme können in zwei Kontexten erstellt werden: als eigenständige Wertpapier-Alarme oder als Strategie-Alarme innerhalb einer Portfolio-Strategie.

## Eigenständige Wertpapier-Alarme
Eigenständige Alarme werden direkt auf einem Wertpapier in einer Watchlist erstellt, ohne dass eine Portfolio-Strategie erforderlich ist. Über das Kontextmenü eines Wertpapiers in der Watchlist wählen Sie **Alarm hinzufügen**, um einen neuen Alarm zu konfigurieren. Diese Alarme werden unabhängig von der AlgoTop-Hierarchie ausgewertet und eignen sich für die einfache Überwachung einzelner Wertpapiere.

Bei der Erstellung eines eigenständigen Alarms wird automatisch ein Wertpapier-Eintrag (AlgoSecurity) ohne übergeordnete Anlageklasse angelegt. Diesem Eintrag wird dann die gewählte Strategie zugewiesen.

## Strategie-Alarme
Innerhalb der hierarchischen Portfolio-Strategie (AlgoTop) können Strategien auf verschiedenen Ebenen zugewiesen werden. Wenn die Bedingungen einer solchen Strategie erfüllt sind, erzeugt das System einen Alarm. Diese Alarme stehen im Kontext der gesamten Portfolio-Strategie und berücksichtigen die hierarchische Struktur von Anlageklassen und Wertpapieren.

## Alarm-Übersicht (Mandanten-Alarme)
Die Übersicht aller Alarme eines Mandanten wird in einer Baumtabelle dargestellt. Auf der obersten Ebene erscheinen die Wertpapiere (AlgoSecurity), darunter als Kindknoten die zugewiesenen Strategien (AlgoStrategy). In der Spalte **Kontext** wird angezeigt, ob es sich um einen eigenständigen Alarm oder einen Alarm aus einer Portfolio-Strategie handelt.

Jede Strategie verfügt über ein aktivierbares Kontrollkästchen. Ist dieses aktiviert, wird die Strategie bei der nächsten Alarm-Auswertung berücksichtigt. So können einzelne Strategien vorübergehend deaktiviert werden, ohne sie löschen zu müssen. Über das Kontextmenü können Strategien auch bearbeitet oder gelöscht werden.

## Alarm-Auswertung
Die Alarm-Auswertung in GT erfolgt in zwei Stufen.

**Stufe 1 (ereignisgesteuert)** wertet einfache Preisalarme unmittelbar nach der Aktualisierung der Innertag-Kursdaten aus. Hierzu gehören die **Absolute Gewinn/Verlust Warnung**, die **Preis im Bestand Gewinn/Verlust Warnung** und die **Gewinn/Verlust Warnung in einer Periode**. Diese Alarme benötigen keine historischen Kursdaten und können daher sofort berechnet werden.

**Stufe 2 (geplant)** wird periodisch als Hintergrundaufgabe ausgeführt und wertet indikatorbasierte Alarme aus. Hierzu gehören der **Gleitender Durchschnitt Kreuzungsalarm**, der **RSI Schwellenwert-Alarm** und der **Benutzerdefinierte Ausdrucksalarm**. Diese Alarme laden bei Bedarf historische Kursdaten für die Berechnung von technischen Indikatoren.

## Alarm-Lebenszyklus
Wenn eine Alarm-Bedingung erfüllt wird, erstellt das System einen Alarm-Datensatz mit den Details der ausgelösten Bedingung. Anschliessend wird der Benutzer über das interne Nachrichtensystem von GT benachrichtigt. Der Benutzer kann die Nachricht lesen, die Situation beurteilen und dann selbständig über die gewünschte Aktion entscheiden. GT führt keine automatischen Transaktionen aus.

## Deduplizierung
Um doppelte Alarmmeldungen zu vermeiden, prüft das System vor dem Erstellen eines neuen Alarms, ob am gleichen Tag bereits ein Alarm für dasselbe Wertpapier mit demselben Nachrichtentyp generiert wurde. In diesem Fall wird kein neuer Alarm erstellt. Damit wird sichergestellt, dass ein Benutzer nicht mehrfach am gleichen Tag über dieselbe Bedingung informiert wird.
