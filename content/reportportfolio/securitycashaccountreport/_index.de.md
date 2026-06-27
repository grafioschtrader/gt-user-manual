---
title: "Anlageklassen mit Cash"
date: 2025-11-18T22:54:47+01:00
draft: false
weight: 40
archetype: "default"
---
Der Report «Anlageklassen mit Cash» bietet eine umfassende Portfolio-Allokationsanalyse, die sowohl Wertpapiere als auch Barguthaben nach Anlageklassen gruppiert darstellt. Dieser Report ist das zentrale Werkzeug zur strategischen Vermögensaufteilung, da er zeigt, wie das Gesamtvermögen über verschiedene Anlageklassen wie Aktien, Anleihen, Rohstoffe und Barmittel verteilt ist. Barguthaben werden dabei als spezielle Anlageklassen behandelt, um eine vollständige Übersicht über die gesamte Asset-Allokation zu ermöglichen.

## Funktionsweise
Der Report gruppiert alle Wertpapiere und Barguthaben des Mandanten nach ihren Anlageklassen. Dabei werden Wertpapiere gemäss ihrer tatsächlichen Anlageklasse klassifiziert (z.B. Aktien, Anleihen, Immobilien, Rohstoffe), während Barguthaben als Pseudo-Wertpapiere in speziellen Anlageklassen dargestellt werden. Die Gruppierung erfolgt dabei wie folgt:
- **Wertpapiere**: Werden gemäss ihrer Anlageklasse gruppiert.
- **Barguthaben**: Werden als spezielle Anlageklassen behandelt:
  - **Hauptwährung**: Barguthaben in der Hauptwährung des Mandanten erscheinen als eigenständige Anlageklasse
  - **Fremdwährungen**: Barguthaben in Fremdwährungen werden separat als weitere Anlageklasse dargestellt

Alle Transaktionen bis zum gewählten Datum werden berücksichtigt, um eine zeitpunktgenaue Darstellung der Asset-Allokation zu gewährleisten. Die Berechnung erfolgt dabei über alle Portfolios und Depots des Mandanten hinweg, wobei alle Werte automatisch in die Hauptwährung umgerechnet werden.

## Gruppierung nach Anlageklassen
Die primäre Gruppierung erfolgt nach Anlageklassen, wobei jede Anlageklasse als separate Gruppe mit allen zugehörigen Wertpapieren und Positionen angezeigt wird. Für jede Gruppe werden folgende Informationen ausgewiesen:

- Eine **Gruppenkopfzeile** mit dem Namen der Anlageklasse
- Alle **Wertpapiere** dieser Anlageklasse mit ihren individuellen Positionen
- Eine **Gruppensumme** mit den aggregierten Werten über alle Wertpapiere dieser Anlageklasse

Am Ende des Reports erscheint die **Gesamtsumme** über alle Anlageklassen hinweg, die das gesamte Vermögen des Mandanten repräsentiert.

## Spalten im Report
Die Spalten entsprechend dem [Depot-Bereicht](../securityaccountreport/)

### Gruppensummen und Gesamtsummen
Für jede Anlageklasse werden Zwischensummen angezeigt. Diese Summenzeilen sind farblich hervorgehoben und zeigen die aggregierten Werte aller Wertpapiere und Barpositionen innerhalb der Anlageklasse. Die Gruppensumme umfasst:

- Die Summe aller Gewinne und Verluste der Anlageklasse
- Den Gesamtwert aller Positionen in dieser Anlageklasse

Am Ende des Reports erscheint die Gesamtsumme über alle Anlageklassen hinweg, die das gesamte Vermögen des Mandanten (Wertpapiere plus Barguthaben) in der Hauptwährung ausweist.

## Filteroptionen
Hier verweisen wir auf die [Filteroptionen](../securityaccountreport/) von Depotbericht.

## Kontextmenü und Interaktionen
Durch Rechtsklick auf eine beliebige Stelle in der Tabelle oder über das Menü-Icon öffnet sich das Kontextmenü mit folgenden Funktionen:

### Spalten anzeigen
Diese Funktion öffnet einen Dialog zur Spaltenkonfiguration, der es ermöglicht, einzelne Spalten ein- oder auszublenden. Dies ist besonders nützlich, um die Ansicht auf die relevanten Informationen zu fokussieren oder um bei begrenztem Bildschirmplatz eine übersichtlichere Darstellung zu erreichen.

Die Spaltenkonfiguration wird gespeichert und bleibt auch nach dem Schliessen und erneuten Öffnen des Reports erhalten. So kann jeder Benutzer seine individuelle Ansicht konfigurieren.

Typische Anwendungsfälle für die Spaltenkonfiguration:
- Ausblenden von Detailspalten für eine kompakte Übersicht
- Fokus auf Gewinne und Gesamtwerte
- Anzeige nur der wichtigsten Kennzahlen für eine Schnellübersicht
- Anpassung an verschiedene Bildschirmgrössen oder Auflösungen

