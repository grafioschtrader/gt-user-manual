---
title: "Portfolio"
date: 2026-08-07T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
Meistens ist das Portfolio die Abbildung einer einzelnen Handelsplattform. Es können mehrere **Bankkonten** und **Depots** in einem **Portfolio** erstellt werden. 
+ Einem **Portfolio** wird eine **Währung** zugewiesen, diese kann sich von der **Währung** des **Klienten** unterscheiden. 
+ Ein Portfolio kann nicht gelöscht werden, solange es noch ein Depot oder ein Bankkonto hat.

## Erstellen und bearbeiten Portfolio
Ein Portfolio wird über den **Navigationsbereich** erstellt, bearbeitet und gelöscht.
+ **Erstellen** eines **Portfolio** über **Kontextmenü** auf dem **statischen Element** Portfolios.
+ **Bearbeiten** eines **Portfolios** über **Kontextmenü** auf entsprechenden **Element** des Portfolios.
+ **Löschen** eines **Portfolios** über **Kontextmenü** auf entsprechenden **Element** des Portfolios.

### Eigenschaften
Die **Eigenschaften** des Portfolios können jederzeit vollständig geändert werden.
- **Name Portfolio bzw. Bank**: Der **Name** des Portfolios, dieser Name muss für einen Klient einzigartig sein.
- **Währung**: Die Auswertungen auf diesem **Portfolio** bezieht sich auf diese **Währung**. Wird sie geändert, erzeugt GT die auf die neue Portfoliowährung fehlenden [Währungspaare](../../watchlistinstrument/instrument/currencypair/) und baut die Bestandstabellen neu auf. Beides geschieht in einer Hintergrundaufgabe, die Auswertungen sind daher erst nach deren Abschluss vollständig.
- **Geschlossen bis**: Transaktionen mit einem Datum an oder vor diesem Datum können weder hinzugefügt noch geändert oder gelöscht werden. Ein hier gesetztes Datum hat Vorrang vor dem entsprechenden Datum des Klienten. Eine ausführliche Beschreibung mit den Ausnahmen findet sich unter ["Geschlossen bis" in Klient](../client/#eigenschaften).
