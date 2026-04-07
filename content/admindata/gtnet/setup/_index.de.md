---
title: "GTNet und Nachrichten"
date: 2026-03-29T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GTNet ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
In dieser Ansicht werden alle bekannten GTNet-Instanzen verwaltet und die Kommunikation mit diesen gesteuert.

### Einrichtung des eigenen Servers
Bevor eine Kommunikation mit anderen Teilnehmern möglich ist, muss der eigene GT-Server eingetragen werden. Dabei ist wichtig, dass der Server von aussen erreichbar ist. Nach der Erfassung des eigenen Servers wird dessen ID als Referenz gespeichert. Anschliessend können weitere erreichbare Instanzen für die Kommunikation erfasst werden.

### Server-Übersicht
Die Tabelle zeigt alle bekannten GTNet-Instanzen mit folgenden Informationen:
- **Domain**: Die Basis-URL der Instanz für die M2M-Kommunikation.
- **Zeitzone**: Die Zeitzone des Servers, hilft bei der Einschätzung der Betriebszeiten.
- **Server Online**: Zeigt den aktuellen Online-Status der Instanz an (Online, Offline oder Unbekannt).
- **Nutzung eingeschränkt**: Gibt an, ob die Instanz derzeit voll ausgelastet ist und keine Datenanfragen bearbeiten kann.
- **Austausch Letzter Preis**: Status des Intraday-Kursaustauschs mit dieser Instanz.
- **Austausch Entität**: Status des historischen Datenaustauschs mit dieser Instanz.

### Unterschied zwischen «Nutzung eingeschränkt» und «Offline»
Diese beiden Zustände unterscheiden sich grundlegend in ihrer Auswirkung auf die Kommunikation.

**Nutzung eingeschränkt** bedeutet, dass der Server erreichbar ist, aber aufgrund hoher Auslastung keine Datenanfragen bearbeiten kann. In diesem Zustand werden keine Anfragen für Intraday-Kurse oder historische Kurse an diesen Server gesendet. Statusmeldungen wie «Wartung geplant», «Betrieb eingestellt» oder «Einstellungen aktualisiert» werden jedoch weiterhin übertragen. Der Server kann eingehende Nachrichten empfangen und darauf antworten. Diese Einstellung kann über die Benutzeroberfläche für den eigenen Server aktiviert werden, um bei Kapazitätsengpässen die Last zu reduzieren.

**Offline** bedeutet, dass der Server nicht erreichbar ist. Es findet keinerlei Kommunikation statt. Dieser Status wird automatisch erkannt, wenn der Server nicht antwortet, oder kann durch Senden einer «Server ist jetzt offline»-Nachricht vor dem Herunterfahren signalisiert werden.

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
GTNet unterstützt zwei Arten von Preisdaten für den Austausch. Die detaillierten Abläufe sind auf separaten Seiten beschrieben:
- **[Austausch letzter Preis](lastprice/)**: Aktuelle Tageskurse für Wertpapiere und Währungspaare. Wird über die Watchlist ausgelöst.
- **[Austausch historische Kurse](historicalprice/)**: Tägliche Schlusskurse für die Vergangenheit. Ermöglicht das Auffüllen von Datenlücken.

### Server-Konfiguration
Für jeden Server können verschiedene Einstellungen vorgenommen werden:
- **Serverliste weitergeben**: Legt fest, ob die eigene Serverliste an Dritte weitergegeben werden darf, um die Netzwerk-Entdeckung zu fördern.
- **Tägliches Anfragelimit**: Maximale Anzahl von Datenanfragen, die dieser Server pro Tag an die eigene Instanz stellen darf.
- **Server-Erstellung erlauben**: Bestimmt, ob unbekannte Server beim ersten Handshake automatisch zur Liste hinzugefügt werden dürfen.

### Export und Import der GTNet-Daten
Beim Migrieren einer Datenbank von einem Server auf einen anderen wird die GTNet-Identität (Authentifizierungstoken, Peer-Verbindungen, Nachrichtenhistorie) überschrieben. Um den bestehenden GTNet-Zustand auf dem neuen Server wiederherzustellen, bietet GTNet eine Export- und Import-Funktion.

