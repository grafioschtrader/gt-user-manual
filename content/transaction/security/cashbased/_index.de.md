---
title: "Vermögenswert verbuchte Transaktion"
date: 2021-06-16T22:54:47+01:00
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

## Besodere Ereignisse mit mehreren Transaktionen
Manchmal gibt es **Ereignisse** die mit mehreren Transaktionen nachgebildet werden können. Bei der Nachbildung dieser Ereignisse in Transaktionen wird **temporär der Kontosaldo** betroffen sein. Letztendlich muss überlegt werden, ob die Nachbildung des Ereignisses den Kontostand verändert oder unverändert hinterlässt und was die Auswirkungen auf das oder die Depots sind. Aus der Betrachtung dieser Ausgangslage und dem gewünschten Resultat können die notwendigen Transaktionen meistens einfach abgeleitet werden.

### Wertschriftentransfer
In GT wird ein **Wertschriftentransfer** mit einer **Wertschriftenauslieferung** und **Wertschrifteneinlieferung** wie folgt durchgeführt:
1. Am Tag des **Wertschriftentransfer** wird eine **Verkaufstransaktion** auf der gehaltenen Position dieses **Instruments** durchgeführt. Mögliche Transfergebühren werden als **Transaktionskosten** eingegeben.
2. Der durch diese Verkaufstransaktion anfallende Betrag, exklusiv Transaktionskosten wird mit einem **Kontoübertrag** zu einem Konto des **Depots** der **Wertschrifteneinlieferung** übertragen.
3. Dasselbe **Instrument** bei dem **Depot** der **Wertschrifteneinlieferung** erneut kaufen, dabei wird derselbe **Kurs** und dasselbe **Datum** für die Transaktionszeit wie beim Verkauf eingesetzt.
4. Die **Schritte 1-3** für alle Instrumente dieses **Wertschriftentransfer** wiederholen. Wobei bei **Schritt 2** die Summe mehrere **Wertschriftenverkäufe** sein kann.

Durch diese Methode kann in **GT** immer noch nachvollzogen werden, wie erfolgreich das entsprechende **Instrument** gehandelt wurde. In **GT** kann auf der Ebene **Portfolios** unter **Depot** und in der **Watchlist** die gesamte Historie **aller Trades** eines **Instruments** eingesehen werden. Zusätzliche Vorteile dieser möglicherweise einzigen Methode:
- Der **Verlust bzw. Gewinn** wird bis zum Zeitpunkt des **Wertschriftentransfer** dem Depot der **Wertschriftenauslieferung** angerechnet.
- Die möglichen **Transfergebühren** werden korrekt dem **beteiligten Instrument** und **Depot** zugeordnet.
- Der **Saldo** der daran beteiligten **Bankkonten** ist vor dem Schritt 1 und nach Schritt 3 **derselbe**. Natürlich exklusiv der Transfergebühren.

### Spin-off
...