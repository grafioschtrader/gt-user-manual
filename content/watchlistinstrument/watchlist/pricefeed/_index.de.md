---
title: "Kurs Datenfeed Ansicht"
date: 2021-05-01T22:54:47+01:00
draft: false
weight: 20
archetype: "default"
---
Diese Watchlist dient primär der Überwachung der **historischen**- und **Innertag Kursdaten**.

## Funktionen
Die spezifischen Funktionen dieser **Watchlist-Ansicht** verlangt die **Benutzerrechte** eines **Privilegierten Benutzer**, andernfalls werden sie nicht angeboten.

### Probleme mit Kursdaten
Es ist nicht immer klar, ob ein Konnektor für ein entsprechendes Instrument nicht mehr funktioniert oder ob es nicht mehr gehandelt wird. In jedem Fall wird einer oder beide Wiederholungszähler ihr Limit erreichen. Gerade ETFs neigen dazu, vom Markt zu verschwinden, in einem solchen Fall sollte das "**Aktiv bis oder Fälligkeitsdatum**" entsprechend angepasst werden.

#### Dialog "Kursdaten Probleme hinzufügen"
Damit können Instrumente einer **leeren Watchlist** hinzugefügt werden, wessen **Wiederholungszähler** die **Limite** erreicht hat. Werden bei einem Wertpapier sowohl der Innertag- wie auch die historische Kursdaten nicht mehr aktualisiert, so ist dies ein Hinweis das möglicherweise dieses Wertpapier nicht mehr gehandelt wird. Falls dies nicht zutrifft, besteht beispielsweise die Möglichkeit, mit den beiden nachfolgenden Funktionen die Wiederholungszähler mit einer erfolgreichen Nachführung, der Innertag bzw. historischen Kursdaten, auf 0 zu setzen.  
- Das Wertpapier muss noch am Handel sein, d.h. "**Aktiv bis oder Fälligkeitsdatum**" muss jünger als das aktuelle Datum sein. Bei Währungspaaren gibt es diese Einschränkung nicht.
- **Tage seit letztmals erfolgreich**: Zwischen dem aktuellen Datum minus dieser Anzahl von Tagen muss der Konnektor letztmalig funktioniert haben. Dies kann helfen, die Anzahl der gefundenen Instrumente zu minimieren.

#### Reparatur historischer Daten
 Es wird versucht die **historischen Kursdaten** aller Instrumente der Watchlist zu aktualisieren, wessen historischen **Wiederholungszähler grösser 0** ist. Dabei kommt das Limit 'gt.history.retry' der "**Globalen Einstellungen** zur Anwendung, weitere Informationen siehe [Globale Einstellungen](../../../admindata/globalsettings). Mit dieser Funktion wird dieses Limit der möglichen Versuche ignoriert.

#### Reparatur Innertag Daten
Es wird versucht die **Innertag Kursdaten** aller Instrumente der Watchlist zu aktualisieren, wessen Innertag **Wiederholungszähler grösser 0** ist. Dabei wird das Limit 'gt.intra.retry', weitere Informationen siehe [Globale Einstellungen](../../../admindata/globalsettings), der möglichen Versuche ignoriert.

## Expandierende Tabellenzeile
Die aufgeklappte Tabellenzeile zeigt fast alle Eigenschaften des Instruments. Zur besseren Übersicht erfolgt die Darstellung gruppiert. Je nach Instrument können sowohl die angezeigten Eigenschaften als auch deren Gruppierung variieren. 
