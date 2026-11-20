---
title: "Aktualität der Kurse"
date: 2026-10-21T10:54:47+01:00
draft: false
weight: 40
archetype: "default"
---
Ein Instrument erhält nicht ewig Kurse. Ein Wertpapier verschwindet nach einem Konkurs von der Börse, ein ETF wird geschlossen, oder eine Datenquelle stellt die Lieferung für ein einzelnes Instrument ein. Die Position behält aber ihren Wert und muss weiterhin bewertet werden. Die Einfärbungen einzelner Zellen in den Tabellen der Watchlist und im Depotbericht sollen Ihnen deshalb auf einen Blick zeigen, wo ein angezeigter Kurs möglicherweise ungenau ist. Statt Datumsangaben selbst miteinander zu vergleichen, sehen Sie sofort, welches Instrument einen zweiten Blick verdient. Jede Einfärbung ist zudem als Quickinfo erklärt, wenn Sie mit der Maus auf der Zelle verweilen. Eine ungefärbte Zelle bedeutet, dass GT nichts Auffälliges feststellen konnte.

| Spalte | Einfärbung | Bedeutung | Watchlist-Ansichten |
|---|---|---|---|
| **Zeit/Datum** | rötlich, zunehmend | Der angezeigte Kurs wurde seit einigen Handelstagen nicht erneuert. | Performanz, Kurs Datenfeed, Indikatoren |
| **Kurs** | gelb | Der Kurs wurde nie gehandelt, sondern beim Füllen von Lücken berechnet. | Performanz, Indikatoren |
| **Jüngstes EOD** | rötlich, zunehmend | Es fehlen historische Kurse von Handelstagen, die bereits geliefert sein müssten. | Kurs Datenfeed, Indikatoren, Performanz |
| **Ältestes EOD** | rötlich, zunehmend | Die historischen Kurse beginnen erst nach **Handel ab**. | Kurs Datenfeed |
| **Ältestes EOD** | gelb | Die historischen Kurse reichen weiter zurück als **Handel ab**. | Kurs Datenfeed |

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

## Rötliche Spalte "Jüngstes EOD"
Die Spalte **Jüngstes EOD** nennt den Tag des jüngsten gespeicherten historischen Schlusskurses. Sie wird umso kräftiger rot hinterlegt, je mehr Handelstage danach fehlen, deren Schlusskurs bereits vorliegen müsste. Damit erkennen Sie eine historische Datenquelle, die nicht mehr liefert, auch dann, wenn die Innertag Kurse weiterhin eintreffen. Das ist wichtig, weil Auswertungen wie Rendite, Korrelation oder die Indikatoren auf den historischen Kursen beruhen.

Ein Schlusskurs von gestern ist nicht automatisch ein Problem. Die historischen Kurse eines Handelstages werden erst nach Börsenschluss geladen, je nach Einstellung zu einem festen Zeitpunkt oder pro Handelsplatz einige Zeit nach dessen Handelsschluss, siehe [Automatische Aktualisierung nach Börsenschluss]({{% relref "/watchlistinstrument/externaldata/historyquote/pricedata" %}}). GT richtet sich deshalb nicht nach der Uhr, sondern nach der **Letzten Kursaktualisierung** des Handelsplatzes, die Sie in der Tabelle der [Handelsplätze]({{% relref "/basedata/instrumentbased/stockexchange" %}}) sehen. Erwartet wird ein Handelstag nur, wenn diese Aktualisierung mindestens drei Stunden nach seinem Handelsschluss stattfand. Zwischen Börsenschluss und der nächsten Aktualisierung bleibt die Zelle also ungefärbt, auch wenn der gestrige Kurs noch fehlt.

{{< mermaid >}}
flowchart TD
    A[Juengstes EOD eines Wertpapiers] --> B{Historische Datenquelle oder abgeleitetes Instrument?}
    B -->|Nein| N[Keine Einfaerbung]
    B -->|Ja| C[Juengster Handelstag, dessen Handelsschluss mindestens drei Stunden vor der letzten Kursaktualisierung des Handelsplatzes lag]
    C --> D[Hoechstens bis Aktiv bis oder Faelligkeitsdatum]
    D --> E{Fehlen Handelstage zwischen Juengstes EOD und diesem Tag?}
    E -->|Nein| N
    E -->|Ja| R[Roetlich je nach Anzahl fehlender Handelstage]
{{< /mermaid >}}

