---
title: "Stapelverarbeitungs Monitor"
date: 2021-05-27T22:54:47+01:00
draft: false
weight : 40
chapter: true
---
## Stapelverarbeitungs Monitor
GT ist eine **Client-Server-Anwendung**, das Back-End ist für den **24/7 Betrieb** konzipiert. Dies ermöglicht die **sequentielle** Ausführung von unterschiedlichen **Hintergrundaufgaben**. Ob die Ausführung einer solcher Hintergrundaufgabe erfolgreich war, kann mit dem **Stapelverarbeitungs Monitor** überwacht werden. Diese **Hintergrundaufgaben** können durch zwei bestimmte **Ereignisse** entstehen:
- **Zeitgesteuert**: In der Konfigurationsdatei **`application.properties`** sind einige Zeitgesteuerte Aufgaben mit einer Ausführungszeit als Eigenschaft definiert.
- **Datenveränderungen**: Wenn bestimmte Daten ändern, so kann dies Auswirkungen auf andere Daten haben. Beispielsweise falls über den Split-Kalender ein möglicher relevanter Split entdeckt wird, dabei werden eine oder mehrere zusätzliche Hintergrundaufgaben für dessen Abarbeitung erstellt.
  + **Datenänderungen durch Datenquellen**: Gewisse **zeitgesteuerte** Hintergrundaufgaben verändern Daten, welche wiederum eine neue Hintergrundaufgabe erzeugen.
  + **Datenänderungen durch den Benutzer**: Beispielsweise dadurch das der Benutzer bei einem neuen oder bestehenden Instrument den Split- oder Dividenden-Konnektor ändert.

Wird keine Hintergrundaufgabe ausgeführt, so wird in einem Abfragezyklus von 15 Sekunden nach anstehenden Hintergrundaufgabe abgefragt.

### Warum dieser Monitor?
Leider funktioniert fast keine Software fehlerfrei, dies ist bei **GT** nicht anders. Der Stapelverarbeitungs Monitor bietet folgende Vorteile:
- Einfachere Überwachung von möglichen Fehler im einer Hintergrundaufgabe mittels "**Expandierende Tabellenzeile** als über die allgemeine **Logdatei** von GT.
- Manuelles erstellen von Hintergrundaufgaben, damit kann beispielsweise eine **fehlgeschlagene Hintergrundaufgabe** erneut durchgeführt werden.
- Prüfung ob Hintergrundaufgabe überhaupt erstellt wurde und Einsicht über die **Laufzeit** dieser.

{{% notice info %}}
Es ist möglich, dass bestimmte in `application.properties` definierte **zeitgesteuerte Hintergrundaufgaben** losgelöst von diesem **Stapelverarbeitungs Monitor** durchgeführt werden, weil diese nicht von den oben erwähnten Vorteilen profitieren.
{{% /notice %}}

### Eigenschaften und Tabellenspalten
Jeder Benutzer kann den Stapelverarbeitungs Monitor einsehen. Für die **Bearbeitung** sind **Administrator-Benutzerrechten** erforderlich. Die einzelnen Hintergrundaufgaben werden hier zurzeit nicht beschrieben, dies verlangt teilweise ein grosses Wissen über die Architektur von GT. Es gibt jedoch Hintergrundaufgaben die sowohl auf **private** wie auch auf **geteilte Daten** ihre Auswirkungen haben. Beispielsweise wirkt die Verarbeitung eines Splits auf die geteilten Daten, weil das entsprechende Wertpapier angepasst werden muss. Jedoch hat dies auch direkte Auswirkungen auf die privaten Daten der Klienten, welche eine Position dieses Wertpapiers besitzen.
- **Informationsobjekt** und ID **Entität**: Manchmal müssen an die Hintergrundaufge bestimmte Parameter übergeben werden, dies geschieht durch die beiden Eigenschaften.
- **Priorität**: Falls die Zeit von mehreren Aufgaben der Eigenschaft "**Frühste Startzeit**" in der Vergangenheit liegt, erfolgt die Reihenfolge der Ausführung gemäss ihrer **Priorität**.
- **Status**: Dies ist der Status des Prozesses, wie "**Warten**", "**Läuft**", "**Ausgeführt**" usw.

