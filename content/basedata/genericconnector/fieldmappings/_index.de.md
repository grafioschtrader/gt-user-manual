---
title: "Feldzuordnungen"
date: 2026-02-25T12:00:00+01:00
draft: false
weight: 30
archetype: "default"
---
Jeder Endpunkt benötigt **Feldzuordnungen**, die angeben, welches Feld in der API-Antwort welchem internen GT-Feld entspricht. Die verfügbaren Zielfelder hängen vom Feed-Typ ab.
## Zielfelder für historische Endpunkte
- **`date`**: Handelsdatum
- **`open`**: Eröffnungskurs
- **`high`**: Tageshöchstkurs
- **`low`**: Tagestiefkurs
- **`close`**: Schlusskurs
- **`volume`**: Handelsvolumen
## Zielfelder für Intraday-Endpunkte
- **`last`**: Letzter gehandelter Kurs
- **`open`**: Eröffnungskurs
- **`high`**: Tageshöchstkurs
- **`low`**: Tagestiefkurs
- **`volume`**: Handelsvolumen
- **`prevClose`**: Vortagesschlusskurs
- **`changePercentage`**: Veränderung in Prozent
- **`timestamp`**: Zeitstempel der letzten Kursnotierung
## Felder der Feldzuordnung
- **Zielfeld**: Das interne GT-Feld (siehe Listen oben).
- **Quellausdruck**: Wo der Wert in der API-Antwort zu finden ist. Die Bedeutung hängt vom Antwortformat ab: bei JSON der Eigenschaftsname oder Punkt-Pfad, bei HTML (Regex-Gruppen) die Nummer der Capture-Gruppe, bei HTML (Multi-Selektor) ein CSS-Selektor.
- **CSV-Spaltenindex**: 0-basierter Spaltenindex (nur bei CSV-Format).
- **Teiler-Ausdruck**: Optionaler Divisor für Einheitenumrechnung (z.B. `100` wenn der Anbieter Kurse in Unterwährungseinheiten liefert).
- **Erforderlich**: Ob dieses Feld zwingend vorhanden sein muss, damit ein Datensatz als gültig akzeptiert wird.
