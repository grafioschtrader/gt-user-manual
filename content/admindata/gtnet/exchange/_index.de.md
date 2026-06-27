---
title: "Austausch Preisdaten"
date: 2026-01-04T22:54:47+01:00
draft: false
weight: 30
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GTNet ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Diese Ansicht ermöglicht die Konfiguration des Preisdatenaustauschs für einzelne Instrumente. Der Austausch wird für Wertpapiere und Währungspaare in separaten Ansichten konfiguriert.

### Übersicht der Ansichten
Die Konfiguration erfolgt in zwei Tabs:
- **Wertpapiere**: Zeigt alle Wertpapiere mit zusätzlichen Informationen wie ISIN, Ticker-Symbol, Währung, Aktivierungsdatum und Anlageklasse.
- **Währungspaare**: Zeigt alle Währungspaare mit ihrem Namen.

### Die vier Austausch-Optionen
Für jedes Instrument können vier unabhängige Optionen konfiguriert werden:

| Option | Beschreibung |
|--------|--------------|
| **Erhalte Intraday Preis** | Das Instrument empfängt Intraday-Preise von GTNet-Partnern |
| **Erhalte historische Preisdaten** | Das Instrument empfängt historische Kurse von GTNet-Partnern |
| **Sende Intraday Preise** | Die eigenen Intraday-Preise dieses Instruments werden an GTNet-Partner gesendet |
| **Sende historische Preisdaten** | Die eigenen historischen Kurse dieses Instruments werden an GTNet-Partner gesendet |

{{% notice info %}}
Die Empfangs-Optionen («Erhalte...») bestimmen, ob das Instrument beim GTNet-Preisaustausch berücksichtigt wird. Nur Instrumente mit aktivierter Option werden bei der Watchlist-Aktualisierung über GTNet abgefragt.
{{% /notice %}}

### Arbeiten mit der Tabelle

#### Filter und Suche
- **Nur aktive Wertpapiere** (nur bei Wertpapieren): Zeigt nur Wertpapiere an, die aktuell aktiv sind (Aktivierungsdatum liegt in der Zukunft).
- **Suchfeld**: Ermöglicht die Filterung der Tabelle nach beliebigen Begriffen.

#### Massenbearbeitung (nur für Administratoren)
Benutzer mit Administratorrechten können über die Kontrollkästchen im Tabellenkopf alle gefilterten Einträge gleichzeitig ändern:
- Das Aktivieren eines Kopf-Kontrollkästchens setzt die entsprechende Option für **alle aktuell gefilterten Zeilen** auf aktiv.
- Das Deaktivieren setzt die Option für alle gefilterten Zeilen auf inaktiv.

{{% notice note %}}
Die Massenbearbeitung betrifft nur die aktuell sichtbaren (gefilterten) Zeilen. Mit dem Suchfeld kann die Auswahl gezielt eingeschränkt werden.
{{% /notice %}}

#### Speichern der Änderungen
Änderungen werden nicht automatisch gespeichert. Nach dem Bearbeiten muss die **Speichern**-Schaltfläche betätigt werden. Die Schaltfläche ist nur aktiv, wenn Änderungen vorliegen.

Beim Verlassen der Ansicht mit ungespeicherten Änderungen erscheint eine Bestätigungsabfrage.

### Lieferanten-Details
Durch Klicken auf den Erweiterungspfeil einer Zeile werden Details zu den GTNet-Lieferanten angezeigt, die Daten für dieses Instrument bereitstellen:
- **Remote Domain**: Die Domain des GTNet-Servers
- **Server Status**: Der aktuelle Status des Servers (Im Betrieb, Geschlossen, Wartungsmodus)
- **Preistyp**: Ob es sich um Intraday- oder historische Preise handelt
- **Letzte Aktualisierung**: Wann zuletzt Daten von diesem Lieferanten empfangen wurden

#### Rolle bei der Anfrage-Filterung
Die Lieferanten-Details sind nicht nur informativ, sondern spielen eine aktive Rolle im Preisaustauschprozess. Bei Anfragen nach Preisdaten von GTNet-Lieferanten verwendet das System diese Details, um zu filtern, welche Instrumente an welche Lieferanten gesendet werden:

- **Offene Server (AC_OPEN)**: An diese Server werden nur Instrumente gesendet, für die ein Lieferanten-Detail-Eintrag existiert. Wenn für ein Instrument bei einem bestimmten Lieferanten kein Lieferanten-Detail registriert ist, wird dieses Instrument nicht in die Anfragen an diesen Lieferanten aufgenommen. Dies verhindert unnötige Anfragen für Instrumente, die der Lieferant nicht bereitstellen kann.
- **Push-Open Server (AC_PUSH_OPEN)**: Diese Lieferanten erhalten alle Instrumente unabhängig von den Lieferanten-Details. Sie pflegen aktive Verbindungen mit aktuellen Daten und können möglicherweise Daten auch für Instrumente bereitstellen, die noch nicht in ihren Lieferanten-Details registriert sind.

Die Lieferanten-Details werden durch die [Hintergrundaufgabe zur Austausch-Synchronisation](../../taskdatachangemonitor/taskdescription/#23---gtnet-austauschkonfigurationen-synchronisieren) befüllt, die täglich ausgeführt wird und auch manuell über die [GTNet-Einrichtung](../setup/) angestossen werden kann.

### Einschränkungen bei Wertpapieren
Bei **inaktiven Wertpapieren** (Aktivierungsdatum liegt in der Vergangenheit) sind die Intraday-Optionen deaktiviert:
- «Erhalte Intraday Preis» kann nicht aktiviert werden
- «Sende Intraday Preise» kann nicht aktiviert werden

Dies liegt daran, dass für inaktive Wertpapiere keine aktuellen Tageskurse mehr abgerufen werden.

### Kontextmenü
Über das Kontextmenü (Rechtsklick auf eine Zeile) kann das zugrunde liegende Wertpapier oder Währungspaar bearbeitet werden. Dies öffnet den entsprechenden Bearbeitungsdialog.

### Zusammenspiel mit dem Preisaustausch
Die hier konfigurierten Optionen bestimmen das Verhalten beim GTNet-Preisaustausch:
- Instrumente mit aktivierter **«Erhalte...»**-Option werden beim Preisaustausch berücksichtigt und können Daten von Remote-Servern empfangen.
- Instrumente mit aktivierter **«Sende...»**-Option stellen ihre lokalen Daten anderen GTNet-Instanzen zur Verfügung.

Die detaillierten Abläufe des Preisaustauschs sind beschrieben unter:
- [Austausch letzter Preis](../setup/lastprice/)
- [Austausch historische Kurse](../setup/historicalprice/)
