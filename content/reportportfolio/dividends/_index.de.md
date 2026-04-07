---
title: "Dividende und Zins"
date: 2026-03-12T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
Die Auswertung **Dividende und Zins** bietet eine jahresweise Übersicht aller Dividenden- und Zinserträge sowie der damit verbundenen Kosten und Steuern. Sie steht ausschliesslich auf **Mandantenebene** zur Verfügung und analysiert alle Transaktionen über alle Portfolios hinweg. Alle Werte werden automatisch in die Hauptwährung umgerechnet.

{{% notice style="warning" icon="fa fa-wrench" title="In Bearbeitung" %}}
Die Steuerdaten-Funktionalität wurde noch nicht abschliessend geprüft und kann sich noch ändern.
{{% /notice %}}

## Aufbau der Auswertung
Die Auswertung gliedert sich in drei Ebenen. Die **Haupttabelle** zeigt eine Zusammenfassung pro Jahr mit aggregierten Werten für Dividenden, Zinsen, Kosten und Steuern. Durch Aufklappen einer Zeile werden zwei Detailansichten sichtbar: die **Wertpapier-Ansicht** zeigt alle Dividendenzahlungen pro Wertpapier für das gewählte Jahr, die **Bankkonto-Ansicht** zeigt alle Zinserträge und Gebühren pro Konto. Diese hierarchische Struktur ermöglicht sowohl einen schnellen Überblick über die Jahre als auch detaillierte Analysen auf Wertpapier- und Kontoebene.
Die Haupttabelle zeigt für jedes Jahr folgende Spalten: **Jahr**, **Kontozins** für die Summe aller Zinserträge von Bankkontos in Hauptwährung, **Steuer Transaktion** für alle bei Transaktionen angefallenen Steuern, **Bezahlte Transaktionen** für die Anzahl der kostenpflichtigen Transaktionen, **Durchschnitt Transaktionskosten** für die durchschnittlichen Kosten pro Transaktion, **Transaktionskosten** für die Gesamtsumme aller Ordergebühren, **Gebühren** für Konto- und Depotführungskosten, **Automatisch Steuer** für bei Dividenden automatisch abgezogene Quellensteuern, **Steuerpflichtiger Betrag** für den steuerpflichtigen Anteil der Erträge und **Real erhalten** für die tatsächlich erhaltenen Dividenden und Zinsen nach Abzug aller Steuern. Die Spalte **Wert Jahresende** zeigt den Gesamtwert aller Wertpapiere am Jahresende. Die Fusszeile zeigt die Gesamtsummen über alle Jahre hinweg. Für Schweizer Mandanten (Mandantenland CH) werden zusätzlich die Spalten **ICTax Erträge** (CHF) und **ICTax Steuerwert total** (CHF) angezeigt. Diese Spalten basieren auf importierten [Steuerdaten](../../admindata/taxdata/) und ermöglichen einen direkten Vergleich mit den offiziellen Steuerwerten.
## Detailansicht Wertpapiere
In der ersten Detailansicht werden alle Wertpapiere aufgelistet, die im gewählten Jahr Dividenden ausgeschüttet haben. Die Spalten umfassen **Name** und **ISIN** des Wertpapiers, **Währung** des Wertpapiers, **Wechselkurs** zum Jahresende, **Stückzahl** am Jahresende, **Schlusskurs** am Jahresende, **Steuerfreier Ertrag** für steuerfreie Dividendenanteile, **Automatisch Steuer** für abgezogene Quellensteuern, **Steuerpflichtiger Betrag** für den zu versteuernden Anteil, **Real erhalten** für die tatsächlich erhaltenen Dividenden und **Wert Jahresende** für den Gesamtwert der Position. Für Schweizer Mandanten werden zusätzlich **ICTax Erträge** und **ICTax Steuerwert total** pro Wertpapier angezeigt. Durch weiteres Aufklappen einer Wertpapierzeile werden alle einzelnen Dividendentransaktionen dieses Wertpapiers für das Jahr angezeigt. Diese können direkt bearbeitet werden.
## Detailansicht Bankkontos
In der zweiten Detailansicht werden alle Bankkontos aufgelistet, die im gewählten Jahr Zinsen generiert oder Gebühren verursacht haben. Die Spalten umfassen **Name** und **Währung** des Kontos, **Schlusskurs** als Saldo am Jahresende, **Gebühren Konto** und **Gebühren Depot** jeweils in Kontowährung und in Hauptwährung, **Automatisch Steuer** für abgezogene Steuern, **Steuerpflichtiger Betrag** für den zu versteuernden Anteil, **Real erhalten** für die tatsächlich erhaltenen Zinsen, **Barsaldo** als aktueller Kontostand und **Barsaldo** in Hauptwährung. Durch weiteres Aufklappen einer Kontozeile werden alle Zins- und Gebührentransaktionen dieses Kontos für das Jahr angezeigt. Diese können direkt bearbeitet werden.
## Funktionen
Die Haupttabelle kann nach allen Spalten sortiert werden. Die Sortierung erfolgt standardmässig nach Jahr aufsteigend. In den Detailansichten können die Wertpapiere und Kontos ebenfalls nach allen Spalten sortiert werden.
In den Detailansichten können durch Aufklappen die einzelnen Transaktionen eines Wertpapiers oder Kontos angezeigt und bearbeitet werden. Die Bearbeitungsfunktionen entsprechen denen in der Auswertung [Transaktionen](../transactionlist/). Nach jeder Änderung wird der Bericht automatisch neu geladen um die aktuellen Werte anzuzeigen.
## Steuerauszug
Die folgenden Funktionen unterstützen die Erstellung eines eCH-0196-Steuerauszugs für Schweizer Mandanten. Jede Einstellung wird in einem unterschiedlichen Bereich gespeichert — beachten Sie die nachfolgenden Hinweise.
### Wertpapiere ausschliessen
Am Zeilenanfang in der Detailansicht Wertpapiere befindet sich die Checkbox **Ausschl.** (Ausschliessen): Durch Aktivierung wird das Wertpapier für das jeweilige Jahr vom Steuerauszug-Export ausgeschlossen. Diese Einstellung wird in der **Datenbank pro Mandant, Jahr und Wertpapier** (Tabelle `tax_security_year_config`) gespeichert und ist somit für alle Benutzer desselben Mandanten gültig.
### Kontoauswahl
Über das Menü **Ansicht** und die Option **Auswertung über Depots** kann ein Dialog geöffnet werden, um die Auswertung auf bestimmte Depots und Bankkontos zu beschränken. In diesem Dialog können Sie auswählen, welche Depots für die Dividendenauswertung und welche Bankkontos für die Zinsauswertung berücksichtigt werden sollen. In der Kopfzeile der Tabelle wird angezeigt, wie viele Depots und Kontos aktuell ausgewählt sind. Diese Einstellung wird im **Local Storage des Browsers** pro Benutzer gespeichert — sie wird nicht mit anderen Benutzern geteilt und wird beim Wechsel auf einen anderen Browser oder ein anderes Gerät zurückgesetzt.
### Auf Jahresende filtern
Über das Menü **Ansicht** und die Option **Transaktionen nur bis Jahresende anzeigen** kann ein Umschalter aktiviert werden, der in der Detailansicht der Wertpapiere nur Transaktionen bis zum 31.12. des jeweiligen Jahres anzeigt. Dies ist nützlich, um die Steuerdaten auf das relevante Kalenderjahr einzugrenzen.
### Export
Über das Menü **Ansicht** und die Option **Steuerauszug exportieren** kann ein Dialog geöffnet werden, um einen eCH-0196-Steuerauszug zu generieren. Der Dialog enthält folgende Felder: **Steuerjahr** (standardmässig das Vorjahr), **Kanton** (Auswahl aus 26 Schweizer Kantonen), **Institutsname** (Pflichtfeld), **LEI** (optional), **Kundennummer** (Pflichtfeld), **Vorname**, **Nachname**, **TIN/AHV-Nummer** (optional). Der Export wird als ZIP-Datei heruntergeladen, die eine XML-Datei (eCH-0196 v2.2.0) und eine PDF-Übersicht enthält. Die Export-Einstellungen (Kanton, Name, Kundennummer usw.) werden in der **Datenbank pro Mandant** gespeichert und sind somit für alle Benutzer desselben Mandanten vorausgefüllt und geteilt.

{{% notice note %}}
Der Steuerauszug-Export und die ICTax-Spalten sind nur bei Mandantenland **CH** (Schweiz) verfügbar. Die ICTax-Daten müssen vorher durch den **Administrator** unter [Steuerdaten](../../admindata/taxdata/) importiert werden. Der Export und die Ausschluss-Funktion stehen jedem Benutzer zur Verfügung.
{{% /notice %}}
