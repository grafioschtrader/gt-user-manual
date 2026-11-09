---
title: "Historische Kurse nach dem Import"
date: 2026-10-20T12:00:00+02:00
draft: false
weight: 20
archetype: "default"
---
Ein [Export der persönlichen Daten]({{% relref "/intro/settings" %}}) enthält die historischen Kurse der noch aktiven Instrumente nicht, weil sich diese wieder beschaffen lassen. Die neue Instanz lädt sie nach dem Import selbst nach. Diese Seite beschreibt, woher die Kurse kommen, was dafür vorbereitet sein muss und was zu tun ist, wenn nach dem ersten Lauf noch Kurse fehlen.

## Die Kurse kommen von den Konnektoren
Das erste Laden der historischen Kurse erfolgt über die **Konnektoren** der Instrumente, also über die Datenquellen, die bei jedem Instrument als historische Datenquelle eingetragen sind. Auch wenn Ihre Instanz bereits mit [GTNet]({{% relref "/admindata/gtnet" %}}) verbunden ist, wird ein Instrument ohne Kurshistorie zuerst bei seinem Konnektor angefragt. So zeigt sich gleich zu Beginn, ob die Konnektoren der neuen Instanz richtig eingerichtet sind.
GTNet springt erst später ein. Scheitert der Konnektor eines Instruments wiederholt und erreicht dessen **Wiederholungszähler historisch** den Wert von `gt.history.retry`, übernimmt GTNet die Lieferung als Ersatz, sofern beim Instrument die Option **Erhalte historische Preisdaten** gesetzt ist. Für Instrumente, die bereits eine Kurshistorie haben, fragt die tägliche Aktualisierung bei eingerichtetem GTNet zuerst GTNet an und erst danach den Konnektor. Die Einzelheiten beschreibt [GTNet historische Kursdaten]({{% relref "/admindata/gtnet/exchange/historicalprice" %}}).

## Zuerst die API-Keys der Konnektoren erfassen
Viele Datenquellen liefern nur mit einem gültigen API-Key. Die API-Keys gehören zur Instanz und werden **nicht exportiert**. In der neuen Instanz müssen Sie diese deshalb unter [Verbindungs-API-Key]({{% relref "/admindata/connectorapikey" %}}) erneut erfassen. Fehlt der API-Key einer Datenquelle, scheitert deren Konnektor und der **Wiederholungszähler historisch** der betroffenen Instrumente steigt.
Am besten erfassen Sie die API-Keys deshalb unmittelbar nach dem ersten Start, bevor das Laden der Kurse beginnt.
Möglicherweise verfügen Sie in der neuen Instanz nicht über alle API-Keys, welche die importierten Instrumente benötigen, etwa weil die Quelleninstanz ein kostenpflichtiges Abonnement einer Datenquelle nutzte. In diesem Fall müssen Sie die Konnektoren der betroffenen Instrumente anpassen und im Instrument unter **Historische Datenquelle** und gegebenenfalls **Innertag Datenquelle** eine Datenquelle wählen, die in der neuen Instanz zur Verfügung steht. Welche Instrumente eine bestimmte Datenquelle nutzen, sehen Sie in der **Kurs Datenfeed Ansicht** einer Watchlist, die sich nach diesen beiden Spalten filtern lässt.

## Das Laden startet automatisch
Etwa fünf bis zehn Minuten nach dem ersten Start des Back-Ends legt GT die Hintergrundaufgabe [30 - Historische- und Innertag-Kursaktualisierung]({{% relref "/admindata/taskdatachangemonitor/taskdescription/pricedata#JOB30" %}}) an, welche die fehlenden Kurse lädt. Danach läuft diese Aufgabe täglich zu dem in `gt.eod.cron.quotation` festgelegten Zeitpunkt.
Waren die API-Keys zum Zeitpunkt des automatischen Laufs noch nicht vollständig, ergänzen Sie diese und lösen die Aufgabe 30 im [Stapelverarbeitungs Monitor]({{% relref "/admindata/taskdatachangemonitor" %}}) von Hand aus, indem Sie eine neue Hintergrundaufgabe dieses Typs erstellen. Sie müssen also nicht bis zum nächsten täglichen Lauf warten.

{{< mermaid >}}
flowchart TD
    A[Import abgeschlossen, Back-End gestartet] --> B[API-Keys unter Verbindungs-API-Key erfassen]
    B --> C[Nach 5 bis 10 Minuten startet Aufgabe 30 automatisch]
    C --> D{Fehlen noch Kurse?}
    D -- Nein --> E[Kurshistorie vollständig]
    D -- Ja, API-Keys fehlten --> F[API-Keys ergänzen]
    F --> G[Aufgabe 30 von Hand erstellen]
    G --> D
    D -- Ja, Zähler ausgeschöpft --> H[Aufgabe 49 setzt die Wiederholungszähler zurück]
    H --> G
{{< /mermaid >}}

## Der Vorgang kann lange dauern
Für jedes aktive Instrument wird die gesamte Kurshistorie geladen, oft über viele Jahre. Bei mehreren hundert Instrumenten und kostenlosen Datenquellen mit Abfragegrenzen dauert dies lange, auf einem Kleinrechner wie einem Raspberry Pi besonders. Ein einzelner Lauf der Aufgabe 30 ist auf eine Stunde begrenzt; was danach noch fehlt, lädt der nächste Lauf. Bis alle Instrumente eine Kurshistorie haben, können deshalb mehrere Läufe nötig sein.
Solange die Kurshistorie eines Instruments fehlt, bleiben in der **Performanz Ansicht** einer Watchlist Spalten wie **Seit Jahresbeginn** und **Zeitrahmen** leer, obwohl der letzte Kurs laufend aktualisiert wird.

{{% callout warning %}}
Starten Sie das Back-End nicht neu, solange die Aufgabe 30 läuft. Ein abgebrochener Lauf wird nicht fortgesetzt. Die fehlenden Kurse lädt erst der nächste Lauf oder eine von Hand erstellte Aufgabe 30.
{{% /callout %}}

## Wenn ein Konnektor ausgesetzt hat
Bei jedem Lauf, in dem der Konnektor eines Instruments scheitert, steigt dessen **Wiederholungszähler historisch** um eins. Erreicht der Zähler den Wert von `gt.history.retry`, fragt GT diesen Konnektor nicht mehr an. Fehlten die API-Keys über mehrere Läufe, kann dies bereits viele Instrumente betreffen. In der **Kurs Datenfeed Ansicht** einer Watchlist erkennen Sie diese Instrumente an einem Wiederholungszähler grösser als null. Nachdem die API-Keys erfasst sind, setzt die Hintergrundaufgabe [49 - Wiederholungszähler für Konnektor(en) bei aktiven Instrumenten zurücksetzen]({{% relref "/admindata/taskdatachangemonitor/taskdescription/pricedata#JOB49" %}}) die Zähler für einen einzelnen oder alle Konnektoren zurück. Danach lösen Sie die Aufgabe 30 erneut von Hand aus.
Instrumente, deren Konnektor schon in der Quelleninstanz keine Kurse mehr lieferte, sind in der Datei `broken_history_connectors.txt` des Exports aufgeführt. Deren Kurse sind im Export enthalten, ein neuer Konnektor ist für sie aber trotzdem nötig, damit künftige Kurse hinzukommen.
