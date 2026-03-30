---
title: "Austausch letzter Preis"
date: 2026-02-04T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GTNet ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Der Austausch von Intraday-Preisdaten (letzter Preis) ermöglicht es, aktuelle Kursdaten mit anderen GTNet-Instanzen zu teilen und zu empfangen. Der Prozess wird durch die Watchlist ausgelöst und folgt einem definierten Ablauf.

### Auslöser: Die Watchlist
Der Intraday-Preisdatenaustausch wird über die **Watchlist** ausgelöst. Wenn eine Watchlist aktualisiert wird, prüft das System für jedes Instrument, ob es für den GTNet-Empfang konfiguriert ist:
- Nur Instrumente mit aktivierter Option **GTNet Letzter Preis empfangen** werden für den Austausch berücksichtigt.
- Instrumente ohne diese Option werden ausschliesslich über den konfigurierten Konnektor aktualisiert.

### Identifikation der Instrumente
Instrumente werden eindeutig identifiziert durch:
- **Wertpapiere**: ISIN + Währungscode
- **Währungspaare**: Basiswährung + Kurswährung

### Ablauf des Preisaustauschs
Der Austausch folgt einem mehrschrittigen Prozess, bei dem verschiedene Datenquellen nacheinander abgefragt werden:

{{< mermaid >}}
flowchart TD
    A[Watchlist-Aktualisierung] --> B{GTNet aktiviert?}
    B -->|Nein| K[Nur Konnektoren verwenden]
    B -->|Ja| C[Instrumente nach GTNet-Empfang filtern]
    C --> D{Eigener Server Push-Offen?}
    D -->|Ja| E[Eigenen Preispool abfragen]
    E --> F{Alle Preise gefunden?}
    F -->|Ja| L[Fertig]
    F -->|Nein| G[Push-Offen-Server abfragen]
    D -->|Nein| G
    G --> H[Offen-Server abfragen]
    H --> I[Konnektoren für restliche Instrumente]
    I --> J{Eigener Server Push-Offen?}
    J -->|Ja| M[Eigenen Preispool aktualisieren]
    J -->|Nein| N{Eigener Server Offen?}
    M --> L
    N -->|Ja| O[Async: Aktualisierte Preise<br/>an Push-Offen-Server senden]
    N -->|Nein| L
    O --> L
{{< /mermaid >}}

### Detaillierter Ablauf

#### Schritt 1: Filterung nach GTNet-Konfiguration
Das System trennt die Instrumente der Watchlist in zwei Gruppen:
- **GTNet-fähige Instrumente**: Instrumente mit aktivierter Option «GTNet Letzter Preis empfangen»
- **Nur-Konnektor-Instrumente**: Alle anderen Instrumente

#### Schritt 2: Eigenen Preispool abfragen (nur Push-Offen-Server)
Wenn der eigene Server als **Push-Offen** konfiguriert ist, wird zuerst der lokale Preispool abgefragt. Dies ermöglicht schnelle Antworten ohne Netzwerkkommunikation, falls die Preise bereits im Pool vorhanden sind.

#### Schritt 3: Push-Offen-Server abfragen
Für noch nicht gefüllte Instrumente werden alle konfigurierten Push-Offen-Server abgefragt:
- Die Server werden nach **Priorität** sortiert.
- Bei gleicher Priorität wird ein Server **zufällig** ausgewählt (Lastverteilung).
- Jede Anfrage enthält:
  - Liste der Instrumente (ISIN/Währung bzw. Währungspaar)
  - Zeitstempel des letzten bekannten Preises für jedes Instrument
- Die Antwort enthält nur Preise, die **neuer** sind als die in der Anfrage angegebenen.

{{< mermaid >}}
sequenceDiagram
    autonumber
    participant L as Lokaler Server (Offen)
    participant P as Push-Offen-Server
    participant O as Offen-Server
    participant C as Konnektor

    L->>P: Anfrage: Preise für Instrument X, Y, Z + Zeitstempel
    Note over P: Hat neuere Preise für X und Y
    P-->>L: Antwort: Preise für X und Y
    Note over L: X und Y aktualisiert,<br/>Z noch offen.<br/>Zeitstempel von P merken.
    L->>O: Anfrage: Preis für Instrument Z + Zeitstempel
    Note over O: Hat keinen neueren Preis
    O-->>L: Antwort: Keine neueren Daten
    Note over L: Z noch nicht gefüllt
    L->>C: Fallback: Preis für Z abrufen
    C-->>L: Aktueller Preis für Z
    Note over L: Z aktualisiert.<br/>Ergebnis an Frontend senden.
    rect rgb(230, 245, 255)
    Note over L,P: Asynchroner Push-Back (nur Offen-Server)
    L-)P: Push: Neuere Preise für Z
    Note over P: Preispool aktualisiert
    P--)L: Bestätigung
    end
{{< /mermaid >}}

