---
title: "Kurs Datenfeed"
date: 2021-05-01T22:54:47+01:00
draft: false
weight : 20
chapter: true
---
## Kurs Datenfeed
Diese Watchlist dient primär der Überwachung der **historischen** und **Innertag Kursdaten**.

### Funktionen
Die spezifischen Funktionen dieser **Watchlist-Ansicht** verlangt die **Benutzerrechte** eines **Privilegierten Benutzer**, andernfalls werden sie nicht angeboten.

#### Reparatur historischer Daten
 Es wird versucht die **historischen Kursdaten** aller Instrumente der Watchlist zu aktualisieren, wessen historischen **Wiederholungszähler grösser 0** ist. Dabei kommt das Limit 'gt.history.retry' der "**Globalen Einstellungen** zur Anwendung, weitere Informationen siehe [Globale Einstellungen](../../../admindata/globalsettings). Mit dieser Funktion wird dieses Limit der möglichen Versuche ignoriert.

#### Reparatur Innertag Daten
Es wird versucht die **Innertag Kursdaten** aller Instrumente der Watchlist zu aktualisieren, wessen Innertag **Wiederholungszähler grösser 0** ist. Dabei wird das Limit 'gt.intra.retry', weitere Informationen siehe [Globale Einstellungen](../../../admindata/globalsettings), der möglichen Versuche ignoriert.

### Expandierende Tabellenzeile
Die expandierte Tabellenzeile zeigt fast alle Eigenschaften des Instrumentes an.
