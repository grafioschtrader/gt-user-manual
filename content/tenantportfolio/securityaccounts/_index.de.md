---
title: "Depots"
date: 2026-09-28T12:00:00+02:00
draft: false
weight: 15
archetype: "default"
---
Das Depot ist ein "Behälter" für Ihre gehaltenen Instrumente, ohne dieses können keine **Transaktionen** mit Instrumenten durchgeführt werden.
+ Ein **Portfolio** kann mehrere **Depots** enthalten - in der Regel ist es meistens nur eines.
+ Ein Depot kann für sich alleine ausgewertet werden oder über alle **Depots** eines **Portfolios**.
+ Es gibt eine Beschränkung bezüglich der Gesamtzahl der Depots.
+ Ein Depot kann nicht gelöscht werden, solange es noch von einer Transaktion referenziert wird.

## Erstellen und bearbeiten Depot
Ein **Depot** wird über den **Navigationsbereich** erstellt, bearbeitet und gelöscht.
+ **Erstellen** eines **Depot** über **Kontextmenü** auf dem **statischen Element** Depots.
+ **Bearbeiten** eines **Depot** über **Kontextmenü** auf entsprechenden **Element** des Depot.
+ **Löschen** eines **Depot** über **Kontextmenü** auf entsprechenden **Element** des Depot.

