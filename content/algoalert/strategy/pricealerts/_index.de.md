---
title: "Preis-Alarme"
date: 2026-09-26T12:00:00+02:00
draft: false
weight: 20
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="In Bearbeitung - Zielversion V0.40.0" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Preisalarme werden bei neuen Innertag-Kursen und über die einstellbare Hintergrundplanung geprüft. Periodenalarme benötigen zusätzlich einen historischen Vergleichskurs. Die [Regeln zur Alarm-Auswertung](../../alert/) erläutern Börsenzeiten, Wochenenden und fehlende Daten. Ausgewertet werden sie nur in der [Portfolioüberwachung](../../algo/#portfolioüberwachung) Ihres eigenen Portfolios und nie in einer Simulation, denn ein Alarm meldet nur, dass eine Bedingung eingetreten ist, und nennt weder Richtung noch Menge eines Auftrags. Weshalb das so ist und welche Strategie stattdessen in einer Simulation handelt, erläutert [Welche Strategie wo wirkt](../../#welche-strategie-wo-wirkt).

## Absoluter Gewinn/Verlust
Diese Strategie überwacht den aktuellen Kurs eines Wertpapiers gegen ein unteres und oberes Preislimit. Unterschreitet der Kurs das untere Limit oder überschreitet er das obere Limit, wird ein Alarm ausgelöst. Dieser Alarmtyp ist ausschliesslich auf der Wertpapier-Ebene verfügbar und eignet sich zur Überwachung von festen Kursgrenzen, beispielsweise für Kaufkurse oder Gewinnziele.

| Parameter | Beschreibung |
|---|---|
| **Unters Limit (L)** | Das untere Preislimit ab 0. Bei Unterschreitung wird ein Alarm ausgelöst. |
| **Oberes Limit (U)** | Das obere Preislimit ab 0. Bei Überschreitung wird ein Alarm ausgelöst. |

**Beispiel:** Ein Wertpapier wird bei 100 CHF gehandelt. Mit einem unteren Limit von 90 CHF und einem oberen Limit von 120 CHF wird der Benutzer benachrichtigt, sobald der Kurs unter 90 CHF fällt oder über 120 CHF steigt.

## Bestand Gewinn/Verlust
Diese Strategie überwacht den prozentualen Gewinn oder Verlust einer gehaltenen Position gegenüber ihrem Einstandswert. Der Alarm wird ausgelöst, wenn der Gewinn oder Verlust den konfigurierten Prozentwert überschreitet. Wird das Wertpapier nicht gehalten, findet keine Auswertung statt. Zusätzlich lassen sich wie beim absoluten Alarm Kursgrenzen setzen, die nur ausgewertet werden, solange die Position gehalten wird. Mindestens eines der vier Felder muss ausgefüllt sein. Diese Strategie ist auf allen drei Ebenen verfügbar, also auf der Portfoliobasierten Strategie, der Anlageklasse und dem Wertpapier. Auf der Portfoliobasierten Strategie gilt sie für die Wertpapiere ihrer Watchlist, auf einer Anlageklasse für deren Wertpapiere.

| Parameter | Beschreibung |
|---|---|
| **Gewinn (G)** | Gewinn-Schwellenwert in Prozent (1-500%). Bei Überschreitung wird ein Alarm ausgelöst. |
| **Verlust (L)** | Verlust-Schwellenwert in Prozent (1-500%). Bei Überschreitung wird ein Alarm ausgelöst. |
| **Oberes Limit (U)** | Kursgrenze ab 0. Ein Alarm entsteht, wenn der Kurs sie von unten nach oben kreuzt. |
| **Unters Limit (L)** | Kursgrenze ab 0. Ein Alarm entsteht, wenn der Kurs sie von oben nach unten kreuzt. |

**Beispiel:** Eine Position wurde bei einem Einstandspreis von 50 CHF erworben. Mit einem Gewinn-Schwellenwert von 20% und einem Verlust-Schwellenwert von 10% wird der Benutzer benachrichtigt, wenn der Positionswert um mehr als 20% steigt oder um mehr als 10% fällt.

## Gewinn/Verlust in einer Periode
Diese Strategie überwacht die Kursveränderung eines Wertpapiers über einen definierten Zeitraum. Der Alarm wird ausgelöst, wenn der Kurs innerhalb der konfigurierten Anzahl von Tagen um mehr als den festgelegten Prozentsatz steigt oder fällt. Als Vergleichswert dient der letzte Schlusskurs an oder vor dem Tag, auf den die Periode zurückweist; die Periode zählt Kalendertage. Diese Strategie ist ausschliesslich auf der Wertpapier-Ebene verfügbar.

| Parameter | Beschreibung |
|---|---|
| **Periode (P)** | Anzahl der Tage für den Beobachtungszeitraum (1-999 Tage). |
| **Gewinn (G)** | Gewinn-Schwellenwert in Prozent (1-500%). |
| **Verlust (L)** | Verlust-Schwellenwert in Prozent (1-500%). |

**Beispiel:** Mit einer Periode von 30 Tagen, einem Gewinn von 15% und einem Verlust von 10% wird der Benutzer benachrichtigt, wenn sich der Kurs innerhalb von 30 Tagen um mehr als 15% nach oben oder 10% nach unten bewegt hat.
