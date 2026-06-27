---
title: "Einstellungen"
date: 2026-06-16T22:54:47+01:00
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
Sie können ihre Daten jederzeit exportieren und diese dann für den Import in ein neues MariaDB-Schema von Grafioschtrader verwenden. Mit diesem Vorgehen können Sie sich von einer geteilten GT-Instanz verabschieden, um eine **neue persönliche Instanz von GT zu starten**, oder die neu erstellte Datenbank mit entsprechenden Werkzeugen selbst bearbeiten bzw. auswerten.

Der Export ist als Umzugshilfe gedacht und wird bewusst klein gehalten: Er enthält alles, was sich nicht wieder beschaffen lässt, und überlässt alles Reproduzierbare den Datenanbietern, welche es nach dem ersten Start der neuen Instanz erneut liefern.

### Inhalt der Exportdatei
Der Export wird als Datei **`gtPersonalData.zip`** heruntergeladen und enthält drei Dateien.
- Die Datei **`gt_ddl.sql`** erstellt das leere Datenbankschema.
- Die Datei **`gt_data.sql`** enthält Ihre eigentlichen Daten.
- Die Datei **`broken_history_connectors.txt`** listet jene noch aktiven Instrumente auf, deren Konnektor für historische Kurse nicht mehr funktioniert. Diese Datei ist immer vorhanden; sind keine solchen Instrumente betroffen, wird dies darin entsprechend vermerkt.

### Was exportiert wird
Nebst Ihren **persönlichen Daten** werden die meisten **geteilten Daten** unabhängig vom Besitz exportiert. Immer enthalten sind Ihre Struktur (Portfolios, Konten, Transaktionen, Watchlists, Daueraufträge, Korrelationen und Transaktions-Importe), die von Ihnen verwendeten Instrumente samt Ihren **privaten Wertpapieren**, die benötigten Währungspaare sowie die **Dividenden und Splits**. Ebenso exportiert werden geteilte Referenzdaten wie Börsen und Handelskalender, Anlageklassen, Steuertabellen, die Definitionen der Datenanbieter und die globalen Einstellungen.

Die **historischen Kursdaten** werden nur dann exportiert, wenn sie sich nicht wieder beschaffen lassen: für nicht mehr aktive Instrumente, für Währungspaare, für Instrumente des risikofreien Zinssatzes, für das bewahrte Altdaten-Archiv sowie neu auch für noch aktive Instrumente, deren Konnektor keine Kurse mehr liefert. Letztere finden Sie in der Datei `broken_history_connectors.txt`.

Nach dem Import **erneut beschafft** werden die historischen Kurse der noch aktiven Instrumente; diese werden einige Minuten nach dem ersten Start des Back-Ends durch die Konnektoren geladen. Die aktuellen Tages- bzw. Intraday-Kurse werden ohnehin laufend live abgerufen, und Ihre Bestände werden beim Import aus den Transaktionen neu berechnet. Nicht exportiert werden Nachrichten sowie die interne Aufgabenliste.

Die folgende Tabelle fasst zusammen, was im Export enthalten ist.

| Daten | Im Export enthalten? |
|-------|----------------------|
| Persönliche Struktur, Transaktionen, Watchlists | Ja |
| Eigene und private Instrumente, Währungspaare | Ja |
| Dividenden und Splits | Ja |
| Geteilte Referenzdaten (Börsen, Anlageklassen, Anbieter, Einstellungen) | Ja |
| Historische Kurse nicht aktiver Instrumente, Währungspaare, Altdaten-Archiv | Ja |
| Historische Kurse aktiver Instrumente mit defektem Konnektor | Ja (neu) |
| Historische Kurse aktiver Instrumente | Nein – nach dem Import neu geladen |
| Intraday-/Tageskurse | Nein – werden live abgerufen |
| Bestände/Positionen | Nein – aus Transaktionen neu berechnet |

{{% notice note %}}
Die historischen Kursdaten eines Instrumentes werden **nicht exportiert**, falls dieses zum Zeitpunkt des Exportes plus einen Monat noch **aktiv** ist; sie werden nach dem ersten Aufstarten des Back-Ends der neuen GT-Instanz nach einigen Minuten durch die entsprechenden Konnektoren geladen. Eine **Ausnahme** bilden noch aktive Instrumente, deren Konnektor für historische Kurse nicht mehr funktioniert: Deren Kurse werden mitexportiert, da sie sich sonst nicht wiederherstellen liessen.
{{% /notice %}}

### Vom Export zur neuen Instanz
Das folgende Diagramm zeigt den gesamten Ablauf vom Export bis zur laufenden neuen Instanz.

{{< mermaid >}}
flowchart TD
    A[Export in GT auslösen] --> B[gtPersonalData.zip]
    B --> C[gt_ddl.sql – leeres Schema]
    B --> D[gt_data.sql – Ihre Daten]
    B --> E[broken_history_connectors.txt – Bericht]
    C --> F[Schema in neuer Datenbank erstellen]
    D --> G[Daten importieren]
    F --> G
    G --> H[Back-End das erste Mal starten]
    H --> I[Konnektoren laden fehlende historische Kurse]
    H --> J[Bestände aus Transaktionen neu berechnet]
    I --> K[Laufende persönliche GT-Instanz]
    J --> K
{{< /mermaid >}}

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
Sie können die GT-Datenbank beispielsweise mit [phpMyAdmin](https://www.phpmyadmin.net/) auswerten bzw. bearbeiten.

### Export als neue GT-Instanz benutzen
Wir erwähnt, können Sie Ihre exportierten Daten als Grundlage für eine neue GT-Instanz benutzen. Für die Installation von GT folgen Sie dem [wiki auf GitHub](https://github.com/grafioschtrader/grafioschtrader/wiki). Nach der Installation kann der Export importiert werden.
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
Sie können ihre Daten zusammen mit dem Benutzerkonto löschen. Nach der Bestätigung dieser Löschaktion werden Sie von Grafioschtrader abgemeldet.

Solange Sie noch andere Personen betreuen oder Ihr Portfolio mit jemandem teilen, ist die Löschung **gesperrt**. Sie müssen zuerst diese Verbindungen auflösen. Betreuen Sie als Berater eigenständige Klienten, müssen Sie diese zuvor löschen; das Vorgehen ist unter [Mehrere Klienten verwalten]({{< relref "/tenantportfolio/client/managedclients" >}}) beschrieben. Haben Sie einer anderen Person Lesezugriff auf Ihr eigenes Portfolio gegeben, müssen Sie diese Freigaben zuvor widerrufen; siehe dazu [Lesezugriff auf das eigene Portfolio teilen]({{< relref "/tenantportfolio/client/sharereadaccess" >}}). Eine Übersicht über beide Funktionen finden Sie unter [Geteilter Zugriff – Grundlagen]({{< relref "/tenantportfolio/client/sharedaccess" >}}). Versuchen Sie die Löschung trotzdem, weist Sie Grafioschtrader mit einer entsprechenden Meldung darauf hin, was noch zu löschen oder zu widerrufen ist.

{{% notice warning %}}
Diese Aktion kann nicht rückgängig gemacht werden!!
{{% /notice %}}