### Eigenschaften
Alle Eigenschaften des Portfolios können jederzeit vollständig geändert werden.
- **Name Depot**: Ein Namen für Ihr Depot wird an verschiedenen Stellen in GT angezeigt.
- **Handelsplattform Plan**: Wahl des Handelsplattform Plan für das entsprechende Depot. Weiter Information unter [Handelsplattform Plan]({{% ref "/basedata/tradingplatformplan" %}}).
- **Handelsperioden** (Tabelle unterhalb des Formulars): Definiert, welche Instrumententypen in diesem Depot erlaubt sind und optional in welchem Zeitraum. Siehe [Handelsperioden](#handelsperioden) weiter unten.
- **Aktiv bis oder Fälligkeitstag**: Optionales Datum, bis zu dem Buchungen auf diesem Depot möglich sind; das Verhalten entspricht jenem beim Konto. Siehe [Stilllegung eines Kontos]({{% ref "/tenantportfolio/cashaccount#stilllegung-eines-kontos" %}}).
- **Steuerbefreiter Anleger (unbekannt / nein / ja)**: Gibt an, ob der Inhaber des Depots von Transaktionssteuern befreit ist, beispielsweise eine Vorsorgeeinrichtung bei der Schweizer Stempelsteuer. Das Feld erscheint nur, wenn der regelbasierte Handel eingeschaltet ist, und wirkt nur in der Steuerschätzung einer [historischen Wiederholung]({{% ref "/algoalert/historicalrun/taxmodel" %}}). Handelt das Depot über einen Schweizer Händler, wählen Sie nein oder ja; bei unbekannt kann die Stempelsteuer nicht geschätzt werden.

### Gebührenmodell
Ein Depot kann optional ein eigenes Gebührenmodell definieren, das das vom Handelsplattform Plan geerbte Modell überschreibt. Dies ist nützlich, wenn Sie mit Ihrem Broker spezielle Konditionen oder Sondertarife ausgehandelt haben, die vom Standardgebührenmodell des Plans abweichen.

Wenn ein Depot ein eigenes Gebührenmodell hat, hat dieses Vorrang vor dem Gebührenmodell des Handelsplattform Plans bei der Kostenschätzung und dem Vergleich. Ist kein depotspezifisches Gebührenmodell definiert, wird wie bisher das Modell des Plans verwendet. Courtagen und Depotgebühren werden gemeinsam überschrieben. Der Währungstarif wird unabhängig vererbt: Ein Depotmodell nur mit Währungstarif behält die Courtagen und Depotgebühren des Plans; Courtagenregeln ohne Währungsabschnitt behalten dessen Währungstarif. Eine passende Währungsregel mit null Prozent ersetzt den geerbten Aufschlag. Dieselben aufgelösten Abschnitte bestimmen die Kosten einer historischen Wiederholung; siehe [Gebühren in der Wiederholung]({{% ref "/algoalert/historicalrun/fees" %}}).

Der Gebührenmodell-Editor wird über das Kontextmenü auf dem Depot-Knoten im Navigationsbaum geöffnet: Rechtsklick auf ein Depot → **Gebührenmodell bearbeiten...**. Der Editor verwendet dasselbe YAML-Format und denselben Monaco-Editor wie das Gebührenmodell des Handelsplattform Plans. Details zur YAML-Syntax, verfügbaren Variablen und EvalEx-Funktionen finden Sie unter [Handelsplattform Plan — Gebührenmodell]({{% ref "/basedata/tradingplatformplan" %}}); den Abschnitt `fx` beschreibt [Aufschläge bei der Währungsumrechnung]({{% ref "/basedata/tradingplatformplan#aufschläge-bei-der-währungsumrechnung" %}}).

Die gemeinsamen Hilfen und die Schaltfläche **Prüfen** sind unter [YAML bearbeiten und prüfen]({{% ref "/intro/userinterface#yaml-editor" %}}) beschrieben. **Gebührenschätzung testen** verwendet Ihre noch ungespeicherten Änderungen; erst **Speichern** übernimmt sie. Ebenso testet das Panel **Währungsumrechnung** das ungespeicherte Depotdokument, ergänzt um den Währungstarif des Handelsplattform Plans, wo das Depotdokument keinen eigenen Abschnitt `fx` hat. Leeren und speichern Sie das optionale Depotmodell, wenn wieder das Modell des Handelsplattform Plans gelten soll.
Die Korrektheit des konfigurierten Gebührenmodells kann mit dem [Gebührenmodellvergleich]({{% ref "/reportportfolio/transactioncosts" %}}) im Transaktionskosten-Report überprüft werden. Dort wird die Abweichung zwischen den tatsächlich erfassten Transaktionskosten und den vom Gebührenmodell geschätzten Kosten für alle Kauf-/Verkaufstransaktionen des Depots dargestellt. Im gleichen Register werden die erfassten Wechselkurse mit dem Währungstarif verglichen; siehe [Beobachtete Wechselkursabweichung vom Tagesschlusskurs]({{% ref "/reportportfolio/transactioncosts#beobachtete-wechselkursabweichung-vom-tagesschlusskurs" %}}).

Regeln für freie Trades, beispielsweise ein freier Trade pro Quartal, zählen ausschliesslich die früheren Trades **dieses** Depots. Gewährt ein Broker ein solches Freikontingent über mehrere Depots derselben Bankbeziehung, lässt es sich nicht abbilden; halten Sie die Auswirkung stattdessen als Annahme in einem depotspezifischen Gebührenmodell fest.

### Handelsperioden
Handelsperioden schränken ein, welche Instrumententypen in einem Depot gehandelt werden dürfen. Jede Zeile in der Handelsperioden-Tabelle repräsentiert eine erlaubte Kombination aus Instrumententyp und Anlageklassenkategorie innerhalb eines Zeitraums.

**Wenn keine Handelsperioden definiert sind, ist jeglicher Handel erlaubt** — dies stellt die Abwärtskompatibilität für bestehende Depots sicher.

#### Tabellenspalten
| Spalte | Pflicht | Beschreibung |
|--------|---------|--------------|
| **Instrument** | Ja | Der spezielle Anlageinstrumententyp (z.B. Direktanlage, ETF, CFD usw.). |
| **Anlageklasse** | Nein | Die Anlageklassenkategorie (z.B. Aktien, Obligationen). Falls leer, sind alle Kategorien für den gewählten Instrumententyp erlaubt. |
| **Datum von** | Ja | Startdatum ab dem der Handel erlaubt ist. Standardwert ist 01.01.2000. |
| **Datum bis** | Nein | Enddatum bis zu dem der Handel erlaubt ist. Falls leer, ist der Handel zeitlich unbegrenzt erlaubt. |

#### Standardwerte
Beim Erstellen eines neuen Depots werden automatisch zwei Handelsperioden hinzugefügt:
- **Aktien / Direktanlage** (ab 01.01.2000, kein Enddatum)
- **Aktien / ETF** (ab 01.01.2000, kein Enddatum)

Diese Standardwerte können nach Bedarf angepasst oder entfernt werden.

#### Bearbeitungsregeln
- Bei **bestehenden Zeilen** (bereits gespeichert) kann nur die Spalte **Datum bis** geändert werden. Um den Instrumententyp oder die Anlageklassenkategorie zu ändern, muss die Zeile gelöscht und neu erstellt werden.
- Bei **neuen Zeilen** können alle Spalten bearbeitet werden.

#### Validierungsregeln
- **Überschneidungsprüfung**: Zwei Handelsperioden mit demselben Instrumententyp und derselben Anlageklassenkategorie dürfen keine sich überschneidenden Zeiträume haben.
- **Transaktionskonflikt beim Löschen**: Eine Handelsperiode kann nicht gelöscht werden, wenn bereits Transaktionen für diesen Instrumententyp in diesem Depot existieren.
- **Transaktionskonflikt bei Datum bis**: Das **Datum bis** kann nicht auf ein Datum vor der letzten bestehenden Transaktion für diesen Instrumententyp gesetzt werden.

Beim Erstellen einer [Wertpapiertransaktion]({{% ref "/transaction/security" %}}) wird der Instrumententyp des Wertpapiers gegen die Handelsperioden des Ziel-Depots geprüft. Wenn keine passende Handelsperiode das Transaktionsdatum abdeckt, wird die Transaktion abgelehnt.

## Wertpapier transferieren
Aus der Wertpapiertabelle eines Depots kann ein Wertpapier in ein anderes Depot desselben Mandanten transferiert werden. Klicken Sie mit der **rechten Maustaste** auf die Zeile des gewünschten Wertpapiers → **Wertpapier transferieren**. Der Menüpunkt ist nur verfügbar, wenn das Wertpapier offene Positionen hat (Stückzahl grösser als 0) und kein Marginprodukt ist.

GT erzeugt dabei automatisch eine Verkaufstransaktion im Quelldepot und eine Kauftransaktion im Zieldepot zum Schlusskurs des gewählten Transferdatums. Die vollständige Dokumentation zu Dialogfeldern, Ablauf und Rückgängigmachen finden Sie unter [Wertpapiertransfer]({{% ref "/basedata/securityaction/securitytransfer" %}}).