#### Expandierende Tabellenzeile
Falls der Indikator für eine expandierende Tabellenzeile vorliegt. Konnte die Hintergrundaufgabe nicht Fehlerfrei durchgeführt werden. Diese Informationen können für den Softwareentwickler möglicherweise auf für den Administrator sehr hilfreich sein.

### Erstellen bzw. kopieren und bearbeiten Hintergrundaufgaben
Das **Erstellen** einer Hintergrundaufgabe kommt fast nie zur Anwendung. Eher wird eine fehlgeschlagene Hintergrundaufgabe kopiert und nochmals durchgeführt, nach dem möglicherweise die dazu notwendigen Daten korrigiert wurden.

### Löschen von Hintergrundaufgaben
Nicht laufende Hintergrundaufgen manuell können gelöscht werden. Zusätzlich löscht das System basierend auf der Eigenschaft **Endzeit** auch Hintergrundaufgaben. Die Aufbewahrungszeit in Tagen ist über `gt.task.data.days.preserve`in **Globale Einstellungen** definiert.

### Unterbrechen einer laufenden Hintergrundaufgabe
Möglicherweise muss eine Hintergrundaufgabe unterbrochen bzw. abgebrochen werden. Zurzeit kann nur die laufende Hintergrundaufgabe mit der **ID 0** durch den **Administrator** abgebrochen werden. Es ist nicht garantiert, dass dies in jedem Fall gelingen wird.

{{% notice info %}}
Diese Funktion wurde implementiert, da die täglich durchgeführte Hintergrundaufgabe mit der **ID 0** innerhalb eines Jahres zweimal sich selbst nicht beendet hatte.
{{% /notice %}}

{{< youtube psAdA4b1bGw >}}

### Ein Beispiel des Zusammenspiel von Hintergrundaufgaben
Für die **Erkennung** und **Nachführung** von **Splits** in GT aus externen Datenquellen, sind unterschiedliche **Hintergrundaufgaben** beteiligt. Es kann mehrere Tage dauern, bis der aus einem Split-Kalender gelesene Split auch beim entsprechenden Wertpapier der externen Datenquellen korrekt abgebildet ist. Im folgenden ist dieser Prozess in einem vereinfachten Flowchart dargestellt, wobei der Hexagon-Knoten für eine Hintergrundaufgabe steht:
{{< mermaid >}}
graph TD;
    A{{Zeitgesteuert täglich}} --> B(Ein oder mehrere Split-Kalender lesen)
    B --> |Möglicher Split für ein Wertpapier gefunden| C{{+2 Minuten: Datenveränderung}}
    C --> D(Split des entsprechenden Wertpapiers einlesen)
    D --> |Problematik Name Wertpapiers im Kalender und bei GT| E{Split gefunden?}
    E -->|Nein| G{{+ 1 Tag: Datenveränderung}}
    G --> D
    E -->|Ja| H{{+ 1 Tag Datenveränderung}}
    H --> I(Historische Kursdaten des Wertpapiers einlesen)
    I --> |GT benötigt Split bereinigte Kursdaten| K{Enthalten historische Kursdaten diesen Split?}
    K --> |Nein| H
    K --> |Ja| L{{+0: Datenveränderung}}
    L --> M(Bestand des betreffenden Wertpapiers der betroffenen Klienten für diesen Split nachführen)
{{< /mermaid >}}
Es ist möglich das dieser Prozess den Namen des Wertpapiers aus dem Split-Kalender falsch erkennt, d.h. es wird versucht, einen Split auf ein in GT bestehendes Wertpapier anzuwenden. Damit wird erfolglos täglich das entsprechende Wertpapier nach diesem Split geprüft. In einem solchen Fall sollte die entsprechende wartende Hintergrundaufgabe mittels diesem Monitor gelöscht werden.