Über das Kontextmenü der Server-Übersicht (nicht zeilenabhängig) steht der Menüpunkt **«GTNet-Daten exportieren»** zur Verfügung. Diese Funktion erzeugt eine SQL-Datei namens `gtnet_export.sql`, die alle relevanten GTNet-Tabellen als DELETE- und INSERT-Anweisungen enthält. Exportiert werden die Kernkonfiguration, also Verbindungen, Token, Nachrichtenhistorie, Austauscheinstellungen und Austauschprotokolle.

Zum Wiederherstellen wird über denselben Kontextmenüpunkt **«GTNet-Daten importieren»** ein Datei-Upload-Dialog geöffnet, in dem die zuvor exportierte `.sql`-Datei ausgewählt werden kann. Nach erfolgreichem Import plant das System automatisch zwei Hintergrundaufgaben, die etwa fünf Minuten später ausgeführt werden: eine Konfigurationssynchronisation mit den verbundenen Peers sowie eine Überprüfung des Server-Status. Abgeleitete Daten wie Instrumenten-Pool, Lieferantendetails und Preisdaten-Pool werden dabei vom Import nicht erfasst, sondern durch diese Hintergrundaufgaben automatisch neu aufgebaut. Dadurch bleibt die Exportdatei kompakt und die abgeleiteten Daten entsprechen dem tatsächlichen Zustand des neuen Servers.

{{% notice note %}}
Export und Import sind ausschliesslich für Administratoren verfügbar.
{{% /notice %}}

### Nachrichten
Der untere Bereich der Ansicht zeigt die Nachrichtenhistorie mit der ausgewählten Instanz. Die Nachrichten werden in einer Hierarchie dargestellt, wobei zusammengehörige Nachrichten gruppiert werden. Jede Nachricht zeigt Richtung (gesendet/empfangen), Nachrichtentyp und Status.

### Nachrichtenkategorien
GTNet verwendet ein nachrichtenbasiertes Kommunikationsprotokoll, bei dem jede Nachricht einer von drei Kategorien angehört:

**Anfragen** sind Nachrichten, die eine Antwort vom Empfänger erfordern. Der Absender erhält den Status «Antwort erwartet», bis der Empfänger reagiert. Zu den Anfragen gehören «Erste Kontaktaufnahme», «Token-Erneuerung», «Abfrage dieser Serverliste» und «Anfrage für Datenaustausch».

**Antworten** sind Reaktionen auf eingegangene Anfragen. Je nach Anfrage kann die Antwort eine Annahme oder eine Ablehnung sein. Beispielsweise wird auf eine «Erste Kontaktaufnahme» entweder mit «Kontaktaufnahme erfolgreich» oder «Kontaktaufnahme abgelehnt» geantwortet.

**Ankündigungen** sind einseitige Mitteilungen, die keine Antwort erfordern. Dazu gehören «Server ist jetzt offline», «Einstellungen aktualisiert» sowie die Wartungs- und Betriebseinstellungsmeldungen. Ankündigungen werden je nach Typ an alle verbundenen Peers gleichzeitig gesendet.

### Lebenszyklus einer Peer-Verbindung
Die Beziehung zwischen zwei GTNet-Instanzen durchläuft verschiedene Zustände. Das folgende Diagramm zeigt den typischen Lebenszyklus:
{{< mermaid >}}
stateDiagram-v2
    [*] --> KeineVerbindung
    KeineVerbindung --> HandshakeAusstehend: Erste Kontaktaufnahme
    HandshakeAusstehend --> Verbunden: Kontaktaufnahme erfolgreich
    HandshakeAusstehend --> KeineVerbindung: Kontaktaufnahme abgelehnt
    Verbunden --> Verbunden: Token-Erneuerung
    Verbunden --> DatenaustauschAktiv: Datenanfrage angenommen
    DatenaustauschAktiv --> Verbunden: Datenaustausch widerrufen
    DatenaustauschAktiv --> DatenaustauschAktiv: Token-Erneuerung

    state KeineVerbindung: Keine Verbindung
    state HandshakeAusstehend: Handshake ausstehend
    state DatenaustauschAktiv: Datenaustausch aktiv
{{< /mermaid >}}

Nach der ersten Kontaktaufnahme und einer erfolgreichen Antwort werden Authentifizierungstoken ausgetauscht und die Verbindung ist hergestellt. Ab diesem Zeitpunkt kann ein Datenaustausch angefragt werden. Wird die Anfrage akzeptiert, erfolgt der automatische Austausch von Preisdaten. Der Datenaustausch kann jederzeit durch eine «Datenaustausch widerrufen»-Nachricht beendet werden. Token können bei bestehender Verbindung jederzeit erneuert werden.

