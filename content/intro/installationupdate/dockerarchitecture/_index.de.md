---
title: "Docker-Architektur"
date: 2026-09-26T12:00:00+02:00
draft: false
weight: 57
archetype: "default"
---
Eine Docker-Installation von Grafioschtrader besteht im Normalbetrieb aus drei Containern. Ein vierter Container kommt nur hinzu, wenn DuckDNS die wechselnde öffentliche IP-Adresse des Docker-Hosts nachführen soll. Alle Container gehören zu demselben Docker-Compose-Projekt und kommunizieren über ein internes Netzwerk.

{{% notice style="info" title="Drei oder vier Container?" %}}
Die Container `web`, `backend` und `mariadb` sind immer erforderlich. Der Container `duckdns` ist optional. Für das Frontend wird kein zusätzlicher Container benötigt: Die fertige Angular-Oberfläche ist bereits im `web`-Container enthalten.
{{% /notice %}}

Wer die Images aus dem Quellcode baut, sieht zusätzlich temporäre Maven- und Node.js-Baustufen. Diese werden nur zum Erzeugen des Backends und der Angular-Oberfläche verwendet und sind keine weiteren Container der laufenden Installation.

## Aufbau der Installation

| Container | Aufgabe | Von aussen erreichbar |
|---|---|---|
| `web` | Caddy liefert die Angular-Oberfläche aus, nimmt HTTP- und HTTPS-Anfragen entgegen und leitet Anfragen an die Schnittstellen zum Backend weiter. Bei einer öffentlichen Domain beschafft und erneuert Caddy das TLS-Zertifikat automatisch. | Ja, normalerweise über Port 80 und 443. |
| `backend` | Führt Grafioschtrader aus, verarbeitet die Fachlogik, führt geplante Aufgaben aus und verbindet sich mit der Datenbank sowie externen Daten- und Maildiensten. | Nein, nur über den `web`-Container. |
| `mariadb` | Speichert Benutzer, Portfolios, Transaktionen, Instrumente, Kurse und weitere Anwendungsdaten dauerhaft. | Nein, nur im internen Docker-Netzwerk. |
| `duckdns` | Hält bei Bedarf eine DuckDNS-Subdomain auf der aktuellen öffentlichen IP-Adresse des Docker-Hosts. | Nein; dieser Container ist optional. |

Das folgende Diagramm zeigt den normalen Datenfluss. Nur der `web`-Container veröffentlicht Ports des Docker-Hosts. Backend und Datenbank bleiben im internen Docker-Netzwerk.

{{< mermaid >}}
graph LR
    U["Browser"]
    M["SMTP-Mailserver"]
    Q["Externe Datenquellen"]
    L["Let's Encrypt"]
    DNS["DuckDNS"]
    subgraph H["Docker-Host"]
        direction LR
        W["web<br/>Caddy und Angular"] -->|"API, GTNet und WebSocket"| B["backend<br/>Grafioschtrader"]
        B -->|"Datenbankzugriff"| D[("mariadb<br/>Datenbank")]
        DD["duckdns<br/>(optional)"]
    end
    U <-->|"HTTP/HTTPS"| W
    B -->|"E-Mail"| M
    B -->|"Kurse und Ereignisse"| Q
    W -.->|"TLS-Zertifikat"| L
    DD -.->|"Aktualisiert die Domain"| DNS
{{< /mermaid >}}

Der Browser lädt die Oberfläche unter `/grafioschtrader/` vom `web`-Container. Anfragen an die Programmierschnittstellen, GTNet und WebSockets nimmt ebenfalls Caddy entgegen und leitet sie intern an das Backend auf Port 8080 weiter. Das Backend erreicht MariaDB unter ihrem Docker-Dienstnamen auf Port 3306. Diese beiden internen Ports werden nicht auf dem Docker-Host freigegeben.

## Wo befinden sich Konfiguration und Daten?

Container können bei einem Update ersetzt werden. Dauerhafte Daten liegen deshalb ausserhalb ihrer beschreibbaren Container-Schichten: entweder als Datei im Docker-Verzeichnis oder in einem von Docker verwalteten Volume.

