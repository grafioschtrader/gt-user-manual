---
title: "Steuerdaten"
date: 2026-08-24T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
GT unterstützt den Import von **ICTax-Kurslisten** der Eidgenössischen Steuerverwaltung (ESTV). Die importierten Daten ermöglichen einen direkten Vergleich der eigenen Dividenden- und Zinserträge mit den offiziellen Steuerwerten und den Export eines **eCH-0196-Steuerauszugs**. Die Verwaltung der Steuerdaten (Import, Reimport und Löschen) steht ausschliesslich Benutzern mit **Administrator**-Benutzerrechten zur Verfügung. Die Nutzung der importierten Daten (Vergleich und Export) steht jedem Benutzer im Bericht [Dividende und Zins](../../reportportfolio/dividends/) zur Verfügung. Die Ansicht **Steuerdaten** erreichen Sie im Navigationsbereich auf dem statischen Unterelement **Steuerdaten** unter **Administrative Daten**.

{{% notice style="warning" icon="fa fa-wrench" title="In Bearbeitung" %}}
Die Steuerdaten-Funktionalität wurde noch nicht abschliessend geprüft und kann sich noch ändern.
{{% /notice %}}

{{< mermaid >}}
graph TD
    subgraph Admin["Administrator"]
        A[Steuerland erstellen] --> B[Steuerjahr erstellen]
        B --> C[ICTax Kursliste ZIP hochladen]
        C --> D[Import verarbeitet nur<br/>gehaltene ISINs]
        C --> J[Devisenkurse des<br/>Steuerjahrs übernehmen]
        J -.-> K[Kurs bei Bedarf<br/>korrigieren]
        D -.-> E[Reimport nach<br/>neuen Wertpapieren]
    end
    subgraph User["Benutzer"]
        F[Dividende und Zins öffnen]
        F --> G[ICTax-Spalten vergleichen]
        G --> H[Wertpapiere vom<br/>Steuerauszug ausschliessen]
        H --> I[Steuerauszug exportieren<br/>eCH-0196 ZIP]
    end
    D --> F
    J --> F
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
Die importierten ICTax-Daten erscheinen im Bericht [Dividende und Zins](../../reportportfolio/dividends/) als zusätzliche Spalten für den Vergleich und können dort für den Steuerauszug-Export verwendet werden. Diese Spalten erscheinen nur, wenn beim Klienten das [Land](../../tenantportfolio/client/#eigenschaften) auf **Schweiz** gesetzt ist.
{{% /notice %}}

## Devisenkurse der Steuerbehörde
Eine Kursliste enthält nicht nur die Steuerwerte der Wertpapiere, sondern auch die offiziellen Devisenkurse. GT übernimmt diese beim Hochladen automatisch und ordnet sie dem Steuerjahr zu. Es handelt sich dabei in der Regel um die Schlusskurse des letzten Börsentages im Monat Dezember, welche als Steuerwert per 31. Dezember gelten. Da diese Kurse aus einer anderen Quelle und zu einem anderen Zeitpunkt erhoben werden als die Kurse der Datenanbieter von GT, weichen sie meist leicht von den Kursen ab, welche GT für dasselbe Währungspaar geladen hat. Für die Steuererklärung ist der Kurs der Steuerbehörde massgebend, weshalb GT diesen bevorzugt verwendet.
Um die Kurse eines Steuerjahrs zu sehen, klappen Sie die Zeile des Steuerjahrs mit dem Pfeil am Zeilenanfang auf. Darunter erscheint eine Tabelle mit einer Zeile pro Währung und den Spalten **Währung**, **Stückelung**, **Jahresendkurs**, **Jahresmittelkurs**, **Korrektur Jahresendkurs** und **Korrektur Jahresmittelkurs**. Die **Stückelung** gibt an, auf wie viele Einheiten der Fremdwährung sich ein Kurs bezieht; bei Währungen mit kleinem Wert wie dem japanischen Yen oder der dänischen Krone ist das üblicherweise 100. Enthält ein Steuerjahr keine Devisenkurse, lässt sich die Zeile nicht aufklappen.
Die beiden Kurse der Steuerbehörde sind nicht veränderbar. Ein Administrator kann jedoch in den beiden Korrekturspalten einen eigenen Kurs erfassen, beispielsweise wenn der publizierte Kurs für die eigene Berechnung zu grob gerundet ist. Ein erfasster Korrekturwert ersetzt den publizierten Kurs; bleibt das Feld leer, gilt weiterhin der publizierte Kurs. Eine Korrektur bleibt auch dann erhalten, wenn die Kursliste desselben Steuerjahrs später erneut hochgeladen oder reimportiert wird.

{{% notice note %}}
Der **Jahresendkurs** wird im Bericht [Dividende und Zins](../../reportportfolio/dividends/) und im Steuerauszug für die Bewertung von Positionen in Fremdwährung per Jahresende verwendet, sofern beim Klienten das [Land](../../tenantportfolio/client/#eigenschaften) auf **Schweiz** und die Hauptwährung auf **CHF** gesetzt ist. Am deutlichsten wirkt sich das auf Geldkonten in Fremdwährung aus, da für diese in der Kursliste kein eigener Steuerwert existiert. Fehlt für eine Währung ein Kurs, verwendet GT wie bisher den eigenen Jahresendkurs des Währungspaars. Der **Jahresmittelkurs** wird zurzeit lediglich importiert und angezeigt, aber noch nicht für Berechnungen verwendet.
{{% /notice %}}

{{% notice note %}}
Eine Kursliste, welche lediglich die Änderungen gegenüber einer früheren Ausgabe enthält, führt keine Devisenkurse mit. Für ein Steuerjahr sollte deshalb mindestens einmal die vollständige Kursliste hochgeladen werden.
{{% /notice %}}
