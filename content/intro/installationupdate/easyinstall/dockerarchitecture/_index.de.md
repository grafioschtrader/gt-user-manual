---
title: "Docker-Architektur"
date: 2026-11-02T12:00:00+01:00
draft: false
weight: 20
aliases: ["/intro/installationupdate/dockerarchitecture/"]
archetype: "default"
---
Eine Docker-Installation von Grafioschtrader besteht im Normalbetrieb aus drei Containern. Ein vierter Container kommt nur hinzu, wenn DuckDNS die wechselnde öffentliche IP-Adresse des Docker-Hosts nachführen soll. Alle Container gehören zu demselben Docker-Compose-Projekt. Diese Seite beschreibt den Aufbau; wie Grafioschtrader mit Docker installiert, aktualisiert und entfernt wird, steht im Wiki unter [Grafioschtrader with Docker](https://github.com/grafioschtrader/grafioschtrader/wiki/Grafioschtrader-with-Docker).

{{% callout style="info" title="Drei oder vier Container?" %}}
Die Container `web`, `backend` und `mariadb` sind immer erforderlich. Der Container `duckdns` ist optional. Für das Frontend wird kein zusätzlicher Container benötigt: Die fertige Angular-Oberfläche ist bereits im `web`-Container enthalten.
{{% /callout %}}

Wer die Images aus dem Quellcode baut, sieht zusätzlich temporäre Maven- und Node.js-Baustufen. Diese werden nur zum Erzeugen des Backends und der Angular-Oberfläche verwendet und sind keine weiteren Container der laufenden Installation.

## Aufbau der Installation

| Container | Aufgabe | Von aussen erreichbar |
|---|---|---|
| `web` | Caddy liefert die Angular-Oberfläche aus, nimmt HTTP- und HTTPS-Anfragen entgegen und leitet Anfragen an die Schnittstellen zum Backend weiter. Bei einer öffentlichen Domain beschafft und erneuert Caddy das TLS-Zertifikat automatisch. | Ja, normalerweise über Port 80 und 443. |
| `backend` | Führt Grafioschtrader aus, verarbeitet die Fachlogik, führt geplante Aufgaben aus und verbindet sich mit der Datenbank sowie externen Daten- und Maildiensten. | Nein, nur über den `web`-Container. |
| `mariadb` | Speichert Benutzer, Portfolios, Transaktionen, Instrumente, Kurse und weitere Anwendungsdaten dauerhaft. | Nein, nur im internen Docker-Netzwerk. |
| `duckdns` | Hält bei Bedarf eine DuckDNS-Subdomain auf der aktuellen öffentlichen IP-Adresse des Docker-Hosts. | Nein; dieser Container ist optional. |

Das folgende Diagramm zeigt den normalen Datenfluss. Nur der `web`-Container veröffentlicht Ports des Docker-Hosts. Backend und Datenbank bleiben in den internen Docker-Netzwerken.

{{< mermaid >}}
graph LR
    U["Browser"]
    M["SMTP-Mailserver"]
    Q["Externe Datenquellen"]
    L["Let's Encrypt"]
    DNS["DuckDNS"]
    subgraph H["Docker-Host"]
        direction LR
        W["web<br/>Caddy und Angular"] -->|"API, GTNet und WebSocket<br/>(Netzwerk proxy)"| B["backend<br/>Grafioschtrader"]
        B -->|"Datenbankzugriff<br/>(Netzwerk default)"| D[("mariadb<br/>Datenbank")]
        DD["duckdns (optional)<br/>Netzwerk des Hosts"]
    end
    U <-->|"HTTP/HTTPS"| W
    B -->|"E-Mail"| M
    B -->|"Kurse und Ereignisse"| Q
    W -.->|"TLS-Zertifikat"| L
    DD -.->|"Aktualisiert die Domain"| DNS
{{< /mermaid >}}

Der Browser lädt die Oberfläche unter `/grafioschtrader/` vom `web`-Container. Anfragen an die Programmierschnittstellen, GTNet und WebSockets nimmt ebenfalls Caddy entgegen und leitet sie intern an das Backend auf Port 8080 weiter. Das Backend erreicht MariaDB unter ihrem Docker-Dienstnamen auf Port 3306. Diese beiden internen Ports werden nicht auf dem Docker-Host freigegeben. Ist eine Domain eingerichtet, kann Caddy die Oberfläche zusätzlich unter einer bei der Installation angegebenen LAN-Adresse ausliefern, dort allerdings nur über unverschlüsseltes HTTP.

Die Container sind auf zwei interne Netzwerke verteilt. Im Netzwerk `proxy` sind nur Caddy und das Backend; Caddy hat dort eine feste Adresse, und nur von dieser Adresse übernimmt das Backend die Adresse des Browsers. So werden fehlgeschlagene Anmeldungen für jede Absenderadresse getrennt gezählt. Im Netzwerk `default` erreicht das Backend die Datenbank; Caddy hat zu MariaDB keine Verbindung. Der optionale `duckdns`-Container nutzt das Netzwerk des Hosts, damit er dessen öffentliche Adresse ermitteln kann. Hat der Docker-Host IPv6, erhalten die Container bei der Installation oder beim nächsten Update ebenfalls IPv6. Damit erreicht das Backend auch Adressen, die nur über IPv6 erreichbar sind, beispielsweise die eigene Domain an einem DS-Lite-Anschluss oder andere GTNet-Instanzen.

## Wo befinden sich Konfiguration und Daten?

Container können bei einem Update ersetzt werden. Dauerhafte Daten liegen deshalb ausserhalb ihrer beschreibbaren Container-Schichten: entweder als Datei im Docker-Verzeichnis oder in einem von Docker verwalteten Volume.

| Ablage | Inhalt und Bedeutung |
|---|---|
| `docker/.env` | Enthält die Infrastrukturkonfiguration, beispielsweise Datenbankpasswörter, JWT-Geheimnis, Administrator-E-Mail, Mailserver, Domain, veröffentlichte Ports, Speichergrössen und gewünschte GT-Version. Diese Datei enthält Geheimnisse und muss zusammen mit der Datenbank gesichert werden. |
| `docker/config/application-production.properties` | Enthält zusätzliche Einstellungen für das Verhalten von Grafioschtrader, beispielsweise Zeitpläne oder Protokollierungsstufen. Das Verzeichnis wird schreibgeschützt als `/config` in den Backend-Container eingebunden. |
| `docker/config/gt-cacerts` | Nur vorhanden, wenn das Zertifikat eines Datenanbieters von Hand importiert wurde. Ist diese Liste vertrauenswürdiger Zertifikate in `.env` eingetragen, verwendet das Backend sie anstelle der im Image enthaltenen. |
| `docker/docker-compose.yml`, Dockerfiles, `Caddyfile`, `mariadb-init` und die Skripte im Docker-Verzeichnis, darunter `install.sh`, `update.sh` und `uninstall.sh` | Beschreiben den Aufbau der Installation beziehungsweise führen Installation, Update und Entfernen aus. Sie enthalten keine Benutzer- oder Portfoliodaten. Ab Version 0.37.2 bringt das Update-Skript diese Dateien auf den Stand der neuen Version. |
| `docker/docker-compose.override.yml` | Ergänzt die Datei `docker-compose.yml` um eigene Anpassungen. Installation und Update legen sie an, wenn der Host IPv6 hat, damit die Container IPv6 erhalten. Eine bestehende Datei verändert ein Update nie. |
| Docker-Volume `db_data` | Enthält die eigentlichen MariaDB-Daten. Docker vergibt normalerweise den vollständigen Namen `grafioschtrader_db_data`. Das Volume bleibt beim Stoppen oder Ersetzen der Container erhalten. |
| Docker-Volumes `caddy_data` und `caddy_config` | Enthalten die von Caddy verwalteten Zertifikate und Laufzeitdaten. Auch diese Volumes bleiben bei einem Containerwechsel erhalten. |
| `docker/gt-backup-…`, `docker/gt-env-….bak` und `docker/gt-config-….bak` | Werden vom Update-Skript als Datenbank- und Konfigurationssicherungen angelegt. Manuell erzeugte Datenbanksicherungen können ebenfalls im Docker-Verzeichnis abgelegt werden. |
| `docker/.env.backup.…` | Sicherung der bisherigen `.env`, wenn das Installationsskript erneut ausgeführt wurde und die Konfiguration geändert hat. |
| `docker/gt-deployment-….tar.gz` und `docker/.gt-deployment.sha256` | Das Archiv enthält die Installationsdateien, die das Update-Skript ersetzt hat, im Zustand vor dem Update. Die zweite Datei hält fest, welche Fassung jeder Installationsdatei zuletzt ausgeliefert wurde; daran erkennt das nächste Update eigene Änderungen. Sie sollte nicht bearbeitet werden. |
| Container-Images | Enthalten das Grafioschtrader-Backend beziehungsweise Caddy mit der fertig gebauten Angular-Oberfläche. Sie enthalten keine dauerhaften Benutzer- oder Portfoliodaten und werden bei einem Update ausgetauscht. |

Der physische Speicherort eines benannten Volumes hängt von Docker Engine und dem Betriebssystem ab. Die Daten sollten nicht direkt in diesem internen Verzeichnis bearbeitet werden. Für Sicherung und Wiederherstellung werden die Docker-Compose-Befehle aus dem Abschnitt «Backup and restore» im [Docker-README](https://github.com/grafioschtrader/grafioschtrader/tree/master/docker#backup-and-restore) verwendet.

## Ressourcen auf Raspberry Pi und anderen Kleincomputern

Das Installationsskript erkennt den Arbeitsspeicher des Hosts einmalig und legt in `.env` passende Werte für den Java-Speicher des Backends und den Datenbankpuffer von MariaDB fest. Die Datei `/etc/mysql/my.cnf` des Docker-Hosts hat keinen Einfluss auf den MariaDB-Container, und das Datenbank-Volume liegt im Datenverzeichnis von Docker. Wie diese Werte angepasst, weitere MariaDB-Einstellungen eingebunden oder die Datenbank auf eine SSD gelegt werden, beschreibt der Abschnitt «Memory and storage on Raspberry Pi and SBC hosts» im [Docker-README](https://github.com/grafioschtrader/grafioschtrader/tree/master/docker#memory-and-storage-on-raspberry-pi-and-sbc-hosts).

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

{{% callout style="warning" title="Einmaliger Schritt beim Update von Version 0.37.1 oder älter" %}}
Das Update-Skript dieser Versionen aktualisiert sich noch nicht selbst. Vor dem ersten Update wird deshalb einmalig das neue Update-Skript von Hand geholt; danach wird wie gewohnt aktualisiert. Die Befehle stehen im Abschnitt «Update to a new release» des [Docker-README](https://github.com/grafioschtrader/grafioschtrader/tree/master/docker#update-to-a-new-release).
{{% /callout %}}

Die Befehle für Installation, Update und Entfernen beschreibt das Wiki unter [Grafioschtrader with Docker](https://github.com/grafioschtrader/grafioschtrader/wiki/Grafioschtrader-with-Docker). Alle weiteren Einstellungen, Sicherung und Störungsbehebung sind im [Docker-README](https://github.com/grafioschtrader/grafioschtrader/tree/master/docker) beschrieben.
