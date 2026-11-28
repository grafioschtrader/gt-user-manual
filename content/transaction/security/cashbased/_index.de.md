---
title: "Vermögenswert verbuchte Transaktion"
date: 2026-11-20T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
Mit der Ausnahme der Instrumente **Forex** und **CFD** wird beim **Kauf** der Vermögenswert verbucht, d.h. der Kontostand sinkt um den Transaktionsbetrag das entsprechende Depot steigt mehr oder weniger um diesen Vermögenswert.

## Kaufen und Verkaufen
Aufgrund der Wahl der Instrumentes kann sich die **Sichtbarkeit** der **Eigenschaften** verändern. Bei einer Anleihe und Wandelanleihe kann beispielsweise der aufgelaufene Zins eingegeben werden.

### Eigenschaften Kaufen und Verkaufen
- **Aufgelaufener Zins**: Bei **Anleihen** und **Wandelanleihen** erscheint dieses Eingabefeld für den Marchzins.

## Verkaufen
Es können nur gehaltene Positionen mit ihrer maximalen Stückzahl verkauft werden.

## Zins/Dividende
Bei der Eingabe von Zins bzw. Dividende wird geprüft ob zur **Transaktionszeit** auch die angegeben Stückzahl gehalten wurde. Bei einer **Dividende** gibt es den **Ex-Tag** und den **Zahltag**, wobei der Zahltag immer das jüngere Datum ist. Letztendlich können Sie für ein **verkauftes Instrument** noch **Dividende** erhalten, da der **Ex-Tag** vor und der **Zahltag** nach dem Verkauf stattfand. In einem solchen Fall muss der **Ex-Tag** bei der **Transaktion** eingegeben werden, damit die Plausibilisierung der Transaktion erfolgreich wird.
-  Für ein **Storno** kann eine **negative Dividende bzw. Zins** erfasst werden. Aber es sollte nicht für aufgelaufenen Zins verwendet werden.

### Eigenschaften Zins/Dividende
- **Unterliegt Steuer**: Die meisten Erträge unterliegen der Steuer, daher ist das Kontrollkästchen aktiviert. Jedoch gibt es auch steuerbefreite Erträge. Beispielsweise steuerbefreite Kapitalgewinne in der Schweiz, in solchen fällen deaktivieren Sie das Kontrollkästchen.

## Video mit Praxis von Wertpapiertransaktionen
Wo werden Wertpapiertransaktionen in GT erfasst und bearbeitet. Dies wird anhand von Beispielen mit einem ETF und einer Anleihe erläutert. Dabei beinhaltet die Dividende eine vom Instrument abweichende Währung und bei der Anleihe wird der Marchzins erklärt.
- Kauf, Dividende und Verkauf eines ETF
- Kauf, Zins und Verkauf einer Anleihe

{{< youtube BiEIfaGTus4 >}}

## Besondere Ereignisse mit mehreren Transaktionen
Manche **Ereignisse** werden mit mehreren Transaktionen abgebildet. Dabei verändert sich der **Kontosaldo** vorübergehend. Entscheidend ist, ob das Ereignis den Kontostand am Ende verändert oder unverändert lässt und wie es sich auf das oder die Depots auswirkt. Aus dieser Ausgangslage und dem gewünschten Resultat lassen sich die notwendigen Transaktionen meistens einfach ableiten.

### Wertpapiertransfer
Für die Verschiebung eines Wertpapiers in ein anderes Depot gibt es die Funktion [Wertpapiertransfer]({{% ref "/basedata/securityaction/securitytransfer" %}}). Sie erzeugt in einem Schritt den Verkauf im bisherigen Depot, den Kontoübertrag zwischen den Bankkonten der beiden Portfolios und den Kauf im neuen Depot, alle zum Schlusskurs des Transferdatums. Der Kontostand ist danach derselbe wie vorher. Der **Gewinn bzw. Verlust** bis zum Transferdatum bleibt dem bisherigen Depot zugeordnet, und die gesamte Historie **aller Trades** eines **Instruments** bleibt auf der Ebene **Portfolios** unter **Depot** und in der **Watchlist** einsehbar. Transfergebühren erfasst die Funktion nicht. Sie werden separat als **Konto- und Depotkosten** gebucht.

### Spin-off
Bei einem Spin-off (Abspaltung) gliedert ein Unternehmen einen Geschäftsbereich in eine eigene Gesellschaft aus und teilt seinen Aktionären deren Aktien zu, beispielsweise eine neue Aktie für vier gehaltene Aktien. Die Aktien der Muttergesellschaft bleiben im Depot, die neuen Aktien kommen hinzu. Ein Teil des bisherigen Einstands geht dabei auf die neuen Aktien über. Diesen Anteil veröffentlicht das Unternehmen in der Regel. Fehlt er, ergibt er sich aus dem Verhältnis der Schlusskurse am ersten gemeinsamen Handelstag.

Erfassen Sie eine Abspaltung nicht selbst mit Verkäufen und Käufen. Ein Administrator erfasst sie einmal als [Abspaltung]({{% ref "/basedata/securityaction/spinoff" %}}) unter den Wertpapieraktionen, und jeder Mandant wendet sie auf seine Depots an. GT bucht dann die nötigen Transaktionen, teilt den Einstand auf und behält die ursprünglichen Kaufdaten bei. Haben Sie eine Abspaltung früher von Hand gebucht, erkennt die Vorschau diese Buchungen und schlägt vor, das Depot zu überspringen.