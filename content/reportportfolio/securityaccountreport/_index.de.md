---
title: "Depot Report"
date: 2025-10-27T22:54:47+01:00
draft: false
weight: 30
archetype: "default"
---
Der **Depot Report** bietet eine umfassende Übersicht über alle gehaltenen Wertpapierpositionen eines Portfolios oder Mandanten mit flexiblen Gruppierungsmöglichkeiten. Dieser Report ist das zentrale Werkzeug zur Analyse der aktuellen Vermögensstruktur und Performance einzelner Positionen. Alle Werte werden automatisch in die Hauptwährung (Portfolio- oder Mandantenwährung) konvertiert, wobei historische Wechselkurse für eine präzise Bewertung verwendet werden.

## Aufruf des Reports
Der Depot Report kann auf drei Ebenen aufgerufen werden:

1. **Auf Mandantenebene**: Über das Hauptmenü → **Depots und Konten** → **Portfolios** → **Depot Report**
   - Zeigt alle Wertpapierpositionen über alle Portfolios und Depots des Mandanten
   - Werte werden in der Mandantenwährung dargestellt
   - Umfassendste Ansicht über das gesamte Vermögen

2. **Depotebene**: Innerhalb eines einzelnen Portfolios
   - Zeigt nur Positionen des ausgewählten Portfolios
   - Werte werden in der Portfoliowährung dargestellt
   - Fokussierte Ansicht auf Portfolio-spezifische Bestände

2. **Einzelnes Depot**: Innerhalb eines einzelnen Portfolios
   - Zeigt nur Positionen des ausgewählten Portfolios
   - Werte werden in der Portfoliowährung dargestellt
   - Fokussierte Ansicht auf Portfolio-spezifische Bestände

## Was beinhaltet der Report?
Der Report zeigt eine tabellarische Übersicht aller gehaltenen Wertpapierpositionen mit detaillierten Informationen zu Bestand, aktueller Bewertung und Performance. Die Positionen werden nach einem wählbaren Kriterium gruppiert, wobei für jede Gruppe Zwischensummen und am Ende eine Gesamtsumme über alle Positionen angezeigt werden. Der Report berücksichtigt dabei sowohl offene als auch optional geschlossene Positionen und unterstützt verschiedene Finanzinstrumente wie Aktien, Anleihen, ETFs, Fonds, Derivate und Forex.

## Stichdatum (Referenzdatum)
Ein zentrales Merkmal des Depot Reports ist das **Stichdatum** (Until Date), das die Bewertung zu einem bestimmten Zeitpunkt ermöglicht. Das Stichdatum bestimmt, welcher Schlusskurs für die Bewertung der Positionen verwendet wird und welche Transaktionen in die Berechnung einfliessen. Standardmässig wird das aktuelle Datum verwendet, sodass die Positionen mit den aktuellsten verfügbaren Kursen bewertet werden.

Durch Änderung des Stichdatums kann die Portfoliostruktur zu einem beliebigen historischen Zeitpunkt analysiert werden. Dies ist besonders nützlich für Jahresabschlüsse, Steuererklärungen oder die Analyse der Portfolioentwicklung zu bestimmten Stichtagen. Das System verwendet die Schlusskurse des gewählten Datums und berücksichtigt nur Transaktionen, die bis zu diesem Datum ausgeführt wurden. Das Stichdatum wird im Session Storage gespeichert und bleibt über verschiedene Reports hinweg erhalten, bis es explizit geändert oder auf das aktuelle Datum zurückgesetzt wird.

{{% notice tip %}}
Über den Knopf „Heute” neben dem Eingabefeld kann das Datum auf heute zurückgesetzt werden.
{{% /notice %}}

## Spaltenübersicht
Die Haupttabelle zeigt folgende Informationen für jede Wertpapierposition:

| Spalte | Beschreibung |
|--------|-------------|
| **Name** | Name des Wertpapiers mit Instrumentenicon |
| **Instrument** | Icon zur Visualisierung des Finanzinstrument-Typs |
| **Bestand** | Anzahl der gehaltenen Einheiten (bei Anleihen × 100 für Nennwert) |
| **Währung** | Handelswährung des Wertpapiers |
| **Datum/Zeit** | Zeitpunkt des letzten Kurses |
| **Letzter Kurs** | Aktueller Schlusskurs in Wertpapierwährung |
| **Gewinn/Verlust** | Gewinn oder Verlust in Wertpapierwährung |
| **Gewinn/Verlust (MC)** | Gewinn oder Verlust in Mandanten-/Portfoliowährung |
| **Kontowert Wertpapier** | Aktueller Marktwert in Wertpapierwährung |
| **Kontowert Wertpapier (MC)** | Aktueller Marktwert in Mandanten-/Portfoliowährung |

