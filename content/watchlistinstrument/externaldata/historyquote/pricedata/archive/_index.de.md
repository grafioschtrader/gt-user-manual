---
title: "Archiv der historischen Kursdaten"
date: 2026-06-26T08:00:00+01:00
draft: false
weight: 10
archetype: "default"
---
Wenn sich der **Datenanbieter** eines Wertpapiers ändert oder ein Split eine vollständige Neueinlesung der historischen Kursdaten auslöst, kann ein Teil der älteren Historie beim neuen Anbieter nicht mehr verfügbar sein. Das **Archiv** bewahrt die verdrängten Kurse auf, damit sie wieder in die aktuelle Kursreihe eingefügt werden können. Bestehende Kursdaten gehen so nicht verloren, selbst wenn der neue Anbieter nur eine begrenzte Historie liefert.

## Wann das Archiv gefüllt wird
Das Archiv wird ausschliesslich automatisch gefüllt. Eine manuelle Archivierungsfunktion gibt es bewusst nicht. In zwei Situationen werden die aktuellen Kurse zuerst archiviert und anschliessend die Tabelle aus dem neuen Datenanbieter neu aufgebaut.

### Wechsel des Datenanbieters
Wird das Wertpapier von der bisherigen Börse genommen oder ein anderer Konnektor für die historischen Kursdaten eingestellt, kann der neue Datenanbieter unter Umständen keine Kurse mehr für den älteren Teil der Historie liefern. Bevor die aktuellen Daten verworfen und vom neuen Anbieter geholt werden, kopiert GT die bisherigen Kurse in das Archiv.

### Wiedereinlesung nach einem Split
Wird ein Split auf ein Wertpapier angewendet, werden sämtliche historischen Kursdaten beim aktiven Datenanbieter neu eingelesen. Einige Anbieter liefern jedoch nur Daten der letzten 10 bis 20 Jahre. Ohne Archiv würde die ältere Historie bei jedem Split verloren gehen. Auch hier werden die bisherigen Kurse vor der Neueinlesung in das Archiv übernommen.

## Ablauf
Die folgende Darstellung zeigt den Ablauf vom Auslösen der Aufgabe bis zur erneuerten Kursreihe.

{{< mermaid >}}
flowchart TD
    A[Datenanbieter geändert oder<br/>Job 35 ausgelöst] --> B[Aktuelle Kurse ins Archiv kopiert]
    B --> C[Aktuelle Kurstabelle geleert]
    C --> D[Kurse vom neuen Datenanbieter laden]
    D --> E{Deckt der neue Anbieter<br/>alle archivierten Daten ab?}
    E -->|Ja| F[Archiv wird automatisch geleert]
    E -->|Nein| G[Fehlende Daten aus dem Archiv ergänzen<br/>Splitfaktor wird dabei angewendet]
    G --> H[Archiv bleibt als Sicherung erhalten]
{{< /mermaid >}}

Mehr zur auslösenden Hintergrundaufgabe steht unter [Hintergrundaufgaben]({{< relref "/admindata/taskdatachangemonitor/taskdescription" >}}) bei Job 35.

## Aufruf der Archivansicht
Im Kontextmenü der Ansicht **Historische Kurse** steht der Eintrag **«Archivierte historische Kursdaten anzeigen»** zur Verfügung. In Klammern wird die Anzahl der zurzeit archivierten Tageskurse für dieses Wertpapier angezeigt, zum Beispiel **«Archivierte historische Kursdaten anzeigen (0)»**, wenn noch nichts archiviert wurde. Auch bei leerem Archiv ist der Eintrag verfügbar, damit eine bestehende Sicherung über den Import wieder eingespielt werden kann.

Der Klick wechselt die Anzeige am Ort in die Archivansicht. Mit **«Zurück zu den aktuellen historischen Kursdaten»** im Kontextmenü der Archivansicht kehren Sie wieder zurück.

## Einzelne archivierte Kurse bearbeiten und löschen
Einzelne archivierte Tageskurse lassen sich – wie die aktuellen historischen Kursdaten – bearbeiten und löschen. Auf einem ausgewählten Eintrag bietet das Kontextmenü die Einträge **«Bearbeiten Archivierte historische Kursdaten»** und **«Löschen Archivierte historische Kursdaten»**. Dabei gelten dieselben [Benutzerrechte]({{< relref "/intro/userrights" >}}) wie bei den aktuellen Kursen: Der Besitzer des Wertpapiers sowie Benutzer der Privilegierten- oder Administratoren-Gruppe ändern und löschen direkt, alle übrigen Benutzer können über einen [Datenänderungswunsch]({{< relref "/basedata" >}}) eine Änderung an einem archivierten Kurs vorschlagen, die anschliessend genehmigt werden muss. Das Löschen setzt das Recht zur direkten Bearbeitung voraus. Beim Bearbeiten bleiben der Handelstag und das Transferdatum unveränderlich; nur die Kurswerte lassen sich anpassen. Einzelne Kurse können nicht neu angelegt werden – das Archiv wird ausschliesslich automatisch oder über den Import befüllt.

## Weitere Funktionen in der Archivansicht
Im Kontextmenü stehen zusätzlich folgende Funktionen für das gesamte Archiv zur Verfügung.

- **Split auf archivierte Daten anwenden** — Öffnet einen Dialog für das Erfassen eines nachträglich erkannten Splits (Splitdatum sowie Verhältnis). Es werden nur archivierte Daten vor dem gewählten Splitdatum angepasst. Diese Funktion ist gedacht, wenn ein Split nur beim alten Datenanbieter bekannt war und bei der aktuellen Erfassung des Wertpapiers nicht eingetragen ist.
- **Alle archivierten Daten löschen** — Entfernt nach einer Sicherheitsabfrage sämtliche archivierten Daten dieses Wertpapiers. Erst ausführen, wenn die aktuelle Kursreihe als vollständig betrachtet wird.
- **Import historischer Daten** — Lädt eine zuvor gesicherte CSV-Datei in das Archiv. Geeignet, wenn die archivierten Daten ausserhalb von GT vorliegen und wieder eingespielt werden sollen.
- **Exportieren der Tabellendaten als CSV-Datei** — Lädt das vollständige Archiv dieses Wertpapiers herunter. Das Exportformat entspricht dem Importformat.

## CSV-Format für Export und Import
Die Datei verwendet einen **Strichpunkt** als Feldtrennzeichen. Die erste Zeile enthält die Spaltenüberschriften und legt die Reihenfolge fest; die Spalten dürfen in beliebiger Reihenfolge stehen. Verbindlich sind **date** (Handelstag) und **close** (Schlusskurs); die übrigen Spalten **open**, **high**, **low**, **volume** sowie **transferDate** sind optional. Fehlt **transferDate** in einer importierten Zeile, wird das heutige Datum eingesetzt. Bereits archivierte Daten desselben Tages werden bei einem erneuten Import nicht überschrieben.

{{% notice style="info" title="Automatischer Lebenszyklus" %}}
Das Archiv wird nur dann automatisch gelöscht, wenn der neue Datenanbieter sämtliche zuvor archivierten Tage selbst liefert. Trägt er nur einen Teil bei, bleibt das Archiv als Sicherung erhalten. Die endgültige Entfernung erfolgt in diesem Fall manuell über **«Alle archivierten Daten löschen»**.
{{% /notice %}}
