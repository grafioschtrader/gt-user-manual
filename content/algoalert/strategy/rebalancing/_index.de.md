---
title: "Portfolio-Neugewichtung"
date: 2026-02-12T12:00:00+01:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Die Portfolio-Neugewichtung (Rebalancing) ist eine Strategie, die periodisch die tatsächliche Allokation des Portfolios mit den Zielgewichtungen vergleicht. Wenn die Abweichung einen konfigurierten Schwellenwert überschreitet, werden Handlungsempfehlungen für Kauf- oder Verkaufstransaktionen generiert, um die ursprüngliche Zielallokation wiederherzustellen.

## Funktionsweise
Das Rebalancing vergleicht für jede Anlageklasse den aktuellen Anteil am Gesamtportfolio mit dem konfigurierten Zielanteil. Weicht der Ist-Anteil um mehr als die konfigurierte **Minimale erlaubte Abweichung** vom Soll-Anteil ab, wird ein Alarm generiert. Dieser enthält die Information, welche Anlageklasse über- oder untergewichtet ist und welches Volumen umgeschichtet werden sollte.

Die Neugewichtung wird nicht automatisch ausgeführt, sondern dient als Hinweis für den Benutzer, der die empfohlenen Transaktionen manuell durchführen kann.

## Parameter
Das Rebalancing wird auf der obersten Ebene (Portfolio-Strategie) konfiguriert. Auf der Ebene der Anlageklassen und Wertpapiere kann zusätzlich eine individuelle Gewichtung festgelegt werden.

### Konfiguration auf Portfolio-Ebene

| Parameter | Beschreibung |
|---|---|
| **Anzahl Umschichtungen pro Jahr** | Legt fest, wie oft pro Jahr das Portfolio auf Abweichungen geprüft wird. Zulässige Werte: 1 bis 53. |
| **Minimale erlaubte Abweichung** | Der Schwellenwert in Prozent, ab dem eine Abweichung vom Zielanteil als signifikant gilt und ein Alarm ausgelöst wird. Zulässige Werte: 1% bis 49%. |

### Konfiguration auf Anlageklassen-/Wertpapier-Ebene

| Parameter | Beschreibung |
|---|---|
| **Gewichtung** | Die Zielgewichtung dieser Anlageklasse oder dieses Wertpapiers innerhalb der übergeordneten Ebene in Prozent. Zulässige Werte: 1% bis 100%. |

{{% notice style="info" title="Hinweis" %}}
Die Integration des Rebalancings in die bestehenden Portfolio-Berichte mit einer Darstellung von Soll-/Ist-Vergleichen ist für eine zukünftige Version geplant.
{{% /notice %}}
