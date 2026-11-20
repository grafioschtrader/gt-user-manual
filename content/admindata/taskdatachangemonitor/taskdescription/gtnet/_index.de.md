---
title: "GTNet"
date: 2026-10-16T12:00:00+02:00
draft: false
weight: 10
archetype: "default"
---
Die folgenden Hintergrundaufgaben halten die Verbindung zu den anderen GTNet-Instanzen aufrecht und pflegen die dabei entstehenden Daten. Die Aufgabe 20 prüft nach dem Start der Anwendung, welche Instanzen erreichbar sind. Die Aufgaben 23 und 24 gleichen die Austauschkonfigurationen und die Einstellungen mit den Partnern ab, die Aufgaben 25 und 26 stellen Nachrichten im Hintergrund zu, und die Aufgabe 22 verdichtet die Austauschprotokolle und löscht alte Nachrichten. Die Aufgabe 51 nutzt GTNet, um Wertpapiere für Importpositionen zu finden. Wie GTNet als Ganzes funktioniert, ist unter [GTNet]({{% relref "/admindata/gtnet" %}}) beschrieben.

## 20 - Online-Status Updates der GTNet Instanzen nachführen {#JOB20}
Dieser Aufgabentyp prüft und aktualisiert den Online- und Beschäftigt-Status aller konfigurierten GTNet-Server. Für jeden Peer mit abgeschlossenem Handshake wird eine Ping-Nachricht gesendet und die Erreichbarkeit anhand der Antwort ermittelt. Anschliessend werden die Flags für Online- und Beschäftigt-Status sowie der Server-Status der einzelnen Austauschkategorien entsprechend aktualisiert. Übersprungen werden dabei Peers, die ausser Betrieb sind, sowie Peers, die sich gerade in einem angekündigten Wartungsfenster befinden: Erstere sollen durch einen zufällig erfolgreichen Ping nicht wieder in Betrieb genommen werden, bei Letzteren würde eine geplante Abschaltung sonst als Ausfall festgehalten. Die Aufgabe wird rund 30 Sekunden nach dem Start der Anwendung einmalig ausgeführt, damit der Server zuvor vollständig initialisiert und für HTTP-Anfragen bereit ist.

Damit der angezeigte Online-Status die tatsächliche Erreichbarkeit der GT-Applikation widerspiegelt, gelten folgende Regeln:
- **Online**: Der Ping wurde erfolgreich zugestellt und der Peer hat mit einer gültigen GTNet-Protokollantwort geantwortet. Eine offene TCP-Verbindung allein reicht nicht aus – HTTP-Fehlerantworten eines vorgeschalteten Reverse-Proxys (beispielsweise 502 oder 503) führen zu Offline.
- **Offline**: Der Peer ist nicht erreichbar oder antwortet nur mit einem HTTP-Fehler.
- **Unbekannt**: Der ausgehende Handshake mit dem Peer ist nicht abgeschlossen (Token für ausgehende Anfragen fehlt). Ohne dieses Token kann kein authentifizierter Ping gesendet werden; ein allenfalls noch gesetzter «Online»-Status aus einer eingehenden Handshake-Phase wird deshalb auf «Unbekannt» zurückgesetzt.

Zusätzlich kann eine administrative Person den Status einer einzelnen Instanz jederzeit über den Kontextmenüpunkt **«Status jetzt prüfen»** in der [GTNet-Server-Übersicht](../../../gtnet/setup/) neu prüfen lassen, ohne auf einen Serverneustart warten zu müssen.

## 22 - GTNet-Austauschprotokolle aggregieren und alte Nachrichten löschen {#JOB22}
Dieser Aufgabentyp führt zwei Wartungsaufgaben durch: die Aggregation von Protokolleinträgen und das Löschen alter Austauschnachrichten.

**Protokollaggregation**: Die GTNet-Austauschprotokolleinträge werden rollierend von kürzeren zu längeren Zeiträumen aggregiert. Die Aggregation erfolgt in mehreren Stufen: Einzelne Einträge werden zu täglichen Zusammenfassungen aggregiert, tägliche zu wöchentlichen, wöchentliche zu monatlichen und monatliche zu jährlichen. Der globale Parameter **g.gnet.log.aggregate.days** steuert die Schwellenwerte für jede Stufe im Format «D=1,W=7,M=30,Y=365».

**Nachrichtenlöschung**: Alte Austauschnachrichten werden automatisch gelöscht, um den Speicherbedarf zu reduzieren. Der globale Parameter **g.gnet.del.message.recv** steuert die Aufbewahrungsdauer im Format «LP=1,HP=5,SL=5,SS=10»:
- **LP** (LastPrice): Anzahl Tage, bevor Letztpreis-Austauschnachrichten gelöscht werden
- **HP** (HistoryPrice): Anzahl Tage, bevor historische Preisaustauschnachrichten gelöscht werden
- **SL** (SecurityLookup): Anzahl Tage, bevor Wertpapiersuche-Nachrichten gelöscht werden
- **SS** (ServerStatus): Anzahl Tage, bevor die Server-Statusmeldungen «Server ist jetzt offline» und «Einstellungen aktualisiert» gelöscht werden

Der Standardzeitplan läuft täglich um 3:00 Uhr UTC. Die aggregierten Daten werden in der Ansicht [Austauschprotokoll](../../../../admindata/gtnet/exchangelog/) angezeigt. Weitere Informationen zu den Konfigurationsparametern finden Sie unter [Globale Einstellungen GTNet](../../../../admindata/gtnet/globalsettings/).