**Wichtig**: Alle monetären Spalten sind farblich hervorgehoben. Positive Werte (Gewinne) erscheinen in Grün, negative Werte (Verluste) in Rot. 
## Gruppierung
Eine besondere Stärke des Depot Reports ist die flexible Gruppierungsfunktion. Über das Dropdown-Menü können Positionen nach verschiedenen Kriterien gruppiert werden:

### Gruppierung nach Währung
Die Standardgruppierung erfolgt nach der Handelswährung der Wertpapiere. Dies ermöglicht eine schnelle Übersicht über die Währungsexposition des Portfolios. Für jede Währung werden die Positionen zusammengefasst und eine Zwischensumme über alle Positionen in dieser Währung gebildet. Diese Gruppierung ist besonders hilfreich zur Identifikation von Währungsrisiken und zur Analyse der geografischen Diversifikation.

### Gruppierung nach Anlageklasse
Bei dieser Gruppierung werden Positionen nach ihrer Anlageklasse (Asset Class Category Type) sortiert. Das System unterscheidet dabei zwischen Aktien, Anleihen, ETFs, Fonds, Derivaten und weiteren Kategorien. Diese Ansicht ermöglicht eine klassische Asset Allocation Analyse und zeigt, wie das Vermögen auf verschiedene Anlageklassen verteilt ist.

### Gruppierung nach Finanzinstrument
Diese Gruppierung erfolgt nach dem Spezialinvestment-Instrument (Special Investment Instrument). Dabei werden Positionen nach spezifischeren Merkmalen wie direkten Aktienanlagen, Index-ETFs, Bond-ETFs, aktiv verwalteten Fonds, CFDs, Forex-Positionen und anderen kategorisiert. Dies erlaubt eine detailliertere Analyse als die reine Anlageklassen-Gruppierung.

### Gruppierung nach Unterkategorie
Die Gruppierung nach Unterkategorie bietet die feinkörnigste Aufteilung. Positionen werden nach ihrer spezifischen Unterkategorie sortiert, beispielsweise nach Anlageklasse und zusätzlich nach Regionen. Diese Gruppierung ermöglicht sehr detaillierte Analysen der Portfoliostruktur.

### Gruppierung nach Anlageklassen-Kombination
Diese kombinierte Gruppierung fasst mehrere Anlageklassen-Attribute zusammen und bietet damit eine ausgewogene Sicht zwischen Übersichtlichkeit und Detailtiefe. Sie eignet sich besonders für Portfolios mit vielfältigen Instrumenten.

## Zwischensummen und Gesamtsumme
Für jede Gruppe werden automatisch Zwischensummen berechnet und in einer separaten Zeile unterhalb der letzten Position der Gruppe angezeigt. Die Zwischensummen-Zeile ist optisch hervorgehoben und zeigt folgende aggregierte Werte:

- Gruppenbezeichnung (z.B. "EUR" bei Währungsgruppierung oder "Aktie" bei Anlageklassen-Gruppierung)
- Wechselkurs (bei Währungsgruppierung)
- Summe der Gewinne/Verluste in Gruppenwährung
- Summe der Gewinne/Verluste in Hauptwährung
- Summe der Kontowerte in Gruppenwährung
- Summe der Kontowerte in Hauptwährung

Am Ende der Tabelle wird eine **Gesamtsumme** über alle Gruppen hinweg angezeigt. Diese Zeile ist besonders hervorgehoben und zeigt die aggregierten Werte des gesamten Portfolios in der Hauptwährung. Die Gesamtsumme ermöglicht einen schnellen Überblick über das Gesamtvermögen und die kumulierte Performance.

## Geschlossene Positionen einblenden
Standardmässig werden nur aktuell offene Positionen mit einem Bestand grösser als Null angezeigt. Über die Menüoption "Zeige geschlossene Positionen" können auch historische, bereits geschlossene Positionen in den Report einbezogen werden. Dies ist nützlich für die Analyse vergangener Investitionen oder zur Nachvollziehbarkeit von Verkaufsentscheidungen.

