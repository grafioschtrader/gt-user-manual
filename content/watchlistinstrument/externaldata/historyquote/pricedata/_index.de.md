---
title: "Historische Kurse"
date: 2025-04-26T22:54:47+01:00
draft: false
weight: 12
archetype: "default"
---
Wird ein **Instrument** in einer **Watchliste** oder **Depot** selektiert ist die Funktion **Tagesendkurse als Tabelle** verfügbar. Zusätzlich sind sie auch über "**Fehlende Tagesendkurse**" bzw. "**Vollständigkeit historischer Wertpapier-Kursdaten**" erreichbar. Diese zeigt im **Zusatzbereich** eine Tabelle mit den historischen Kursdaten und anderen nützlichen Informationen bezüglich dieser **historischen Kursdaten** an.

### Zusätzliche Überwachung historischer Kursdaten
Es gibt einige Funktionen in GT für die Überwachung der **historischen Kursdaten**. Diese finden Sie unter [Vollständigkeit historischer Kursdaten](../../../../admindata/historyquotequality/) oder [Kurs Datenfeed](../../../watchlist/pricefeed/).

### Funktionen auf historischen Kursdaten
Zu den üblichen Bearbeitungsfunktionen einer Informationsklasse gibt es zusätzliche Unterstützung für eine möglichst vollständige Historie der Kursdaten.

#### Erstellen und bearbeiten historischer Kursdaten
Nur der **Ersteller** des **Wertpapieres** oder ein Benutzer mit ausreichend Rechten kann historische Kursdaten neu erstellen bzw. löschen.
+ **Erstellen** eines **Historischer Kurses** über **Kontextmenü**.
+ **Bearbeiten** eines **Historischer Kurses** auf dem **selektieren historischen Kurs**.
+ **Löschen** eines **Historischer Kurses** auf dem **selektieren historischen Kurs**.

#### Exportieren als CSV-Datei
Die historischen Tagesdaten können exportiert werden. Das Exportformat entspricht dem Importformat.

#### Importieren historischer Daten
Das Importformat können Sie dem Exportformat entnehmen. Beim Importieren von historischen Tagesdaten muss das Datum und der Schlusskurs vorhanden sein. Aus der ersten Zeile wird die Zuordnung der Spalte zun den Importfelder ermittelt. Im folgenden Beispiel wurde das Datum in der ersten Spalte und der Schlusskurs in der zweiten Spalte erwartet. Als Feldbegrenzer wird ein **Strichpunkt** erwartet. 
```
date;close;volume;open;high;low
18.02.2021;78;;;;
```
Bestehende historische Tagesdaten können mit einem Import nicht überschrieben werden.

#### Lineares befüllen fehlender Kursdaten
Beispielsweise ist es möglich das ein Instrument von der Börse genommen wird, um es mit einem anderen Instrument zu verschmelzen. In dieser Zeit werden keine Kursdaten geliefert, jedoch benötigt GT die für die Berechnung der periodischen Performance möglichst vollständig die historischen Kursdaten. Mit der Funktion **Lineares befüllen fehlender Kursdaten** werden die Kurse der fehlender Handelstage befüllt. Optional können mögliche Wochenende Kurse auf den Freitag derselben Woche verschoben werden, falls dieser Freitag keine Kursdaten hatte.
- Die möglichen Handelstage werden anhand des Handelskalenders des jeweiligen Instruments ermittelt.  Dabei wird auch das Nachführungsdatum des entsprechenden Handelskalenders berücksichtigt. Jüngere Datums als dieses lösen keine Befüllung aus. Berücksichtigt werden die Daten bis zum aktuellen Datum minus 1 Tag bzw. solange das Instrument gehandelt wird.

### Eigenschaften und Tabellenspalten
Die Eigenschaften werden hier nicht weiter besprochen, da sebsterklärend oder durch Quckinfo unterstützt.