---
title: "GT-Netz und Nachrichten"
date: 2026-01-05T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GT-Net ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
In dieser Ansicht werden alle bekannten GT-Net-Instanzen verwaltet und die Kommunikation mit diesen gesteuert.

### Einrichtung des eigenen Servers
Bevor eine Kommunikation mit anderen Teilnehmern möglich ist, muss der eigene GT-Server eingetragen werden. Dabei ist wichtig, dass der Server von aussen erreichbar ist. Nach der Erfassung des eigenen Servers wird dessen ID als Referenz gespeichert. Anschliessend können weitere erreichbare Instanzen für die Kommunikation erfasst werden.

### Server-Übersicht
Die Tabelle zeigt alle bekannten GT-Net-Instanzen mit folgenden Informationen:
- **Domain**: Die Basis-URL der Instanz für die M2M-Kommunikation.
- **Zeitzone**: Die Zeitzone des Servers, hilft bei der Einschätzung der Betriebszeiten.
- **Server Online**: Zeigt den aktuellen Online-Status der Instanz an (Online, Offline oder Unbekannt).
- **Nutzung eingeschränkt**: Gibt an, ob die Instanz derzeit nur eingeschränkt Anfragen akzeptiert.
- **Austausch Letzter Preis**: Status des Intraday-Kursaustauschs mit dieser Instanz.
- **Austausch Entität**: Status des historischen Datenaustauschs mit dieser Instanz.

### Unterschiedliches Verhalten von Offen-Server und Push-Offen-Server
Das Verhalten dieser beiden Server-Typen beim Teilen von Preisdaten unterscheidet sich grundlegend. Um einen regen Austausch nützlicher Preisdaten zu ermöglichen, sollten sich die Instanzen möglichst auf Push-Offen-Server konzentrieren. Das Offen-Server-Konzept ist eher für Spezialfälle gedacht.
{{% notice info %}}
Der Preisaustausch sollte nicht dazu führen, dass die Konnektoren der einzelnen Instrumente nachlässig verwaltet werden. Die Konnektoren bleiben die primäre Datenquelle.
{{% /notice %}}

#### Grundlegende Unterschiede der Server-Typen
| Eigenschaft | Offen-Server | Push-Offen-Server |
|-------------|--------------|-------------------|
| Datenspeicherung | Direkt auf lokalen Instrumenten | Separater Preisdaten-Pool |
| Unabhängigkeit | Benötigt lokale Instrumente | Kann ohne lokale Instrumente betrieben werden |
| Austausch | Kann asymmetrisch sein | Immer bidirektional |
| Ideal für | Spezialfälle, wenige Partner | Zentraler Austauschknoten, viele Partner |

#### Identifikation der Instrumente
- **Wertpapiere**: ISIN + Währungscode
- **Währungspaare**: Basiswährung + Kurswährung

#### Lieferantenauswahl und Lastverteilung
Die Reihenfolge der Server-Abfragen unterscheidet sich je nach Server-Typ:

**Push-Offen-Server** verwenden eine einfache prioritätsbasierte Auswahl:
- Niedrigere Prioritätswerte (consumerUsage) werden zuerst abgefragt.
- Server mit gleicher Priorität werden **zufällig** ausgewählt, um die Last gleichmässig zu verteilen.

**Offen-Server** verwenden eine optimierte punktebasierte Auswahl, um erfolgreiche Datenaustausche zu maximieren:
1. **Primär**: **Punktzahl** = Abdeckung × Erfolgsrate (höher ist besser)
   - *Abdeckung*: Anzahl der angefragten Instrumente, die dieser Lieferant unterstützt (aus GTNetSupplierDetail)
   - *Erfolgsrate*: Verhältnis der erfolgreich aktualisierten Entitäten aus den letzten 30 Tagen (Standard 1.0 wenn keine Historie)