Geschlossene Positionen werden mit einem leeren Bestand angezeigt und zeigen die realisierten Gewinne oder Verluste. Bei aktivierter Option erscheint ein Häkchen-Symbol neben dem Menüpunkt, und die Tabelle wird automatisch neu geladen und zeigt dann auch die geschlossenen Positionen an.

## Zeilenexpansion für Transaktionsdetails
Jede Position in der Tabelle kann durch Klick auf das Pfeil-Symbol vor dem Wertpapiernamen erweitert werden. In der expandierten Ansicht werden alle Transaktionen angezeigt, die zu dieser Position beigetragen haben. Dies umfasst Käufe, Verkäufe, Dividendenzahlungen und gegebenenfalls Finanzierungskosten bei Margin-Positionen.

Die Transaktionsdetails zeigen die vollständige Historie der Position und ermöglichen direkte Bearbeitungsaktionen. Über das Kontextmenü (Rechtsklick) oder das Hauptmenü können neue Transaktionen erfasst werden:

- **Kaufen**: Neue Kauf-Transaktion für die Position erfassen
- **Verkaufen**: Verkaufs-Transaktion erfassen (nur wenn Bestand vorhanden)
- **Dividende**: Dividendenzahlung erfassen (nicht bei Margin-Produkten)

Nach der Erfassung oder Bearbeitung einer Transaktion wird der Report automatisch neu geladen, und die betroffene Position bleibt expandiert, sodass die Änderungen sofort sichtbar sind.

## Chart-Visualisierung
Über die Menüoption "Zeige Diagramm" kann eine grafische Darstellung der Portfoliostruktur aufgerufen werden. Der Chart öffnet sich in einem separaten Bereich unterhalb der Hauptansicht und bietet verschiedene Visualisierungen je nach gewählter Gruppierung.

Die Chart-Darstellung verwendet Plotly und bietet umfangreiche Interaktionsmöglichkeiten. Sie können in den Chart hineinzoomen, einzelne Datenpunkte per Mauszeiger inspizieren und Datenserien ein- oder ausblenden. Die Hover-Funktion zeigt detaillierte Informationen zu jeder Gruppe oder Position an.

Je nach Gruppierung werden unterschiedliche Chart-Typen verwendet:

**Bei Gruppierung nach Währung**: Ein Kreisdiagramm (Pie Chart) zeigt die prozentuale Verteilung des Vermögens auf die verschiedenen Währungen. Dies ermöglicht eine schnelle visuelle Erfassung der Währungsexposition.

**Bei Gruppierung nach Anlageklasse**: Ein Balkendiagramm zeigt die Gewichtung der verschiedenen Anlageklassen und deren Performance. Positive und negative Gewinne werden farblich unterschiedlich dargestellt.

**Bei anderen Gruppierungen**: Abhängig von der gewählten Gruppierung werden geeignete Visualisierungen generiert, die die Struktur und Performance des Portfolios optimal darstellen.

{{% notice style="info" title="Chart-Synchronisation" %}}
Der Chart wird automatisch aktualisiert, wenn die Gruppierung geändert oder Daten neu geladen werden. Die Chart-Definition wird an die Chart-Komponente weitergereicht und ermöglicht so eine konsistente Darstellung über verschiedene Ansichten hinweg.
{{% /notice %}}

## Kontextmenü und Zusatzfunktionen
Durch Rechtsklick auf eine Position öffnet sich ein Kontextmenü mit zusätzlichen Funktionen:

**Transaktionserfassung**: Die bereits beschriebenen Funktionen zum Erfassen von Käufen, Verkäufen und Dividenden sind auch über das Kontextmenü verfügbar.

**Zeitreihen anzeigen**: Für jedes Wertpapier können historische Kursdaten als Zeitreihe angezeigt werden. Dies öffnet eine neue Ansicht mit einem interaktiven Chart der Kursentwicklung.

**URL-Links**: Für viele Wertpapiere sind externe Links verfügbar, die zu detaillierten Informationen auf Finanzwebseiten führen. Diese werden dynamisch basierend auf dem Wertpapier und seiner Börse generiert.

## Besondere Behandlung von Margin-Produkten
Margin-Positionen wie CFDs oder Forex werden im Report speziell behandelt. Bei diesen Produkten unterscheidet sich der Kontowert vom reinen Marktwert, da Hebel und Sicherheitsleistungen berücksichtigt werden müssen.