### Zeige Diagramm
Diese Funktion aktiviert oder deaktiviert die Diagrammansicht unterhalb der Tabelle. Das Diagramm wird dynamisch basierend auf den aktuellen Daten generiert und bietet eine visuelle Aufbereitung der Asset-Allokation über die verschiedenen Anlageklassen.

### Zeige geschlossene Positionen
Mit dieser Option können auch Wertpapiere angezeigt werden, die zwischenzeitlich vollständig verkauft wurden. Die Option wird mit einem Häkchen markiert, wenn sie aktiv ist. Dies ist besonders nützlich für:
- Analyse der realisierten Gewinne und Verluste
- Steuerliche Auswertungen
- Historische Performance-Analysen
- Vollständige Transaktionsübersicht

## Expandierbare Zeilen
Jede Position kann durch Klick auf das Expander-Symbol expandiert werden. Die expandierte Ansicht zeigt unterschiedliche Informationen, je nachdem ob es sich um ein Wertpapier oder ein Barguthaben handelt:

### Expandierung bei Wertpapieren
In der expandierten Ansicht werden alle **Transaktionen** für dieses Wertpapier angezeigt. Dies ermöglicht eine detaillierte Nachverfolgung aller Käufe, Verkäufe, Dividenden und anderen Transaktionen, die zu der aktuellen Position geführt haben.

Die Transaktionsliste zeigt für jede Transaktion:
- Das Transaktionsdatum
- Den Transaktionstyp (Kauf, Verkauf, Dividende, etc.)
- Die Anzahl der Einheiten
- Den Preis
- Die Transaktionskosten
- Den Gesamtbetrag

### Expandierung bei Barguthaben
Bei den als Pseudo-Wertpapier dargestellten Barguthaben zeigt die expandierte Ansicht alle **Kontotransaktionen**, die zu diesem Barguthaben geführt haben. Dies umfasst:
- Einzahlungen und Auszahlungen
- Interne Überweisungen zwischen Konten
- Kontogebühren
- Zinseinnahmen
- Wertpapiertransaktionen, die das Barguthaben beeinflusst haben

Dies ermöglicht eine lückenlose Nachverfolgung aller Geldbewegungen und zeigt transparent, wie sich das aktuelle Barguthaben zusammensetzt.

### Kontextmenü auf Transaktionen
Bei expandierten Zeilen können die einzelnen Transaktionen bearbeitet werden. Ein Rechtsklick auf eine Transaktion öffnet das Kontextmenü mit entsprechenden Bearbeitungsmöglichkeiten. Weiterführende Informationen unter [Wertpapiertransaktionen](../../../transaction/security/) bzw. [Kontotransaktionen](../../../transaction/cashaccount/).

## Diagrammansicht
Die Diagrammansicht ist ein mächtiges Werkzeug zur visuellen Analyse der Asset-Allokation. Sie wird über die Funktion «Zeige Diagramm» im Kontextmenü aktiviert und erscheint unterhalb der Tabellendarstellung.

### Darstellungsform
Das Diagramm wird als gestapeltes Balkendiagramm oder Tortendiagramm dargestellt, wobei jedes Segment eine Anlageklasse repräsentiert. Die Grösse jedes Segments entspricht dem Anteil dieser Anlageklasse am Gesamtvermögen. Diese Darstellung ermöglicht eine direkte visuelle Erfassung der Vermögensverteilung.

### Interaktive Funktionen
Das Diagramm ist vollständig interaktiv und bietet folgende Möglichkeiten:
- **Hover-Funktion**: Beim Überfahren eines Segments mit der Maus werden detaillierte Informationen angezeigt, einschliesslich des genauen Wertes und prozentualen Anteils der Anlageklasse
- **Zoom**: Durch Klicken und Ziehen kann in das Diagramm hineingezoomt werden
- **Pan**: Im gezoomten Zustand kann das Diagramm verschoben werden
- **Reset**: Durch Doppelklick auf das Diagramm wird die Originalansicht wiederhergestellt
- **Legende**: Die Legende erklärt die Farbcodierung und kann durch Klick auf einzelne Elemente zur Filterung verwendet werden

### Interpretation des Diagramms
Das Diagramm ermöglicht verschiedene Analysen:

**Asset-Allokation**: Die relative Grösse der Segmente zeigt sofort, wie das Vermögen über die verschiedenen Anlageklassen verteilt ist. Eine ausgewogene Verteilung deutet auf eine diversifizierte Anlagestrategie hin.

**Liquiditätsgrad**: Der Anteil der Barguthaben (Hauptwährung und Fremdwährungen) zeigt, wie viel liquide Mittel verfügbar sind. Ein sehr niedriger Anteil kann auf einen hohen Investitionsgrad hinweisen, während ein sehr hoher Anteil auf ungenutztes Kapital hindeutet.

**Diversifikation**: Eine gleichmässige Verteilung über mehrere Anlageklassen deutet auf eine gut diversifizierte Strategie hin, während eine starke Konzentration auf eine oder zwei Anlageklassen ein höheres Klumpenrisiko signalisiert.

