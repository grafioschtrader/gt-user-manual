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