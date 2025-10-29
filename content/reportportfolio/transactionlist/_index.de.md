---
title: "Transaktionen"
date: 2025-10-28T22:54:47+01:00
draft: false
weight: 60
archetype: "default"
---

Der **Transaktionen-Report** bietet eine umfassende Übersicht aller finanziellen Transaktionen innerhalb eines Mandanten. Dieser Report ermöglicht es, alle Wertpapier- und Kontotransaktionen zu verwalten, zu filtern und zu analysieren. Er ist das zentrale Werkzeug für die Überwachung aller Ein- und Austräge im Portfolio.

## Aufruf des Reports
Der Transaktionen-Report kann auf mehreren Ebenen aufgerufen werden:

1. **Auf Mandantenebene**: Über das Hauptmenü → **Depots und Konten** → **Portfolios** → **Transaktionen**
   - Zeigt alle Transaktionen über alle Portfolios und Konten des Mandanten
   - Umfassendste Ansicht aller Finanzbewegungen

2. **Auf Portfolioebene**: Innerhalb eines einzelnen Portfolios → **Transaktionen**
   - Zeigt nur Transaktionen des ausgewählten Portfolios
   - Fokussierte Ansicht auf Portfolio-spezifische Aktivitäten

3. **Auf Konto-Ebene**: Innerhalb eines Cashkontos oder Wertpapierdepots
   - Zeigt nur Transaktionen des ausgewählten Kontos
   - Ideal für die Kontrollierung einzelner Konten

## Was beinhaltet der Report?
Der Transaktionen-Report zeigt alle Arten von Finanztransaktionen in einer tabellarischen Ansicht:

### Wertpapiertransaktionen
- **Kauf**: Wertpapierkäufe aller Art
- **Verkauf**: Wertpapierverkäufe
- **Dividende**: Dividendenzahlungen und Zinsen auf Wertpapiere
- **Finanzierungskosten**: Kosten für Margin-Positionen (können positiv oder negativ sein)

### Kontotransaktionen
- **Einzahlung**: Geldeinzahlungen auf Cashkonten
- **Auszahlung**: Geldabhebungen von Cashkonten
- **Zinsen Konto**: Zinserträge auf Cashkonten
- **Gebühren**: Konto- und Depotführungsgebühren

### Kontoüberträge
- **Kontoübertrag**: Überweisungen zwischen zwei Cashkonten innerhalb des Mandanten
- Diese werden als verbundene Transaktionen dargestellt

## Spaltenübersicht
Die Tabelle zeigt folgende Informationen für jede Transaktion:

| Spalte | Beschreibung | Filterbar |
|--------|-------------|-----------|
| **Datum** | Datum und Uhrzeit der Transaktion | Ja (Datumsbereich) |
| **Cashaccount** | Name des betroffenen Cashkontos | Ja (Auswahlfilter) |
| **Kontowährung** | Währung des Cashkontos | Ja (Auswahlfilter) |
| **Transaktionstyp** | Art der Transaktion (Kauf, Verkauf, Einzahlung, etc.) | Ja (Auswahlfilter) |
| **Wertpapier** | Name des Wertpapiers (nur bei Wertpapiertransaktionen) | Ja (Auswahlfilter) |
| **Stückzahl** | Anzahl der gehandelten Einheiten (nur Wertpapiere, immer positiv) | Ja (Numerisch) |
| **Kurs/Dividende** | Preis pro Einheit oder Dividendenbetrag | Ja (Numerisch) |
| **Währung** | Währung des Wertpapiers/der Transaktion | Ja (Auswahlfilter) |
| **Wechselkurs** | Umrechnungskurs zur Kontowährung (falls unterschiedlich) | Ja (Numerisch) |
| **Steuern** | Gezahlte Steuern für die Transaktion | Ja (Numerisch) |
| **Gebühren** | Transaktionskosten (Ordergebühren, etc.) | Ja (Numerisch) |
| **Kassabetrag** | Auswirkung auf den Kassenbestand (positiv/negativ) | Ja (Numerisch) |

**Wichtig**:
- Der **Kassabetrag** wird in **Rot** dargestellt, wenn er negativ ist (Geldabfluss)
- Der **Kassabetrag** erscheint in normaler Farbe, wenn er positiv ist (Geldzufluss)
- Die Sortierung erfolgt standardmässig nach **Datum** absteigend (neueste zuerst)
- Die **Stückzahl** ist bei Wertpapiertransaktionen immer positiv; die Transaktionsart (Kauf/Verkauf) bestimmt die Richtung