Der **Kontowert Wertpapier** zeigt bei Margin-Produkten den tatsächlichen Wert der Position inklusive Gewinn oder Verlust, während der Marktwert die Bewertung ohne Hebelwirkung darstellen würde. Finanzierungskosten (positive oder negative) werden separat als eigener Transaktionstyp erfasst und fliessen in die Gesamtperformance ein.

## Währungskonvertierung und Wechselkurse
Alle Wertpapiere und deren Bewertungen werden automatisch in die Hauptwährung konvertiert. Das System verwendet dabei historische Wechselkurse zum jeweiligen Stichdatum, um eine präzise Bewertung unabhängig von den ursprünglich verwendeten Handelswährungen zu gewährleisten.

Die Konvertierung erfolgt transparent und nachvollziehbar. Bei der Gruppierung nach Währung wird der verwendete Wechselkurs in der Gruppenkopfzeile angezeigt. Für Positionen in der Hauptwährung beträgt der Wechselkurs 1,0.
Spalten ohne Währungs-Suffix zeigen immer Werte in der Originalwährung des Wertpapiers. Spalten mit einem Währungskürzel stellen dagegen die konvertierten Werte in der Hauptwährung dar.

## Typische Anwendungsfälle
Der Depot Report unterstützt verschiedene Analyseansätze für die Verwaltung und Optimierung des Portfolios:

**Vermögensübersicht**: Die wichtigste Funktion ist die schnelle Übersicht über das gesamte investierte Vermögen und dessen aktuelle Bewertung. Die Gesamtsumme zeigt das Portfolio-Gesamtvermögen der Investitionen auf einen Blick.

**Asset Allocation Analyse**: Durch Gruppierung nach Anlageklasse kann überprüft werden, ob die strategische Asset Allocation eingehalten wird oder Anpassungen erforderlich sind.

**Währungsrisiko-Analyse**: Die Gruppierung nach Währung zeigt die Exposition gegenüber verschiedenen Währungen und hilft, Währungsrisiken zu identifizieren und zu steuern.

**Performance-Tracking**: Die Gewinn/Verlust-Spalten zeigen die Performance einzelner Positionen und ermöglichen die Identifikation von Top-Performern und Verlustbringern.

**Rebalancing-Planung**: Durch Vergleich der Ist-Gewichtungen mit den Soll-Gewichtungen können notwendige Umschichtungen identifiziert werden.

**Steuerliche Planung**: Die Übersicht über realisierte und unrealisierte Gewinne unterstützt steuerliche Entscheidungen, beispielsweise zur Nutzung von Verlustverrechnungstöpfen.

**Portfoliobereinigung**: Durch Einblendung geschlossener Positionen kann überprüft werden, welche Wertpapiere bereits verkauft wurden und wie sich vergangene Investitionsentscheidungen entwickelt haben.

## Einschränkungen und Hinweise
Bei der Verwendung des Depot Reports sollten folgende Aspekte beachtet werden:

Die Bewertung basiert auf den verfügbaren Schlusskursen zum gewählten Stichdatum. Fehlende Kursdaten können zu ungenauen Bewertungen führen. Es wird empfohlen, regelmässig die Vollständigkeit der Kursdaten zu überprüfen und gegebenenfalls fehlende Kurse nachzutragen oder den Datenanbieter zu wechseln.

Bei Margin-Produkten werden die Kontowerte basierend auf komplexen Berechnungen ermittelt, die Hebel, Finanzierungskosten und Sicherheitsleistungen berücksichtigen. Diese Berechnungen können je nach Produkt und Broker variieren, und die angezeigten Werte sollten mit den Broker-Abrechnungen abgeglichen werden.

Die Gruppierung und Summenbildung erfolgt ausschliesslich auf Basis der im System hinterlegten Stammdaten der Wertpapiere. Änderungen an Anlageklassen oder Kategorisierungen wirken sich sofort auf die Gruppierung aus.

Geschlossene Positionen werden mit einem Bestand von Null und den realisierten Gewinnen angezeigt. Bei Positionen, die durch mehrere Käufe und Verkäufe vollständig geschlossen wurden, wird die kumulierte Performance über alle Transaktionen hinweg berechnet.

Das Stichdatum wirkt sich global auf alle Reports aus, die dieses Datum verwenden. Eine Änderung des Stichdatums in einem Report beeinflusst auch andere Reports wie den Periodenertrag oder Transaktionskosten-Report.
