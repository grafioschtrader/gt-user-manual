---
title: "Transaktionsimport"
date: 2021-03-30T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
## Transaktionsimport
Viele Benutzer handeln schon einige Jahre an der Börse und möchten Ihre Transaktionen möglichst importieren und nicht manuell erfassen. Daher gibt es in GT einen **Transaktionsimport**, für diesen Import gibt es mehrere Möglichkeiten, wobei sich das einte Verfahren besser für einen **initialen** und das andere eher für den **laufenden Import** eignet.

{{% notice note %}}
**Warum der Transaktionsimport auf der Ebene eines einzelnen Depots?** Der CSV-Import ermöglicht auch Kontotransaktionen, trotzdem befindet sich der Transaktionsimport auf der Ebene eines einzelnen Depots. Ein Portfolio kann mehrere Depots enthalten, durch diese erzwungene Vorselektion muss das Depot nicht aus den zu importierenden Dokumenten ermittelt werden.
{{% /notice %}}

### Initialen Import 
Für den initialen Import wird der **CSV-Import** empfohlen, falls die Handelsplattform möglichst ein vollständiges CSV-Dokument anbietet. Leider fehlen solchen Exports oftmals die Gebühren oder Steuerdaten und eignen sich nur eingeschränkt für den Import von Wertpapier Transaktionen. Die andere Alternative ist **GT-Transform-Import**, dabei können alle PDF-Dokumente einer Verzeichnisstruktur eingelesen, anonymisiert und das Resultat als Text-Datei exportiert werden. Diese Text-Datei wird danach in GT hochgeladen.

### Laufender Import
Mit dem laufenden Import ist der Import nach dem initialen Import gemeint, dabei erfolgt der Import einer Transaktion relativ zügig nach deren Ausführung auf der Handelsplattform. Die meisten Handelsplattformen stellen kurze Zeit nach der Ausführung einer Transaktion ein entsprechendes PDF-Dokument zur Verfügung. Dieses PDF-Dokument kann möglicherweise mit Drag & Drop in GT importiert werden.

### Voraussetzung für den Import
Der Import wird erst angeboten, falls das **Depot** mit dem entsprechenden **Handelsplattform Plan** verknüpft ist. Mit dieser Verknüpfung wird der Import mit den entsprechenden **Importvorlagen** aus der **Import Vorlagengruppe** importiert.

### CSV-Import
Vor dem Hochladen des CSV-Dokuments muss bei mehreren Importvorlagen die entsprechende ausgewählt werden. Bei der Auswahl wird die "**templateId**" mit dem Zweck der Vorlage angezeigt. Für die Ermittlung der korrekten Importvorlage, bittet sich an, zuerst die Kopfzeile des CSV-Dokuments mit der Importvorlage zu vergleichen, siehe dazu auch [Import Transaktion Vorlage](../../../basedata/imptranstemplate/createimptranstemplate/).

#### Erste Zeile des CSV-Dokuments
Die erste Zeile der CSV-Datei dient der Zuordnung der **Spalten** zu den entsprechenden **Felder**. Diese Zuordnung basiert auf der Felder-Definition in der Importvorlage. In der Regel muss der Benutzer keine Anpassungen in dieser Zeile vornehmen.

### PDF-Import
Der PDF-Import kann bisher nur auf Wertpapiertransaktionen angewendet werden.

#### GT-PDF-Transform
Mit dieser **Desktop JavaFX Applikation** können eine Vielzahl von PDF-Dokumente transformiert und anonymisiert werden. Für den initialen Import wird diese Anwendung empfohlen, siehe dazu auch "[Import von Wertpapiertransaktion als PDF](../../../transaction/security/)" und "[GT-PDF-Transform](./gtpdftransform)".

#### Mit Drag & Drop PDF nach GT übertragen
Mit **Drag & Drop** kann der Import von wenigen oder einzelner Transaktionen bewerkstelligt werden. Falls der PDF-Import fehlschlägt, wird ein Importgruppe erstellt und in die **Ansicht Transaktionsimport** übergegangen. Dabei wird der **Name Importgruppe** auch aus dem aktuellen Datum und Ortszeit abgeleitet.  
{{% notice warning %}}
Falls Sie Bedenken bezüglich des Datenschutz Ihrer genutzten GT-Instanz haben, sollten Sie diese Variante möglicherweise nicht anwenden.
{{% /notice %}}

