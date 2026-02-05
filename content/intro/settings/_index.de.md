---
title: "Einstellungen"
date: 2021-05-02T22:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
Die Einstellungen sind unter Menüpunkt **Einstellugen** in der **Menüleiste** verfügbar.

## Passwort ändern
Damit können Sie ihr bestehendes Passwort ändern. Nach der Änderung des Passwortes müssen Sie sich neu anmelden.

## Spitzname und Land/Sprache ändern
Sie können den **Spitznamen** und die Eigenschaft "**Sprache und Land**" anpassen.

## Persönliche Daten exportieren
Sie können ihre Daten jederzeit exportieren und diese dann für den Import in eine neues MariaDB Grafioschtrader Schema importieren. Mit diesem Vorgehen können Sie sich von einer geteilten GT-Instanz verabschieden, um eine **neue persönliche Instanz von GT starten** oder die neu kreierte Datenbank mit entsprechenden Werkzeugen selbst bearbeiten bzw. auswerten. 

### Was exportiert wird
Nebst den **persönlichen Daten** werden mit der Ausnahme der Instrumente die meisten **geteilten Daten** exportiert. Es wird unabhängig des Besitztum einer Entität exportiert.
- **Instrumente**: Exportiert werden die bei Ihnen in den Watchlist und in Transaktionen verwendeten Instrumente, zudem auch die **privaten Wertpapiere**. 

{{% notice note %}}
Die historischen Kursdaten eines Instrumentes werden **nicht exportiert**, falls dieses beim Zeitpunkt des Exportes plus einen Monat noch **aktiv** ist. Die historischen Kursdaten werden mit dem ersten Aufstarten des Back-End der neuen GT-Instanz nach einigen Minuten durch die entsprechenden Konnektoren geladen.
{{% /notice %}}

### Export in Datenbank importieren
Falls Sie den Export gemäss Ihren Bedürfnissen weiter verarbeiten möchten, könnten Sie wie folgt vorgehen. Im folgenden wurde hier die Datenbank **grafioschtrader_s** und nicht **grafioschtrader** genannt.
````
# Unzip file gtPersonalData.zip
unzip gtPersonalData.zip
# Erstellen der Datenbank mit Ihrem gewünschten Passwort für grafioschtrader
mysql -u root -p -e "create database grafioschtrader_s; GRANT ALL PRIVILEGES ON grafioschtrader_s.* TO grafioschtrader@localhost IDENTIFIED BY 'YOUR_DB_PASSWORD'"
# GT-Datenbank Schema erstellen, das Passwort von oben ('YOUR_DB_PASSWORD') wird nachgefragt
mysql -ugrafioschtrader -p --default-character-set=utf8 grafioschtrader_s < gt_ddl.sql
# Import der persönlichen Daten, das Passwort von oben ('YOUR_DB_PASSWORD') wird nachgefragt
mysql -ugrafioschtrader -p --default-character-set=utf8 grafioschtrader_s < gt_data.sql 
# MariaDB-InnoDB hat einen Fehler, siehe MDEV-28327. Daher noch das folgende Kommando:
mysqlcheck -p -a grafioschtrader
````
Sie können die GT-Datenbank beispielsweise mit [phpMyAdmin](//www.phpmyadmin.net/) auswerten bzw. bearbeiten.

### Export als neue GT-Instanz benutzen
Wir erwähnt, können Sie Ihre exportierten Daten als Grundlage für eine neue GT-Instanz benutzen. Für die Installation von GT folgen Sie dem [wiki auf GitHub](//github.com/grafioschtrader/grafioschtrader/wiki). Nach der Installation kann der Export importiert werden.
```
# Beende Grafioschtrader Service falls er läuft 
sudo systemctl stop grafioschtrader.service
# Unzip file gtPersonalData.zip
unzip gtPersonalData.zip
# Löschen der bestehenden Datenbank
mysql -u root -p -D grafioschtrader -e "DROP DATABASE grafioschtrader"
# Erstellen der Datenbank mit Ihrem gewünschten Passwort für grafioschtrader
mysql -u root -p -e "create database grafioschtrader; GRANT ALL PRIVILEGES ON grafioschtrader.* TO grafioschtrader@localhost IDENTIFIED BY 'YOUR_DB_PASSWORD'"
# GT-Datenbank Schema erstellen, das Passwort von oben ('YOUR_DB_PASSWORD') wird nachgefragt
mysql -ugrafioschtrader -p --default-character-set=utf8 grafioschtrader < gt_ddl.sql
# Import der persönlichen Daten, das Passwort von oben ('YOUR_DB_PASSWORD') wird nachgefragt
mysql -ugrafioschtrader -p --default-character-set=utf8 grafioschtrader < gt_data.sql 
# MariaDB-InnoDB hat einen Fehler, siehe MDEV-28327. Daher noch das folgende Kommando:
mysqlcheck -p -a grafioschtrader
# Grafioschtrader Service starten falls er dieser existiert
sudo systemctl start grafioschtrader.service
```
{{% notice warning %}}
Die GT-Instanz bzw. Quelleninstanz des Exportes muss der gleichen oder **tieferen Version** der neuen GT-Instanz bzw. Zielinstanz entsprechen.
{{% /notice %}}
{{< youtube 2xmkteNucfs >}}

## Löschen meiner Daten und des Benutzerkontos
Sie können ihr Daten mit dem Benutzerkonto löschen. Nach der Bestätigung dieser Löschaktion werden Sie von Grafioschtrader abgemeldet.
{{% notice warning %}}
Diese Aktion kann nicht rückgängig gemacht werden!!
{{% /notice %}}
