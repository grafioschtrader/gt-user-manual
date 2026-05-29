---
title: "GT-PDF-Transform"
date: 2021-08-03T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
GT-PDF-Transform ist eine Open Source Desktop **JavaFX Applikation**, die es ermöglicht, eine Vielzahl von PDF Dateien zu transformieren und mit der nachfolgenden Option einer Anonymisierung. Sie können beispielsweise rekursiv alle PDF-Dateien einer Verzeichnisstruktur einlesen. Danach können in einer Massenverarbeitung die nicht für den Import relevanten Textzeilen entfernt werden. Falls Ihre Wertpapiertransaktion mehrheitlich als PDF-Dateien vorliegen, können Sie diese mit GT-PDF-Transform verarbeiten und danach in GT importieren.

## GT-PDF-Transform installieren
Im GitHub Repository [gt-pdf-transform](//github.com/grafioschtrader/gt-pdf-transform) befinden sich die Information zur Installation und zur Erstellung von **GT-PDF-Transform**.

## Transformieren für Transaktionsimport
GT-PDF-Transform ist für den initialen Import besonders geeignet, siehe dazu [Transaktionsimport](../.) und [Import von Wertpapiertransaktion als PDF](../../../../transaction/security).

### Datenschutz der GT-Instanz
Der Transaktionsimport wird vorwiegend im **Back-End** verarbeitet. Die **Transformation** ist ausreichend, falls Sie keine Bendenken bezüglich des Datenschutzes Ihrer genutzter **GT-Instanz** haben. Andernfalls muss zusätzlich noch eine Anonymisierung bzw. Pseudonymisierung statt finden.

#### Anonymisieren und pseudonymisieren
Beim anonymisieren werden die meisten personenbezogenen Zeilen mach der Transformation aus dem Dokument entfernt, da diese für den erfolgreichen Transaktionsimport irrelevant sind. Diese Zeilen befinden sich meistens am Anfang und Ende eines PDF-Dokumentes. Die Zeilen mit personenbezogenen Texte die sich innerhalb des [auslesbaren Bereiches](../../../../basedata/imptranstemplate/createimptranstemplate/pdf/) befinden, sollten nur mit vorsicht gelöscht werden. Diese Daten sollten vorsichtshalber pseudonymisiert werden.

## Pseudonymisierung für das GT-Projekt
Mit GT-PDF-Transform können transformiert und pseudonymisierte PDF-Dokumente der Versionsverwaltung [Import transaction template](//github.com/grafioschtrader/gt-import-transaction-template) des GT-Projekt auf GitHub zur Verfügung gestellt werden. Dabei sollten in den transformierten PDF-Dokumente die Zeilen nicht gelöscht sondern mit **anonymisierten Werten ersetzt** werden, somit ist das Dokument **pseudonymisiert**. Bitte beachten Sie dazu die Instruktionen auf [Import transaction template](//github.com/grafioschtrader/gt-import-transaction-template).

Das Video **Datenschutz und Pseudonymisierung**:
{{< youtube sn024iNnG_E >}}

## Unterteilung Benutzeroberfläche
Nebst der **Menüleiste** unterteilt sich die Benutzeroberfläche in drei Bereiche, **Dateien**, **Ersetzung** und **PDF**.
- **Dateien**: Die importierten PDF-Dokumente sind in einer Tabelle aufgelistet. Pro Tabellenzeile ein PDF-Dokument.
- **Ersetzung**: Textersetzung sind in dieser Tabelle angezeigt. Die einzelnen Tabellenzellen können bearbeitet werden. 
- **PDF**: Hier wird das transformierte PDF-Dokument gemäss der Selektion von **Dateien** angezeigt. Das rechte Textfeld zeigt das **transformierte PDF mit Ersetzung**.

## Allgemeine Funktionsweise
1. Starten von GT-PDF-Transform
2. Importieren von PDF-Dokumenten mit Menüleiste -> Datei -> "Neuer PDF Import".
   - Es können einzelne PDF-Dateien oder wahlweise rekursiv alle PDF-Dateien einer Verzeichnisstruktur importiert werden.
   - **Verzeichnis importieren**: Es gibt zusätzliche Eingabefelder falls dieser Auswahlkasten markiert ist, andernfalls können mehrere selektierte Dateien importiert werden.
     - **Dateinamensmuster ausschliessen**: Dabei kann ein regulärar Ausdruck für den Ausschluss von Dateinamen angegeben werden. Beispielsweise werden mit dem regulären Ausdruck "(ver|kauf)" alle Dateien die im Namen "ver" oder "kauf" enthalten nicht importiert.
     - **Unterverzeichnisse einbinden**: Mit dieser Option werden die PDF-Dokumenten rekursiv aus allen Unterverzeichnisse importiert.
3. Die Ausführungen in diesem Schritt sind abhängig vom Ziel des Exportes. Er ist für das **anonymisieren und pseudonymisieren** gedacht.
4. Die zu exportierenden Dateien im **Dateien-Bereich** markieren.
5. Exportieren der markierten PDF-Dateien als Text über die Menüleiste -> Datei -> "Export marikerte PDF als Text"

**Das Grundlagenvideo:**
{{< youtube 5f3NuBP8_T8 >}}

### Schritt 3: Anonymisieren und pseudonymisieren
Wie oben erwähnt, sollten bei bestimmten Konstellationen eine Anonymisierung bzw. Pseudonymisierung angewendet werden. Im linken **Textfeld** des Bereichs **PDF** kann mittels dem Kontextmenü die Funktion "Entferne Text" auf einem **markierten Text** ausgeführt werden. Dabei entsteht ein neuer Eintrag im Bereich **Ersetzung**. Im rechten **Textfeld** kann umgehend das Resultat dieser **Ersetzung** betrachtet werden. Die **Ersetzungen** werden auf jedes einzelne Dokument angewendet.

#### Ersetzung bearbeiten
Die Tabellenzeilen des Bereichs **Ersetzung** können editiert werden. Mit einem Einfach-Klick auf die entsprechende Zelle ändert sich der Wert der Zelle beim **Auswahlkasten** bzw. der **Editiermodus** bei einem **Texteingabefeld** wird aktiviert. Die **Eingabetaste** wird für den Zeilenumbruch genutzt, daher wird **Shift** + **Eingabetaste** der **Zellenwert übernommen** und das editieren beendet. Die Benutzung von [regülären Ausdrücken]( //www.tutego.de/blog/javainsel/2018/09/einfuehrung-in-regulaere-ausdruecke-mit-der-java-api/) ist möglich.

{{% notice info %}}
Für den Kenner der Programmiersprache Java, für die Ersetzung kommt die Methode replaceFirst zur Anwendung.
{{% /notice %}}

#### Ersetzung speichern
Der Inhalt des Bereichs **Ersetzung** kann für eine weitere Sitzung gespeichert werden. Die Funktionen für das Speichern und Laden der Ersetzung befinden sich unter Menüleiste -> Datei.
