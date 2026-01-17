---
title: "Wertpapiertransaktion"
date: 2021-04-22T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
Eine Wertpapiertransaktion beinhaltet auch die Transaktion von einem Instrument. Dabei unterstützt GT unterschiedliche Buchungsansätze. Beispielsweise wird der Handel von Forex anders Verbucht als der Börsendeal mit einem ETF.

## Allgemeine Plausibilitäten
- Die Anzahl in einer Transaktion wird geprüft. Sie können beispielsweise von einer im Depot gehaltenen Aktie nicht mehr Stücke verkaufen als Sie zum Zeitpunkt des Verkaufes halten.

## Bestimmung Basiswährung und Kurswährung
Es muss ein Währungspaar bestimmt werden, falls sich die Währung des Instruments und des Kontos unterscheiden. Die **Basiswährung** wird durch das **Instrument** bestimmt und die **Kurswährung** durch das **Konto**. Hat ein Instrument die Währung USD und das Konto die CHF so ergibt dies das Währungspaar USD/CHF.

### Transaktionsauszug mit unterschiedlicher Basiswährung
Manchmal weisen die Dokumente der Wertpapiertransaktion eine andere **Basiswährung** auf als von **GT** erwartet. In solchen Fällen kann das rechts neben den Eingabefelder des **Kurs/Div/usw**, **Steuer** usw. zusätzliche Eingabefeld für die Entgegennahme des Wertes in der **Kurswährung** genutzt werden. Der Wert dieses Eingabefeldes dient nur der  **Berechnung** des entsprechenden Wertes der Basiswährung. Diese Berechnung geschieht nur vom Wert der **Kurswährung** in Richtung **Basiswährung**, d.h. der Eingabewert der **Basiswährung** wird entsprechend des **Wechselkurses** und dem Eingabewert der **Basiswährung** angepasst.

{{% notice note %}}
Manchmal wird die Dividende nicht in der Währung des Instruments bezahlt oder das **Portfolio** hat kein Konto in der entsprechenden Währung. In solchen Fällen ergibt dies automatisch ein Währungspaar entsprechend den oben ausgeführten Regeln. Der eingegebene bzw. übernommene Wechselkurs sollte ziemlich genau einen Wechselkurs an diesem Tag abbilden, andernfalls führt dies zu ungenauen Berechnungen bezüglich der Währungsgewinne.
{{% /notice %}}

## Unterschiedliche Buchungsansätze
Mit GT können unterschiedliche Anlageprodukte gehandelt werden. Durch die Art der **Transaktion** gibt es grundsätzlich  zwei unterschiedliche Buchungsansätze in GT.

### Buchung des zugrundeliegenden Vermögenswertes
Beim Handel von Aktien, ETF usw. wird der zugrundeliegende **Vermögenswert** beim Kaufen oder Verkaufen **verbucht**.

### Buchung der Preisdifferenz
Bei der Buchung der Preisdifferenz gibt es eine Transaktion, welche die Position öffnet, dazu eine oder mehrere Transaktionen die diese geöffnete Position schliessen. Erst das Schliessen der Position erzeugt eine kontorelevante Buchung, dabei wird die **Preisdifferenz** verbucht. Im weiteren wird diese **Transaktion** in diese Dokumentation auch **Margin basierte Transaktion** genannt.

## Einfluss von Splits
Splits erfordern ihrerseits keiner Aktivität wenn Sie ein Wertpapier zum Zeitpunkt eines Splits halten. GT errechnet die Anzahl gehaltener Einheiten immer entsprechend dem Wertpapier zugeordneten Splits.

## Transaktion erstellen und bearbeiten
In **GT** kann ein bisher nicht gehandeltes Instrument nur erstmalig über eine Watchlist gekauft werden. Andernfalls sind Zukäufe über die Ansicht **Depot** bzw. **benanntes Depot** möglich.

### Kauftransaktion erstellen
Eine bisher nicht gehandeltes Instrument kann nur erstmalig über eine Watchlist gekauft werden. Andernfalls sind **Zukäufe** über die Ansicht **Depot** bzw. **benanntes Depot** möglich. Im Eingabefeld "**Wertpapier**" kann das Instrument der entsprechenden **Watchlist** ausgewählt werden. Vorausgewählt in diesem Eingabefeld ist das in der **Watchlist** selektierte Instrument.

### Bestehende Transaktion bearbeiten und löschen
Die Funktion "**Bearbeiten Transaktion**" über das Kontextmenü wird in vielen unterschiedlichen Ansichten angeboten, dabei muss die entsprechende Transaktion selektiert sein. Es gibt spezielle Ansichten für **Transaktionen** und allgemeine Ansichten die sich pro Tabellenzeile auf ein **Konto** oder ein **Instrument** beziehen. Die zweitere beinhaltet oftmals eine Auflistung der Transaktionen über die expandierende Tabellenzeile. In diesen **expandierten Tabellenzeilen** funktioniert das "**Bearbeiten Transaktion**" auch.

## Eigenschaften und Tabellenspalten
Unabhängig vom Buchungsansatz teilen alle Transaktionen bestimmte Eigenschaften. Diese werden hier beschrieben soweit diese nicht selbsterklärend sind:
- **Wechselkurs**: Siehe oben "**Transaktionsauszug mit unterschiedlicher Basiswährung**".
- **Kontobuchung**: Diese wird berechnet und dient auch der Überprüfung der anderen Eingaben.

## Import von Wertpapiertransaktion als PDF 
Viele Handelsplattformen bieten die ausgeführten Transaktionen als **PDF-Datei** an. Diese können sofort oder einige Stunden nach der Ausführung der Transaktion herunter geladen werden. GT unterstützt den Import solcher **PDF-Dateien**, wobei pro Transaktion ein PDF vorliegen muss. Es ist zu beachten, dass solche PDF-Dateien auf dem **Back-End** verarbeitet werden. Das folgende Flowchart gibt Hinweise bezüglich der Art der Verarbeitung von Transaktionen falls diese als PDF vorliegen. Weitere Hinweise zu diesem Thema finden Sie unter [Import Vorlagengruppe](../../basedata/imptranstemplate/) und [Transaktionsimport](../../tenantportfolio/securityaccounts/transactionimport).
{{< mermaid >}}
graph TB
    A(Wertpapiertransaktion als PDF) --> B{Viele Transkationen?};
    B -- Anzahl >= 10 --> C[GT-PDF-Transform];
    C -- Import --> D[Transaktionsimport];
    B -- Anzahl < 10 --> G{GT Instanz vertrauenswürdig?};
    G -- Nein --> I{Trotzdem Import};
    I -- Ja --> C;
    I -- Nein --> K[Manuell erfassen];
    K --> F[(GT Database)];
    G -- Ja -> Drag and Drop ----> E{Tranaktion/en erkannt?};
    D -- Import ergänzen/korrigieren --> D;
    E -- Nein --> D;
   
{{< /mermaid >}}