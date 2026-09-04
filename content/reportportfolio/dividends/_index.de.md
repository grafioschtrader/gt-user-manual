---
title: "Dividende und Zins"
date: 2026-08-09T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
Die Auswertung **Dividende und Zins** bietet eine jahresweise Übersicht aller Dividenden- und Zinserträge sowie der damit verbundenen Kosten und Steuern. Sie steht ausschliesslich auf **Mandantenebene** zur Verfügung und analysiert alle Transaktionen über alle Portfolios hinweg. Alle Werte werden automatisch in die Hauptwährung umgerechnet.

{{% notice style="warning" icon="fa fa-wrench" title="In Bearbeitung" %}}
Die Steuerdaten-Funktionalität wurde noch nicht abschliessend geprüft und kann sich noch ändern.
{{% /notice %}}

## Aufbau der Auswertung
Die Auswertung gliedert sich in drei Ebenen. Die **Haupttabelle** zeigt eine Zusammenfassung pro Jahr mit aggregierten Werten für Dividenden, Zinsen, Kosten und Steuern. Durch Aufklappen einer Jahreszeile werden zwei Detailansichten sichtbar: die **Detailansicht Wertpapiere** zeigt alle Wertpapiere mit ihren Erträgen für das gewählte Jahr, die **Detailansicht Bankkontos** zeigt alle Zinserträge und Gebühren pro Konto. Durch nochmaliges Aufklappen einer Wertpapier- oder Kontozeile erscheinen schliesslich die einzelnen Transaktionen. Diese hierarchische Struktur ermöglicht sowohl einen schnellen Überblick über die Jahre als auch detaillierte Analysen auf Wertpapier- und Kontoebene.
### Spalten der Haupttabelle
Die Haupttabelle enthält eine Zeile pro Kalenderjahr. Sämtliche Beträge sind in der Hauptwährung des Mandanten angegeben. Die Fusszeile zeigt die Gesamtsummen über alle Jahre hinweg.
- **Jahr**: Das Kalenderjahr, auf das sich alle Werte der Zeile beziehen.
- **Kontozins**: Die Summe aller Zinserträge der Bankkontos in diesem Jahr.
- **Steuer Transaktion**: Die Summe der Steuern, die bei Kauf- und Verkaufstransaktionen angefallen sind, beispielsweise Stempelabgaben.
- **Bezahlte Transaktionen**: Die Anzahl der Transaktionen, bei denen Transaktionskosten bezahlt wurden.
- **Durchschnitt Transaktionskosten**: Die durchschnittlichen Kosten pro bezahlte Transaktion, also die Transaktionskosten geteilt durch die Anzahl der bezahlten Transaktionen.
- **Transaktionskosten**: Die Summe aller Ordergebühren (Courtagen) des Jahres.
- **Konto- und Depotkosten**: Die Summe der Gebühren, die für die Konto- und Depotführung als Gebührentransaktionen erfasst wurden.
- **Finanzierungskosten**: Die Summe der Finanzierungskosten von Margin-Produkten wie CFD. Diese Spalte ist standardmässig ausgeblendet und wird nur angezeigt, wenn entsprechende Positionen vorhanden sind.
- **Automatisch Steuer**: Die Summe der Steuern, die bei Dividenden- und Zinszahlungen direkt abgezogen wurden, beispielsweise Verrechnungssteuer oder ausländische Quellensteuer.
- **zu versteuern**: Der steuerpflichtige Bruttoertrag des Jahres. Er umfasst die Kontozinsen sowie jene Dividenden- und Zinstransaktionen, bei denen das Kontrollkästchen **Unterliegt Steuer** aktiviert ist. Die automatisch abgezogene Steuer wird dabei zum erhaltenen Betrag hinzugerechnet.
- **Erhaltene Div/Zins**: Die tatsächlich gutgeschriebenen Dividenden und Zinsen, also nach Abzug der automatisch bezahlten Steuern.
- **Wert Jahresende**: Der Gesamtwert aller Wertpapierpositionen am 31. Dezember des jeweiligen Jahres. Margin-Produkte wie CFD und Forex sind in diesem Total nicht enthalten: ihr unrealisierter Gewinn/Verlust steckt bereits in der Spalte **Saldo + Margin + Fin.-kosten** der Bankkontos und würde beim Ablesen des Gesamtvermögens sonst doppelt gezählt.

