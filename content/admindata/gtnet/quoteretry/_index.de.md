---
title: "Wiederholungszähler und GTNet-Fallback"
date: 2026-06-26T22:54:47+01:00
draft: false
weight: 25
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GTNet ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}

GT führt für jedes Instrument zwei Wiederholungszähler: den «Wiederholungszähler historisch» für die historischen und den «Wiederholungszähler Innertag» für die Innertag-Kursdaten. Diese Zähler dokumentieren laufend, ob der konfigurierte Konnektor zuverlässig liefert. Sobald GTNet aktiviert und für ein Instrument freigeschaltet ist, kann es als Fallback einspringen, ohne die Wiederholungszähler des Konnektors zu verfälschen. Damit bleibt der Konnektor-Status für die Überwachung sichtbar, gleichzeitig erhält der Benutzer weiterhin Kursdaten.

## Drei Bänder pro Wiederholungszähler
Der gleiche Zähler durchläuft drei aufeinanderfolgende Bänder. Die Grenzen werden durch die globalen Einstellungen «gt.history.retry» bzw. «gt.intra.retry» (Konnektor-Limit) und «gt.gtnet.quote.retry» (zusätzliches GTNet-Budget) bestimmt. Die folgende Tabelle beschreibt das Verhalten am Beispiel der historischen Kurse.

| Wertbereich | Bedeutung | Wer versucht zu laden? |
|---|---|---|
| 0 bis gt.history.retry − 1 | Konnektor wird als funktionierend betrachtet | Der Konnektor lädt. Bei freigeschalteten Instrumenten kann GTNet zusätzlich Daten liefern. |
| gt.history.retry bis gt.history.retry + gt.gtnet.quote.retry − 1 | Konnektor-Limit erreicht, GTNet-Fallback aktiv | Nur GTNet versucht zu laden, sofern «Erhalte historische Preisdaten» für das Instrument aktiviert ist. Der Konnektor wird übersprungen. |
| ab gt.history.retry + gt.gtnet.quote.retry | Konnektor und GTNet erschöpft | Es findet keine automatische Aktualisierung mehr statt. Eine manuelle Rücksetzung ist erforderlich. |

Für Innertag-Kurse gilt die gleiche Bandstruktur mit «gt.intra.retry» und der Option «Erhalte Intraday Preis». Die Einstellungen finden Sie unter [Globale Einstellungen GTNet](../globalsettings/).

## Regeln für die Veränderung des Zählers
Damit der Zähler ein verlässliches Signal über den Konnektor-Zustand bleibt, gelten klare Regeln für jede automatische Aktualisierung.

| Ereignis | Wirkung auf den Zähler |
|---|---|
| Konnektor-Erfolg | Zähler wird auf 0 gesetzt. |
| Konnektor-Misserfolg | Zähler wird um 1 erhöht. |
| GTNet-Erfolg | Zähler wird auf höchstens gt.history.retry begrenzt. Liegt der Zähler bereits unterhalb des Konnektor-Limits, bleibt er unverändert. Liegt er im Fallback-Band, wird er exakt auf gt.history.retry gesetzt, sodass das volle GTNet-Budget wieder zur Verfügung steht. |
| GTNet-Misserfolg | Zähler wird um 1 erhöht, jedoch höchstens bis zum absoluten Limit. |

### Wann der Zähler auf 0 zurückgesetzt wird
Der Wert 0 entsteht in der Regel durch einen erfolgreichen Konnektor-Aufruf. Daneben gibt es zwei weitere Wege:

- **Erfolgreicher Konnektor-Aufruf**: Sowohl die regulären Hintergrundaufgaben als auch die manuell ausgelösten Funktionen «Reparatur historische Daten» und «Reparatur Innertag Daten» auf der [Kurs Datenfeed Ansicht](../../../watchlistinstrument/watchlist/pricefeed/) führen genau dann zu einer Rücksetzung auf 0, wenn der Konnektor tatsächlich Daten liefert. Bleibt der Konnektor stumm oder wirft einen Fehler, ändert sich der Zähler nicht oder wird sogar erhöht. Auch das Bearbeiten eines Instruments löst eine erneute Datenabfrage aus, sobald sich der Konnektor oder die URL-Erweiterung ändern; auch hier wird der Zähler nur bei einem erfolgreichen Konnektor-Aufruf auf 0 gesetzt.
- **Hintergrundaufgabe 49 «Wiederholungszähler für Konnektor(en) bei aktiven Instrumenten zurücksetzen»**: Diese Aufgabe setzt den Zähler unmittelbar auf 0, ohne den Konnektor erst aufzurufen. Sie eignet sich, um nach Behebung eines Konnektor-Ausfalls schnell wieder den Konnektor-Pfad zu aktivieren. Mehr dazu in der [Aufgabenbeschreibung](../../taskdatachangemonitor/taskdescription/).
- **Direktes Bearbeiten des Instruments**: Beim Bearbeiten eines Wertpapiers oder Währungspaars kann der Wiederholungszähler direkt geändert werden. Damit lässt sich der Zähler gezielt für ein einzelnes Instrument zurücksetzen, ohne den Konnektor aufzurufen.