### Ihre Dokumente für das GT-Projekt
Möglicherweise gibt es für Ihre **Handelsplattform** in GT noch keine **Importvorlagen**. Diese können Sie selbst erstellen, siehe dazu [Vorgehen beim Erstellen einer PDF-Importvorlage](../../../basedata/imptranstemplate). Möchten Sie diese Arbeit anderen überlassen, so wären wir Ihnen dankbar für Ihre pseudonymisierte CSV- und/oder PDF-Dokumente. Diese können Sie uns gemäss den Instruktionen auf [Import transaction template](//github.com/grafioschtrader/gt-import-transaction-template) zukommen lassen.
{{< youtube xzXmhcBHSHM >}}

### Ansicht Transaktionsimport
**Transaktionsimport** ist eine **Zwischenstufe** nach dem Import und bevor der Import zur Transaktion wird. Nur der erfolgreiche Import mit **Drag & Drop** benötigt die **Ansicht Transaktionsimport** nicht. Alle anderen Arten des Importes werden über diese **Ansicht** erledigt. Diese Ansicht gibt Aufschluss über den **Status** einer potenziellen Transaktion. Zudem können gewisse Korrekturen an **Importposition** angebracht werden. Unterschiedliche Imports derselben potenziellen Transaktionen können diese qualitativ verbessern. Letztendlich wird in dieser Ansicht die **Importposition** zu einer **realen Transaktion** verarbeitet.

#### Erstellen und bearbeiten Importgruppe
Es kann mehrere **Importgruppen** geben, diese werden vom Benutzer oder beim fehlgeschlagenen **Drag & Drop** von **PDF** durch dsa System erzeugt.
- **Erstellen Importgruppe**: Erstellen Sie eine neue Importgruppe. Nebst dem Namen kann auch eine Bemerkung angebracht werden. Innerhalb eines **Depot** muss der **Name** einzigartig sein.
- **Bearbeiten Importgruppe**: Name und Bemerkung können geändert werden.
- **Löschen Importgruppe**: Falls es keine **Importpositionen** innerhalb einer **Importgruppe** mehr gibt, kann diese  gelöscht werden.

#### Voraussetzung für die Transaktionsfähigkeit
Das System führt Zuordnungen und Berechnungen aufgrund der importierten Werte durch. Eine **Importposition** wird **Transaktionsfähig**, falls folgende Schritte erfolgreicht durchgeführt werden können.
- **Wertpapier Zuordnung**: Aufgrund der **ISIN** oder des **Symbol/Ticker** geschieht die Zuordnung eines bestehenden Wertpapieres.
- **Bankkonto Zuordnung**: Mit der Angabe des Wertes von **Feld "cac"**, siehe [Importvorlage](../../../basedata/imptranstemplate/createimptranstemplate/), geschieht die Zuordnung eines existierenden Bankkontos dieses Portfolios.
- **Gesamtsumme Überprüfung**: Aus den Angaben der importierten Transaktion wird die **Gesamtsumme** errechnet, diese muss mit dem entsprechenden Wert des **Feld "ta"** übereinstimmen.

Ein Teil der angebotenten Funktionen auf der **Importposition** können helfen, die oben genannten Bedienungen zu erreichen.

#### Eigenschaften und Tabellenspalten
Bestimmte Tabellenspalten können ein- und ausgeblendet werden, siehe dazu [Bedienelemte](../../../intro/userinterface/user_setting_ui_controls).
- **Hat Transaktion**: Die Importposition wurde schon zu einer Transaktion verarbeitet. 
- **Hat vielleicht Transaktion**: Aufgrund des **Datums der Transaktion**, **Anzahl** und das **Transaktionstype** erkannte daa System eine übereinstimmente existierende Transaktion im aktuellen Depot. Es ist nicht möglich, für eine solche Importposition eine Transaktion zu erzeugen.

#### Funktionen markierte Transaktionsimport-Ansicht
Es gibt Funktionen die losgelöst von einer in der Tabelle ausgewählten Zeile funktionieren, dies sind die alle Funktionen für das Hochladen von Dateien und die oben beschriebenen Funktionen bezüglich der **Importgruppe**.

#### Funktionen auf selektierte Importposition/en
Diese Ansicht unterstützt die Mehrfachauswahl, daher muss jede selektierte Importposition die verlangten Voraussetzung für zu wählende Funktion unterstützen, andernfalls wird der Menüpunkt nicht aktiv geschaltet.

##### Korrekturfunktionen
Die folgenden Funktionen dienen der Korrektur an **Importposition**, damit diese **Transaktionsfähig** werden:
- **Differenz Gesamtsumme akzeptieren**: Dabei wird der Wert der "**Errechnete Gesamtsumme**" akzeptiert und die entsprechende Importposition wird **Transaktionsfähig**. Diese Funktion sollten nur angewendet werden, falls die Abweichung der berechneten zur realen Gesamtsumme sehr gering ist. 
- **Korrigiere Multiplikation auf Gesamtsumme**: Diese Funktion kann möglicherweise das Problem **Transaktionsfähigkeit** der Importposition bezüglich der gescheiterten **Gesamtsumme Überprüfung** beheben.
   + Manchmal ist der Wechselkurs im importierten Dokument nicht exakt angegeben, diese Funktion korrigiert den Wechselkurs entsprechend.
   + In GT ist der **Zins in Prozenten** massgebend für die Berechnung der **Gesamtsumme** des Zinses, dieser wird möglicherweise als Jahreszins augewiesen, obwohl der massgebende Zins eine kürzere Zeitdauer abdeckt. Daher kann diese Funktion den **Zins in Prozenten** entsprechend der **Gesamtsumme** anpassen.
- **Instrument zuweisen**: Eier Wertpapiertransaktion können Sie eine Instrument zuweisen. Dazu öffnet sich der [Suchdialog für Instrumente](../../../watchlistinstrument/instrument/searchdialog).
- **Bankkonto zuweisen**: Hiermit können sie der Importposition ein Bankkonto zuweisen.

{{% notice style="info" title="Automatische Korrektur von Zinsen und Dividenden bei Wertpapieren" %}}
Grundsätzlich erwartet GT eine Transaktion in der Währung des Instruments. Insbesondere ETFs können in verschiedenen Währungen gehandelt werden, obwohl sie nur eine Fondswährung haben. Die Auszahlung von Zinsen oder Dividenden erfolgt in der Fondswährung. In solchen Fällen kann es sein, dass die Handelsplattform keine Währungsumrechnung vornimmt. Beim Import einer solchen Transaktion ergänzt GT die Transaktion mit einem Währungskurs, der anhand des Handelsdatums ermittelt wird. Selbstverständlich werden der Zins- oder Dividendenbetrag und eventuelle Steuern automatisch angepasst. Diese Ergänzung erfolgt bei der Erstellung der Transaktion und ist daher nur bei der erstellten Transaktion sichtbar.
{{% /notice %}}

##### Transaktionsfunktionen
Die folgenden Funktionen beziehen sich auf das Erstellen einer Transaktion.
- **Erstelle Transaktion**: Die Transformation einer Importposition in eine Transaktion ist nur möglich, falls die Importposition noch **keine Transaktion** und keine "**vielleicht Transaktion**" hat.
- **Übergehe die vielleicht Transaktion**: Das System erkannte die Importposition als mögliche Transaktion und hat diese entsprechend so ausgezeichnet, siehe Eigenschaft **Hat vielleicht Transaktion**. Mit dieser Funktion kann diese Erkennung durch das System für die selektierte/n Importpositionen ausgeschaltet werden.
- **Prüfe auf bestehende Transaktion**: Falls die Funktion "**Übergehe die vielleicht Transaktion**" zur Anwendung kam, aktiviert diese Funktion die Prüfung der Importposition auf bestehende mögliche Transaktion.

#### Expandierende Tabellenzeile
Die **expandierende Tabellenzeile** hat zwei unterschiedliche Ansichten:
- **Importposition erkannt**: Das erkannte Ergebnis mit den **Zugeordneten Werte**, die verwendete **Importvorlage** und der **Importstatus** werden angezeigt.
- **Dokument nicht erkannt**: Es gibt eine tabellarische Ansicht mit allen **Importvorlagen** der **Vorlagengruppe**. Dabei erfolgt die Anzeige mit dem letzten erfolgreich erkannten **Feld** pro **Importvorlage**.

#### Transaktionsimport in der Praxis
{{< youtube uKzyETfcWRk >}}
