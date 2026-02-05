---
title: "GTNet"
date: 2026-01-20T22:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GTNet ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
GTNet (Grafioschtrader-Netzwerk) ist ein dezentrales Peer-to-Peer-System, das mehreren Grafioschtrader-Instanzen den Austausch von Finanzdaten ermöglicht. Jede Installation kann gleichzeitig als Datenanbieter und Datennutzer im Netzwerk agieren.

### Konzept und Funktionsweise
GTNet verbindet verschiedene Grafioschtrader-Instanzen miteinander und ermöglicht den gegenseitigen Austausch von Kursdaten. Dabei kann jede Instanz selbst entscheiden, ob sie Daten bereitstellt, empfängt oder beides gleichzeitig tut. Der Austausch erfolgt automatisiert über eine sichere Maschine-zu-Maschine-Kommunikation (M2M).

### Arten des Datenaustauschs
GTNet unterstützt drei Arten von Daten für den Austausch:
- **Intraday-Kurse**: Aktuelle Tageskurse für Wertpapiere und Währungspaare mit OHLCV-Daten (Eröffnung, Hoch, Tief, Schluss, Volumen).
- **Historische Kurse**: Tägliche Schlusskurse für die Vergangenheit.
- **Wertpapier-Metadaten**: Stammdaten von Wertpapieren wie ISIN, Name, Anlageklasse, Börseninformationen und Konnektoreinstellungen. Dies ermöglicht das Übernehmen von Wertpapierkonfigurationen aus anderen GTNet-Instanzen.

### Verbindungsaufbau und Authentifizierung
Der Verbindungsaufbau zwischen zwei GTNet-Instanzen erfolgt über ein Handshake-Verfahren. Beim ersten Kontakt werden Authentifizierungstoken ausgetauscht, die für die weitere sichere Kommunikation verwendet werden. Jede Instanz kann selbst entscheiden, ob sie unbekannte Server automatisch akzeptiert oder nur vordefinierte Server zulässt.

### Nachrichten und Statusmeldungen
GTNet nutzt ein nachrichtenbasiertes Kommunikationsprotokoll für verschiedene Zwecke:
- **Handshake-Nachrichten**: Zum Herstellen und Bestätigen von Verbindungen zwischen Instanzen.
- **Statusnachrichten**: Zur Mitteilung von Änderungen wie Online/Offline-Status, Wartungsmodus oder Kapazitätsauslastung.
- **Datenanfragen**: Zum Anfordern und Bewilligen des Austauschs bestimmter Datenarten.

### Anfragelimits und Laststeuerung
Um Server vor Überlastung zu schützen, können tägliche Anfragelimits definiert werden. Diese gelten sowohl für eingehende Anfragen von anderen Instanzen als auch für ausgehende Anfragen an andere Server. Zusätzlich kann eine Instanz als «ausgelastet» markiert werden, wodurch nur noch Statusnachrichten kommuniziert werden.

---

## Erste Schritte mit GTNet
Dieser Abschnitt bietet eine Schritt-für-Schritt-Anleitung zur Einrichtung Ihrer GT-Instanz für die Teilnahme am GTNet-Netzwerk.