| Ablage | Inhalt und Bedeutung |
|---|---|
| `docker/.env` | Enthält die Infrastrukturkonfiguration, beispielsweise Datenbankpasswörter, JWT-Geheimnis, Administrator-E-Mail, Mailserver, Domain, veröffentlichte Ports, Speichergrössen und gewünschte GT-Version. Diese Datei enthält Geheimnisse und muss zusammen mit der Datenbank gesichert werden. |
| `docker/config/application-production.properties` | Enthält zusätzliche Einstellungen für das Verhalten von Grafioschtrader, beispielsweise Zeitpläne oder Protokollierungsstufen. Das Verzeichnis wird schreibgeschützt als `/config` in den Backend-Container eingebunden. |
| `docker/docker-compose.yml`, Dockerfiles, `Caddyfile`, `install.sh` und `update.sh` | Beschreiben den Aufbau der Installation beziehungsweise führen Installation und Update aus. Sie enthalten keine Benutzer- oder Portfoliodaten. Ab Version 0.37.2 bringt das Update-Skript diese Dateien auf den Stand der neuen Version. |
| Docker-Volume `db_data` | Enthält die eigentlichen MariaDB-Daten. Docker vergibt normalerweise den vollständigen Namen `grafioschtrader_db_data`. Das Volume bleibt beim Stoppen oder Ersetzen der Container erhalten. |
| Docker-Volumes `caddy_data` und `caddy_config` | Enthalten die von Caddy verwalteten Zertifikate und Laufzeitdaten. Auch diese Volumes bleiben bei einem Containerwechsel erhalten. |
| `docker/gt-backup-…`, `docker/gt-env-….bak` und `docker/gt-config-….bak` | Werden vom Update-Skript als Datenbank- und Konfigurationssicherungen angelegt. Manuell erzeugte Datenbanksicherungen können ebenfalls im Docker-Verzeichnis abgelegt werden. |
| `docker/gt-deployment-….tar.gz` und `docker/.gt-deployment.sha256` | Das Archiv enthält die Installationsdateien, die das Update-Skript ersetzt hat, im Zustand vor dem Update. Die zweite Datei hält fest, welche Fassung jeder Installationsdatei zuletzt ausgeliefert wurde; daran erkennt das nächste Update eigene Änderungen. Sie sollte nicht bearbeitet werden. |
| Container-Images | Enthalten das Grafioschtrader-Backend beziehungsweise Caddy mit der fertig gebauten Angular-Oberfläche. Sie enthalten keine dauerhaften Benutzer- oder Portfoliodaten und werden bei einem Update ausgetauscht. |

Der physische Speicherort eines benannten Volumes hängt von Docker Engine und dem Betriebssystem ab. Die Daten sollten nicht direkt in diesem internen Verzeichnis bearbeitet werden. Für Sicherung und Wiederherstellung werden die im Docker-Verzeichnis beschriebenen Docker-Compose-Befehle verwendet.

## Ressourcen auf Raspberry Pi und anderen Kleincomputern

