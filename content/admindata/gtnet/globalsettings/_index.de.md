---
title: "Globale Einstellungen GTNet"
date: 2026-01-18T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GTNet ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}

Die GTNet-Funktionalität wird über spezifische globale Parameter gesteuert. Diese Parameter befinden sich in der Ansicht [Globale Einstellungen](../../globalsettings/) und sind nach dem Präfix «gt.gtnet» zu finden.

### Vom Administrator verwaltete Parameter
Diese Parameter können vom Administrator konfiguriert werden, um das Verhalten von GTNet anzupassen.

#### gt.gtnet.use
Aktiviert oder deaktiviert die gesamte GTNet-Funktionalität. Ein Wert von 0 bedeutet deaktiviert, ein Wert ungleich 0 aktiviert GTNet. Standardmässig ist GTNet deaktiviert. Ohne diese Aktivierung findet kein Datenaustausch mit anderen Instanzen statt.

#### gt.gtnet.use.log
Steuert die Protokollierung des Datenaustauschs. Wenn aktiviert (Wert ungleich 0), werden alle Austauschvorgänge zwischen den Instanzen aufgezeichnet. Diese Protokolle können in der Ansicht [Austauschprotokoll](../exchangelog/) eingesehen werden. Die Protokollierung ist standardmässig deaktiviert und kann für Diagnosezwecke oder zur Überwachung der Netzwerkaktivität aktiviert werden.

#### gt.gtnet.lastprice.delay.seconds
Definiert eine zusätzliche Verzögerung in Sekunden für die Berechnung des Frischeschwellenwerts bei Letztpreisanfragen. Dieser Wert wird zusammen mit dem Watchlist-Timeout verwendet, um zu bestimmen, ob ein empfangener Preis noch aktuell genug ist. Preise, die älter als (aktuelle Zeit - Watchlist-Timeout - dieser Verzögerungswert) sind, werden als veraltet abgelehnt. Der Standardwert beträgt 300 Sekunden (5 Minuten).

#### gt.gtnet.del.message.recv
Steuert die Aufbewahrungsdauer für Preisaustauschnachrichten im PropertyString-Format. Das Format ist «LP=Tage,HP=Tage», wobei:
- **LP** (LastPrice): Anzahl Tage, bevor Letztpreis-Austauschnachrichten (Codes 60, 61) gelöscht werden
- **HP** (HistoryPrice): Anzahl Tage, bevor historische Preisaustauschnachrichten (Codes 80, 81) gelöscht werden

