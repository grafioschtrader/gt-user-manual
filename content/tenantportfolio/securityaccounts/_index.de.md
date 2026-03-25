---
title: "Depots"
date: 2026-02-26T22:54:47+01:00
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
- **Günstigste Transaktionskosten**: Diese Eingabe wird zurzeit nicht verwendet. Wird in einer zukünftigen GT Version für die Umsetzung der Problemlösung ["Abweichung zur Realität"]({{% ref "/reportportfolio" %}}) genutzt.

### Gebührenmodell
Ein Depot kann optional ein eigenes Gebührenmodell definieren, das das vom Handelsplattform Plan geerbte Modell überschreibt. Dies ist nützlich, wenn Sie mit Ihrem Broker spezielle Konditionen oder Sondertarife ausgehandelt haben, die vom Standardgebührenmodell des Plans abweichen.

Wenn ein Depot ein eigenes Gebührenmodell hat, hat dieses Vorrang vor dem Gebührenmodell des Handelsplattform Plans bei der Kostenschätzung und dem Vergleich. Ist kein depotspezifisches Gebührenmodell definiert, wird wie bisher das Modell des Plans verwendet.

Der Gebührenmodell-Editor wird über das Kontextmenü auf dem Depot-Knoten im Navigationsbaum geöffnet: Rechtsklick auf ein Depot → **Gebührenmodell bearbeiten...**. Der Editor verwendet dasselbe YAML-Format und denselben Monaco-Editor wie das Gebührenmodell des Handelsplattform Plans. Details zur YAML-Syntax, verfügbaren Variablen und EvalEx-Funktionen finden Sie unter [Handelsplattform Plan — Gebührenmodell]({{% ref "/basedata/tradingplatformplan" %}}).
Die Korrektheit des konfigurierten Gebührenmodells kann mit dem [Gebührenmodellvergleich]({{% ref "/reportportfolio/transactioncosts" %}}) im Transaktionskosten-Report überprüft werden. Dort wird die Abweichung zwischen den tatsächlich erfassten Transaktionskosten und den vom Gebührenmodell geschätzten Kosten für alle Kauf-/Verkaufstransaktionen des Depots dargestellt.

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