Die Anleitung für die [klassische Installation auf einem Raspberry Pi 4 oder 5](https://github.com/grafioschtrader/grafioschtrader/wiki/Installation-on-Raspberry-Pi-4-or-5) enthält Empfehlungen für Java, MariaDB, Auslagerungsspeicher und Datenträger. Diese Empfehlungen gelten grundsätzlich auch für Docker, werden dort aber an anderen Stellen konfiguriert.

Das Installationsskript erkennt den verfügbaren Arbeitsspeicher einmalig und schreibt passende Ausgangswerte in `.env`:

| Arbeitsspeicher des Hosts | Java-Heap des Backends (`JAVA_OPTS`) | MariaDB-Puffer (`DB_INNODB_BUFFER_POOL_SIZE`) |
|---|---|---|
| Etwa 2 GB | `-Xms128m -Xmx896m` | `384M` |
| Etwa 4 GB | `-Xms256m -Xmx1792m` | `1G` |
| 8 GB oder mehr | `-Xms512m -Xmx2048m` | `2G` |

Wenn Grafioschtrader den Host mit anderen Anwendungen teilt oder der Arbeitsspeicher geändert wurde, können diese Werte in `.env` angepasst werden. Danach werden die betroffenen Container neu erstellt:

```bash
docker compose up -d --force-recreate mariadb backend
```

Die Datei `/etc/mysql/my.cnf` des Docker-Hosts hat keinen Einfluss auf den MariaDB-Container. Zusätzliche MariaDB-Einstellungen werden über eine eigene Konfigurationsdatei in den Container eingebunden. Ein vollständiges Beispiel sowie Hinweise zu speicherintensiven Einstellungen enthält der Abschnitt «Memory and storage on Raspberry Pi and SBC hosts» im [Docker-README](https://github.com/grafioschtrader/grafioschtrader/tree/master/docker#memory-and-storage-on-raspberry-pi-and-sbc-hosts). Die Grösse des InnoDB-Puffers wird weiterhin über `DB_INNODB_BUFFER_POOL_SIZE` in `.env` festgelegt.

Auslagerungsspeicher wird auf dem Docker-Host konfiguriert. Er kann beim lokalen Bau der Images auf einem Gerät mit wenig Arbeitsspeicher helfen; für den normalen Betrieb sind die veröffentlichten Images vorzuziehen. Das Datenbank-Volume `db_data` liegt im Datenverzeichnis von Docker. Soll die Datenbank auf einer SSD oder NVMe liegen, muss deshalb der Docker-Speicherort beziehungsweise dieses Volume dorthin gelegt werden. Bei einer bestehenden Installation ist davor eine geprüfte Datenbanksicherung erforderlich.

## Start und Update

Beim Start wartet das Backend, bis MariaDB betriebsbereit ist. Danach führt es notwendige Datenbankmigrationen automatisch aus. Der `web`-Container kann bereits Verbindungen annehmen, während das Backend noch startet. Beim Update werden vor allem die Images und Container ersetzt; `.env`, das Verzeichnis `config` sowie die benannten Volumes bleiben bestehen.

### Installationsdateien beim Update

Ein neues Image verändert die Dateien auf dem Docker-Host nicht. Bringt eine neue Version eine geänderte `docker-compose.yml` mit, beispielsweise einen umbenannten Einstellungsnamen für das Backend oder eine neue Zustandsprüfung, käme diese Änderung ohne weiteres Zutun nie auf dem Host an; im ungünstigsten Fall startet das neue Backend dann nicht. Deshalb holt `update.sh` zuerst die Installationsdateien der gewünschten Version: den Inhalt des Docker-Verzeichnisses sowie zwei Hilfsskripte, mit denen das Update Einstellungen umbenennt und die täglichen Downloadzeiten verteilt. Erst danach sichert es die Datenbank und wechselt die Images. Ändert sich dabei `update.sh` selbst, übernimmt die neue Fassung und führt das Update von Anfang an aus.

Eine Datei wird nur ersetzt, solange sie noch unverändert so vorliegt, wie eine frühere Version sie ausgeliefert hat. Wurde eine Installationsdatei auf dem Host von Hand angepasst, entscheidet das Update folgendermassen:

{{< mermaid >}}
graph TD
    A["Installationsdatei auf dem Host"] --> B{"Stimmt sie mit der<br/>neuen Version überein?"}
    B -->|"Ja"| K["Nichts zu tun"]
    B -->|"Nein"| C{"Von Hand angepasst?"}
    C -->|"Nein"| E["Wird durch die neue<br/>Fassung ersetzt"]
    C -->|"Ja"| D{"Ändert die neue<br/>Version diese Datei?"}
    D -->|"Nein"| F["Eigene Anpassung<br/>bleibt erhalten"]
    D -->|"Ja"| G["Update hält an,<br/>nichts wird verändert"]
{{< /mermaid >}}

Hält das Update an, nennt es die betroffenen Dateien. Eigene Einstellungen gehören möglichst in `.env` oder in das Verzeichnis `config`, denn diese beiden fasst ein Update nie an. Nachdem die Anpassung dorthin verschoben wurde, wird das Update mit der Option `--replace-modified` wiederholt; die genannten Dateien werden dann durch die neue Fassung ersetzt. Vor jedem Ersetzen legt das Skript eine Sicherung `gt-deployment-….tar.gz` mit dem bisherigen Stand an, sodass keine Anpassung verloren geht. Mit der Option `--skip-files` bleiben alle Installationsdateien unverändert, und es werden nur die Images gewechselt.

{{% notice style="warning" title="Einmaliger Schritt beim Update von Version 0.37.1 oder älter" %}}
Das Update-Skript dieser Versionen aktualisiert sich noch nicht selbst. Vor dem Update auf 0.37.2 wird deshalb einmalig das neue Update-Skript geholt und danach wie gewohnt aktualisiert. Die Befehle werden im Docker-Verzeichnis ausgeführt:

```bash
git -C .. fetch --depth 1 origin tag V0.37.2
git -C .. checkout V0.37.2 -- docker/update.sh
./update.sh 0.37.2
```
{{% /notice %}}

Die Installations-, Update-, Sicherungs- und Diagnosebefehle sind im [Docker-Verzeichnis des Grafioschtrader-Projekts](https://github.com/grafioschtrader/grafioschtrader/tree/master/docker) beschrieben.