Wie bei **Zeit/Datum** werden nur Handelstage des Handelsplatzes gezählt, Wochenenden und Feiertage färben die Zelle nie ein. Ab zehn fehlenden Handelstagen wird die Einfärbung nicht mehr kräftiger. Für ein Wertpapier, das nicht mehr gehandelt wird, erwartet GT keine Kurse nach dem **Aktiv bis oder Fälligkeitsdatum**. Liegt dieses Datum mehr als rund drei Monate zurück, wird die Spalte nicht mehr beurteilt. Instrumente ohne historische Datenquelle, deren Kurse Sie selbst pflegen, werden nicht eingefärbt.

Währungspaare gehören zu keinem Handelsplatz. Sie werden am allgemeinen Handelskalender gemessen. Weil ihre historischen Kurse einmal täglich geladen werden, gilt der letzte abgeschlossene Handelstag bei ihnen noch nicht als fehlend.

{{% callout style="note" title="Ein heller Rotton" %}}
Manche Datenanbieter stellen den Schlusskurs erst einige Zeit nach Börsenschluss bereit. Lief die Kursaktualisierung früher, fehlt ein Handelstag, bis die nächste Aktualisierung ihn nachholt. Ein einzelner hell eingefärbter Tag ist deshalb meist harmlos. Erst eine Einfärbung, die von Tag zu Tag kräftiger wird, weist auf eine Datenquelle hin, die nicht mehr liefert. In diesem Fall hilft ein Blick auf den **Wiederholungszähler historisch** in der [Kurs Datenfeed Ansicht](../pricefeed/).
{{% /callout %}}

## Rötliche oder gelbe Spalte "Ältestes EOD"
Die Spalte **Ältestes EOD** der [Kurs Datenfeed Ansicht](../pricefeed/) nennt den Tag des ältesten gespeicherten historischen Schlusskurses. Sie wird mit **Handel ab** des Wertpapiers verglichen, denn ab diesem Datum sollten historische Kurse vorliegen.

Beginnen die historischen Kurse später, so wird die Zelle rötlich hinterlegt, und zwar umso kräftiger, je grösser die Lücke ist. Die Datenquelle hat in diesem Fall den Anfang der Kursgeschichte nie geliefert, und für Auswertungen über lange Zeiträume oder für Transaktionen aus dieser frühen Zeit fehlen die Kurse. Gezählt werden hier Kalendertage. Ein heller Rotton von wenigen Tagen entsteht beispielsweise, wenn **Handel ab** auf ein Wochenende fällt, und ist unbedenklich. Ab einer Lücke von einem Jahr wird die Einfärbung nicht mehr kräftiger.

Reichen die historischen Kurse dagegen weiter zurück als **Handel ab**, so wird die Zelle gelb hinterlegt. Das ist kein Problem der Datenquelle, sondern ein Hinweis darauf, dass **Handel ab** möglicherweise zu spät gesetzt ist; es lohnt sich, dieses Datum zu prüfen. Währungspaare kennen kein Datum **Handel ab** und werden in dieser Spalte nie eingefärbt.

## Leere Felder statt veralteter Angaben
Jede Datenquelle liefert nur einen Teil der Innertag Angaben. Wechseln Sie die Innertag Datenquelle eines Instruments auf eine, die beispielsweise nur den letzten Kurs kennt, so bleiben die übrigen Felder wie **Vortag** oder **Differenz Vortag** künftig leer, statt weiterhin die Werte der früheren Datenquelle anzuzeigen. Ein leeres Feld bedeutet also, dass die aktuelle Datenquelle diese Angabe nicht liefert. Dasselbe gilt für einen Kurs, der aus den historischen Kursdaten stammt: ein Schlusskurs kennt keinen Handelsverlauf, deshalb sind diese Felder dort ebenfalls leer.

{{% callout style="info" title="Wo Sie das Datum des ersatzweisen Kurses sehen" %}}
Die Spalte **Jüngstes EOD** nennt den Tag, aus dem ein aus den historischen Kursdaten stammender Kurs kommt. In der **Kurs Datenfeed Ansicht** und in der Ansicht **Indikatoren** ist sie standardmässig sichtbar, in der **Performanz Ansicht** blenden Sie sie bei Bedarf über **Spalten anzeigen** ein, siehe [Bedienelemente](../../../intro/userinterface/user_setting_ui_controls).
{{% /callout %}}
