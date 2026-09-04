---
title: "Erste Schritte mit GTNet"
date: 2026-08-31T22:54:47+01:00
draft: false
weight: 3
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GTNet ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Diese Seite führt Schritt für Schritt durch die Einrichtung der eigenen GT-Instanz für die Teilnahme am GTNet-Netzwerk. Was GTNet überhaupt ist, welche Rollen eine Instanz einnehmen kann und wo im Programm es wirkt, beschreibt die [Übersicht](../).

## Voraussetzungen
Für die Teilnahme am GTNet muss der eigene Grafioschtrader-Server von aussen über HTTPS erreichbar sein. Stellen Sie sicher, dass Ihr Server für die Maschine-zu-Maschine-Kommunikation (M2M) korrekt konfiguriert ist. Bei Problemen mit der externen Erreichbarkeit konsultieren Sie den [Leitfaden zur Problemlösung](https://github.com/grafioschtrader/grafioschtrader/wiki/Problem-solving) im Wiki.

## Schritt 1: GTNet in den globalen Einstellungen aktivieren
Aktivieren Sie zunächst die GTNet-Funktionalität über die [Globalen Einstellungen GTNet](../globalsettings/):
- Setzen Sie **g.gnet.use** auf einen Wert grösser als 0, um GTNet zu aktivieren
- Optional können Sie **g.gnet.use.log** aktivieren, um den Datenaustausch zu protokollieren

## Schritt 2: Eigene Instanz registrieren
Bevor die Kommunikation mit anderen Teilnehmern möglich ist, müssen Sie Ihren eigenen Server in der Ansicht [GTNet Setup](../setup/) registrieren:
1. Öffnen Sie die GTNet Setup-Ansicht
2. Erstellen Sie einen Eintrag für Ihren eigenen Server mit Ihrer Domain-URL
3. Nach dem Speichern wird Ihre Instanz-ID in der globalen Einstellung **g.gnet.my.entry.id** gespeichert

{{% notice tip %}}
Falls Sie eine bestehende Datenbank auf einen neuen Server migrieren, sollten Sie vor der Migration die GTNet-Daten auf dem alten Server exportieren und anschliessend auf dem neuen Server importieren. Dadurch bleiben Authentifizierungstoken, Peer-Verbindungen und Nachrichtenhistorie erhalten. Siehe [Export und Import der GTNet-Daten](../setup/#export-und-import-der-gtnet-daten) für Details.
{{% /notice %}}

## Schritt 3: Remote-Instanzen hinzufügen
Fügen Sie eine oder mehrere entfernte GTNet-Instanzen hinzu, um die Kommunikation aufzubauen:
1. Fügen Sie in der Ansicht [GTNet Setup](../setup/) Einträge für entfernte Server hinzu
2. **Empfehlung**: Bevorzugen Sie Gegenparts, die im Modus «Push Offen» arbeiten, da diese einen aktiven Datenpool pflegen und einen beidseitigen Austausch auch ohne lokale Instrumente ermöglichen
3. Konfigurieren Sie das tägliche Abfragelimit und andere Einstellungen nach Bedarf

## Schritt 4: Erste Verbindung herstellen
Fordern Sie direkt den Datenaustausch an, wodurch der Handshake-Prozess automatisch durchgeführt wird:
1. Wählen Sie den Eintrag des entfernten Servers aus
2. Verwenden Sie das Kontextmenü, um eine **Anfrage für Datenaustausch**-Nachricht zu senden
3. Das System führt automatisch den Handshake und den Token-Austausch durch, falls dies der erste Kontakt ist
4. Warten Sie auf die Antwort (Annahme oder Ablehnung)
5. Nach der Annahme wird der Datenaustausch aktiviert und die Authentifizierungstoken werden für zukünftige Kommunikation gespeichert

## Schritt 5: Automatische Antworten konfigurieren (Empfohlen)
Um nicht jede eingehende Anfrage manuell beantworten zu müssen, konfigurieren Sie automatische Antwortregeln in der Ansicht [Automatische Antwort](../autoanswer/):
1. Definieren Sie Regeln für verschiedene Nachrichtentypen (z.B. Erstkontakt, Datenaustausch-Anfragen)
2. Legen Sie Bedingungen fest, z.B. Tageszeit, tägliche Anfragezahl oder Domain-Muster
3. Bestimmen Sie, ob Anfragen automatisch akzeptiert oder abgelehnt werden sollen
4. Konfigurieren Sie Wartezeiten nach Ablehnungen, um wiederholte Anfragen zu verhindern

{{% notice tip %}}
Ohne automatische Antwortregeln erfordert jede eingehende Anfrage eine manuelle Genehmigung durch den Administrator. Das Einrichten geeigneter Regeln reduziert den administrativen Aufwand erheblich.
{{% /notice %}}

## Schritt 6: Wertpapiere für den Austausch konfigurieren
Legen Sie fest, welche Instrumente am Datenaustausch teilnehmen sollen:
1. Öffnen Sie die Ansicht [Kursdaten austauschen](../exchange/)
2. Konfigurieren Sie für jedes Wertpapier oder Währungspaar die vier Austauschoptionen:
   - Erhalte Intraday Preis
   - Erhalte historische Preisdaten
   - Sende Intraday Preise
   - Sende historische Preisdaten
3. Speichern Sie Ihre Änderungen

## Wie es weitergeht
Sobald der Austausch läuft, zeigt [GTNet überwachen](../monitoring/), woran sich ablesen lässt, ob und mit wem tatsächlich Daten fliessen.