#### Schritt 4: Offen-Server abfragen
Für Instrumente, die noch keinen aktuellen Preis haben, werden anschliessend die Offen-Server abgefragt. Diese werden nach einem **Score** sortiert, der sich aus Abdeckung (Coverage) multipliziert mit der Erfolgsrate berechnet. Server mit höherem Score werden zuerst abgefragt. Bei gleichem Score wird nach Priorität (niedrigere zuerst) und dann zufällig sortiert. Die Erfolgsrate wird aus den Austauschprotokollen der letzten 30 Tage ermittelt (Standard: 1.0 wenn keine Historie vorhanden). Offen-Server:
- Lesen aus ihren lokalen Instrumenten (kein separater Pool).
- Die Anfrage wird gefiltert: Es werden nur Instrumente gesendet, die der jeweilige Server unterstützt.
- Der Austausch kann **asymmetrisch** sein (abhängig von der Konfiguration «Austausch Preisdaten Wertpapiere»).

#### Schritt 5: Konnektoren für verbleibende Instrumente
Instrumente, die weder vom eigenen Pool noch von Remote-Servern Preise erhalten haben, werden über ihre konfigurierten **Konnektoren** (Datenquellen wie Yahoo, Finnhub usw.) aktualisiert.

#### Schritt 6: Eigenen Preispool aktualisieren (nur Push-Offen-Server)
Wenn der eigene Server als Push-Offen konfiguriert ist, werden die über Konnektoren erhaltenen Preise im lokalen Preispool gespeichert. Dadurch stehen diese Preise für zukünftige Anfragen von anderen Instanzen zur Verfügung.

#### Schritt 7: Asynchrones Zurücksenden an Push-Offen-Server (nur Offen-Server)
Wenn der eigene Server als **Offen** (nicht Push-Offen) konfiguriert ist, sendet er nach Abschluss des Austauschs **asynchron** aktualisierte Preise an alle zuvor kontaktierten Push-Offen-Server zurück. Dieser Vorgang:
- Läuft im Hintergrund und blockiert nicht die Antwort an das Frontend.
- Sendet nur Preise, die **neuer** sind als die ursprünglich vom Push-Offen-Server empfangenen.
- Ermöglicht es Push-Offen-Servern, ihren Preispool mit Daten anzureichern, die sie selbst nicht haben.
- Erstellt automatisch neue Instrument-Einträge im Preispool des Push-Offen-Servers, falls diese noch nicht existieren.

{{% notice tip %}}
Dieser bidirektionale Datenfluss sorgt dafür, dass Push-Offen-Server von den Konnektoren der Offen-Server profitieren können. Ein Offen-Server, der Preise über seinen Konnektor erhält, teilt diese automatisch mit den Push-Offen-Servern, von denen er zuvor Daten angefragt hat.
{{% /notice %}}

### Unterschiede zwischen Server-Typen

| Aspekt | Push-Offen-Server | Offen-Server |
|--------|-------------------|--------------|
| Datenspeicherung | Separater Preispool | Direkt auf lokalen Instrumenten |
| Austausch | Immer bidirektional | Kann asymmetrisch sein |
| Abfragereihenfolge | Wird zuerst abgefragt | Wird nach Push-Offen abgefragt |
| Sortierung | Priorität + Zufallsauswahl | Score (Coverage × Erfolgsrate) |
| Instrument-Filterung | Alle GTNet-Instrumente | Nur unterstützte Instrumente |
| Eigener Pool | Wird vor Remote-Servern geprüft | Nicht vorhanden |
| Konnektor-Updates | Werden in Pool gespeichert | Nur lokale Instrumente |
| Push-Back | Empfängt Preise von Offen-Servern | Sendet async Preise an kontaktierte Push-Offen-Server |

### Prioritäten und Lastverteilung
Die Reihenfolge der Server-Abfragen unterscheidet sich je nach Server-Typ:

**Push-Offen-Server** werden nach **Priorität** (consumerUsage) sortiert:
- Niedrigere Prioritätswerte werden zuerst abgefragt.
- Server mit gleicher Priorität werden **zufällig** ausgewählt, um die Last gleichmässig zu verteilen.

**Offen-Server** werden nach **Score** sortiert:
- Score = Abdeckung × Erfolgsrate (aus den letzten 30 Tagen)
- Höhere Score-Werte werden zuerst abgefragt.
- Bei gleichem Score wird nach Priorität (niedrigere zuerst) und dann zufällig sortiert.

Die Abfrage wird beendet, sobald alle Instrumente einen aktuellen Preis haben.

{{% notice info %}}
Der Preisaustausch basiert auf Zeitstempelvergleichen. Es wird immer der neuere Preis bevorzugt, unabhängig davon, von welchem Server er stammt.
{{% /notice %}}