## Filteroptionen
Der Report bietet umfangreiche Filtermöglichkeiten:

### Spaltenfilter
Jede Spalte kann einzeln gefiltert werden:

1. **Datumsfilter**:
   - Exaktes Datum
   - Datumsbereich (vor, nach, zwischen)
   - Kalender-Auswahl für einfache Eingabe

2. **Auswahlfilter** (Dropdown):
   - Cashaccount: Filtern nach spezifischem Konto
   - Kontowährung: Filtern nach Währung
   - Transaktionstyp: Filtern nach Transaktionsart
   - Wertpapier: Filtern nach spezifischem Wertpapier
   - Währung: Filtern nach Transaktionswährung

3. **Numerische Filter**:
   - Stückzahl: Grösser, kleiner, gleich, zwischen
   - Kurs: Preisbereich definieren
   - Wechselkurs: Bestimmte Kursbereiche
   - Steuern, Gebühren, Kassabetrag: Beliebige numerische Bereiche

### Mehrfachfilter
- Mehrere Filter können gleichzeitig angewendet werden
- Filter werden mit UND-Verknüpfung kombiniert
- Filter bleiben aktiv, bis sie manuell entfernt werden

### Multi-Sortierung
- Durch Klick auf Spaltenüberschriften sortierbar
- Mehrfachsortierung durch Halten der Strg/Cmd-Taste möglich
- Sortierrichtung durch wiederholtes Klicken umkehrbar

## Bearbeitungsfunktionen
### Transaktion bearbeiten
Eine bestehende Transaktion kann auf verschiedene Weisen bearbeitet werden:

**Aufruf**:
1. Rechtsklick auf die Transaktion → **Transaktion bearbeiten**
2. Oder: Transaktion markieren → Im Menü "Transaktion bearbeiten" wählen

**Bearbeitbare Felder** (je nach Transaktionstyp):
- Datum und Uhrzeit der Transaktion
- Stückzahl (bei Wertpapieren)
- Kurs/Preis
- Steuern
- Gebühren
- Notiz
- Ex-Datum (bei Dividenden)
- Steuerlich relevant (bei Zinsen/Dividenden)

**Wichtig**:
- Das Konto und das Wertpapier können nach Erstellung nicht mehr geändert werden
- Bei Margin-Positionen: Verbundene Transaktionen werden automatisch aktualisiert
- Bei Kontoüberträgen: Beide Seiten der Transaktion werden gemeinsam bearbeitet

### Transaktion löschen
**Aufruf**:
1. Rechtsklick auf die Transaktion → **Transaktion löschen**
2. Oder: Transaktion markieren → Im Menü "Transaktion löschen" wählen

**Bestätigung**:
- System fordert Bestätigung vor dem Löschen
- Bei Kontoüberträgen: Beide verbundenen Transaktionen werden gelöscht
- Bei Margin-Positionen: Prüfung auf offene verbundene Positionen

**Achtung**: Das Löschen einer Transaktion ist endgültig und kann nicht rückgängig gemacht werden!

### In Kontoübertrag umwandeln
Eine einzelne Ein- oder Auszahlung kann nachträglich in einen Kontoübertrag umgewandelt werden.

**Voraussetzungen**:
- Transaktion muss vom Typ "Einzahlung" oder "Auszahlung" sein
- Transaktion darf noch nicht mit einer anderen Transaktion verbunden sein

**Aufruf**:
1. Rechtsklick auf die Transaktion → **In Kontoübertrag umwandeln**
2. Dialog öffnet sich zur Auswahl des Zielkontos/Quellkontos
3. System erstellt automatisch die verbundene Gegenbuchung

**Anwendungsfall**: Wenn eine Auszahlung ursprünglich als normale Auszahlung erfasst wurde, aber tatsächlich ein interner Transfer war.

## Kontextmenü
Das Kontextmenü (Rechtsklick auf eine Transaktion) bietet folgende Optionen:

1. **Transaktion bearbeiten**
   - Verfügbar: Immer für existierende Transaktionen
   - Öffnet den entsprechenden Bearbeitungsdialog

2. **Transaktion löschen**
   - Verfügbar: Immer für existierende Transaktionen
   - Löscht die Transaktion nach Bestätigung

3. **In Kontoübertrag umwandeln**
   - Verfügbar: Nur für nicht verbundene Ein-/Auszahlungen
   - Erstellt verbundene Gegenbuchung auf anderem Konto

