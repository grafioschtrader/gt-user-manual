---
title: "Diagramme"
date: 2026-10-02T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
Zur Auswertung [Dividende und Zins]({{% relref "/reportportfolio/dividends" %}}) gehören sechs Diagramme, welche die Erträge und Kosten der Tabelle grafisch aufbereiten. Sie zeigen, in welchen Monaten Ausschüttungen eintreffen, wie sich Erträge und Kosten über die Jahre entwickeln und aus welchen Anlageklassen die Erträge stammen. Einzelne Wertpapiere werden bewusst nicht aufgeschlüsselt, da ein Diagramm mit vielen Wertpapieren unlesbar würde; dafür steht die **Detailansicht Wertpapiere** der Tabelle zur Verfügung.
## Aufruf und Bedienung
Über das Menü **Ansicht** oder das Kontextmenü der Auswertung öffnet **Zeige Diagramme** die Diagramme im [Zusatzbereich]({{% relref "/intro/userinterface" %}}). Oberhalb des Diagramms wählen Sie mit **Diagrammtyp** eines der sechs Diagramme; die Auswahl wird im Browser gespeichert und beim nächsten Öffnen wiederhergestellt.
Diagramm und Tabelle arbeiten zusammen. Drei der Diagramme beziehen sich auf ein einzelnes Jahr, das im Titel des Diagramms steht. Dieses Jahr wählen Sie, indem Sie in der Haupttabelle eine Jahreszeile anklicken; ein Hinweis unter dem Diagrammtyp erinnert daran. Haben Sie noch keine Zeile angeklickt, zeigt das Diagramm das neuste Jahr mit Ausschüttungen. Schränken Sie die Auswertung über **Auswertung über Depots** auf bestimmte Depots und Bankkontos ein, gilt diese Auswahl auch für die Diagramme, und sie werden neu geladen. Dasselbe geschieht, wenn Sie in der Tabelle eine Transaktion ändern.
Wie bei allen Diagrammen in GT blendet ein Klick auf einen Eintrag der Legende die zugehörige Datenreihe aus oder wieder ein, und beim Überfahren mit der Maus erscheinen die genauen Beträge. Hat ein gewähltes Jahr keine Erträge, erscheint anstelle eines leeren Diagramms der Hinweis **Keine Erträge in diesem Jahr**.
## Was die Diagramme zählen
Alle Beträge sind in der Hauptwährung des Mandanten angegeben und werden aus denselben Transaktionen und mit denselben Wechselkursen berechnet wie die Tabelle. Die Jahressummen der Diagramme stimmen deshalb mit der Haupttabelle überein: Dividenden und Zinsen netto ergeben zusammen die Spalte **Erhaltene Div/Zins**, der **Kontozins** entspricht der gleichnamigen Spalte, und die Finanzierungskosten von CFD und Forex ergeben zusammen die Spalte **Finanzierungskosten**.
Die Diagramme unterscheiden zwischen **Dividenden** und **Zinsen**. Als Zinsen gelten die Ausschüttungen von Instrumenten der Anlageklassen-Kategorien **Anleihen** und **Wandelanleihe**. Das betrifft direkt gehaltene Anleihen ebenso wie Anleihen-Fonds und Anleihen-ETF. Alle übrigen Ausschüttungen, etwa von Aktien, Aktien-ETF oder Immobilienfonds, gelten als Dividenden. Massgebend ist also die [Anlageklasse]({{% relref "/basedata/instrumentbased/assetclass" %}}) des Instruments.
Erträge werden **netto** dargestellt, also mit dem Betrag, der dem Bankkonto tatsächlich gutgeschrieben wurde. Die bei der Ausschüttung abgezogene Quellensteuer erscheint als helleres Segment in der Farbe des Ertrags direkt darüber. Netto und Quellensteuer zusammen ergeben den Bruttobetrag. Einem Ertrag wird der Monat seines Buchungsdatums zugeordnet, nicht der **Ex-Tag**.
{{% notice style="note" title="Quellensteuer auf Kontozinsen" %}}
Die Quellensteuer in den Diagrammen umfasst nur die Ausschüttungen von Wertpapieren. Eine auf Kontozinsen abgezogene Steuer, etwa die Verrechnungssteuer, ist nicht enthalten. Deshalb kann die Summe der Quellensteuer in den Diagrammen kleiner sein als die Spalte **Automatisch Steuer** der Haupttabelle.
{{% /notice %}}
## Die Diagramme
### Erträge pro Monat
Dieses Diagramm zeigt für das gewählte Jahr die Ausschüttungen der zwölf Monate als gestapelte Balken: **Dividenden netto** und darüber die **Quellensteuer auf Dividenden**, gefolgt von **Zinsen netto** und der **Quellensteuer auf Zinsen**. So erkennen Sie, in welchen Monaten viel Geld eintrifft und in welchen kaum etwas, was etwa bei der Planung von Entnahmen hilft. Der **Kontozins** ist zunächst ausgeblendet und kann über die Legende zugeschaltet werden.
### Erträge und Kosten pro Jahr
Hier stehen alle Jahre nebeneinander. Die Erträge, also Dividenden, Zinsen, Kontozins und die zugehörige Quellensteuer, wachsen von der Nulllinie nach oben. Die Kosten, also **Finanzierungskosten CFD** und **Finanzierungskosten Forex**, wachsen nach unten; ein negativer Kontozins erscheint ebenfalls unterhalb der Nulllinie. Die Linie **Nettoertrag** verbindet für jedes Jahr die Summe aus Dividenden netto, Zinsen netto, Kontozins und Finanzierungskosten. Damit sehen Sie auf einen Blick, ob die Erträge von Jahr zu Jahr wachsen und wie stark die Finanzierungskosten von Margin-Positionen sie schmälern.
{{% notice style="info" title="Konto- und Depotkosten" %}}
Die **Konto- und Depotkosten** sind im Diagramm zunächst ausgeblendet und können über die Legende zugeschaltet werden. Sie sind im **Nettoertrag** nicht enthalten, da sie unabhängig von den Erträgen anfallen.
{{% /notice %}}
### Ausschüttungen nach Anlageklasse pro Jahr
Dieses Diagramm zeigt die Ausschüttungen netto jedes Jahres als gestapelte Balken, aufgeteilt nach Anlageklasse. Jede Anlageklasse wird mit ihrer Kategorie, Unterkategorie und ihrem Instrumenttyp bezeichnet. Damit lässt sich verfolgen, wie sich die Herkunft der Erträge über die Jahre verschiebt, etwa von Aktien hin zu Anleihen.
### Anteil der Anlageklassen im Jahr
Das Ringdiagramm zeigt für das gewählte Jahr, welchen prozentualen Anteil jede Anlageklasse an den Ausschüttungen netto hat. Es beantwortet die Frage, wie stark die Erträge eines Jahres von einzelnen Anlageklassen abhängen.
{{% notice style="note" title="Grenzen der Anlageklassen-Diagramme" %}}
Damit die Farben unterscheidbar bleiben, werden höchstens sieben Anlageklassen einzeln dargestellt, und zwar jene mit den grössten Ausschüttungen. Alle weiteren werden unter **Übrige Anlageklassen** zusammengefasst. Das Ringdiagramm kann zudem keine negativen Anteile darstellen; eine Anlageklasse mit negativer Summe fehlt darin.
{{% /notice %}}
### Erträge Jahr × Monat
Diese Übersicht zeigt jedes Jahr als Zeile und jeden Monat als Spalte. Die Farbe einer Zelle steht für die Summe aus Dividenden, Zinsen und Kontozins netto in diesem Monat: je dunkler das Blau, desto höher der Ertrag. Beim Überfahren einer Zelle erscheint zusätzlich die abgezogene Quellensteuer. So werden wiederkehrende Ausschüttungsmonate und das Wachstum der Erträge über viele Jahre gleichzeitig sichtbar. Finanzierungskosten sind in dieser Darstellung nicht enthalten, und ein Monat mit negativem Ertrag erscheint gleich hell wie ein Monat ohne Ertrag.
### Kumulierte Erträge im Vergleich zu den Vorjahren
Für jedes Jahr mit Erträgen zeigt eine Linie, wie sich die Summe aus Dividenden, Zinsen und Kontozins netto von Januar bis Dezember aufbaut. Das gewählte Jahr ist mit einer kräftigen blauen Linie hervorgehoben, das Vorjahr in Orange; alle übrigen Jahre erscheinen grau, damit der Vergleich auch bei vielen Jahren lesbar bleibt. Liegt die Linie des laufenden Jahres über jener des Vorjahres, haben Sie bis zu diesem Monat mehr eingenommen als im Vorjahr. Finanzierungskosten sind in dieser Darstellung nicht enthalten.
