---
title: "Austauschprotokoll"
date: 2026-01-09T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GTNet ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Diese Ansicht zeigt die Statistiken des Datenaustauschs zwischen den GTNet-Instanzen. Die Protokollierung muss über die [globale Einstellung](../globalsettings/) «gt.gtnet.use.log» aktiviert werden.

### Reiter für Austauscharten
Die Ansicht ist in zwei Reiter unterteilt, die den verschiedenen Arten des Datenaustauschs entsprechen:
- **Letzter Preis**: Statistiken zum Austausch von aktuellen Tageskursen.
- **Historische Kurse**: Statistiken zum Austausch von historischen Kursdaten.

### Haupttabelle
Die Haupttabelle zeigt alle GTNet-Instanzen auf, mit denen ein Datenaustausch stattgefunden hat. Jede Zeile kann erweitert werden, um detaillierte Statistiken anzuzeigen.

### Anbieter- und Verbraucher-Statistik
Für jede Instanz werden zwei separate Statistiken angezeigt:
- **Anbieter-Statistik**: Zeigt die Daten an, die von dieser Instanz an andere gesendet wurden.
- **Verbraucher-Statistik**: Zeigt die Daten an, die von anderen Instanzen empfangen wurden.

### Tabellenspalten der Statistik
Die Statistiken werden in einer hierarchischen Baumstruktur dargestellt, gruppiert nach Zeitperiode. Die Aggregation erfolgt automatisch von einzelnen Anfragen zu täglichen, wöchentlichen, monatlichen und jährlichen Zusammenfassungen.

| Spalte | Beschreibung |
|--------|--------------|
| Bezeichnung | Die Zeitperiode der Statistik (z.B. Datum, Kalenderwoche, Monat oder Jahr). |
| Gesendet | Anzahl der Entitäten (Wertpapiere oder Währungspaare), die in Anfragen gesendet wurden. |
| Aktualisiert | Anzahl der Entitäten, für die neue oder aktualisierte Kursdaten bereitgestellt wurden. |
| In Antwort | Anzahl der Entitäten, die in den Antworten enthalten waren. |
| Anfragen | Anzahl der Anfragen, die in dieser Periode stattgefunden haben. |

### Aggregationsperioden
Die Protokolleinträge werden automatisch nach folgenden Perioden zusammengefasst:
- **Einzeln**: Jede einzelne Anfrage wird separat protokolliert.
- **Täglich**: Zusammenfassung aller Anfragen eines Tages.
- **Wöchentlich**: Zusammenfassung aller Anfragen einer Kalenderwoche.
- **Monatlich**: Zusammenfassung aller Anfragen eines Monats.
- **Jährlich**: Zusammenfassung aller Anfragen eines Jahres.

Die Aggregation älterer Einträge erfolgt durch einen Hintergrundprozess, um die Datenmenge überschaubar zu halten.