2. **Sekundär**: Priorität (consumerUsage, niedriger ist besser)
3. **Tertiär**: Zufällige Auswahl bei Servern mit gleicher Punktzahl und Priorität

Diese Optimierung stellt sicher, dass Offen-Server mit besserer Instrumentenabdeckung und höheren historischen Erfolgsraten zuerst abgefragt werden, wodurch unnötige Anfragen an Server reduziert werden, die wahrscheinlich keine nützlichen Daten liefern.

{{% notice note %}}
**Zusammenfassung der Kommunikationsszenarien:**
- **Push-Offen ↔ Push-Offen**: Beide nutzen und aktualisieren den gemeinsamen Preispool. Bidirektionaler Austausch.
- **Offen → Push-Offen**: Der Offen-Server erhält Preise aus dem Pool des Push-Offen-Servers und aktualisiert seine lokalen Instrumente.
- **Offen ↔ Offen**: Beide aktualisieren ihre lokalen Instrumente direkt. Asymmetrischer Austausch möglich.
{{% /notice %}}

### Arten des Datenaustauschs
GT-Net unterstützt zwei Arten von Preisdaten für den Austausch. Die detaillierten Abläufe sind auf separaten Seiten beschrieben:
- **[Austausch letzter Preis](lastprice/)**: Aktuelle Tageskurse für Wertpapiere und Währungspaare. Wird über die Watchlist ausgelöst.
- **[Austausch historische Kurse](historicalprice/)**: Tägliche Schlusskurse für die Vergangenheit. Ermöglicht das Auffüllen von Datenlücken.

### Server-Konfiguration
Für jeden Server können verschiedene Einstellungen vorgenommen werden:
- **Serverliste weitergeben**: Legt fest, ob die eigene Serverliste an Dritte weitergegeben werden darf, um die Netzwerk-Entdeckung zu fördern.
- **Tägliches Anfragelimit**: Maximale Anzahl von Datenanfragen, die dieser Server pro Tag an die eigene Instanz stellen darf.
- **Server-Erstellung erlauben**: Bestimmt, ob unbekannte Server beim ersten Handshake automatisch zur Liste hinzugefügt werden dürfen.

### Nachrichten
Der untere Bereich der Ansicht zeigt die Nachrichtenhistorie mit der ausgewählten Instanz. Die Nachrichten werden in einer Hierarchie dargestellt, wobei zusammengehörige Nachrichten gruppiert werden. Jede Nachricht zeigt Richtung (gesendet/empfangen), Nachrichtentyp und Status.

### Nachrichtentypen
GT-Net verwendet verschiedene Nachrichtentypen für unterschiedliche Zwecke:
- **Erste Kontaktaufnahme**: Initialer Handshake zur Verbindungsherstellung.
- **Kontaktaufnahme erfolgreich/abgelehnt**: Antwort auf eine Kontaktanfrage.
- **Abfrage dieser Serverliste**: Anfrage zur Weitergabe bekannter Server.
- **Anfrage für Datenaustausch**: Anforderung zum Austausch von Preisdaten.
- **Server ist jetzt offline/online**: Statusänderung der Verfügbarkeit.
- **Server ist ausgelastet**: Mitteilung über eingeschränkte Kapazität.
- **Einstellungen aktualisiert**: Mitteilung, dass Servereinstellungen (Tägliches Anfragelimit, Anfrage akzeptieren, Server-Status, Max. Limit) geändert wurden.
- **Wartung geplant**: Ankündigung eines geplanten Wartungsfensters.
- **Betrieb eingestellt**: Ankündigung der dauerhaften Einstellung des Dienstes.

### Versenden von Nachrichten
Über das Kontextmenü können Nachrichten an die ausgewählte Instanz gesendet werden. Je nach Verbindungsstatus und vorherigen Nachrichten stehen unterschiedliche Optionen zur Verfügung. Bei Anfragen, die eine Antwort erfordern, wird der Status «Antwort erwartet» angezeigt.
