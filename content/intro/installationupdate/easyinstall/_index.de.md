---
title: "Einfache Installation"
date: 2026-10-12T15:00:00+02:00
draft: false
weight: 56
archetype: "default"
---
Für eine Installation von Grafioschtrader (GT) gibt es zwei geführte Wege: den Linux-Installer für eine klassische Installation auf Debian oder Ubuntu und die Installation mit Docker. Beide stellen die nötigen Fragen selbst, erzeugen die Passwörter auf Wunsch, richten die Datenbank ein und beschaffen bei einer öffentlichen Domain das TLS-Zertifikat von Let's Encrypt. Diese Seite vergleicht die beiden Wege; ihren Aufbau beschreiben die Unterseiten [Linux-Installer](./linuxinstaller/) und [Docker-Architektur](./dockerarchitecture/). Die einzelnen Befehle stehen im Wiki unter [Installation with the Linux installer](https://github.com/grafioschtrader/grafioschtrader/wiki/Installation-with-the-Linux-installer) und [Grafioschtrader with Docker](https://github.com/grafioschtrader/grafioschtrader/wiki/Grafioschtrader-with-Docker).

## Gemeinsame Voraussetzungen
Ohne Domain ist GT nur im lokalen Netzwerk über HTTP erreichbar. Für den Zugriff aus dem Internet braucht es eine Domain, die auf den Rechner zeigt. Wer dafür die kostenlose dynamische Domain von [DuckDNS](https://www.duckdns.org/) nutzt, legt das Konto und die Subdomain vor der Installation selbst auf duckdns.org an und hält das Token bereit; keiner der beiden Wege kann eine Subdomain anlegen. Beide fragen, über welche Adressfamilie der Rechner von aussen erreichbar ist: IPv4 mit Portweiterleitung im Router, nur IPv6 (beispielsweise bei einem Anschluss mit DS-Lite) oder beides. In jedem Fall muss der Router die TCP-Ports 80 und 443 zu diesem Rechner durchlassen, bei IPv4 als Portweiterleitung und bei IPv6 als Freigabe in der Firewall. Port 80 wird für die Ausstellung und Erneuerung des Zertifikats benötigt.

## Linux-Installer und Docker im Vergleich

| | Linux-Installer | Docker |
|---|---|---|
| Rechner | Debian 12 und 13, Ubuntu 24.04 und 26.04 (auch Raspberry Pi OS und Armbian auf dieser Basis) mit systemd | Linux-Server mit Docker Engine und Compose-Plugin, auf x86-64 oder 64-Bit-ARM wie dem Raspberry Pi |
| Was entsteht | GT als Systemdienst; das Backend wird auf dem Rechner aus dem Quellcode gebaut, die Oberfläche ab etwa 4 GB Arbeitsspeicher ebenfalls, sonst wird sie fertig heruntergeladen | drei Container aus fertigen Images, optional ein vierter für DuckDNS |
| Webserver | nginx oder Apache; eine vorhandene HTTPS-Site kann GT aufnehmen | Caddy im Container `web` |
| TLS-Zertifikat | certbot, oder ein vorhandenes Zertifikat, oder ein vorgeschalteter Proxy | Caddy beschafft und erneuert es selbst |
| DuckDNS | eigener systemd-Timer; ein bereits vorhandener Updater wird erkannt | eigener Container, auf Wunsch |
| Lokales Netzwerk | immer zusätzlich über die LAN-Adresse per HTTP | mit Domain nur über die Domain |
| Andere Dienste auf dem Rechner | vorhandene MariaDB, Webseiten, Java- und Node.js-Versionen und belegte Ports werden erkannt und bleiben unverändert | GT läuft abgeschottet in Containern; nur die Ports 80 und 443 werden belegt |
| Update | `gtupdate.sh` als Benutzer `grafioschtrader` | `update.sh` im Docker-Verzeichnis |
| Deinstallation | `gt-install.sh --uninstall` | `uninstall.sh` |

## Welcher Weg passt?

{{< mermaid >}}
graph TD
    A["Wo soll GT laufen?"] --> B{"Debian oder Ubuntu<br/>mit systemd?"}
    B -->|"Nein"| D["Docker"]
    B -->|"Ja"| C{"Soll GT neben anderen Diensten<br/>wie MariaDB oder Webseiten laufen?"}
    C -->|"Ja, und diese sollen<br/>gemeinsam genutzt werden"| L["Linux-Installer"]
    C -->|"Nein"| E{"Ist Docker bereits<br/>in Gebrauch?"}
    E -->|"Ja"| D
    E -->|"Nein"| L
    L --> F{"Belegt ein anderer Proxy<br/>wie Caddy die Ports 80 und 443?"}
    F -->|"Ja"| G["Linux-Installer ohne eigenen Webserver;<br/>der Proxy übernimmt die Routen"]
    F -->|"Nein"| H["Linux-Installer mit nginx oder Apache"]
{{< /mermaid >}}

Der Linux-Installer eignet sich, wenn GT einen Rechner mit anderen Anwendungen teilt oder wenn Sie die einzelnen Bestandteile wie bei einer manuellen Installation vor sich haben möchten. Docker eignet sich, wenn GT getrennt von allem anderen laufen soll oder Docker auf dem Rechner ohnehin verwendet wird. Auf einem Raspberry Pi mit wenig Arbeitsspeicher spart Docker zudem den Bau des Backends, weil die Images fertig bezogen werden.

## Deinstallation
Eine mit dem Linux-Installer erstellte Installation lässt sich mit demselben Programm wieder entfernen. Der Aufruf `sudo bash gt-install.sh --uninstall` zeigt zuerst, was entfernt wird, und entfernt nach der Bestätigung nur das, was der Installer selbst eingerichtet hat. Andere Webseiten, Dienste und Java-Versionen auf dem Server bleiben unverändert, ebenso Dateien, die nach der Installation von Hand geändert wurden. Die Datenbank mit Ihren Daten und die installierten Programmpakete bleiben standardmässig erhalten. Erst die Optionen `--remove-database` und `--remove-packages` entfernen auch diese. Vor dem Löschen der Datenbank legt der Installer im Verzeichnis `/root` eine Sicherung an und verlangt zusätzlich die Eingabe von `delete-database`.
{{% callout info %}}
Solange die behaltene Datenbank auf dem Server liegt, lässt sich Grafioschtrader dort nicht erneut installieren. Stellen Sie Ihre Daten in diesem Fall aus der Sicherung oder aus [Persönliche Daten exportieren](../../settings/#persönliche-daten-exportieren) wieder her.
{{% /callout %}}
Eine Installation mit Docker wird mit dem Skript `uninstall.sh` im Docker-Verzeichnis entfernt. Es behält die Datenbank und die Zertifikate, ausser diese werden mit der Option `--purge` ausdrücklich gelöscht.