Der gültige Bereich für beide Werte ist 1-10 Tage. Der Standardwert ist «LP=1,HP=5», was bedeutet, dass Letztpreis-Nachrichten nach 1 Tag und historische Preisnachrichten nach 5 Tagen gelöscht werden. Diese Bereinigung wird durch die [Hintergrundaufgabe 22](../../../admindata/taskdatachangemonitor/taskdescription/#22---gtnet-austauschprotokolle-aggregieren-und-alte-nachrichten-löschen) durchgeführt.

#### gt.gtnet.log.aggregate.days
Steuert die Schwellenwerte für die Aggregation von Austauschprotokollen im PropertyString-Format. Das Format ist «D=Tage,W=Tage,M=Tage,Y=Tage», wobei:
- **D**: Anzahl Tage, bevor einzelne Einträge zu täglichen Zusammenfassungen aggregiert werden
- **W**: Anzahl Tage, bevor tägliche Einträge zu wöchentlichen aggregiert werden
- **M**: Anzahl Tage, bevor wöchentliche Einträge zu monatlichen aggregiert werden
- **Y**: Anzahl Tage, bevor monatliche Einträge zu jährlichen aggregiert werden

Der Standardwert ist «D=1,W=7,M=30,Y=365». Diese Aggregation wird durch die [Hintergrundaufgabe 22](../../../admindata/taskdatachangemonitor/taskdescription/#22---gtnet-austauschprotokolle-aggregieren-und-alte-nachrichten-löschen) durchgeführt.

### Vom System verwaltete Parameter
Diese Parameter werden automatisch vom System gesetzt und sollten nicht manuell geändert werden.

#### gt.gtnet.my.entry.id
Enthält die eindeutige ID der eigenen Instanz im GTNet. Dieser Wert wird automatisch gesetzt, wenn der eigene Server in der Ansicht [GTNet und Nachrichten](../setup/) eingetragen wird. Die ID dient als Referenz für die Identifikation der eigenen Instanz im Netzwerk.

#### gt.gtnet.exchange.sync.timestamp
Speichert den Zeitstempel der letzten erfolgreichen Synchronisation der austauschbaren Entitäten. Dieser Wert wird automatisch aktualisiert, nachdem ein Synchronisationsvorgang abgeschlossen wurde. Er wird verwendet, um zu ermitteln, welche Einträge seit der letzten Synchronisation geändert wurden.

---

## Statussynchronisation zwischen Servern
GTNet hält die Serverstatusinformationen automatisch zwischen verbundenen Instanzen synchron. Dieser Abschnitt erklärt, wann Statusänderungen an Remote-Server gesendet werden und wann Updates von Remote-Servern Ihre Instanz erreichen.

### Ausgehende Statusänderungen (Ihr Server → Remote-Server)
Statusänderungen Ihres Servers werden an alle Remote-Server gesendet, mit denen eine etablierte Kommunikation besteht (abgeschlossener Handshake mit ausgetauschten Tokens).

#### Automatische Broadcasts

| Ereignis | Nachrichtentyp | Auslöser |
|----------|----------------|----------|
| Server-Start | Server ist jetzt online | Wenn die Anwendung startet und GTNet aktiviert ist (`gt.gtnet.use` > 0) |
| Server-Stopp | Server ist jetzt offline | Wenn die Anwendung ordnungsgemäss heruntergefahren wird |
| Auslastungsänderung | Server ist ausgelastet / Server ist nicht mehr ausgelastet | Wenn das Kontrollkästchen «Nutzung eingeschränkt» in [GTNet und Nachrichten](../setup/) geändert wird |
| Einstellungsänderung | Einstellungen aktualisiert | Wenn Einstellungen wie Tägliches Anfragelimit, Anfrage akzeptieren, Server-Status oder Max. Limit in [GTNet und Nachrichten](../setup/) geändert werden |

#### Manuelle Broadcasts
Die folgenden Statusnachrichten können manuell über das Kontextmenü in der Ansicht [GTNet und Nachrichten](../setup/) gesendet werden:

| Nachrichtentyp | Zweck |
|----------------|-------|
| Wartung geplant | Ankündigung eines geplanten Wartungsfensters mit Start- und Endzeit |
| Wartung abgesagt | Absage eines zuvor angekündigten Wartungsfensters |
| Betrieb eingestellt | Ankündigung der dauerhaften Einstellung des Dienstes mit Wirksamkeitsdatum |
| Betriebseinstellung abgesagt | Absage einer zuvor angekündigten Einstellung |

### Eingehende Statusänderungen (Remote-Server → Ihr Server)
Ihr Server empfängt Statusaktualisierungen von Remote-Servern auf zwei Arten:

#### 1. Explizite Statusnachrichten
Wenn ein Remote-Server eine Statusänderung sendet (online, offline, ausgelastet, Wartung usw.), empfängt und speichert Ihr Server diese Nachricht. Diese Nachrichten sind in der Nachrichtenhistorie der Ansicht [GTNet und Nachrichten](../setup/) sichtbar.

#### 2. Automatische Einstellungssynchronisation
Jede von einem Remote-Server empfangene Nachricht enthält die aktuelle GTNet-Konfiguration des Absenders im Nachrichtenumschlag. Ihr Server extrahiert und aktualisiert automatisch die Einstellungen des Remote-Servers aus diesem Umschlag, wodurch sichergestellt wird, dass:
- Tägliche Anfragelimits immer aktuell sind
- Anfrage-Akzeptanz-Status (Geschlossen, Offen, Push-Offen) synchronisiert sind
- Server-Status (Keine, Offen, Push-Offen) auf dem neuesten Stand sind
- Maximale Instrumentenlimits aktuell sind

Das bedeutet, dass auch ohne explizite Statusnachrichten Ihre lokale Kopie der Einstellungen eines Remote-Servers durch regelmässige Kommunikation synchron bleibt.

### Server-Lebenszyklus-Ereignisse

#### Anwendungsstart
Wenn Ihr GT-Server startet:
1. Wenn `gt.gtnet.use` aktiviert ist (Wert > 0), wird die GTNet-Funktionalität aktiviert
2. Eine «Server ist jetzt online»-Nachricht wird automatisch an alle verbundenen Peers gesendet
3. Remote-Server aktualisieren ihre Aufzeichnungen, um Ihren Server als online anzuzeigen

#### Anwendungsstopp
Wenn Ihr GT-Server ordnungsgemäss heruntergefahren wird:
1. Eine «Server ist jetzt offline»-Nachricht wird automatisch an alle verbundenen Peers gesendet
2. Remote-Server aktualisieren ihre Aufzeichnungen, um Ihren Server als offline anzuzeigen

{{% notice note %}}
Wenn der Server abstürzt oder gewaltsam beendet wird, kann die Offline-Nachricht nicht gesendet werden. In diesem Fall erkennen Remote-Server die Nichtverfügbarkeit erst, wenn sie versuchen zu kommunizieren und keine Antwort erhalten.
{{% /notice %}}

### Praktische Auswirkungen

#### Für Administratoren
- Änderungen im GTNet und Nachrichten-Bearbeitungsdialog werden sofort an alle verbundenen Peers gesendet
- Keine manuelle Aktion erforderlich, um Peers über Konfigurationsänderungen zu informieren
- Die Nachrichtenhistorie zeigt sowohl gesendete als auch empfangene Statusnachrichten für Prüfzwecke

#### Für Netzwerkteilnehmer
- Ihr Server respektiert automatisch die aktuellen Limits und Status von Remote-Servern
- Wenn ein Remote-Server ausgelastet wird, reduziert Ihr Server die Anfragehäufigkeit
- Wenn ein Remote-Server offline geht, werden keine Anfragen versucht, bis er wieder online ist