## Tabelleneinstellungen
### Paginierung
- Standardmässig werden **50 Transaktionen** pro Seite angezeigt
- Wählbar: 20, 30, 50 oder 100 Zeilen pro Seite
- Navigation über Paginierung-Controls am unteren Tabellenrand

### Spaltenbreiten
- Spalten haben vordefinierte Breiten für optimale Lesbarkeit
- Breiten sind auf die Datenlänge optimiert
- Fixe Breiten für konsistente Darstellung

### Selektion
- Einzelne Zeile kann durch Klick markiert werden
- Markierte Zeile wird hervorgehoben
- Kontextmenü und Hauptmenü reagieren auf Selektion

## Datenaktualisierung
Der Report wird automatisch aktualisiert, wenn:
- Eine neue Transaktion erstellt wird
- Eine bestehende Transaktion bearbeitet wird
- Eine Transaktion gelöscht wird
- Ein Kontoübertrag erstellt wird

Die Aktualisierung erfolgt unmittelbar nach der Aktion, ohne dass die Seite neu geladen werden muss.

## Spezielle Transaktionstypen
### Margin-Positionen
**Eröffnung**:
- Transaktion vom Typ "Kauf" oder "Verkauf"
- Stückzahl ist immer positiv; die Transaktionsart bestimmt die Richtung

**Finanzierungskosten**:
- Separate Transaktion vom Typ "Finanzierungskosten"
- Verbunden mit der Eröffnungstransaktion
- Kann positiv (Gutschrift) oder negativ (Belastung) sein

**Schliessung**:
- Gegengeschäft zur Eröffnung
- System berechnet automatisch Gewinn/Verlust
- Verbindung zur Eröffnungstransaktion bleibt erhalten

### Dividenden nach Verkauf
**Sonderfall**: Dividende wird nach Verkauf des Wertpapiers ausgezahlt

**Erfassung**:
- Transaktion vom Typ "Dividende"
- Ex-Datum muss angegeben werden (Stichtag für Dividendenberechtigung)
- Wertpapier-Referenz bleibt erhalten, auch wenn Position bereits verkauft

### Kontoüberträge
**Struktur**:
- Bestehen aus zwei verbundenen Transaktionen
- Auszahlung auf Quellkonto
- Einzahlung auf Zielkonto
- Beide Transaktionen referenzieren sich gegenseitig

**Währungsumrechnung**:
- Bei unterschiedlichen Kontowährungen
- Wechselkurs wird für beide Transaktionen verwendet
- Betrag wird entsprechend konvertiert

## Typische Anwendungsfälle
1. **Überprüfung einer Transaktion**: Filter nach Wertpapier oder Datum verwenden
2. **Kontrolle der Gebühren**: Filter nach Transaktionstyp "Gebühren" setzen
3. **Dividendenübersicht**: Filter nach Transaktionstyp "Dividende"
4. **Korrektur eines Fehlers**: Transaktion suchen, bearbeiten oder löschen
5. **Nachvollziehen eines Kontoübertrags**: Nach Datum filtern, verbundene Transaktionen prüfen
6. **Analyse der Handelstätigkeit**: Nach Portfolio oder Zeitraum filtern

## Einschränkungen und Hinweise
- **Nachträgliche Änderungen**: Konto und Wertpapier können nach Erstellung nicht geändert werden
- **Löschen**: Bei offenen Margin-Positionen muss erst die Position geschlossen werden
- **Performance**: Bei sehr vielen Transaktionen (>5000) kann die Tabelle langsam werden
- **Währungen**: Umrechnungen basieren auf historischen Kursen zum Transaktionszeitpunkt
- **Zeitzone**: Transaktionszeiten werden in der Systemzeitzone des Servers gespeichert
- **Sortierung**: Transaktionsreihenfolge ist wichtig für Bestandsberechnungen (FIFO-Prinzip)
- **Transaktionsdatum**: Transaktionen können nur ab dem 01.01.2000 erfasst werden

## Tipps für die effektive Nutzung
1. **Regelmässige Kontrolle**: Überprüfen Sie neue Transaktionen nach Import/manueller Erfassung
2. **Filter speichern**: Häufig verwendete Filterkombinationen durch Lesezeichen im Browser speichern
3. **Notizen verwenden**: Fügen Sie Notizen zu besonderen Transaktionen hinzu (z.B. "Stocksplit 2:1")
4. **Verbundene Transaktionen**: Achten Sie auf das Feld "Verbunden mit" bei Margin-Positionen
5. **Exportfunktion**: Für umfangreiche Analysen Export in Excel verwenden (falls verfügbar)
6. **Datensicherung**: Vor Massenänderungen Backup erstellen