## 23 - GTNet-Austauschkonfigurationen synchronisieren {#JOB23}
Dieser Aufgabentyp synchronisiert GTNetExchange-Konfigurationen mit verbundenen GTNet-Peers und aktualisiert die GTNetSupplierDetail-Einträge. Er arbeitet in zwei Modi: Der vollständige Neuerstellungsmodus erstellt alle Lieferantendetaileinträge für jeden Peer neu, während der inkrementelle Modus nur Änderungen seit der letzten Synchronisation synchronisiert. Der Standardzeitplan läuft täglich um 2:00 Uhr UTC im vollständigen Neuerstellungsmodus. Diese Aufgabe kann auch manuell über die [GTNet-Einrichtung](../../../../admindata/gtnet/setup/) ausgelöst werden oder automatisch, nachdem eine Datenaustausch-Anforderung akzeptiert wurde.

## 24 - GTNet-Einstellungsänderungen übertragen {#JOB24}
Dieser Aufgabentyp überträgt Einstellungsänderungen an alle konfigurierten GTNet-Peers. Wenn sich lokale GTNet-Entitätseinstellungen ändern (wie maxLimit, acceptRequest, serverState oder dailyRequestLimit), benachrichtigt diese Aufgabe alle verbundenen Peers durch Senden einer Einstellungsaktualisierungsnachricht. Diese Aufgabe ist nicht geplant, sondern wird automatisch ausgelöst, wenn GTNet-Einstellungen gespeichert werden. Die Ausführung als Hintergrundaufgabe verhindert, dass die Benutzeroberfläche bei Netzwerkoperationen während der Einstellungsänderung blockiert wird.

## 25 - Ausstehende GTNet-Zukunftsnachrichten zustellen {#JOB25}
Dieser Aufgabentyp verarbeitet die Zustellung von Broadcast-Nachrichten, die ein geplantes Ausführungsdatum in der Zukunft haben. Dazu gehören Wartungsfenster-Ankündigungen, Hinweise zur Servereinstellung und deren entsprechende Stornierungsnachrichten. Die Aufgabe führt fünf Operationen aus: Erstellen von Zustellversuchen für neue Partner, deren Handshake nach Erstellung einer ausstehenden Nachricht abgeschlossen wurde, Verarbeiten von Stornierungsnachrichten, Zustellen ausstehender Nachrichten an ihre Ziele, Bereinigen abgelaufener Nachrichten und schliesslich das Setzen jener Instanzen auf «Ausser Betrieb», deren angekündigtes Datum der Betriebseinstellung erreicht ist. Der Standardzeitplan läuft alle 5 Stunden, aber die Aufgabe wird auch sofort ausgelöst, wenn einer dieser Nachrichtentypen gesendet wird; die Umstellung auf «Ausser Betrieb» erfolgt deshalb innerhalb weniger Stunden nach dem angekündigten Datum. Weitere Informationen zu GTNet-Nachrichtentypen finden Sie unter [GTNet-Einrichtung](../../../../admindata/gtnet/setup/), zu den beiden Ankündigungen unter [Wartung und Betriebseinstellung](../../../../admindata/gtnet/setup/availability/).

## 26 - GTNet-Admin-Nachrichten an Ziele zustellen {#JOB26}
Dieser Aufgabentyp stellt ausstehende Admin-Nachrichten an mehrere GTNet-Ziele zu. Die Aufgabe wird automatisch ausgelöst, wenn ein Administrator über die [Admin-Nachrichten](../../../../admindata/gtnet/setup/msgadmin/)-Ansicht eine Nachricht an mehrere ausgewählte Domänen sendet. Durch die asynchrone Verarbeitung im Hintergrund reagiert die Benutzeroberfläche sofort, während die Zustellung separat erfolgt.

Die Aufgabe führt folgende Schritte aus:
- Abfrage aller ausstehenden Zustellversuche für Admin-Nachrichten
- Zustellung der Nachrichten an die jeweiligen Ziele über die M2M-Schnittstelle
- Aktualisierung des Zustellstatus nach erfolgreicher Übertragung
- Protokollierung von fehlgeschlagenen Zustellversuchen

{{% callout info %}}
Diese Aufgabe kann nicht manuell erstellt werden. Sie wird ausschliesslich vom System erstellt, wenn Admin-Nachrichten an mehrere Empfänger gesendet werden.
{{% /callout %}}

## 51 - Importiert Wertpapiere von GTNet für Importpositionen {#JOB51}
Diese Aufgabe entsteht, wenn im [GTNet-Wertpapierimport]({{% relref "/basedata/gtnetsecurityimport" %}}) über das Kontextmenü **Wertpapiere von GTNet importieren** die Abfrage gestartet wird; für dieselbe Importliste wartet höchstens eine Aufgabe. Sie fragt für jede Position ohne verknüpftes Wertpapier die erreichbaren GTNet-Instanzen ab, wählt den besten Treffer und verknüpft damit ein bereits vorhandenes Wertpapier oder erstellt ein neues. Wird die Abfrage aus dem Transaktionsimport heraus gestartet, erhalten auch die passenden Importpositionen der Transaktionen das Wertpapier, erkannt an der ISIN oder am Tickersymbol. Für jedes neu erstellte Wertpapier lädt die Aufgabe gleich anschliessend die historischen Kursdaten, und zwar von der Instanz mit der längsten Kursreihe; liefert keine Instanz Kurse, wird der Konnektor des Wertpapiers verwendet. Neu erstellte Wertpapiere zählen zum täglichen Limit des Benutzers, das Verknüpfen vorhandener Wertpapiere nicht.
