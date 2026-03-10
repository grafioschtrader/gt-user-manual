---
title: "Steuerdaten"
date: 2026-03-09T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
GT unterstützt den Import von **ICTax-Kurslisten** der Eidgenössischen Steuerverwaltung (ESTV). Die importierten Daten ermöglichen einen direkten Vergleich der eigenen Dividenden- und Zinserträge mit den offiziellen Steuerwerten und den Export eines **eCH-0196-Steuerauszugs**. Die Verwaltung der Steuerdaten (Import, Reimport und Löschen) steht ausschliesslich Benutzern mit **Administrator**-Benutzerrechten zur Verfügung. Die Nutzung der importierten Daten (Vergleich und Export) steht jedem Benutzer im Bericht [Dividende und Zins](../../reportportfolio/dividends/) zur Verfügung. Die Ansicht **Steuerdaten** erreichen Sie im Navigationsbereich auf dem statischen Unterelement **Steuerdaten** unter **Administrative Daten**.

{{< mermaid >}}
graph TD
    subgraph Admin["Administrator"]
        A[Steuerland erstellen] --> B[Steuerjahr erstellen]
        B --> C[ICTax Kursliste ZIP hochladen]
        C --> D[Import verarbeitet nur<br/>gehaltene ISINs]
        D -.-> E[Reimport nach<br/>neuen Wertpapieren]
    end
    subgraph User["Benutzer"]
        F[Dividende und Zins öffnen]
        F --> G[ICTax-Spalten vergleichen]
        G --> H[Wertpapiere vom<br/>Steuerauszug ausschliessen]
        H --> I[Steuerauszug exportieren<br/>eCH-0196 ZIP]
    end
    D --> F
{{< /mermaid >}}

## Aufbau der Hierarchietabelle
Die Steuerdaten werden in einer dreistufigen Baumtabelle dargestellt:
- **Land**: Lokalisierter Ländername (z.B. "Schweiz")
- **Steuerjahr**: Jahreszahl innerhalb eines Landes
- **Datei**: Hochgeladene Kurslisten-Datei innerhalb eines Steuerjahrs

Die Tabelle zeigt die Spalten **Name**, **Upload-Datum** und **Datensätze**. Die Spalten Upload-Datum und Datensätze sind nur auf der Datei-Ebene befüllt.

## Erstellen und Verwalten von Steuerdaten
Die Operationen erfolgen über das Kontextmenü und sind abhängig von der selektierten Ebene:
- **Ohne Selektion**: "Steuerland erstellen..." öffnet einen Dialog mit einem Länder-Dropdown zur Auswahl des Steuerlandes.
- **Land selektiert**: "Steuerjahr erstellen..." erstellt ein neues Steuerjahr für das gewählte Land. "Löschen..." entfernt das Land mit allen zugehörigen Steuerjahren und Dateien.
- **Steuerjahr selektiert**: "Steuerdaten hochladen..." öffnet einen Dialog zum Hochladen einer ZIP-Datei mit der ICTax-Kursliste im XML-Format. "Löschen..." entfernt das Steuerjahr mit allen zugehörigen Dateien.
- **Datei selektiert**: "Steuerdaten reimportieren..." verarbeitet die vorhandene Datei erneut gegen die aktuellen ISINs des Mandanten. "Löschen..." entfernt die Datei und alle zugehörigen Steuerdaten.

{{% notice note %}}
Der Import erstellt nur Einträge für ISINs, die der Mandant aktuell hält. Falls nach dem ersten Import neue Wertpapiere hinzugefügt werden, kann ein **Reimport** durchgeführt werden, um die neu passenden ISINs zu erfassen.
{{% /notice %}}

{{% notice note %}}
Die importierten ICTax-Daten erscheinen im Bericht [Dividende und Zins](../../reportportfolio/dividends/) als zusätzliche Spalten für den Vergleich und können dort für den Steuerauszug-Export verwendet werden.
{{% /notice %}}