### Voraussetzungen
Für die Teilnahme am GTNet muss der eigene Grafioschtrader-Server von aussen über HTTPS erreichbar sein. Stellen Sie sicher, dass Ihr Server für die Maschine-zu-Maschine-Kommunikation (M2M) korrekt konfiguriert ist. Bei Problemen mit der externen Erreichbarkeit konsultieren Sie den [Leitfaden zur Problemlösung](https://github.com/grafioschtrader/grafioschtrader/wiki/Problem-solving) im Wiki.

### Schritt 1: GTNet in den globalen Einstellungen aktivieren
Aktivieren Sie zunächst die GTNet-Funktionalität über die [Globalen Einstellungen GTNet](globalsettings/):
- Setzen Sie **gt.gtnet.use** auf einen Wert grösser als 0, um GTNet zu aktivieren
- Optional können Sie **gt.gtnet.use.log** aktivieren, um den Datenaustausch zu protokollieren

### Schritt 2: Eigene Instanz registrieren
Bevor die Kommunikation mit anderen Teilnehmern möglich ist, müssen Sie Ihren eigenen Server in der Ansicht [GTNet Setup](setup/) registrieren:
1. Öffnen Sie die GTNet Setup-Ansicht
2. Erstellen Sie einen Eintrag für Ihren eigenen Server mit Ihrer Domain-URL
3. Nach dem Speichern wird Ihre Instanz-ID in der globalen Einstellung **gt.gtnet.my.entry.id** gespeichert

### Schritt 3: Remote-Instanzen hinzufügen
Fügen Sie eine oder mehrere entfernte GTNet-Instanzen hinzu, um die Kommunikation aufzubauen:
1. Fügen Sie in der Ansicht [GTNet Setup](setup/) Einträge für entfernte Server hinzu
2. **Empfehlung**: Bevorzugen Sie **PUSH_OPEN**-Server, da diese einen aktiven Datenpool pflegen und bidirektionalen Austausch ohne lokale Instrumente ermöglichen
3. Konfigurieren Sie das tägliche Anfragelimit und andere Einstellungen nach Bedarf

### Schritt 4: Erste Verbindung herstellen
Fordern Sie direkt den Datenaustausch an, wodurch der Handshake-Prozess automatisch durchgeführt wird:
1. Wählen Sie den Eintrag des entfernten Servers aus
2. Verwenden Sie das Kontextmenü, um eine **Anfrage für Datenaustausch**-Nachricht zu senden
3. Das System führt automatisch den Handshake und den Token-Austausch durch, falls dies der erste Kontakt ist
4. Warten Sie auf die Antwort (Annahme oder Ablehnung)
5. Nach der Annahme wird der Datenaustausch aktiviert und die Authentifizierungstoken werden für zukünftige Kommunikation gespeichert

### Schritt 5: Automatische Antworten konfigurieren (Empfohlen)
Um nicht jede eingehende Anfrage manuell beantworten zu müssen, konfigurieren Sie automatische Antwortregeln in der Ansicht [Automatische Antwort](autoanswer/):
1. Definieren Sie Regeln für verschiedene Nachrichtentypen (z.B. Erstkontakt, Datenaustausch-Anfragen)
2. Legen Sie Bedingungen fest, z.B. Tageszeit, tägliche Anfragezahl oder Domain-Muster
3. Bestimmen Sie, ob Anfragen automatisch akzeptiert oder abgelehnt werden sollen
4. Konfigurieren Sie Wartezeiten nach Ablehnungen, um wiederholte Anfragen zu verhindern

{{% notice tip %}}
Ohne automatische Antwortregeln erfordert jede eingehende Anfrage eine manuelle Genehmigung durch den Administrator. Das Einrichten geeigneter Regeln reduziert den administrativen Aufwand erheblich.
{{% /notice %}}

### Schritt 6: Wertpapiere für den Austausch konfigurieren
Legen Sie fest, welche Instrumente am Datenaustausch teilnehmen sollen:
1. Öffnen Sie die Ansicht [Kursdaten austauschen](exchange/)
2. Konfigurieren Sie für jedes Wertpapier oder Währungspaar die vier Austauschoptionen:
   - Intraday-Kurse empfangen
   - Historische Kurse empfangen
   - Intraday-Kurse senden
   - Historische Kurse senden
3. Speichern Sie Ihre Änderungen

---

## Wo GTNet verwendet wird
GTNet ist in verschiedene Teile der Anwendung integriert, sowohl automatisch durch das System als auch durch Benutzeraktionen.

### Systemgesteuerte Verwendung
{{< mermaid >}}
graph LR;
    A[Anwendungsstart] -->|Broadcast| B[Server Online Nachricht]
    C[Anwendungsbeendigung] -->|Broadcast| D[Server Offline Nachricht]
    E[Einstellungen geändert] -->|Broadcast| F[Einstellungen aktualisiert Nachricht]
{{< /mermaid >}}

- **Anwendungsstart**: Wenn der GT-Server mit aktiviertem GTNet startet, wird automatisch ein «Online»-Status an alle verbundenen Peers gesendet
- **Anwendungsbeendigung**: Bei ordnungsgemässem Herunterfahren wird ein «Offline»-Status gesendet, um Peers zu informieren
- **Einstellungsänderungen**: Konfigurationsänderungen werden automatisch mit verbundenen Instanzen synchronisiert

### Benutzergesteuerte Verwendung
- **Watchlist-Aktualisierung**: Beim Aktualisieren einer Watchlist können Instrumente, die für den GTNet-Austausch konfiguriert sind, Intraday-Kurse von verbundenen Peers empfangen. Siehe [Letzter Kurs Austausch](setup/lastprice/) für Details.
- **Laden historischer Daten**: Beim Laden historischer Kursdaten kann GTNet Lücken aus Peer-Instanzen füllen. Siehe [Historischer Kursaustausch](setup/historicalprice/) für Details.
- **Wertpapiererstellung**: Beim Erstellen neuer Wertpapiere ermöglicht die [GTNet Wertpapiersuche](../../watchlistinstrument/instrument/securityderived/#gt-net-wertpapiersuche) das Importieren von Wertpapier-Metadaten und Konnektoreinstellungen aus anderen GTNet-Instanzen.

---

## GTNet überwachen
GTNet bietet mehrere Möglichkeiten, den Status und die Aktivität des Datenaustauschs zu überwachen.

### Statische Übersicht: Kursdaten austauschen
Die Ansicht [Kursdaten austauschen](exchange/) bietet eine statische Übersicht darüber, welche Instrumente für den Austausch konfiguriert sind und welche OPEN-Instanzen Daten liefern können:
- Alle Wertpapiere und Währungspaare mit ihren Austauscheinstellungen anzeigen
- Zeilen erweitern, um Lieferantendetails zu sehen (welche Peers Daten für jedes Instrument liefern können)
- Nützlich zum Verstehen des aktuellen Konfigurationsstatus

### Dynamische Ansicht: Austauschprotokoll
Die Ansicht [Austauschprotokoll](exchangelog/) bietet eine dynamische, zeitbasierte Übersicht über die tatsächliche Austauschaktivität:
- Statistiken für Intraday- und historischen Kursaustausch einsehen
- Lieferanten- und Verbraucherstatistiken pro verbundener Instanz verfolgen
- Aggregierte Daten nach Tag, Woche, Monat und Jahr anzeigen
- Erfordert die Aktivierung der Protokollierung über **gt.gtnet.use.log** in den [Globalen Einstellungen](globalsettings/)

### Hintergrundaufgaben
Mehrere Hintergrundaufgaben erledigen GTNet-Operationen automatisch. Diese können im [Aufgaben-Datenänderungsmonitor](../taskdatachangemonitor/) überwacht werden:

| Aufgaben-ID | Name | Beschreibung |
|-------------|------|--------------|
| 20 | Online-Status verfolgen | Prüft und aktualisiert den Online-/Ausgelastet-Status aller konfigurierten GTNet-Server |
| 22 | Austauschprotokolle aggregieren | Fasst Austauschprotokolleinträge zu täglichen, wöchentlichen, monatlichen und jährlichen Perioden zusammen |
| 23 | Konfigurationen synchronisieren | Synchronisiert Austauschkonfigurationen mit verbundenen Peers und aktualisiert Lieferantendetails |
| 24 | Einstellungsänderungen übertragen | Sendet Einstellungsaktualisierungen an alle verbundenen Peers bei lokalen Änderungen |
| 25 | Zukünftige Nachrichten zustellen | Verarbeitet geplante Nachrichten wie Wartungsankündigungen |

Für detaillierte Beschreibungen jeder Aufgabe siehe [Hintergrundaufgaben](../taskdatachangemonitor/taskdescription/).

---

## Einrichtungsseiten
Die detaillierte Einrichtung erfolgt über die folgenden Unterseiten:
