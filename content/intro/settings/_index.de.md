---
title: "Einstellungen"
date: 2026-10-20T12:00:00+02:00
draft: false
weight: 35
archetype: "default"
---
Die Einstellungen sind unter Menüpunkt **Einstellungen** in der **Menüleiste** verfügbar.

## Passwort ändern
Damit können Sie Ihr bestehendes Passwort ändern. Geben Sie zuerst unter **Bisheriges Passwort** Ihr aktuelles Passwort ein und danach das **Neue Passwort** ein zweites Mal unter **Neues Passwort bestätigen**. Stimmen die beiden Eingaben nicht überein, lässt sich der Dialog nicht speichern. Welche Anforderungen ein Passwort erfüllen muss, legt der Administrator der Instanz fest; die Meldung unter dem Eingabefeld nennt die geltende Regel. Standardmässig besteht ein Passwort aus mindestens acht Zeichen mit mindestens einem Buchstaben und einer Zahl. Ist das bisherige Passwort falsch, wird die Änderung abgewiesen.
Nach einer erfolgreichen Änderung werden Sie abgemeldet und melden sich mit dem neuen Passwort wieder an. Ein Demo-Konto kann sein Passwort nicht ändern.
{{% callout note %}}
Verschärft der Administrator die Passwortregel, kann Ihr bestehendes Passwort diese nicht mehr erfüllen. In diesem Fall öffnet sich der Dialog **Passwort ändern** gleich nach der Anmeldung von selbst und lässt sich erst schliessen, wenn Sie ein neues, gültiges Passwort gesetzt haben.
{{% /callout %}}

## Spitzname und Land/Sprache ändern
In diesem Dialog passen Sie drei persönliche Eigenschaften an. Der **Spitzname** ist zwischen 2 und 30 Zeichen lang und muss auf der Instanz eindeutig sein; ist er bereits vergeben, wird die Änderung abgewiesen. Unter **Sprache und Land** wählen Sie die Sprache der Benutzeroberfläche und zugleich die landesübliche Darstellung von Zahlen und Datumsangaben. Mit **Entitätsbesitzung sichtbar** werden in geteilten Daten, etwa bei Instrumenten oder Börsen, jene Einträge hervorgehoben, die Sie selbst erfasst haben; in Tabellen erscheinen diese beispielsweise in Fettschrift.
Nach dem Speichern werden Sie abgemeldet. Die Änderungen gelten ab der nächsten Anmeldung.

## Sprachmodell-Schlüssel
Unter **Sprachmodell-Schlüssel** verwalten Sie die Anbieter für KI-gestützte Vorschläge Ihrer Zusatzfelder. Die Schlüssel gehören zu Ihrem Heimatmandanten. Anfragen mit diesen Schlüsseln werden bei Ihrem Anbieter abgerechnet. Der Administrator bestimmt, welche Anbieter zur Auswahl stehen; Ollama nutzt die Hardware der Instanz und muss ausdrücklich freigegeben werden.
Wählen Sie **Anbieter hinzufügen**, einen Anbieter und optional ein Modell. Ohne Modellangabe gilt das in der Tabelle angezeigte Standardmodell. Eine kleinere Zahl bei **Reihenfolge Auswertung** wird zuerst versucht. Pro Anbieter ist ein Eintrag möglich, standardmässig höchstens vier insgesamt. Über **Aktiv** können Sie einen Anbieter ausschalten, ohne seinen Schlüssel zu löschen. Mit **Tägliches Abfragelimit** begrenzen Sie die Anfragen pro UTC-Tag über diesen Schlüssel und damit Ihre Kosten; leer heisst kein Limit. Ist das Limit erreicht, versucht GT den nächsten Anbieter der Reihenfolge, und erst wenn alle Schlüssel ihr Limit erreicht haben, hält ein Lauf an. Die Spalte **Anfragen heute** zeigt den Verbrauch des laufenden Tages, fehlgeschlagene Versuche eingeschlossen. Unabhängig davon begrenzt die Instanz die Anfragen jedes Benutzers pro Tag, um ihre Rechenzeit zu schützen.
Der Schlüssel wird verschlüsselt gespeichert. Die Tabelle zeigt unter **Schlüsselende** nur seine letzten vier Zeichen. Beim Bearbeiten zeigt das Schlüsselfeld das gespeicherte Schlüsselende als Platzhalter, denn der Schlüssel selbst wird nie an den Browser zurückgegeben; ein leer gelassenes Schlüsselfeld bleibt unverändert. Mit **Schlüssel testen** prüfen Sie ausschliesslich den gewählten Eintrag, auch wenn er inaktiv ist. Der Test zählt zum Abfragelimit des Schlüssels und zum persönlichen Tageslimit der Instanz und zeigt Fehler des Anbieters, etwa einen ungültigen Schlüssel, fehlendes Guthaben oder ein nicht verfügbares Modell.
In einer Simulationsumgebung oder einem verwalteten Mandanten ist dieser Menüpunkt ausgeblendet und die Schlüsselverwaltung gesperrt. KI-Anfragen verwenden auch dort die Schlüssel Ihres Heimatmandanten. Schlüssel werden weder exportiert noch bei einem Mandantentransfer übertragen; richten Sie sie in einer neuen Instanz erneut ein. Beim Löschen des Kontos werden die Schlüssel des Heimatmandanten entfernt.

## KI-Assistenten (MCP)
Über diesen Menüpunkt erlauben Sie einem KI-Assistenten wie Claude Code oder ChatGPT, Ihre Portfolios zu lesen. Er erscheint nur, wenn der Administrator den MCP-Server geöffnet hat. Die Beschreibung finden Sie unter [KI-Assistenten (MCP)]({{% relref "/intro/settings/aiassistant" %}}).
## Persönliche Daten exportieren
Sie können ihre Daten jederzeit exportieren und diese dann für den Import in ein neues MariaDB-Schema von Grafioschtrader verwenden. Mit diesem Vorgehen können Sie sich von einer geteilten GT-Instanz verabschieden, um eine **neue persönliche Instanz von GT zu starten**, oder die neu erstellte Datenbank mit entsprechenden Werkzeugen selbst bearbeiten bzw. auswerten.

Der Export ist als Umzugshilfe gedacht und wird bewusst klein gehalten: Er enthält alles, was sich nicht wieder beschaffen lässt, und überlässt alles Reproduzierbare den Datenanbietern, welche es nach dem ersten Start der neuen Instanz erneut liefern.

### Inhalt der Exportdatei
Der Export wird als Datei **`gtPersonalData.zip`** heruntergeladen und enthält drei Dateien.
- Die Datei **`gt_ddl.sql`** erstellt das leere Datenbankschema.
- Die Datei **`gt_data.sql`** enthält Ihre eigentlichen Daten.
- Die Datei **`broken_history_connectors.txt`** listet jene noch aktiven Instrumente auf, deren Konnektor für historische Kurse nicht mehr funktioniert. Diese Datei ist immer vorhanden; sind keine solchen Instrumente betroffen, wird dies darin entsprechend vermerkt.

### Was exportiert wird
Nebst Ihren **persönlichen Daten** werden die meisten **geteilten Daten** unabhängig vom Besitz exportiert. Immer enthalten sind Ihre Struktur (Portfolios, Konten, Transaktionen, Watchlists, Daueraufträge, Korrelationen und Transaktions-Importe), die von Ihnen verwendeten Instrumente samt Ihren **privaten Wertpapieren**, die benötigten Währungspaare sowie die **Dividenden und Splits**. Zur Struktur gehört auch der [Regelbasierte Handel]({{% relref "/algoalert/algo" %}}) mit seiner Hierarchie, den Strategien und ihren Einstellungen, den Rebalancing-Empfehlungen und dem Stand laufender Strategien. Die darin verwendeten Instrumente werden gleich behandelt wie jene einer Watchlist und samt Dividenden, Splits und abgeleiteten Instrumenten exportiert. Ebenso exportiert werden geteilte Referenzdaten wie Börsen und Handelskalender, Anlageklassen, Steuertabellen, die Definitionen der Datenanbieter und die globalen Einstellungen.

Die **historischen Kursdaten** werden nur dann exportiert, wenn sie sich nicht wieder beschaffen lassen: für nicht mehr aktive Instrumente, für Währungspaare, für Instrumente des risikofreien Zinssatzes, für das bewahrte Altdaten-Archiv sowie neu auch für noch aktive Instrumente, deren Konnektor keine Kurse mehr liefert. Letztere finden Sie in der Datei `broken_history_connectors.txt`.

Nach dem Import **erneut beschafft** werden die historischen Kurse der noch aktiven Instrumente; diese werden einige Minuten nach dem ersten Start des Back-Ends durch die Konnektoren geladen. Die aktuellen Tages- bzw. Intraday-Kurse werden ohnehin laufend live abgerufen, und Ihre Bestände werden beim Import aus den Transaktionen neu berechnet. Nicht exportiert werden Nachrichten, die Alarmmeldungen der Strategien sowie die interne Aufgabenliste. Ebenfalls nicht exportiert werden die [Simulationsumgebungen]({{% relref "/algoalert/historicalrun/environment" %}}) samt den Ergebnissen und dem Ereignisprotokoll ihrer historischen Wiederholungen. Diese erzeugt Grafioschtrader selbst; in der neuen Instanz erhalten Sie sie durch eine erneute Simulation wieder.

Die folgende Tabelle fasst zusammen, was im Export enthalten ist.

| Daten | Im Export enthalten? |
|-------|----------------------|
| Persönliche Struktur, Transaktionen, Watchlists | Ja |
| Eigene und private Instrumente, Währungspaare | Ja |
| Dividenden und Splits | Ja |
| Regelbasierter Handel: Hierarchie, Strategien, Empfehlungen und deren Instrumente | Ja |
| Geteilte Referenzdaten (Börsen, Anlageklassen, Anbieter, Einstellungen) | Ja |
| Historische Kurse nicht aktiver Instrumente, Währungspaare, Altdaten-Archiv | Ja |
| Historische Kurse aktiver Instrumente mit defektem Konnektor | Ja (neu) |
| Historische Kurse aktiver Instrumente | Nein – nach dem Import neu geladen |
| Intraday-/Tageskurse | Nein – werden live abgerufen |
| Bestände/Positionen | Nein – aus Transaktionen neu berechnet |
| Simulationsumgebungen, Ergebnisse und Ereignisprotokoll der Wiederholung | Nein – durch eine erneute Simulation wiederherstellbar |

{{% callout note %}}
Die historischen Kursdaten eines Instrumentes werden **nicht exportiert**, falls dieses zum Zeitpunkt des Exportes plus einen Monat noch **aktiv** ist; sie werden nach dem ersten Aufstarten des Back-Ends der neuen GT-Instanz nach einigen Minuten durch die entsprechenden Konnektoren geladen. Eine **Ausnahme** bilden noch aktive Instrumente, deren Konnektor für historische Kurse nicht mehr funktioniert: Deren Kurse werden mitexportiert, da sie sich sonst nicht wiederherstellen liessen.
{{% /callout %}}

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

Das Laden der historischen Kurse setzt voraus, dass die API-Keys der Konnektoren in der neuen Instanz erfasst sind, und kann lange dauern. Was dabei zu beachten ist, beschreibt die Seite [Historische Kurse nach dem Import]({{% relref "/intro/settings/historicalpriceload" %}}).

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
{{% callout warning %}}
Die GT-Instanz bzw. Quelleninstanz des Exportes muss der gleichen oder **tieferen Version** der neuen GT-Instanz bzw. Zielinstanz entsprechen.
{{% /callout %}}
{{< youtube 2xmkteNucfs >}}

## Löschen meiner Daten und des Benutzerkontos
Sie können ihre Daten zusammen mit dem Benutzerkonto löschen. Nach der Bestätigung dieser Löschaktion werden Sie von Grafioschtrader abgemeldet. Mit dem Benutzerkonto werden auch alle Ihre [Simulationsumgebungen]({{% relref "/algoalert/historicalrun/environment" %}}) samt ihren Ergebnissen gelöscht.

Solange Sie noch andere Personen betreuen oder Ihr Portfolio mit jemandem teilen, ist die Löschung **gesperrt**. Sie müssen zuerst diese Verbindungen auflösen. Betreuen Sie als Berater eigenständige Klienten, müssen Sie diese zuvor löschen; das Vorgehen ist unter [Mehrere Klienten verwalten]({{% relref "/tenantportfolio/client/managedclients" %}}) beschrieben. Haben Sie einer anderen Person Lesezugriff auf Ihr eigenes Portfolio gegeben, müssen Sie diese Freigaben zuvor widerrufen; siehe dazu [Lesezugriff auf das eigene Portfolio teilen]({{% relref "/tenantportfolio/client/sharereadaccess" %}}). Eine Übersicht über beide Funktionen finden Sie unter [Geteilter Zugriff – Grundlagen]({{% relref "/tenantportfolio/client/sharedaccess" %}}). Versuchen Sie die Löschung trotzdem, weist Sie Grafioschtrader mit einer entsprechenden Meldung darauf hin, was noch zu löschen oder zu widerrufen ist.

{{% callout warning %}}
Diese Aktion kann nicht rückgängig gemacht werden!!
{{% /callout %}}
