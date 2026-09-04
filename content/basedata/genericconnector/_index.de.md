---
title: "Generischer Connector"
date: 2026-02-26T12:00:00+01:00
draft: false
weight: 60
archetype: "default"
---
Der **Generische Connector** ermöglicht es, Preisdaten-Konnektoren vollständig über die Benutzeroberfläche zu konfigurieren, ohne Java-Code schreiben zu müssen. Damit können Benutzer Konnektoren für beliebige Datenanbieter anlegen, die historische oder Intraday-Kurse über HTTP-APIs bereitstellen.
Kostenlose Daten-APIs ändern häufig ihre URL-Schemata, Antwortformate oder Feldnamen. Mit dem generischen Connector können Benutzer solche Änderungen eigenständig nachführen, ohne auf ein Software-Update warten zu müssen.
## Datenmodell
Ein generischer Connector besteht aus drei Ebenen:
{{< mermaid >}}
erDiagram
    Connector-Definition ||--o{ Endpunkt : hat
    Connector-Definition ||--o{ HTTP-Header : hat
    Endpunkt ||--o{ Feldzuordnung : hat
{{< /mermaid >}}
+ Die **Connector-Definition** beschreibt den Datenanbieter: Basis-URL, Anzeigename, API-Schlüssel-Bedarf und Ratenlimit-Einstellungen.
+ Ein **Endpunkt** definiert, wie eine bestimmte Art von Preisdaten abgerufen wird — z.B. historische Kurse für Wertpapiere oder Intraday-Kurse für Währungspaare. Jeder Endpunkt enthält die URL-Vorlage, das Antwortformat und die Parsing-Konfiguration.
+ Eine **Feldzuordnung** bildet ein Feld aus der API-Antwort auf ein internes GT-Feld ab (z.B. Datum, Eröffnungskurs, Schlusskurs, Volumen).
+ **HTTP-Header** ermöglichen das Senden zusätzlicher Kopfzeilen bei API-Aufrufen (z.B. `X-Api-Key`).
## Verwaltung
Der generische Connector wird unter **Basisdaten** im Navigationsbereich verwaltet. Die Ansicht ist wie folgt aufgebaut:
+ **Dropdown-Auswahl** oben zur Wahl des Connectors.
+ **Detailanzeige** unterhalb der Auswahl, gegliedert in drei Fieldsets:
  - **Connector-Einstellungen** — grundlegende Identifikation und Fähigkeiten.
  - **Ratenlimit-Einstellungen** — Konfiguration der Anfragebegrenzung.
  - **Erweiterte Einstellungen** — diverse Optionen sowie, falls konfiguriert, eine schreibgeschützte Anzeige der **Token-Konfiguration (YAML)**.
+ **HTTP-Header-Tabelle** — eine editierbare Tabelle zur Verwaltung benutzerdefinierter HTTP-Header.
+ **Endpunkte-Akkordeon** — jeder Endpunkt erscheint als Akkordeon-Panel. Innerhalb jedes Panels:
  - Endpunkt-Detail-Fieldsets (Einstellungen, formatspezifische Optionen, Ticker, Paginierung).
  - Eine eingebettete **Feldzuordnungs-Tabelle** zur Verwaltung der Feldzuordnungen des Endpunkts.
### Connector erstellen und bearbeiten
Über das Kontextmenü (Rechtsklick) stehen folgende Aktionen zur Verfügung:
+ **Erstellen Generischer Connector...**: Neuen Connector anlegen.
+ **Bearbeiten Generischer Connector...**: Ausgewählten Connector bearbeiten.
+ **Löschen Generischer Connector**: Ausgewählten Connector löschen. Diese Option ist deaktiviert, wenn Wertpapiere oder Währungspaare den Connector noch als Preisdatenquelle referenzieren.
+ **Aktivieren...**: Den Connector aktivieren, damit er als Preisdaten-Konnektor zur Verfügung steht (nur Administrator).
+ **Connectors neu laden...**: Alle generischen Connectors aus der Datenbank neu laden und registrieren (nur Administrator).
+ **Erstellen Endpunkt...**: Neuen Endpunkt für den ausgewählten Connector anlegen.
+ **Bearbeiten Endpunkt...**: Ausgewählten Endpunkt bearbeiten (wenn ein Endpunkt-Akkordeon-Panel ausgewählt ist).
+ **Löschen Endpunkt**: Ausgewählten Endpunkt löschen (wenn ein Endpunkt-Akkordeon-Panel ausgewählt ist).
{{% notice style="info" title="Aktivierung durch Administrator" %}}
Ein neu erstellter Connector ist zunächst inaktiv. Erst nach Prüfung und **Aktivierung** durch einen Administrator wird er als Preisdaten-Konnektor registriert und steht in der Konnektor-Auswahl bei Wertpapieren und Währungspaaren zur Verfügung.
{{% /notice %}}
## Benutzerrechte
Der generische Connector verwendet ein nutzungsbasiertes Berechtigungsmodell, das die Bearbeitungsrechte einschränkt, sobald ein Connector produktiv eingesetzt wird:
+ Der **Administrator** kann alle Connector-Bestandteile jederzeit bearbeiten und löschen.
+ Der **Ersteller** (der Benutzer, der den Connector angelegt hat) kann den Connector, dessen HTTP-Header, Endpunkte und Feldzuordnungen frei bearbeiten, **solange kein Endpunkt erfolgreich verwendet wurde**.
+ Sobald ein Endpunkt zum ersten Mal erfolgreich Preisdaten geliefert hat, wird er für den Ersteller gesperrt — Endpunkt-Einstellungen und Feldzuordnungen dieses Endpunkts können dann nur noch vom Administrator geändert werden.
+ Sobald **irgendein** Endpunkt erfolgreich verwendet wurde, werden auch die Connector-Definition und die HTTP-Header für den Ersteller gesperrt.
+ Gesperrte Menüpunkte (Bearbeiten, Löschen) sind im Kontextmenü ausgegraut. Der Ersteller kann aber weiterhin einen neuen, noch nicht verwendeten Endpunkt anlegen und bearbeiten.
+ Sobald alle Endpunkte erfolgreich verwendet wurden, geht der Connector in Systembesitz über und kann nur noch vom Administrator verwaltet werden.
{{% notice style="note" title="Warum wird gesperrt?" %}}
Wenn ein Endpunkt erfolgreich Preisdaten geliefert hat, sind möglicherweise bereits Wertpapiere oder Währungspaare mit diesem Connector verknüpft. Änderungen am Endpunkt oder an der Connector-Definition könnten die Preisdatenversorgung dieser Instrumente unterbrechen. Die Sperre schützt davor, dass produktive Verbindungen versehentlich gestört werden.
{{% /notice %}}
{{% notice style="warning" title="Löschschutz" %}}
Ein Connector kann nicht gelöscht werden, solange Wertpapiere oder Währungspaare ihn als Preisdaten-Konnektor verwenden. Der Menüpunkt **Löschen** ist ausgegraut, wenn der Connector noch von Instrumenten referenziert wird. Um einen solchen Connector zu löschen, müssen zuerst alle betroffenen Instrumente einem anderen Connector zugeordnet werden.
{{% /notice %}}
## Vorgehen zum Erstellen eines generischen Connectors
Das folgende Ablaufdiagramm und die anschliessende Schrittbeschreibung zeigen den typischen Ablauf beim Erstellen eines generischen Connectors durch einen Benutzer ohne Administratorrechte.
{{< mermaid >}}
flowchart TD
    subgraph outside["Vorbereitung ausserhalb von GT"]
        S1["1. Website als Datenanbieter prüfen"]
        S2["2. Möglichen API-Aufruf ermitteln"]
        S3["3. API-Aufruf mit API-Client testen"]
    end
    subgraph inside["Innerhalb von GT"]
        S4["4. Generischen Connector mit Endpunkten und Feldzuordnungen erstellen"]
        S5["5. Endpunkte testen"]
        S6["6. Nachricht an Admin senden zur Aktivierung"]
        S7["7. Wertpapier oder Währungspaar erstellen und zuordnen"]
        S8["8. Stapelverarbeitungsmonitor prüfen"]
        S9{"9. Preisdaten korrekt?"}
        S10["10. Admin benachrichtigen bei Problemen"]
        S11["11. Weitere Korrekturen vornehmen"]
    end
    S1 --> S2 --> S3 --> S4 --> S5 --> S6 --> S7 --> S8 --> S9
    S9 -->|Ja| E(("Fertig"))
    S9 -->|Nein| S10 --> S11 --> S6
{{< /mermaid >}}

1. **Website als Datenanbieter prüfen** — Prüfen Sie zunächst, ob die Website als Datenanbieter genutzt werden kann. Ein LLM kann sehr hilfreich sein, um mögliche Ajax-Anfragen zu finden. Für historische Daten sollte darauf geachtet werden, dass sie über einen langen Zeitraum und als OHLC-Daten verfügbar sind. Das Chart ist häufig die einzige und oft die bessere Alternative zu den angebotenen historischen Kursdaten.
2. **Möglichen API-Aufruf ermitteln** — Finden Sie den passenden API-Aufruf heraus, mit dem die gewünschten Daten abgerufen werden können.
3. **API-Aufruf mit API-Client testen** — Testen Sie diesen API-Aufruf mit einem API-Client. Dadurch lassen sich Hindernisse erkennen, die überwunden werden müssen. Möglicherweise funktioniert der Aufruf aus Authentifizierungsgründen nicht, oder die Website ist anderweitig geschützt. Der HTTP-Header ist hierbei sehr wichtig.
4. **Generischen Connector erstellen** — Erstellen Sie den generischen Connector mit [Connector-Definition](connectordefinition/), [Endpunkten](endpoints/) und [Feldzuordnungen](fieldmappings/).
5. **Endpunkte testen** — Testen Sie die Endpunkte über die [Test- und Fehlerbehebungsfunktion](troubleshooting/).
6. **Nachricht an Admin senden** — Senden Sie eine Nachricht an die Admin-Rolle, damit der funktionsfähige Connector aktiviert werden kann.
7. **Wertpapier oder Währungspaar erstellen und zuordnen** — Erstellen Sie nach der Aktivierung ein Wertpapier oder Währungspaar und verbinden Sie es mit diesem generischen Connector.
8. **Stapelverarbeitungsmonitor prüfen** — Prüfen Sie im [Stapelverarbeitungsmonitor]({{< relref "/admindata/taskdatachangemonitor" >}}), ob der Ladevorgang erfolgreich war oder eine Fehlermeldung ausgegeben wurde.
9. **Preisdaten prüfen** — Sehen Sie sich die Preisdaten des Wertpapiers oder Währungspaares als Tabelle und Chart an.
10. **Admin bei Problemen benachrichtigen** — Wenn die Daten nicht korrekt sind, benachrichtigen Sie die Admin-Rolle. Diese kann das Problem beheben oder den generischen Connector deaktivieren oder löschen.
11. **Weitere Korrekturen vornehmen** — Wenn der Connector noch nicht gelöscht wurde und Hoffnung besteht, ihn zum Laufen zu bringen, nehmen Sie weitere Korrekturen vor und fahren Sie mit Schritt 6 fort.

## Connector-Definition
Die Felder der Connector-Definition sind auf einer eigenen Seite beschrieben: [Connector-Definition](connectordefinition/).
## Beispiel: JSON-API-Connector
Das folgende Beispiel zeigt die Konfiguration für einen fiktiven JSON-basierten Datenanbieter:
**Connector-Definition:**
+ Kurz-ID: `mydata`
+ Domain-URL: `https://api.mydata.com/`
+ Ratenlimit-Typ: Token-Bucket, 5 Anfragen pro 60 Sekunden
**Endpunkt (Historisch, Wertpapier):**
+ URL-Vorlage: `v1/eod/{ticker}?from={fromDate}&to={toDate}&token={apiKey}`
+ Antwortformat: JSON
+ Datenstruktur: Array von Objekten
+ Datenpfad: `results`
+ Datumsformat-Typ: ISO_DATE
**Feldzuordnungen:**
- **`date`**: `t`
- **`open`**: `o`
- **`high`**: `h`
- **`low`**: `l`
- **`close`**: `c`
- **`volume`**: `v`
