---
title: "Linux-Installer"
date: 2026-10-12T15:00:00+02:00
draft: false
weight: 10
archetype: "default"
---
Der Linux-Installer richtet Grafioschtrader (GT) als klassische Installation auf Debian oder Ubuntu ein: Das Backend läuft als Systemdienst, ein Webserver liefert die Oberfläche aus, und MariaDB speichert die Daten. Das Ergebnis entspricht einer manuellen Installation nach dem Wiki, mit den weiter unten beschriebenen Unterschieden. Wie der Installer gestartet und bedient wird, beschreibt das Wiki unter [Installation with the Linux installer](https://github.com/grafioschtrader/grafioschtrader/wiki/Installation-with-the-Linux-installer); diese Seite erklärt den Aufbau.

## Aufbau der Installation

| Bestandteil | Aufgabe | Von aussen erreichbar |
|---|---|---|
| Dienst `grafioschtrader` | Führt das Backend von GT als Benutzer `grafioschtrader` aus, verarbeitet die Fachlogik, führt geplante Aufgaben aus und verbindet sich mit Datenbank, Mailserver und externen Datenquellen. | Nein, das Backend lauscht nur auf `127.0.0.1`. |
| Webserver nginx oder Apache | Liefert die Oberfläche unter `/grafioschtrader/` aus und leitet die Schnittstellen, GTNet und WebSockets an das Backend weiter. Eine eigene Site bedient die LAN-Adresse über HTTP, eine zweite die Domain über HTTPS; alternativ werden die Routen in eine bereits vorhandene HTTPS-Site eingebunden. | Ja, über Port 80 und bei einer Domain 443. |
| MariaDB | Speichert Benutzer, Portfolios, Transaktionen, Instrumente, Kurse und weitere Anwendungsdaten. Eine bereits vorhandene MariaDB wird weiterverwendet. | Nein, nur lokal. |
| certbot | Beschafft das Zertifikat von Let's Encrypt und erneuert es; nach jeder Erneuerung lädt der Webserver es neu. Entfällt bei einem vorhandenen Zertifikat oder einem vorgeschalteten Proxy. | Nein |
| DuckDNS-Timer | Hält bei Bedarf alle fünf Minuten die DuckDNS-Subdomain auf der aktuellen Adresse des Rechners. | Nein, optional |
| Build-Werkzeuge | Java und Maven bauen das Backend aus dem Quellcode, Node.js und Angular CLI ab etwa 4 GB Arbeitsspeicher auch die Oberfläche; auf kleineren Rechnern wird die Oberfläche fertig heruntergeladen. Das gilt bei der Installation wie bei jedem Update. | Nein |

Das folgende Diagramm zeigt den Datenfluss im Betrieb. Nur der Webserver ist von aussen erreichbar.

{{< mermaid >}}
graph LR
    U["Browser"]
    M["SMTP-Mailserver"]
    Q["Externe Datenquellen"]
    L["Let's Encrypt"]
    DNS["DuckDNS"]
    subgraph H["Rechner mit Debian oder Ubuntu"]
        direction LR
        W["nginx oder Apache<br/>Oberfläche"] -->|"API, GTNet und WebSocket"| B["Dienst grafioschtrader<br/>Backend auf 127.0.0.1"]
        B -->|"Datenbankzugriff"| D[("MariaDB")]
        C["certbot"]
        T["DuckDNS-Timer<br/>(optional)"]
    end
    U <-->|"HTTP/HTTPS"| W
    B -->|"E-Mail"| M
    B -->|"Kurse und Ereignisse"| Q
    C -.->|"Zertifikat"| L
    T -.->|"Aktualisiert die Domain"| DNS
{{< /mermaid >}}

## Ablauf der Installation
Der Installer prüft zuerst den Rechner und stellt dann nur die Fragen, die für diesen Rechner gelten. Bevor er etwas verändert, zeigt er den vollständigen Plan mit allen Paketen, Dateien und Diensten; erst nach der Bestätigung führt er ihn aus. Jeder Schritt wird in einem Journal festgehalten. Bricht die Installation ab, etwa durch einen Stromausfall oder einen fehlgeschlagenen Schritt, setzt ein erneuter Aufruf an derselben Stelle fort, mit denselben Antworten und Passwörtern.

{{< mermaid >}}
graph TD
    I["Prüfung des Rechners"] --> F["Fragen in Dialogen"]
    F --> P["Plan mit allen Änderungen"]
    P -->|"Bestätigung"| K["Kern: Pakete, Benutzer, Quellcode,<br/>Datenbank, verschlüsselte Konfiguration"]
    K --> A["Anwendung: Bau, Dienst,<br/>erster Start mit Datenbankmigration"]
    A --> W["Web und TLS: Sites, Zertifikat, DuckDNS"]
    W --> E["Mail-Prüfung"]
    E --> R["Ergebnis"]
    K -.->|"Abbruch"| N["Erneuter Aufruf setzt fort"]
    A -.->|"Abbruch"| N
    W -.->|"Abbruch"| N
{{< /mermaid >}}

## Wo befinden sich Konfiguration und Daten?

| Ablage | Inhalt und Bedeutung |
|---|---|
| `/home/grafioschtrader` | Startskript des Dienstes, `gtvar.sh`, die Update-Skripte und das gebaute Backend `grafioschtrader-server-<Version>.jar`. Im Unterverzeichnis `build/grafioschtrader` liegt der Quellcode, aus dem jedes Update baut. |
| `application.properties` und `application-production.properties` | Im Quellcode unter `backend/grafioschtrader-server/src/main/resources/`. Die erste enthält die verschlüsselten Zugangsdaten und Ihre Einstellungen, die zweite eigene Schlüssel und Einstellungen des Installers wie die lokale Backend-Adresse. Siehe [Einstellungen an application.properties](../../applicationproperties/). |
| `<Dokumentenverzeichnis>/grafioschtrader` | Die gebaute Oberfläche. Das Dokumentenverzeichnis ist `/var/www/<Domain>` oder `/var/www/gt`, wenn Sie kein anderes wählen. |
| `/etc/nginx` beziehungsweise `/etc/apache2` | Die eigenen Sites `grafioschtrader` und `grafioschtrader-domain`, bei einer eingebundenen HTTPS-Site das Snippet `grafioschtrader.conf`. |
| `/etc/systemd/system`, `/etc/sudoers.d`, `/etc/logrotate.d` | Dienst, die Erlaubnis für den Benutzer `grafioschtrader`, den Dienst zu stoppen und zu starten, und die wöchentliche Rotation des Logs. |
| `/var/log/grafioschtrader.log` | Das Log des Backends. |
| `/var/lib/gt-install` | Journal und Ergebnis des Installers. Daran erkennt ein erneuter Aufruf, was bereits erledigt ist. |
| `/root/.gt-install` | Passwörter und Geheimnisse der Installation, nur für root lesbar. |
| MariaDB | Die Datenbank `grafioschtrader` im Datenverzeichnis von MariaDB, die Einstellungen für Puffer und Sperren in `/etc/mysql/mariadb.conf.d/60-grafioschtrader.cnf`. |
| `/opt` | Ein vom Installer bezogenes JDK oder Maven sowie `/opt/nodejs-gt` und `/opt/gt-build-tools`, wenn er sie für den Bau braucht. |

## Zusammenleben mit vorhandenen Diensten
Der Installer verändert nichts, was ihm nicht gehört. Eine vorhandene MariaDB wird weiterverwendet, auch wenn sich root nur mit Passwort anmelden kann; das Passwort wird einmal abgefragt. Andere Webseiten auf demselben nginx oder Apache, auch solche mit PHP, antworten nach der Installation wie zuvor; der Installer prüft dies vor und nach jedem Neuladen des Webservers. Andere Java- und Node.js-Versionen bleiben die Standardversion des Rechners, und belegte Ports werden durch freie ersetzt. Ein bereits laufender DuckDNS-Updater wird erkannt, sodass kein zweiter entsteht. Belegt ein anderer Proxy wie Caddy die Ports 80 und 443, richtet der Installer keinen eigenen Webserver ein und schreibt die Routen als Vorschlag für diesen Proxy in `/root/gt-install-webserver.conf`.

## Update
Ein Update erfolgt wie bei jeder klassischen Installation mit `./gtupdate.sh` als Benutzer `grafioschtrader`, siehe [Update Grafioschtrader](../../newversion/). Es holt den neuen Quellcode, übernimmt Ihre Werte aus `application.properties` in die neue Vorlage, lässt `application-production.properties` unverändert und baut Backend und Oberfläche neu.

## Unterschiede zur manuellen Installation
Die manuelle Installation nach dem Wiki und der Linux-Installer führen zum selben Ergebnis. In einigen Punkten geht der Installer einen anderen Weg, damit er andere Programme auf dem Rechner nicht verändert und sich wieder vollständig entfernen lässt.

| | Manuelle Installation (Wiki) | Linux-Installer | Grund |
|---|---|---|---|
| Java | Archiv in `/usr/local/jdk-25`, als Standard des Rechners eingetragen | Paket der Distribution oder Archiv in `/opt/jdk-<Version>-<Hersteller>`; der Dienst nutzt es direkt | Andere Java-Anwendungen behalten ihre Version. |
| Node.js | NodeSource-Paket für den ganzen Rechner | eine passende vorhandene Version oder ein eigenes Node.js in `/opt/nodejs-gt` | Andere Node.js-Anwendungen behalten ihre Version. |
| Angular CLI | global mit `npm install -g` | in `/opt/gt-build-tools` | Keine globalen npm-Pakete. |
| DuckDNS | Cron-Skript | systemd-Timer, der IPv4 und IPv6 gemäss Ihrer Wahl nachführt | Funktioniert auch bei IPv6-Anschlüssen wie DS-Lite. |
| Log | ohne Rotation | wöchentlich rotiert, acht Kopien | Das Log wächst nicht unbegrenzt. |
| TLS | `certbot --nginx` bzw. `--apache` schreibt die Site um | certbot über das Dokumentenverzeichnis, eigene HTTPS-Site | Fremde Sites bleiben unverändert. |
| Backend | bei nginx auf allen Adressen, bei Apache über AJP | nur auf `127.0.0.1`; die Adressen der vorgeschalteten Proxys stehen in `application-production.properties` | Nur der Webserver erreicht das Backend, und die Sperre nach fehlgeschlagenen Anmeldungen zählt die echte Adresse des Benutzers. |
| Nur im Installer | | Swap-Datei von 2 GB, wenn der Rechner weniger als 4 GB Arbeitsspeicher und keinen Swap hat; Firewall-Regeln für ufw; Fortsetzen nach einem Abbruch; Deinstallation | |
