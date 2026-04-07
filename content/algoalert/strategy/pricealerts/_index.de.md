---
title: "Preis-Alarme"
date: 2026-02-12T12:00:00+01:00
draft: false
weight: 20
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Preisalarme sind einfache schwellenwertbasierte Strategien, die nach jeder Aktualisierung der Innertag-Kursdaten ausgewertet werden (Stufe 1). Sie erfordern keine historischen Kursdaten und können direkt über dynamische Formularfelder konfiguriert werden.

## Absolute Gewinn/Verlust Warnung
Diese Strategie überwacht den aktuellen Kurs eines Wertpapiers gegen ein unteres und oberes Preislimit. Unterschreitet der Kurs das untere Limit oder überschreitet er das obere Limit, wird ein Alarm ausgelöst. Dieser Alarmtyp ist ausschliesslich auf der Wertpapier-Ebene verfügbar und eignet sich zur Überwachung von festen Kursgrenzen, beispielsweise für Kaufkurse oder Gewinnziele.

| Parameter | Beschreibung |
|---|---|
| **Unters Limit** | Das untere Preislimit. Bei Unterschreitung wird ein Alarm ausgelöst. |
| **Oberes Limit** | Das obere Preislimit. Bei Überschreitung wird ein Alarm ausgelöst. |

**Beispiel:** Ein Wertpapier wird bei 100 CHF gehandelt. Mit einem unteren Limit von 90 CHF und einem oberen Limit von 120 CHF wird der Benutzer benachrichtigt, sobald der Kurs unter 90 CHF fällt oder über 120 CHF steigt.

## Preis im Bestand Gewinn/Verlust Warnung
Diese Strategie überwacht den prozentualen Gewinn oder Verlust einer gehaltenen Position. Der Alarm wird ausgelöst, wenn der Gewinn oder Verlust den konfigurierten Prozentwert überschreitet. Diese Strategie ist auf allen Ebenen (Portfolio, Anlageklasse, Wertpapier) verfügbar.

| Parameter | Beschreibung |
|---|---|
| **Gewinn** | Gewinn-Schwellenwert in Prozent (1-500%). Bei Überschreitung wird ein Alarm ausgelöst. |
| **Verlust** | Verlust-Schwellenwert in Prozent (1-500%). Bei Überschreitung wird ein Alarm ausgelöst. |

**Beispiel:** Eine Position wurde bei einem Einstandspreis von 50 CHF erworben. Mit einem Gewinn-Schwellenwert von 20% und einem Verlust-Schwellenwert von 10% wird der Benutzer benachrichtigt, wenn der Positionswert um mehr als 20% steigt oder um mehr als 10% fällt.

## Gewinn/Verlust Warnung in einer Periode
Diese Strategie überwacht die Kursveränderung eines Wertpapiers über einen definierten Zeitraum. Der Alarm wird ausgelöst, wenn der Kurs innerhalb der konfigurierten Anzahl von Tagen um mehr als den festgelegten Prozentsatz steigt oder fällt. Diese Strategie ist auf allen Ebenen verfügbar.

| Parameter | Beschreibung |
|---|---|
| **Periode** | Anzahl der Tage für den Beobachtungszeitraum (1-999 Tage). |
| **Gewinn** | Gewinn-Schwellenwert in Prozent (1-500%). |
| **Verlust** | Verlust-Schwellenwert in Prozent (1-500%). |

**Beispiel:** Mit einer Periode von 30 Tagen, einem Gewinn von 15% und einem Verlust von 10% wird der Benutzer benachrichtigt, wenn sich der Kurs innerhalb von 30 Tagen um mehr als 15% nach oben oder 10% nach unten bewegt hat.
