---
title: "Importvorlage für CSV"
date: 2021-06-27T12:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
Eine **Importvorlage** für eine CSV-Datei macht die Verbindung zwischen den **Tabellenspalten** der CSV-Datei und den für GT verlangten **Felder**. Zusätzlich enthält diese auch CSV-Dokument spezifische **Konfigurationen**. Eine CSV-Datei kann gegenüber dem PDF-Dokument **mehrere Transaktionen** enthalten. Pro Tabellenzeile wird es maximal eine Transaktion in GT erzeugen. Gewisse Transaktionen gehen über mehrere Zeilen, beispielsweise ein Teilverkauf von einem Wertpapier. Auf Grund der Importvorlage muss der Benutzer nicht irgendwelche Anpassungen an Spalten der CSV-Datei vornehmen.
{{% notice note %}}
**Warum ein allgemeiner CSV-Importer nicht die Lösung ist?** Ähnliche Tools wie GT verfügen nur über einen CSV-Importer, d.h. der Benutzer muss eine CSV-Datei gemäss der Vorgabe dieses Tools erstellen. Bei GT nehmen wir Abstand von dieser fehleranfälligen und unnötigen Arbeit in die der Benutzer dadurch gezwungen wird. GT ist die Vermeidung von Bullshit Jobs und somit gibt es die Lösung mit diesen **CSV-Importvorlagen**.
{{% /notice %}}

## Zuordnung Felder
Nebst den allgemeinen Felder gibt es noch zusätzliche Felder die nur sinnvoll von CSV-Dokumenten unterstützt werden können.
- **order**: Dies Feld ist für die Verknüpfung von mehreren Tabellenzeilen vorgesehen oder für den übergreifenden Import mehrere unterschiedliche CSV-Dokumente. Manchmal müssen zwei unterschiedlicher CSV-Dokumente eingelesen werden, damit dies in GT als Transaktion abgebildet werden kann.
- **sn**: Dieses Feld enthält den Namen des Instruments. Zurzeit wird er für die Erkennung des Instruments nicht genutzt.

## Konfiguration
Nebst den allgemeinen Konfiguration werden auf Grund der Dokumentenart noch zusätzliche Konfigurationen benötigt:
- **bond**: Möglicherweise enthält das CSV-Dokument Anleihen mit einen **Kurswert**. Dieser Kurswert könnte mit ein "%" gekennzeichnet sein. Daher verlangt diese Konfiguration den Feldnamen und die entsprechende Kennzeichnung. Beispielsweise mit "quotation|%" wird erwartet, dass die Werte des Feld "quotation" mit einem "%" ergänzt sind.
- **_delimiterField_**: Das Feld-Trennzeichen, welches zur Trennung von Datenfeldern (Tabellenspalten) innerhalb der Datensätze (Zeile) benutzt wird.
- **_templateId_**: Diese unterstützt die eindeutige Zuordnung von Importvorlage zum CSV-Dokument. Sie wird beim Hochladen des CSV-Dokuments zur Selektion angeboten.
- **ignoreLineByFieldValue**: Es ist möglich auf Grund eines **Feldwertes** bestimmte Datenzeilen beim Import zu übergehen. Die Konfiguration **Feld||Zeichenkette** enthält das **Feld** und die entsprechende **Zeichenkette** als Ausschluss der Zeile für den Import. Für die Zeichenkette können auch **reguläre Ausdrücke** verwendet werden Ausnahmsweise wird die Zeichenkette "**||**" als trenner benutzt. Beispielsweise ignoriert die Konfiguration "ignoreLineByFieldValue=ta||-" alle Zeilen die im Feld "ta" ein "-" enthalten.