### Verfügbare Nachrichten nach Zustand
Welche Nachrichten über das Kontextmenü versendet werden können, hängt vom aktuellen Zustand der Beziehung mit dem ausgewählten Peer ab.

Wenn **keine Verbindung** besteht (kein Handshake durchgeführt), steht nur die «Erste Kontaktaufnahme» als gezielte Nachricht zur Verfügung.

Bei einer **bestehenden Verbindung** können folgende Nachrichten gesendet werden: «Token-Erneuerung», «Abfrage dieser Serverliste» und «Admin-Nachricht». Zusätzlich ist «Anfrage für Datenaustausch» verfügbar, sofern der Zielserver Anfragen akzeptiert und noch kein aktiver Austausch besteht. Umgekehrt erscheint «Datenaustausch widerrufen» nur dann, wenn bereits mindestens ein aktiver Austausch existiert. Falls die Serverliste zuvor geteilt wurde, kann diese Berechtigung mit «Meine Serverliste darf nicht mehr abgefragt werden» widerrufen werden.

Für **Broadcast-Nachrichten** (an alle verbundenen Peers gleichzeitig) stehen die folgenden Optionen zur Verfügung: «Server ist jetzt offline» ist immer verfügbar. «Wartungsmodus» erscheint nur, wenn keine offene Wartungsankündigung vorhanden ist. «Betrieb eingestellt» erscheint nur, wenn keine offene Betriebseinstellung vorhanden ist.

### Anfrage-Antwort-Paare
Das folgende Diagramm zeigt die möglichen Antworten auf jede Anfrage:
{{< mermaid >}}
flowchart LR
    A["Erste Kontaktaufnahme"] -->|Erfolgreich| B["Verbindung hergestellt"]
    A -->|Abgelehnt| C["Keine Verbindung"]
    D["Token-Erneuerung"] -->|Akzeptiert| E["Token aktualisiert"]
    D -->|Abgelehnt| F["Bisheriger Token bleibt"]
    G["Abfrage Serverliste"] -->|Erlaubt| H["Liste geteilt"]
    G -->|Nicht erlaubt| I["Nicht geteilt"]
    J["Anfrage Datenaustausch"] -->|Angenommen| K["Austausch aktiv"]
    J -->|Abgelehnt| L["Kein Austausch"]
{{< /mermaid >}}

### Widerrufbare Ankündigungen
Die Ankündigungen «Wartungsmodus» und «Betrieb eingestellt» können nach dem Versenden wieder aufgehoben werden. Solange eine solche Ankündigung offen ist, bietet das Kontextmenü anstelle einer neuen Ankündigung desselben Typs die entsprechende Absage-Option an: «Wartung abgesagt» beziehungsweise «Betriebseinstellung abgesagt». Damit werden die verbundenen Peers darüber informiert, dass der angekündigte Zustand nicht mehr gilt.

### Automatische Antworten
Ohne konfigurierte Regeln erfordert jede eingehende Anfrage eine manuelle Genehmigung durch den Administrator. Um den Betrieb zu vereinfachen, können in der Ansicht [Automatische Antwort](../autoanswer/) regelbasierte Antworten eingerichtet werden. Diese Regeln verwenden Bedingungsausdrücke, die Variablen wie Tageszeit, Zeitzonenunterschied oder Anzahl bestehender Verbindungen auswerten. Wird keine Regel ausgelöst, bleibt die Anfrage zur manuellen Prüfung stehen.

### Versenden von Nachrichten
Über das Kontextmenü können Nachrichten an die ausgewählte Instanz gesendet werden. Die verfügbaren Optionen hängen vom Verbindungsstatus und vorherigen Nachrichten ab, wie oben unter «Verfügbare Nachrichten nach Zustand» beschrieben. Bei Anfragen, die eine Antwort erfordern, wird der Status «Antwort erwartet» angezeigt. Administrative Freitext-Nachrichten werden über den separaten Reiter [Admin-Nachrichten](msgadmin/) verwaltet, der eine eigene Konversationsführung mit Sichtbarkeitssteuerung bietet.
