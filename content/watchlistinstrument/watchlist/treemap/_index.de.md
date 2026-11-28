---
title: "Marktkarte"
date: 2026-11-21T12:00:00+01:00
draft: false
weight: 40
archetype: "default"
---
Die Registerkarte **Marktkarte** zeigt die Aktien einer Watchlist als Treemap. Jedes Rechteck steht für eine Aktie. Seine **Fläche** entspricht der **Marktkapitalisierung** des Unternehmens, seine **Farbe** der aktuellen **prozentualen Kursänderung**: Rot steht für einen Verlust, Grün für einen Gewinn, Grau für eine unveränderte Aktie. So sehen Sie auf einen Blick, welche grossen Titel den Markttag Ihrer Watchlist bewegen.

## Aufbau der Karte
Die Aktien sind nach der **Unterkategorie ihrer Anlageklasse** gruppiert, bei Aktien also nach Land oder Region, etwa «Aktien USA» oder «Aktien Schweiz». Die Gruppen sind nach ihrer gesamten Marktkapitalisierung geordnet, innerhalb einer Gruppe die Aktien nach Fläche. Die Farbe einer Gruppe ist die nach Marktkapitalisierung gewichtete Kursänderung ihrer Aktien. Ein Klick auf eine Gruppe zoomt in diese hinein, die Pfadleiste oberhalb der Karte führt wieder zurück.
Die Farbskala reicht von −3 % über 0 % bis +3 %. Stärkere Änderungen erhalten die Endfarbe, damit ein einzelner Ausreisser die übrigen Rechtecke nicht verblassen lässt. Die Kursänderung steht zusätzlich als Zahl auf jedem Rechteck, die Farbe ist also nie der einzige Träger der Information. Ist für eine Aktie ein Firmenlogo im Katalog vorhanden oder explizit zugewiesen, zeigt die Marktkarte dieses ebenfalls auf dem Rechteck an, siehe [Wertpapier und abgeleitetes Instrument](../../instrument/securityderived/#logo).
Fahren Sie mit der Maus über ein Rechteck, zeigt GT die Marktkapitalisierung in der **Hauptwährung** des Mandanten, die Kursänderung und den Zeitpunkt des Kurses. Mit der rechten Maustaste auf dem Rechteck einer Aktie öffnen Sie das Menü der Kurshistorie, siehe [Tagesendkurse als Linengrafik](../../eodchart). Die Schaltfläche **Aktualisieren** lädt die aktuellen Kurse der Watchlist und zeichnet die Karte neu.

## Woher die Werte stammen
Die Fläche ist die Anzahl der Aktien multipliziert mit dem aktuellen Kurs, umgerechnet in die Hauptwährung zum neuesten Schlusskurs des Währungspaars. Die Anzahl der Aktien stammt aus den [Fundamentaldaten]({{% relref "/watchlistinstrument/watchlist/indicator#fundamentaldaten" %}}). Fehlt sie, verwendet GT ersatzweise die von Yahoo gelieferte Marktkapitalisierung. Die Kursänderung ist die Änderung in der Währung der Börsenkotierung, Wechselkurseffekte sind darin nicht enthalten.
Ausserhalb der Handelszeiten zeigt die Farbe die Änderung der letzten Handelssitzung der jeweiligen Börse. Eine Kursänderung, die mehr als eine Stunde hinter dem Referenzzeitpunkt zurückliegt, gilt als veraltet, und die Aktie wird nicht gezeichnet. Das trifft etwa einen Titel, der kaum gehandelt wird, oder eine Datenquelle, die seit mehreren Sitzungen ausfällt.

## Was nicht dargestellt wird
Berücksichtigt werden nur **Aktien als Direktanlage**. ETFs, Fonds, Anleihen und Währungspaare haben keine Marktkapitalisierung; ihre Anzahl nennt GT unterhalb der Karte. Aktien, die aus einem anderen Grund fehlen, sind dort unter **Nicht dargestellt** nach Grund aufgeführt:
- **keine aktuelle Kursänderung**, etwa weil bereits ein neuerer Schlusskurs als der Innertagkurs vorliegt.
- **Kursänderung ist veraltet**.
- **Marktkapitalisierung fehlt**.
- **Kurs oder Wechselkurs fehlt**.

## Einschränkungen
- Die Marktkapitalisierung von Yahoo wird von keiner zweiten Quelle bestätigt und ist so alt wie der letzte wöchentliche Abruf der Fundamentaldaten.
- Eine neu in die Watchlist aufgenommene Aktie hat erst nach dem nächsten Abruf der Fundamentaldaten eine Fläche und fehlt bis dahin unter **Nicht dargestellt**. Für Administratoren: Der Abruf ist die [Hintergrundaufgabe 56]({{% relref "/admindata/taskdatachangemonitor/taskdescription/pricedata#JOB56" %}}) im [Stapelverarbeitungs Monitor]({{% relref "/admindata/taskdatachangemonitor" %}}).
- Hat ein Unternehmen mehrere Aktiengattungen mit verschiedenen ISIN, erscheint es pro Gattung, und jedes Rechteck kann die Marktkapitalisierung des ganzen Unternehmens tragen. Mehrere Kotierungen derselben ISIN zeigt GT nur einmal, mit dem neuesten Kurs.
- Die Gruppierung hängt von den gepflegten Anlageklassen ab. «Aktien Welt» erscheint beispielsweise als eigene Gruppe.
