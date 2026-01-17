---
title: "Austausch letzter Preis"
date: 2026-01-05T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GT-Net ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Der Austausch von Intraday-Preisdaten (letzter Preis) ermöglicht es, aktuelle Kursdaten mit anderen GT-Net-Instanzen zu teilen und zu empfangen. Der Prozess wird durch die Watchlist ausgelöst und folgt einem definierten Ablauf.

### Auslöser: Die Watchlist
Der Intraday-Preisdatenaustausch wird über die **Watchlist** ausgelöst. Wenn eine Watchlist aktualisiert wird, prüft das System für jedes Instrument, ob es für den GT-Net-Empfang konfiguriert ist:
- Nur Instrumente mit aktivierter Option **GT-Net Letzter Preis empfangen** werden für den Austausch berücksichtigt.
- Instrumente ohne diese Option werden ausschliesslich über den konfigurierten Konnektor aktualisiert.

### Identifikation der Instrumente
Instrumente werden eindeutig identifiziert durch:
- **Wertpapiere**: ISIN + Währungscode
- **Währungspaare**: Basiswährung + Kurswährung

### Ablauf des Preisaustauschs
Der Austausch folgt einem mehrschrittigen Prozess, bei dem verschiedene Datenquellen nacheinander abgefragt werden:

{{< mermaid >}}
flowchart TD
    A[Watchlist-Aktualisierung] --> B{GT-Net aktiviert?}
    B -->|Nein| K[Nur Konnektoren verwenden]
    B -->|Ja| C[Instrumente nach GT-Net-Empfang filtern]
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
    J -->|Nein| L
    M --> L
{{< /mermaid >}}

### Detaillierter Ablauf

#### Schritt 1: Filterung nach GT-Net-Konfiguration
Das System trennt die Instrumente der Watchlist in zwei Gruppen:
- **GT-Net-fähige Instrumente**: Instrumente mit aktivierter Option «GT-Net Letzter Preis empfangen»
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
    participant L as Lokaler Server
    participant P1 as Push-Offen #1 (Prio 1)
    participant P2 as Push-Offen #2 (Prio 2)

    L->>P1: Instrumente + Zeitstempel
    P1-->>L: Neuere Preise
    Note over L: Aktualisiere lokale Instrumente
    L->>P2: Verbleibende Instrumente + Zeitstempel
    P2-->>L: Neuere Preise
    Note over L: Aktualisiere lokale Instrumente
{{< /mermaid >}}

#### Schritt 4: Offen-Server abfragen
Für Instrumente, die noch keinen aktuellen Preis haben, werden die Offen-Server abgefragt. Der Ablauf ist ähnlich wie bei Push-Offen-Servern, jedoch:
- Offen-Server lesen aus ihren lokalen Instrumenten (kein separater Pool).
- Der Austausch kann **asymmetrisch** sein (abhängig von der Konfiguration «Austausch Preisdaten Wertpapiere»).

#### Schritt 5: Konnektoren für verbleibende Instrumente
Instrumente, die weder vom eigenen Pool noch von Remote-Servern Preise erhalten haben, werden über ihre konfigurierten **Konnektoren** (Datenquellen wie Yahoo, Finnhub usw.) aktualisiert.

#### Schritt 6: Eigenen Preispool aktualisieren (nur Push-Offen-Server)
Wenn der eigene Server als Push-Offen konfiguriert ist, werden die über Konnektoren erhaltenen Preise im lokalen Preispool gespeichert. Dadurch stehen diese Preise für zukünftige Anfragen von anderen Instanzen zur Verfügung.

### Unterschiede zwischen Server-Typen

| Aspekt | Push-Offen-Server | Offen-Server |
|--------|-------------------|--------------|
| Datenspeicherung | Separater Preispool | Direkt auf lokalen Instrumenten |
| Austausch | Immer bidirektional | Kann asymmetrisch sein |
| Abfragereihenfolge | Wird zuerst abgefragt | Wird nach Push-Offen abgefragt |
| Eigener Pool | Wird vor Remote-Servern geprüft | Nicht vorhanden |
| Konnektor-Updates | Werden in Pool gespeichert | Nur lokale Instrumente |

### Prioritäten und Lastverteilung
Die Reihenfolge der Server-Abfragen wird durch die **Priorität** (consumerUsage) bestimmt:
- Niedrigere Prioritätswerte werden zuerst abgefragt.
- Server mit gleicher Priorität werden **zufällig** ausgewählt, um die Last gleichmässig zu verteilen.
- Die Abfrage wird beendet, sobald alle Instrumente einen aktuellen Preis haben.

{{% notice info %}}
Der Preisaustausch basiert auf Zeitstempelvergleichen. Es wird immer der neuere Preis bevorzugt, unabhängig davon, von welchem Server er stammt.
{{% /notice %}}
