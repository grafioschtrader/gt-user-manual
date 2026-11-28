---
title: "Portfolio"
date: 2026-11-10T18:00:00+01:00
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
- **Einstandswert-Methode**: In der Grundeinstellung steht hier **Wie Klient**, das Portfolio rechnet dann mit der Methode des [Klienten](../client/#eigenschaften) und folgt jeder späteren Änderung dort. Wählen Sie stattdessen **Gleitender Durchschnitt**, **FIFO (älteste Käufe zuerst)** oder **LIFO (jüngste Käufe zuerst)**, gilt diese Methode für alle Depots dieses Portfolios, unabhängig von der Einstellung des Klienten. Das ist sinnvoll, wenn eine Handelsplattform ihre Auszüge nach einer anderen Methode führt. Auch hier wirkt eine Änderung sofort und rückwirkend. In Auswertungen über mehrere Portfolios kann sie zudem den Gewinn von Verkäufen in anderen Portfolios verschieben, die mit dem gleitenden Durchschnitt rechnen, siehe [Einstandswert-Methode](../../reportportfolio/costbasismethod/#geltungsbereich).
- **Steuerabzug**: Dieses Feld erscheint nur bei einem Klienten mit der Hauptwährung Euro und dem Land Deutschland. Es sagt der [Steuerhilfe Deutschland](../../reportportfolio/germantax/), ob eine inländische Zahlstelle die Steuer auf die Erträge der Konten dieses Portfolios einbehält. Bestehende Portfolios stehen auf **Nicht zugewiesen**: Ihre Erträge erscheinen dann in keinem Block der Steuererklärung, bis Sie **Eine inländische Zahlstelle behält die Steuer ein** oder **Kein inländischer Steuerabzug** wählen. Die Steuerhilfe geht davon aus, dass ein Portfolio die Konten genau eines Instituts enthält. Legen Sie Konten verschiedener Banken in getrennte Portfolios.