**Rebalancing-Bedarf**: Grosse Abweichungen von der angestrebten Asset-Allokation werden sofort sichtbar und können als Signal für ein notwendiges Rebalancing dienen.

### Aktualisierung
Das Diagramm wird automatisch aktualisiert, wenn:
- Das Bis-Datum geändert wird
- Die Option «Zeige geschlossene Positionen» umgeschaltet wird
- Die Daten neu geladen werden

Die Diagrammdarstellung passt sich dabei nahtlos an die neuen Daten an und behält die Farbcodierung bei.

## Typische Anwendungsfälle
Der Report «Anlageklassen mit Cash» unterstützt verschiedene Analyseansätze für die Verwaltung und Optimierung der Asset-Allokation:

### Strategische Asset-Allokation
Der wichtigste Anwendungsfall ist die Überprüfung und Steuerung der strategischen Asset-Allokation. Investoren definieren oft Zielquoten für verschiedene Anlageklassen (z.B. 60% Aktien, 30% Anleihen, 10% Bargeld). Dieser Report zeigt sofort, ob die tatsächliche Allokation von diesen Zielen abweicht und ermöglicht fundierte Rebalancing-Entscheidungen.

### Risikomanagement
Verschiedene Anlageklassen weisen unterschiedliche Risikoprofile auf. Der Report ermöglicht eine schnelle Einschätzung des Gesamtrisikos des Portfolios durch die Verteilung über die Anlageklassen. Eine hohe Konzentration in risikoreichen Anlageklassen (z.B. Aktien, Rohstoffe) deutet auf ein aggressives Profil hin, während eine Betonung von Anleihen und Bargeld ein konservatives Profil signalisiert.

### Liquiditätsmanagement
Die separate Ausweisung von Barguthaben in Hauptwährung und Fremdwährungen ermöglicht eine präzise Liquiditätsplanung. Es ist sofort ersichtlich, wie viel liquide Mittel verfügbar sind und ob diese ausreichend sind für geplante Investitionen oder ob überschüssige Liquidität investiert werden sollte.

### Performance-Attribution
Durch die Gruppierung nach Anlageklassen wird sichtbar, welche Anlageklassen zur Gesamtperformance beitragen und welche Performance-Treiber sind. Dies unterstützt die Analyse, ob die Anlagestrategie erfolgreich ist oder ob Anpassungen notwendig sind.

### Währungsdiversifikation
Bei internationalen Portfolios zeigt der Report nicht nur die Diversifikation über Anlageklassen, sondern auch über Währungen hinweg. Die separate Ausweisung von Fremdwährungsguthaben ermöglicht die Einschätzung des Währungsrisikos.

### Rebalancing-Planung
Grosse Abweichungen von der Ziel-Allokation werden sofort sichtbar. Der Report zeigt, in welchen Anlageklassen Kapital abgezogen und in welchen es investiert werden sollte, um die gewünschte Asset-Allokation wiederherzustellen.

## Besonderheiten
- **Vollständige Vermögensübersicht**: Im Gegensatz zu reinen Wertpapier-Reports, die nur investierte Gelder zeigen, gibt dieser Report ein vollständiges Bild durch die Einbeziehung aller Barguthaben als spezielle Anlageklassen. Dies ermöglicht eine realistische Einschätzung der gesamten Asset-Allokation
- **Behandlung von Barguthaben**: Barguthaben werden als Pseudo-Wertpapiere in speziellen Anlageklassen dargestellt. Dies ermöglicht eine konsistente Darstellung und Berechnung der Asset-Allokation über alle Vermögenswerte hinweg
- **Hauptwährung vs. Fremdwährungen**: Barguthaben in der Hauptwährung des Mandanten werden separat von Fremdwährungsguthaben ausgewiesen. Dies ermöglicht eine differenzierte Betrachtung der Liquidität und des Währungsrisikos
- **Mehrwährungsunterstützung**: Alle Werte werden automatisch in die Hauptwährung des Mandanten umgerechnet, wobei die aktuellen Wechselkurse zum gewählten Stichtag verwendet werden
- **Umfassende Anlageklassen**: Der Report unterstützt alle gängigen Anlageklassen, von traditionellen Aktien und Anleihen über Immobilien und Rohstoffe bis hin zu speziellen Kategorien wie Wandelanleihen und Kreditderivaten
- **Historische Analyse**: Durch die Möglichkeit, ein Bis-Datum zu setzen, können historische Asset-Allokationen analysiert werden. Dies ist wertvoll für die Analyse der Strategieentwicklung über die Zeit oder für steuerliche Auswertungen zu bestimmten Stichtagen
- **Geschlossene Positionen**: Die Option zur Anzeige geschlossener Positionen ermöglicht eine vollständige Übersicht über alle Transaktionen und realisierte Gewinne/Verluste innerhalb eines Zeitraums
- **Transaktionsdetails**: Durch Expandieren der Zeilen können alle zugrundeliegenden Transaktionen sowohl für Wertpapiere als auch für Barguthaben eingesehen werden, was eine lückenlose Nachverfolgung ermöglicht
