---
title: "Grafioschtrader-Import"
date: 2026-08-28T10:00:00+02:00
draft: false
weight: 25
archetype: "default"
---
GT kann seine **eigenen Exporte** wieder einlesen. Dazu gehören der [CSV-Export der Transaktionen](../../../../reportportfolio/transactionlist/) sowie die [Transaktionsbelege als PDF](../../../../transaction/receipt/). Für diesen Rückimport dient eine besondere, mit GT ausgelieferte **Import Vorlagengruppe** mit dem Namen «**Grafioschtrader**».
{{% notice info %}}
Anders als die Vorlagengruppen für eine Handelsplattform, die Sie selbst erstellen, ist die Vorlagengruppe «Grafioschtrader» **einzigartig** und wird bereits fertig mit GT mitgeliefert. Sie sollte weder bearbeitet noch ein zweites Mal angelegt werden. Eine allgemeine Beschreibung finden Sie unter [Import Vorlagengruppe](../../../../basedata/imptranstemplate/).
{{% /notice %}}
## Voraussetzung
Damit der Rückimport angeboten wird, müssen zwei Dinge zusammentreffen. Ein Administrator muss die mitgelieferte Vorlagengruppe «Grafioschtrader» einmalig für die ganze Installation bestimmt haben, siehe [Import Vorlagengruppe](../../../../basedata/imptranstemplate/), und beim [Klient](../../../client/) muss das Auswahlkästchen «**Grafioschtrader-Importvorlagen freischalten**» gesetzt sein. Erst danach steht die nachfolgend beschriebene Auswahl zur Verfügung.
## Anwendung
Sind beide Voraussetzungen erfüllt, erscheint an den beiden Stellen des Transaktionsimports das Auswahlkästchen «**Grafioschtrader-Importvorlagen verwenden**»: einerseits im Dialog zum Erstellen oder Bearbeiten einer **Importgruppe**, andererseits beim Ablegen eines PDF-Dokuments per **Drag & Drop**. Ist das Kästchen gesetzt, verwendet GT für diesen Import die Vorlagen aus der Vorlagengruppe «Grafioschtrader» statt die Vorlagen der normalen **Handelsplattform** des Depots. Dadurch lässt sich ein GT-Export in ein beliebiges **Depot** einlesen, ohne dessen Zuordnung zur Handelsplattform zu ändern.
## Eine Datei pro Depot
Der CSV-Export erzeugt eine Datei pro **Depot**. Lesen Sie jede Datei in das dazugehörige Depot ein. Damit gelangen die Transaktionen wieder in dasselbe Depot, aus dem sie stammen.
## Kontoüberträge verbinden
Ein **Kontoübertrag** zwischen zwei Portfolios wird beim Export auf zwei Dateien verteilt. Werden diese Dateien getrennt importiert, entstehen zunächst zwei unverbundene Buchungen, eine Auszahlung und eine Einzahlung. Der Menüpunkt «**Kontoüberträge verbinden**» auf **Mandantenebene** in der Auswertung [Transaktionen](../../../../reportportfolio/transactionlist/) stellt die Verbindung nachträglich her. Dabei werden nur eindeutig zusammenpassende Paare anhand von Zeitpunkt und Betrag verbunden.
## Einschränkung
Eröffnungs- und Schliessungspositionen von **Margin-Geschäften** werden im Export gekennzeichnet und beim Import übersprungen. Solche Positionen müssen von Hand erfasst werden.
{{% notice note %}}
Dieser Weg über Export und Import mit den Grafioschtrader-Vorlagen dient auch **Testzwecken**. Damit stehen für die Prüfung des Transaktionsimports jederzeit nachvollziehbare Testdaten zur Verfügung und es müssen keine echten Exporte von Banken verwendet werden.
{{% /notice %}}
