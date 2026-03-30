---
title: "Historische Kurse"
date: 2026-01-26T22:54:47+01:00
draft: false
weight: 12
archetype: "default"
---
Wird ein **Instrument** in einer **Watchliste** oder **Depot** selektiert ist die Funktion **Tagesendkurse als Tabelle** verfügbar. Zusätzlich sind sie auch über "**Fehlende Tagesendkurse**" bzw. "**Vollständigkeit historischer Wertpapier-Kursdaten**" erreichbar. Diese zeigt im **Zusatzbereich** eine Tabelle mit den historischen Kursdaten und anderen nützlichen Informationen bezüglich dieser **historischen Kursdaten** an.

## Automatische Aktualisierung nach Börsenschluss
GT bietet zwei Methoden zur automatischen Aktualisierung der historischen Kursdaten. Der Administrator kann über die Einstellung **gt.update.price.by.exchange** in den [globalen Einstellungen](../../../../admindata/globalsettings/) festlegen, welche Methode verwendet wird. Bei einem Wert von **0** wird die zeitgesteuerte Aktualisierung verwendet, bei einem Wert grösser **0** die börsenabhängige Aktualisierung.

### Zeitgesteuerte Aktualisierung (Job 0)
Bei dieser klassischen Methode werden alle Instrumente zu einem festen Zeitpunkt aktualisiert, der in `application.properties` definiert ist. Diese Hintergrundaufgabe führt neben der Kursaktualisierung weitere wichtige Aufgaben aus und kann daher nicht deaktiviert werden. Mehr dazu unter [Hintergrundaufgaben](../../../../admindata/taskdatachangemonitor/taskdescription/).

### Börsenabhängige Aktualisierung
Diese Methode berücksichtigt die individuellen Handelszeiten der verschiedenen Börsen. Die historischen Kursdaten werden für jede Börse separat aktualisiert, sobald eine gewisse Zeit nach deren Handelsschluss vergangen ist. Dies bringt folgende Vorteile:
- **Frühere Datenverfügbarkeit**: Kursdaten von Börsen mit frühem Handelsschluss stehen schneller zur Verfügung, ohne auf später schliessende Börsen warten zu müssen.
- **Bessere Datenqualität**: Die Aktualisierung erfolgt erst, wenn die Kursdaten beim Datenanbieter voraussichtlich vollständig vorliegen.
- **Verteilte Last**: Die Anfragen an die Datenanbieter werden über den Tag verteilt, anstatt alle gleichzeitig zu einem festen Zeitpunkt zu erfolgen.

Das folgende Diagramm zeigt den Ablauf der börsenabhängigen Aktualisierung:
{{< mermaid >}}
flowchart TD
    A[Hintergrundprozess prüft Börsen] --> B{Börse geschlossen + Wartezeit abgelaufen?}
    B -->|Nein| C[Berechne Wartezeit bis nächste Börse bereit]
    C --> D[Warte bis nächste Börse bereit]
    D --> A
    B -->|Ja| E[Hole Kursdaten vom Datenanbieter]
    E --> F{Erfolgreich?}
    F -->|Ja| G[Speichere Kursdaten]
    F -->|Nein| H[Markiere für späteren Wiederholungsversuch]
    G --> A
    H --> A
{{< /mermaid >}}

Instrumente, deren Aktualisierung fehlschlägt, werden für einen späteren Wiederholungsversuch markiert. Dadurch können vorübergehende Probleme beim Datenanbieter automatisch behoben werden.

{{% notice style="info" title="Protokollierung zur Kontrolle" %}}
Für die Überwachung der börsenabhängigen Aktualisierung wird vorübergehend ein Protokoll geführt. Dieses zeigt für jede Börse, wann die Aktualisierung durchgeführt wurde und wie viele Instrumente erfolgreich aktualisiert werden konnten.
{{% /notice %}}

### Zusammenspiel beider Methoden
Auch wenn die börsenabhängige Aktualisierung aktiviert ist, bleibt Job 0 aktiv. Er übernimmt weiterhin wichtige Aufgaben wie die Pflege der Vollständigkeitsdaten, die Aktualisierung der Bestandstabellen und die Erstellung von Aufgaben für Währungspaare ohne historische Kursdaten. Die eigentliche Kursaktualisierung wird bei aktivierter börsenabhängiger Methode jedoch von dieser übernommen.

## Zusätzliche Überwachung historischer Kursdaten
Es gibt einige Funktionen in GT für die Überwachung der **historischen Kursdaten**. Diese finden Sie unter [Vollständigkeit historischer Kursdaten](../../../../admindata/historyquotequality/) oder [Kurs Datenfeed](../../../watchlist/pricefeed/).

## Funktionen auf historischen Kursdaten
Zu den üblichen Bearbeitungsfunktionen einer Informationsklasse gibt es zusätzliche Unterstützung für eine möglichst vollständige Historie der Kursdaten.

### Erstellen und bearbeiten historischer Kursdaten
Nur der **Ersteller** des **Wertpapieres** oder ein Benutzer mit ausreichend Rechten kann historische Kursdaten neu erstellen bzw. löschen.
+ **Erstellen** eines **Historischer Kurses** über **Kontextmenü**.
+ **Bearbeiten** eines **Historischer Kurses** auf dem **selektieren historischen Kurs**.
+ **Löschen** eines **Historischer Kurses** auf dem **selektieren historischen Kurs**.

### Exportieren als CSV-Datei
Die historischen Tagesdaten können exportiert werden. Das Exportformat entspricht dem Importformat.

### Importieren historischer Daten
Das Importformat können Sie dem Exportformat entnehmen. Beim Importieren von historischen Tagesdaten muss das Datum und der Schlusskurs vorhanden sein. Aus der ersten Zeile wird die Zuordnung der Spalte zun den Importfelder ermittelt. Im folgenden Beispiel wurde das Datum in der ersten Spalte und der Schlusskurs in der zweiten Spalte erwartet. Als Feldbegrenzer wird ein **Strichpunkt** erwartet. 
```
date;close;volume;open;high;low
18.02.2021;78;;;;
```
Bestehende historische Tagesdaten können mit einem Import nicht überschrieben werden.

### Lineares befüllen fehlender Kursdaten
Beispielsweise ist es möglich das ein Instrument von der Börse genommen wird, um es mit einem anderen Instrument zu verschmelzen. In dieser Zeit werden keine Kursdaten geliefert, jedoch benötigt GT die für die Berechnung der periodischen Performance möglichst vollständig die historischen Kursdaten. Mit der Funktion **Lineares befüllen fehlender Kursdaten** werden die Kurse der fehlender Handelstage befüllt. Optional können mögliche Wochenende Kurse auf den Freitag derselben Woche verschoben werden, falls dieser Freitag keine Kursdaten hatte.
- Die möglichen Handelstage werden anhand des Handelskalenders des jeweiligen Instruments ermittelt.  Dabei wird auch das Nachführungsdatum des entsprechenden Handelskalenders berücksichtigt. Jüngere Datums als dieses lösen keine Befüllung aus. Berücksichtigt werden die Daten bis zum aktuellen Datum minus 1 Tag bzw. solange das Instrument gehandelt wird.

## Eigenschaften und Tabellenspalten
Die Eigenschaften werden hier nicht weiter besprochen, da sebsterklärend oder durch Quckinfo unterstützt.
