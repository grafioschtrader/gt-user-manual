---
title: "Indikator-Alarme"
date: 2026-02-12T12:00:00+01:00
draft: false
weight: 30
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Indikator-Alarme verwenden technische Indikatoren, die aus historischen Kursdaten berechnet werden. Diese Alarme werden periodisch durch eine Hintergrundaufgabe (Stufe 2) ausgewertet und stehen ausschliesslich auf der Wertpapier-Ebene zur Verfügung.

## Gleitender Durchschnitt Kreuzungsalarm
Dieser Alarm wird ausgelöst, wenn der aktuelle Kurs eines Wertpapiers einen gleitenden Durchschnitt kreuzt. Der Benutzer konfiguriert den Typ des gleitenden Durchschnitts, die Berechnungsperiode und die Kreuzungsrichtung.

| Parameter | Beschreibung |
|---|---|
| **Indikatortyp** | Art des gleitenden Durchschnitts: SMA (einfacher gleitender Durchschnitt) oder EMA (exponentieller gleitender Durchschnitt). |
| **Periode** | Anzahl der Handelstage für die Berechnung des gleitenden Durchschnitts (1-999). |
| **Kreuzungsrichtung** | Die Richtung der Kreuzung, die den Alarm auslöst: ABOVE (Kurs kreuzt von unten nach oben) oder BELOW (Kurs kreuzt von oben nach unten). |

**Beispiel:** Ein Alarm mit Indikatortyp SMA, Periode 200 und Kreuzungsrichtung BELOW benachrichtigt den Benutzer, wenn der Kurs unter den 200-Tage-SMA fällt. Dies ist ein klassisches Signal für einen möglichen Abwärtstrend.

## RSI Schwellenwert-Alarm
Dieser Alarm basiert auf dem Relative Strength Index (RSI), einem Momentum-Indikator, der Werte zwischen 0 und 100 annimmt. Der Alarm wird ausgelöst, wenn der RSI unter den unteren Schwellenwert fällt (überverkauft) oder über den oberen Schwellenwert steigt (überkauft). Mindestens einer der beiden Schwellenwerte sollte konfiguriert werden.

| Parameter | Beschreibung |
|---|---|
| **RSI Periode** | Anzahl der Handelstage für die RSI-Berechnung, typischerweise 14. |
| **Unterer Schwellenwert** | RSI-Wert, unterhalb dessen ein Überverkauft-Alarm ausgelöst wird (z.B. 30). |
| **Oberer Schwellenwert** | RSI-Wert, oberhalb dessen ein Überkauft-Alarm ausgelöst wird (z.B. 70). |

**Beispiel:** Mit einer RSI-Periode von 14, einem unteren Schwellenwert von 30 und einem oberen Schwellenwert von 70 wird der Benutzer benachrichtigt, wenn der RSI unter 30 fällt (mögliche Kaufgelegenheit) oder über 70 steigt (mögliches Verkaufssignal).

## Benutzerdefinierter Ausdrucksalarm
Dieser Alarm ermöglicht die Definition eines freien Ausdrucks, der gegen die aktuellen Kursdaten und technische Indikatoren ausgewertet wird. Die Ausdruckssprache basiert auf der EvalEx-Bibliothek und unterstützt mathematische Operatoren, Vergleiche und logische Verknüpfungen.

| Parameter | Beschreibung |
|---|---|
| **Ausdruck** | Der auszuwertende Ausdruck (maximal 500 Zeichen). |

### Verfügbare Variablen
Folgende Variablen der aktuellen Innertag-Kursdaten stehen im Ausdruck zur Verfügung:
- `price` — Letzter gehandelter Kurs
- `prevClose` — Vortagesschlusskurs
- `open` — Eröffnungskurs
- `high` — Tageshöchstkurs
- `low` — Tagestiefstkurs
- `volume` — Handelsvolumen

### Indikatorfunktionen
Zusätzlich können technische Indikatoren als Funktionen verwendet werden. Diese werden aus den historischen Schlusskursen berechnet:
- `SMA(Periode)` — Einfacher gleitender Durchschnitt, z.B. `SMA(200)`
- `EMA(Periode)` — Exponentieller gleitender Durchschnitt, z.B. `EMA(50)`
- `RSI(Periode)` — Relative Strength Index (0-100), z.B. `RSI(14)`

### Beispielausdrücke
- `price < SMA(200)` — Kurs unter dem 200-Tage-SMA
- `price < SMA(200) AND RSI(14) < 30` — Kurs unter SMA und RSI im überverkauften Bereich
- `EMA(50) > EMA(200)` — Goldenes Kreuz (kurzfristiger EMA über langfristigem)
- `price > 100` — Einfacher Preisschwellenwert

{{% notice style="info" title="Hinweis" %}}
Indikator-Alarme benötigen ausreichend historische Kursdaten für die Berechnung. Stellen Sie sicher, dass für die betroffenen Wertpapiere ein Konnektor für historische Kursdaten konfiguriert ist.
{{% /notice %}}
