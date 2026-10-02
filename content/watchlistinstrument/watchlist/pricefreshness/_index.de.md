---
title: "Aktualität der Kurse"
date: 2026-09-06T10:54:47+01:00
draft: false
weight: 40
archetype: "default"
---
Ein Instrument erhält nicht ewig Kurse. Ein Wertpapier verschwindet nach einem Konkurs von der Börse, ein ETF wird geschlossen, oder eine Datenquelle stellt die Lieferung für ein einzelnes Instrument ein. Die Position behält aber ihren Wert und muss weiterhin bewertet werden. Damit Sie nicht in einen Kurs vertrauen, der gar nicht mehr aktuell ist, kennzeichnet GT in den Tabellen der Watchlist und im Depotbericht, wie belastbar der angezeigte Kurs ist. Zwei Einfärbungen genügen dafür, und beide sind auch als Quickinfo hinterlegt, wenn Sie mit der Maus auf der Zelle verweilen.

## Rötliche Spalte "Zeit/Datum"
Je weiter der angezeigte Kurs zurückliegt, desto kräftiger wird die Spalte **Zeit/Datum** rot hinterlegt. Gemessen wird dabei nicht in Kalendertagen, sondern in **Handelstagen des Handelsplatzes**, an dem das Instrument notiert. Ein Wochenende, ein weltweiter Feiertag oder ein Feiertag genau dieser Börse färbt die Zelle deshalb nie ein — genau das wäre bei einer Zählung nach Kalendertagen der Fall, und ein über Ostern ganz normal versorgtes Instrument sähe dann alarmierend aus. Bei Währungspaaren, die zu keinem Handelsplatz gehören, wird der allgemeine Handelskalender verwendet.

Der laufende Tag zählt bewusst nicht mit. Eine Börse, die noch nicht geöffnet hat, zeigt zu Recht den Kurs des letzten abgeschlossenen Handelstages, und dieser gilt als aktuell. Ab etwa zehn versäumten Handelstagen wird die Einfärbung nicht mehr kräftiger; ob eine Datenquelle seit zwei Monaten oder seit zwei Jahren schweigt, ändert für Sie nichts mehr.

## Gelbe Spalte "Kurs"
Ein gelb hinterlegter **Kurs** bedeutet, dass dieser Kurs nie gehandelt wurde. Er stammt aus den historischen Kursdaten und wurde dort selbst erst durch das Schliessen von Lücken erzeugt, entweder von Ihnen über [Lineares befüllen fehlender Kursdaten](../../externaldata/historyquote/pricedata/) oder automatisch, weil die Datenquelle nur für tatsächlich gehandelte Tage Kurse liefert. Der Wert ist damit ein rechnerischer Zwischenwert und keine Marktinformation.

Ein echter, von der Datenquelle gelieferter Schlusskurs bleibt hingegen ungefärbt. Er ist ein richtiger Kurs; wie alt er ist, sagt Ihnen bereits die Spalte **Zeit/Datum**.

## Welcher Kurs wird überhaupt angezeigt
In der [Performanz Ansicht](../performance/) und im [Depotbericht](../../../reportportfolio/securityaccountreport/) zeigt GT immer den jüngsten verfügbaren Kurs. Ist der jüngste historische Schlusskurs neuer als der zuletzt empfangene Innertag Kurs, so wird dieser Schlusskurs angezeigt, und **Zeit/Datum** nennt dessen Datum. Damit stimmen Watchlist und Depotbericht überein, und ein nicht mehr gehandeltes Instrument wird mit dem Kurs bewertet, den Sie in den historischen Kursdaten gepflegt haben.

Berücksichtigt werden dabei nur Kurse bis und mit dem heutigen Tag. Das ist wichtig, weil eine lineare Befüllung durchaus in die Zukunft reichen kann, etwa bei einer Anleihe, deren **Aktiv bis Datum** der Verfall ist. Ein Kurs für einen noch nicht eingetretenen Handelstag wird nie als aktueller Kurs herangezogen.

{{< mermaid >}}
flowchart TD
    A[Kurs eines Instruments] --> B{Historischer Schlusskurs bis heute neuer als der Innertag Kurs?}
    B -->|Nein| C[Innertag Kurs wird angezeigt]
    B -->|Ja| D{Wurde dieser Schlusskurs gehandelt?}
    D -->|Ja| E[Schlusskurs wird angezeigt]
    D -->|Nein, durch Lueckenfuellung entstanden| F[Schlusskurs wird gelb angezeigt]
    C --> G[Zeit/Datum roetlich je nach versaeumten Handelstagen]
    E --> G
    F --> G
{{< /mermaid >}}

Ein Beispiel: Ein Wertpapier wird seit einem Konkurs nicht mehr gehandelt, und Sie haben die fehlenden Kurse bis gestern linear befüllt. In der Watchlist erscheint dann ein gelber **Kurs**, **Zeit/Datum** zeigt den gestrigen Handelstag und bleibt ungefärbt, denn der Kurs ist so aktuell, wie er sein kann. Haben Sie hingegen nie befüllt, so zeigt **Zeit/Datum** den letzten echten Handelstag und wird kräftig rot.

## Abweichendes Verhalten in der Ansicht "Kurs Datenfeed"
In der [Kurs Datenfeed Ansicht](../pricefeed/) wird bewusst kein Kurs ersetzt. Diese Ansicht dient der Überwachung der Datenquellen, und dort muss **Zeit/Datum** aussagen, wann die Innertag Datenquelle letztmals erfolgreich geliefert hat — nicht, wie alt der Kurs ist, den Sie andernorts sehen. Zusammen mit den beiden Wiederholungszählern und der Spalte **Jüngstes EOD** erkennen Sie so rasch, ob nur die Innertag Versorgung ausgefallen ist oder ob ein Instrument überhaupt keine Kurse mehr erhält.

## Leere Felder statt veralteter Angaben
Jede Datenquelle liefert nur einen Teil der Innertag Angaben. Wechseln Sie die Innertag Datenquelle eines Instruments auf eine, die beispielsweise nur den letzten Kurs kennt, so bleiben die übrigen Felder wie **Vortag** oder **Differenz Vortag** künftig leer, statt weiterhin die Werte der früheren Datenquelle anzuzeigen. Ein leeres Feld bedeutet also, dass die aktuelle Datenquelle diese Angabe nicht liefert. Dasselbe gilt für einen Kurs, der aus den historischen Kursdaten stammt: ein Schlusskurs kennt keinen Handelsverlauf, deshalb sind diese Felder dort ebenfalls leer.

{{% notice style="info" title="Wo Sie das Datum des ersatzweisen Kurses sehen" %}}
Die Spalte **Jüngstes EOD** nennt den Tag, aus dem ein aus den historischen Kursdaten stammender Kurs kommt. In der **Kurs Datenfeed Ansicht** ist sie standardmässig sichtbar, in der **Performanz Ansicht** blenden Sie sie bei Bedarf über **Spalten anzeigen** ein, siehe [Bedienelemente](../../../intro/userinterface/user_setting_ui_controls).
{{% /notice %}}
