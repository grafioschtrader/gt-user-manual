---
title: "Stapelverarbeitungs Monitor"
date: 2025-12-27T22:54:47+01:00
draft: false
weight: 40
archetype: "default"
---
GT ist eine **Client-Server-Anwendung**, das Back-End ist für den **24/7 Betrieb** konzipiert. Dies ermöglicht die **sequentielle** Ausführung von unterschiedlichen **Hintergrundaufgaben**. Ob die Ausführung einer solcher Hintergrundaufgabe erfolgreich war, kann mit dem **Stapelverarbeitungs Monitor** überwacht werden. Diese **Hintergrundaufgaben** können durch zwei bestimmte **Ereignisse** entstehen:
- **Zeitgesteuert**: In der Konfigurationsdatei **`application.properties`** sind einige Zeitgesteuerte Aufgaben mit einer Ausführungszeit als Eigenschaft definiert.
- **Datenveränderungen**: Wenn bestimmte Daten ändern, so kann dies Auswirkungen auf andere Daten haben. Beispielsweise falls über den Split-Kalender ein möglicher relevanter Split entdeckt wird, dabei werden eine oder mehrere zusätzliche Hintergrundaufgaben für dessen Abarbeitung erstellt.
  + **Datenänderungen durch Datenquellen**: Gewisse **zeitgesteuerte** Hintergrundaufgaben verändern Daten, welche wiederum eine neue Hintergrundaufgabe erzeugen.
  + **Datenänderungen durch den Benutzer**: Beispielsweise dadurch das der Benutzer bei einem neuen oder bestehenden Instrument den Split- oder Dividenden-Konnektor ändert.

Wird keine Hintergrundaufgabe ausgeführt, so wird in einem Abfragezyklus von 15 Sekunden nach anstehenden Hintergrundaufgabe abgefragt.

## Warum dieser Monitor?
Leider funktioniert fast keine Software fehlerfrei, dies ist bei **GT** nicht anders. Der Stapelverarbeitungs Monitor bietet folgende Vorteile:
- Einfachere Überwachung von möglichen Fehler im einer Hintergrundaufgabe mittels "**Expandierende Tabellenzeile** als über die allgemeine **Logdatei** von GT.
- Manuelles erstellen von Hintergrundaufgaben, damit kann beispielsweise eine **fehlgeschlagene Hintergrundaufgabe** erneut durchgeführt werden.
- Prüfung ob Hintergrundaufgabe überhaupt erstellt wurde und Einsicht über die **Laufzeit** dieser.

{{% notice info %}}
Es ist möglich, dass bestimmte in `application.properties` definierte **zeitgesteuerte Hintergrundaufgaben** losgelöst von diesem **Stapelverarbeitungs Monitor** durchgeführt werden, weil diese nicht von den oben erwähnten Vorteilen profitieren.
{{% /notice %}}

## Eigenschaften und Tabellenspalten
Jeder Benutzer kann den Stapelverarbeitungs Monitor einsehen. Für die **Bearbeitung** sind **Administrator-Benutzerrechten** erforderlich. Die einzelnen Hintergrundaufgaben werden hier nur teilweise beschrieben, gewisse dieser erfordern ein grosses Wissen über die Architektur von GT. Es gibt jedoch Hintergrundaufgaben die sowohl auf **private** wie auch auf **geteilte Daten** ihre Auswirkungen haben. Beispielsweise wirkt die Verarbeitung eines Splits auf die geteilten Daten, weil das entsprechende Wertpapier angepasst werden muss. Jedoch hat dies auch direkte Auswirkungen auf die privaten Daten der Klienten, welche eine Position dieses Wertpapiers besitzen.
- **Informationsobjekt** und ID **Entität**: Manchmal müssen an die Hintergrundaufge bestimmte Parameter übergeben werden, dies geschieht durch die beiden Eigenschaften.
- **Priorität**: Falls die Zeit von mehreren Aufgaben der Eigenschaft "**Frühste Startzeit**" in der Vergangenheit liegt, erfolgt die Reihenfolge der Ausführung gemäss ihrer **Priorität**.
- **Status**: Dies ist der Status des Prozesses, wie "**Warten**", "**Läuft**", "**Ausgeführt**" usw.

### Expandierende Tabellenzeile
Falls der Indikator für eine expandierende Tabellenzeile vorliegt. Konnte die Hintergrundaufgabe nicht Fehlerfrei durchgeführt werden. Diese Informationen können für den Softwareentwickler möglicherweise auf für den Administrator sehr hilfreich sein.

## Erstellen bzw. kopieren und bearbeiten Hintergrundaufgaben
Das **Erstellen** einer Hintergrundaufgabe kommt fast nie zur Anwendung. Eher wird eine fehlgeschlagene Hintergrundaufgabe kopiert und nochmals durchgeführt, nach dem möglicherweise die dazu notwendigen Daten korrigiert wurden.

## Löschen von Hintergrundaufgaben
Nicht laufende Hintergrundaufgen manuell können gelöscht werden. Zusätzlich löscht das System basierend auf der Eigenschaft **Endzeit** auch Hintergrundaufgaben. Die Aufbewahrungszeit in Tagen ist über `gt.task.data.days.preserve`in **Globale Einstellungen** definiert.

## Unterbrechen einer laufenden Hintergrundaufgabe
Möglicherweise muss eine Hintergrundaufgabe unterbrochen bzw. abgebrochen werden. Zurzeit kann nur die laufende Hintergrundaufgabe mit der **ID 0** durch den **Administrator** abgebrochen werden. Es ist nicht garantiert, dass dies in jedem Fall gelingen wird.

{{% notice info %}}
Diese Funktion wurde implementiert, da die täglich durchgeführte Hintergrundaufgabe mit der **ID 0** innerhalb eines Jahres zweimal sich selbst nicht beendet hatte. Zusätzlich wurde ein Timeout eingeführt. Dabei wird vom System versucht diesen Job zu beendigen, beim Misslingen dieser Aktion erhält die Aufgabe den Status **Zombie**. Dabei läuft dieser Thread weiter aber blockiert nicht die anderen noch nicht ausgeführten Aufgaben nicht. Mit dem Neustart dieser GT-Instanz erhalten solche Aufgaben den Zustand **Zombie/Bereinigt**.
{{% /notice %}}

## Filter für Aufgabentypen
Mit zunehmender Anzahl verschiedener Hintergrundaufgaben kann es schwierig werden, sich auf bestimmte Aufgabenkategorien zu konzentrieren. Über das Menü kann ein Filterdialog geöffnet werden, der eine gezielte Auswahl der anzuzeigenden Aufgabentypen ermöglicht. Die ausgewählten Filtereinstellungen werden im Browser gespeichert und bleiben bei zukünftigen Besuchen erhalten. Sind keine Filtereinstellungen vorhanden, werden alle Hintergrundaufgaben angezeigt.

{{< youtube psAdA4b1bGw >}}