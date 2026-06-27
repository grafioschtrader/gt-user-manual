---
title: "Konto"
date: 2026-06-19T22:54:47+01:00
draft: false
weight: 40
archetype: "default"
---
Wenn Sie sich für GT entschieden haben, nehmen Sie bewusst in Kauf, dass eine **Wertpapiertransaktion** eine **Gegenbuchung** auf einem **Konto** erfordert. GT erlaubt Kontos in unterschiedlichen Währungen.
+ Eine **Transaktion** auf einem **Portfolio** ist erst möglich, falls es minimal ein **Konto** enthält - kann aber  mehrere enthalten.
+ Sobald ein bestimmtes Konto in einer **Transaktion** verwendet wurde, kann es nicht mehr **gelöscht** oder seine **Währung geändert** werden.

## Erstellen und bearbeiten Konto
Ein **Konto** wird im **Hauptbereich** in der **Ansicht Portfolio** erstellt, bearbeitet und gelöscht. Diese **Ansicht** erreichen Sie im **Navigationsbereich** auf dem **Namen** des **Portfolios**.
+ **Erstellen** eines **Kontos** über **Kontextmenü**.
+ **Bearbeiten** eines **Kontos** auf dem **selektierten Konto**.
+ **Löschen** eines **Kontos** auf dem **selektierten Konto**.

### Eigenschaften
Wie oben erwähnt sind die Eigenschaften eingeschränkt veränderbar.
-  **Name Bankkonto**: Der Name des Bankkontos muss in einem Portfolio einzigartig sein. Es ist nützlich die Währung im Name zu nennen.
- **Währung**: Währung des Kontos.
- **Sollzins**: Jährlicher Sollzins in Prozent zur Überziehungskontrolle. Dieses Feld steuert, ob das Konto einen negativen Saldo haben darf. Wird das Feld **leer** gelassen, ist eine Kontoüberziehung **nicht erlaubt** und das Backend lehnt [Transaktionen](../../transaction/) ab, die zu einem negativen Kontostand führen würden. Ein Wert von **0 oder höher** erlaubt die Überziehung zum angegebenen Zinssatz.
- **Depot**: Das Eingabefeld **Depot** erscheint nur, falls ihr Portfolio mehrere **Depots** hat. Innerhalb eines **Portfolios** gibt es keine Einschränkung bezüglich der **Wertpapiertransaktion** bei der Zuordnung eines **Bankkontos**. Dies kann bei den  Portfolio-Auswertungen zu Mehrdeutigkeiten führen, daher sollte bei mehreren Depots und Bankkontos mit derselben Währung diese Zuordnung erfolgen.
- **Aktiv bis oder Fälligkeitstag**: Optionales Datum, bis zu dem Buchungen auf diesem Konto möglich sind. Bleibt das Feld **leer**, ist das Konto unbefristet aktiv. Wird ein Datum gesetzt, gilt das Konto danach als stillgelegt. Weitere Einzelheiten unter [Stilllegung eines Kontos](#stilllegung-eines-kontos).

## Stilllegung eines Kontos
Sobald ein Konto in einer **Transaktion** verwendet wurde, kann es nicht mehr gelöscht werden. Über das Feld **Aktiv bis oder Fälligkeitstag** lässt sich ein nicht mehr genutztes Konto trotzdem aus dem Alltag entfernen, ohne die bestehende Geschichte zu verlieren. Dies gilt gleichermassen für **Konten** und [Depots]({{% ref "/tenantportfolio/securityaccounts" %}}).

Bleibt das Feld leer, ist das Konto unbefristet aktiv. Ein gesetztes Datum darf nicht vor der letzten bereits erfassten **Transaktion** des Kontos liegen. Nach diesem Datum kann keine **Transaktion** mehr auf dem Konto gebucht werden: Beim Erfassen oder Ändern einer [Transaktion]({{% ref "/transaction" %}}) sowie eines [Dauerauftrags]({{% ref "/transaction/standingorder" %}}) wird das Konto abhängig vom gewählten Datum nicht mehr zur Auswahl angeboten, und das Backend weist eine Buchung nach dem Aktiv-bis-Datum ab.

Ein stillgelegtes Konto bleibt im Navigationsbaum sichtbar, wird dort aber durchgestrichen dargestellt, damit die Auswertungen und die Geschichte weiterhin erreichbar bleiben. Auch beim [Transaktionsimport]({{% ref "/tenantportfolio/securityaccounts/transactionimport/viewtransactionimport" %}}) wird eine Position, deren Datum nach dem Aktiv-bis-Datum liegt, mit einer Fehlermeldung pro Zeile abgewiesen und übersprungen; die übrigen Positionen werden weiterhin importiert.