Für Schweizer Mandanten, also wenn beim Klienten das [Land](../../tenantportfolio/client/#eigenschaften) auf **Schweiz** gesetzt ist, werden zusätzlich die Spalten **ICTax Erträge** (CHF) und **ICTax Steuerwert total** (CHF) angezeigt. Diese Spalten basieren auf importierten [Steuerdaten](../../admindata/taxdata/) der Eidgenössischen Steuerverwaltung und ermöglichen einen direkten Vergleich mit den offiziellen Steuerwerten.
## Detailansicht Wertpapiere
Die erste Detailansicht listet alle Wertpapiere auf, die im gewählten Jahr Erträge ausgeschüttet haben oder am Jahresende im Bestand waren. Durch weiteres Aufklappen einer Wertpapierzeile werden alle einzelnen Transaktionen dieses Wertpapiers für das Jahr angezeigt; diese können direkt bearbeitet werden.
- **Ausschl.**: Ein Kontrollkästchen, mit dem das Wertpapier vom Steuerauszug-Export ausgeschlossen werden kann, siehe [Wertpapiere ausschliessen](#wertpapiere-ausschliessen).
- **Name**: Der Name des Wertpapiers. Die Hintergrundfarbe zeigt die Bestandesveränderung im jeweiligen Jahr an, siehe [Hintergrundfarbe der Spalte Name](#hintergrundfarbe-der-spalte-name).
- **Instrument**: Ein Symbol, das den Produkttyp des Wertpapiers anzeigt, beispielsweise Aktie, Anleihe oder ETF.
- **ISIN**: Die internationale Wertpapierkennnummer. Ein gelber Hintergrund weist darauf hin, dass für dieses Wertpapier im angezeigten oder im vorangehenden Steuerjahr eine [Steuerjahr-Korrektur](#steuerjahr-korrekturen) erfasst ist; der Tooltip zeigt die hinterlegte Bemerkung.
- **Währung**: Die Währung, in der das Wertpapier gehandelt wird.
- **Wechselkurse Jahresende**: Der Umrechnungskurs von der Wertpapierwährung in die Hauptwährung am Jahresende. Er wird für die Umrechnung der Beträge in die Hauptwährung verwendet.
- **Menge Jahresende**: Die Stückzahl am 31. Dezember nach allen Transaktionen und Splits. Bei einer im Laufe des Jahres vollständig verkauften Position steht hier null.
- **Kurs Jahresende**: Der Schlusskurs des Wertpapiers am Jahresende in der Wertpapierwährung.
- **Steuerfrei**: Die Summe der Ausschüttungen, die nicht als steuerpflichtig gelten, weil bei der Transaktion das Kontrollkästchen **Unterliegt Steuer** deaktiviert ist. Ein Beispiel sind Kapitalrückzahlungen.
- **Finanzierungskosten**: Die Finanzierungskosten dieser Position bei Margin-Produkten. Diese Spalte ist standardmässig ausgeblendet.
- **Automatisch Steuer**: Die bei den Ausschüttungen direkt abgezogenen Steuern in der Wertpapierwährung. Daneben zeigt eine zweite Spalte denselben Wert umgerechnet in die Hauptwährung; deren Spaltentitel ist mit dem Währungscode der Hauptwährung ergänzt.
- **zu versteuern**: Der steuerpflichtige Bruttoertrag dieses Wertpapiers, einmal in der Wertpapierwährung und einmal in der Hauptwährung.
- **Erhaltene Div/Zins**: Die tatsächlich erhaltenen Ausschüttungen nach Steuerabzug in der Hauptwährung.
- **Wert Jahresende**: Der Wert der Position am Jahresende in der Hauptwährung, also Menge mal Kurs, umgerechnet mit dem Wechselkurs am Jahresende. Bei Margin-Produkten (CFD, Forex) zeigt diese Spalte stattdessen den unrealisierten Gewinn/Verlust der am Jahresende noch offenen Positionen — so, als wären sie zum Jahresendkurs geschlossen worden. Dieser Wert fliesst bewusst nicht in das Jahrestotal der Spalte ein; der Tooltip der Spaltenüberschrift weist darauf hin.

Für Schweizer Mandanten werden zusätzlich **ICTax Erträge** und **ICTax Steuerwert total** pro Wertpapier angezeigt.
### Hintergrundfarbe der Spalte Name
Der Hintergrund der Spalte **Name** zeigt auf einen Blick, wie sich der Bestand der Position im jeweiligen Jahr verändert hat. Der Grund der Färbung wird zusätzlich im Tooltip angezeigt, wenn der Mauszeiger über dem Namen steht.
- {{% badge color="#B6EDB6" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} **Grün**: Die Position wurde in diesem Jahr eröffnet, das heisst der Bestand stieg von null auf eine positive Stückzahl.
- {{% badge color="#FFF48F" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} **Gelb**: Die Position wurde in diesem Jahr vollständig verkauft, der Bestand am Jahresende beträgt null.
- {{% badge color="LightPink" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} **Hellrosa**: Der Bestand war sowohl zu Jahresbeginn als auch am Jahresende null. Das ist der Fall, wenn eine Position im selben Jahr eröffnet und vollständig geschlossen wurde, oder wenn nach dem vollständigen Verkauf in einem früheren Jahr nur noch eine nachträgliche Ausschüttung gutgeschrieben wurde.
- {{% badge color="#FFD39B" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} **Hellorange**: Eine direkt gehaltene Anleihe wurde in diesem Jahr am Fälligkeitstag oder danach zurückgezahlt.

Treffen mehrere Zustände zu, gilt die Reihenfolge Hellorange vor Hellrosa vor Gelb vor Grün.
### Einfärbung der Spalte ICTax Erträge
Bei Schweizer Mandanten wird die Zelle der Spalte **ICTax Erträge** eingefärbt, sobald ihr Wert vom Wert der Spalte **zu versteuern** abweicht. So sind Abweichungen zwischen den erfassten Transaktionen und den offiziellen Steuerdaten sofort erkennbar.
- {{% badge color="#FBE9E9" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} bis {{% badge color="#F0A8A8" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} **Rot**: Der ICTax-Wert ist höher als der Wert in **zu versteuern**.
- {{% badge color="#E9FBE9" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} bis {{% badge color="#A8F0A8" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} **Grün**: Der ICTax-Wert ist tiefer als der Wert in **zu versteuern**.

Je grösser die Abweichung, desto kräftiger die Farbe; ab einer Abweichung von 20 Prozent ist die volle Farbstärke erreicht. Ist ein ICTax-Wert vorhanden, aber kein Betrag in **zu versteuern**, wird die Zelle in der kräftigsten Rotstufe angezeigt — hier fehlt vermutlich die Markierung **Unterliegt Steuer** bei der Transaktion.

Ist für das Wertpapier im angezeigten Jahr eine [Steuerjahr-Korrektur](#steuerjahr-korrekturen) erfasst, gelten stattdessen folgende Markierungen. Der Tooltip der markierten Zellen zeigt die Bemerkung der Korrektur.
- {{% badge color="#B6EDB6" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} **Grün** in der Spalte **zu versteuern**: Dieser Betrag ersetzt den ICTax-Wert, weil in der Korrektur **Steuerbaren Betrag verwenden** aktiviert ist.
- {{% badge color="#FFF48F" %}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{% /badge %}} **Gelb** in der Spalte **ICTax Erträge**: Der Wert wurde durch einen direkt erfassten **Steuerbaren Ertrag** ersetzt; die Abweichungs-Einfärbung entfällt in diesem Fall.
## Detailansicht Bankkontos
In der zweiten Detailansicht werden alle Bankkontos aufgelistet, die im gewählten Jahr Zinsen generiert oder Gebühren verursacht haben. Die Spalten umfassen **Name** und **Währung** des Kontos, **Kurs Jahresende** für den Wechselkurs der Kontowährung am Jahresende, **Kontokosten** und **Depotkosten** jeweils in Kontowährung und in Hauptwährung, **Automatisch Steuer** für abgezogene Steuern, **zu versteuern** für den steuerpflichtigen Anteil, **Erhaltene Div/Zins** für die tatsächlich erhaltenen Zinsen sowie **Barsaldo** als Kontostand am Jahresende in Kontowährung und in Hauptwährung. Bei Margin-Konten stehen zusätzlich die standardmässig ausgeblendeten Spalten **Margin-Erträge**, **Hypothetische Finanzierungskosten** und **Saldo + Margin + Fin.-kosten** zur Verfügung. **Margin-Erträge** ist der unrealisierte Gewinn/Verlust der am Jahresende noch offenen Margin-Positionen, die über dieses Konto abgerechnet werden. **Hypothetische Finanzierungskosten** sind die hochgerechneten Finanzierungskosten seit der letzten verbuchten Finanzierungskosten-Transaktion. **Saldo + Margin + Fin.-kosten** ergibt den Kontostand einschliesslich dieser beiden Werte — zusammen mit dem **Wert Jahresende** der Wertpapiere lässt sich so das Gesamtvermögen am Jahresende direkt aus der Auswertung ablesen. Durch weiteres Aufklappen einer Kontozeile werden alle Zins- und Gebührentransaktionen dieses Kontos für das Jahr angezeigt. Diese können direkt bearbeitet werden.
## Funktionen
Die Haupttabelle kann nach allen Spalten sortiert werden. Die Sortierung erfolgt standardmässig nach Jahr aufsteigend. In den Detailansichten können die Wertpapiere und Kontos ebenfalls nach allen Spalten sortiert werden.
In den Detailansichten können durch Aufklappen die einzelnen Transaktionen eines Wertpapiers oder Kontos angezeigt und bearbeitet werden. Die Bearbeitungsfunktionen entsprechen denen in der Auswertung [Transaktionen](../transactionlist/). Nach jeder Änderung wird der Bericht automatisch neu geladen um die aktuellen Werte anzuzeigen. Aufgeklappte Jahres- und Wertpapierzeilen bleiben dabei erhalten.
Über das **Kontextmenü** einer Dividenden- oder Zinstransaktion kann mit **Steuerstatus umschalten** das Kontrollkästchen **Unterliegt Steuer** direkt umgeschaltet werden, ohne den Bearbeitungsdialog zu öffnen. Diese Funktion steht nur bei gespeicherten Dividenden- und Zinstransaktionen zur Verfügung. Sie funktioniert auch für Zeiträume, die über ["Geschlossen bis"](../../tenantportfolio/client/#eigenschaften) gesperrt sind, da sie weder Bestände noch Kontosaldi verändert.
## Steuerauszug
Die folgenden Funktionen unterstützen die Erstellung eines eCH-0196-Steuerauszugs für Schweizer Mandanten. Jede Einstellung wird in einem unterschiedlichen Bereich gespeichert — beachten Sie die nachfolgenden Hinweise.
### Wertpapiere ausschliessen
Am Zeilenanfang in der Detailansicht Wertpapiere befindet sich die Checkbox **Ausschl.** (Ausschliessen): Durch Aktivierung wird das Wertpapier für das jeweilige Jahr vom Steuerauszug-Export ausgeschlossen. Diese Einstellung wird in der **Datenbank pro Mandant, Jahr und Wertpapier** (Tabelle `tax_security_year_config`) gespeichert und ist somit für alle Benutzer desselben Mandanten gültig.
### Steuerjahr-Korrekturen
In seltenen Fällen ist der berechnete Wert der Spalte **ICTax Erträge** für ein Wertpapier nicht korrekt, beispielsweise nach Kapitalmassnahmen oder bei einer abweichenden Bewertung einzelner Ausschüttungen. Über das **Kontextmenü** einer Wertpapierzeile in der Detailansicht Wertpapiere öffnet der Eintrag **Steuerjahr-Korrekturen...** einen Dialog, in dem dieser Wert pro Steuerjahr übersteuert werden kann. Die Tabelle im Dialog zeigt sämtliche Korrekturen dieses Wertpapiers über alle Steuerjahre und erlaubt das Erfassen, Bearbeiten und Löschen von Einträgen. Pro Wertpapier und Steuerjahr ist genau ein Eintrag möglich. Bei einem neuen Eintrag wird das **Steuerjahr** des Jahres vorgeschlagen, in dessen Detailansicht der Dialog geöffnet wurde; nach dem Speichern kann es nicht mehr geändert werden.
Für die Übersteuerung stehen zwei sich gegenseitig ausschliessende Möglichkeiten zur Verfügung. Mit dem Kontrollkästchen **Steuerbaren Betrag verwenden** ersetzt der Wert der Spalte **zu versteuern** den ICTax-Wert. Alternativ kann unter **Steuerbarer Ertrag** ein Betrag direkt erfasst werden. Zusätzlich kann eine **Bemerkung** mit bis zu 1024 Zeichen hinterlegt werden; der vollständige Text wird angezeigt, wenn der Mauszeiger über der Zelle steht. Für Steuerjahre ohne importierte [Steuerdaten](../../admindata/taxdata/) ist keine Übersteuerung möglich, wohl aber eine Bemerkung — so lässt sich eine Steuerposition auch ohne Korrektur dokumentieren.
Der korrigierte Wert fliesst in die Jahressumme **ICTax Erträge** der Haupttabelle sowie in den [Export](#export) des Steuerauszugs ein. Nach dem Schliessen des Dialogs wird die Auswertung automatisch neu geladen. Die Korrekturen werden in der **Datenbank pro Mandant, Jahr und Wertpapier** (Tabelle `tax_year_correction`) gespeichert und gelten somit für alle Benutzer desselben Mandanten.
Das folgende Diagramm zeigt, welcher Betrag pro Wertpapier und Steuerjahr in den Steuerauszug einfliesst.
{{< mermaid >}}
graph TD
  A[Steuerposition:<br/>Wertpapier + Steuerjahr] --> B{Korrektur<br/>erfasst?}
  B -- nein --> C[Berechneter ICTax-Wert]
  B -- ja --> D{Steuerbaren Betrag<br/>verwenden?}
  D -- ja --> E[Wert der Spalte<br/>zu versteuern]
  D -- nein --> F{Steuerbarer Ertrag<br/>erfasst?}
  F -- ja --> G[Direkt erfasster Betrag]
  F -- nein --> C
{{< /mermaid >}}
### Kontoauswahl
Über das Menü **Ansicht** und die Option **Auswertung über Depots** kann ein Dialog geöffnet werden, um die Auswertung auf bestimmte Depots und Bankkontos zu beschränken. In diesem Dialog können Sie auswählen, welche Depots für die Dividendenauswertung und welche Bankkontos für die Zinsauswertung berücksichtigt werden sollen. In der Kopfzeile der Tabelle wird angezeigt, wie viele Depots und Kontos aktuell ausgewählt sind. Diese Einstellung wird im **Local Storage des Browsers** pro Benutzer gespeichert — sie wird nicht mit anderen Benutzern geteilt und wird beim Wechsel auf einen anderen Browser oder ein anderes Gerät zurückgesetzt.
### Auf Jahresende filtern
Über das Menü **Ansicht** und die Option **Transaktionen nur bis Jahresende anzeigen** kann ein Umschalter aktiviert werden, der in der Detailansicht der Wertpapiere nur Transaktionen bis zum 31.12. des jeweiligen Jahres anzeigt. Dies ist nützlich, um die Steuerdaten auf das relevante Kalenderjahr einzugrenzen.
### Ex-Tag aus Steuerdaten setzen
Über das **Kontextmenü** auf einer Jahreszeile der Haupttabelle steht die Funktion **Ex-Tag aus Steuerdaten setzen** zur Verfügung. Sie ist nur aktiv, wenn für das gewählte Jahr [Steuerdaten](../../admindata/taxdata/) importiert wurden. Die Funktion ergänzt bei den Dividendentransaktionen des Jahres einen fehlenden **Ex-Tag** aus den offiziellen ICTax-Daten. Die Zuordnung erfolgt über die ISIN und die zeitliche Nähe der Transaktion zur jeweiligen Ausschüttung, wobei eine Abweichung von höchstens sieben Tagen akzeptiert wird. Ein bereits erfasster **Ex-Tag** wird nie überschrieben. Nach der Ausführung zeigt eine Meldung, wie viele Ex-Tage gesetzt wurden, wie viele bereits vorhanden waren und wie viele Transaktionen keine Zuordnung fanden; anschliessend wird der Bericht neu geladen. Die Funktion wirkt auch auf Zeiträume, die über ["Geschlossen bis"](../../tenantportfolio/client/#eigenschaften) gesperrt sind, da der Ex-Tag nur für die Steuerauswertung verwendet wird.
### Export
Über das Menü **Ansicht** und die Option **Steuerauszug exportieren** kann ein Dialog geöffnet werden, um einen eCH-0196-Steuerauszug zu generieren. Der Dialog enthält folgende Felder: **Steuerjahr** (standardmässig das Vorjahr), **Kanton** (Auswahl aus 26 Schweizer Kantonen), **Institutsname** (Pflichtfeld), **LEI** (optional), **Kundennummer** (Pflichtfeld), **Vorname**, **Nachname**, **TIN/AHV-Nummer** (optional). Der Export wird als ZIP-Datei heruntergeladen, die eine XML-Datei (eCH-0196 v2.2.0) und eine PDF-Übersicht enthält. Die Export-Einstellungen (Kanton, Name, Kundennummer usw.) werden in der **Datenbank pro Mandant** gespeichert und sind somit für alle Benutzer desselben Mandanten vorausgefüllt und geteilt.

{{% notice note %}}
Der Steuerauszug-Export und die ICTax-Spalten sind nur verfügbar, wenn beim Klienten das [Land](../../tenantportfolio/client/#eigenschaften) auf **Schweiz** gesetzt ist. Die ICTax-Daten müssen vorher durch den **Administrator** unter [Steuerdaten](../../admindata/taxdata/) importiert werden. Der Export und die Ausschluss-Funktion stehen jedem Benutzer zur Verfügung.
{{% /notice %}}