## Visualisierung der Bänder
{{< mermaid >}}
stateDiagram-v2
    direction LR
    [*] --> Konnektor_aktiv

    Konnektor_aktiv : Konnektor aktiv
    Konnektor_aktiv : 0 ≤ Zähler < gt.history.retry
    Konnektor_aktiv : Konnektor lädt Kurse

    GTNet_Fallback : GTNet-Fallback
    GTNet_Fallback : gt.history.retry ≤ Zähler < gt.history.retry + gt.gtnet.quote.retry
    GTNet_Fallback : Nur GTNet versucht (Empfangsoption aktiviert)

    Erschoepft : Konnektor und GTNet erschöpft
    Erschoepft : Zähler ≥ gt.history.retry + gt.gtnet.quote.retry
    Erschoepft : Manuelle Rücksetzung erforderlich

    Konnektor_aktiv --> Konnektor_aktiv : Konnektor-Misserfolg ⇒ Zähler + 1
    Konnektor_aktiv --> Konnektor_aktiv : Konnektor-Erfolg ⇒ Zähler = 0
    Konnektor_aktiv --> GTNet_Fallback   : Zähler erreicht gt.history.retry

    GTNet_Fallback --> GTNet_Fallback : GTNet-Misserfolg ⇒ Zähler + 1
    GTNet_Fallback --> GTNet_Fallback : GTNet-Erfolg ⇒ Zähler = gt.history.retry
    GTNet_Fallback --> Erschoepft       : Zähler erreicht absolutes Limit

    Erschoepft --> Konnektor_aktiv : Rücksetzung des Zählers (siehe «Wann der Zähler auf 0 zurückgesetzt wird»)
{{< /mermaid >}}

Für Innertag-Kurse gilt das gleiche Diagramm mit «gt.intra.retry» und der Option «Erhalte Intraday Preis».

## Praktische Konsequenzen
Aus diesen Regeln ergeben sich einige bewusst gewählte Effekte für den Alltag.

- Ein Instrument kann Kursdaten über GTNet erhalten, auch wenn sein Konnektor noch nie funktioniert hat. Dazu muss die Option «Erhalte historische Preisdaten» bzw. «Erhalte Intraday Preis» aktiviert sein und mindestens ein erreichbarer GTNet-Lieferant die gewünschten Daten anbieten.
- Während GTNet als Fallback aktiv ist, bleibt der Wiederholungszähler erhöht. Die Überwachung der Konnektoren (siehe [Stapelverarbeitungs Monitor](../../taskdatachangemonitor/)) erkennt den Ausfall weiterhin und benachrichtigt den Administrator.
- Der Konnektor bleibt erste Wahl. So können Sie beim Anlegen eines Instruments prüfen, ob der Konnektor korrekt konfiguriert ist, bevor GTNet einspringt.
- Sobald beide Bänder erschöpft sind, hört GT auf, automatisch zu versuchen. Es bleiben die im Abschnitt «Wann der Zähler auf 0 zurückgesetzt wird» beschriebenen Optionen: ein erfolgreicher Konnektor-Aufruf (z.B. über «Reparatur historische Daten» oder «Reparatur Innertag Daten» auf der [Kurs Datenfeed Ansicht](../../../watchlistinstrument/watchlist/pricefeed/)), die Hintergrundaufgabe 49 oder das direkte Bearbeiten des Instruments.

## Verwandte Themen
- [Globale Einstellungen GTNet](../globalsettings/) für die Konfiguration der Limits.
- [Austausch historische Kurse](../exchange/historicalprice/) und [Austausch letzter Preis](../exchange/lastprice/) für die Funktionsweise des GTNet-Datenaustauschs.
- [Kurs Datenfeed Ansicht](../../../watchlistinstrument/watchlist/pricefeed/) für manuelle Reparaturmöglichkeiten.
- [Externe Daten](../../../watchlistinstrument/externaldata/) für die Liste der verfügbaren Konnektoren.
