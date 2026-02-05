---
title: "Rendite und statistische Daten"
date: 2021-11-27T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
Diese Auswertung beinhaltet die **jährliche Rendite** und bestimmte **statistische Daten** für ein **einzelnes Instrument**.
- **Zeitraum**: Die **Rendite** wird über den gesamten Zeitraum der vorliegenden Tagesendkurse ermittelt. Der Zeitraum für die Berechnung der **statistischen Daten** kann möglicherweise über die Eingabe von einem Zeitraum gesteuert werden.
- **Währung Klient und Instrument**: Die Währung des Instruments kann abweichend der Währung des Klienten sein. Daher werden die berechneten Resultate in zwei Spalten dargestellt. Für Währungenspaare und Forex wird diese Berechnung in der Währung des Klienten nicht durchgeführt.

## Rendite
Die Berechnungen basieren auf den letzten verfügbaren Schlusskurse innerhalb eines Jahres, bestenfalls wäre dies vom 31.12 des vorhergehenden Jahres bis zum 31.12 des auszuwertenden Jahres. Falls die **Dividenden** für das entsprechende Instrument vorliegen, werden diese zusätzlich hinzugerechnet, dabei bestimmt der **Ex-Tag** die Jahreszuordnung.
- Die Berechnung schliesst den gesamten Zeitraum ein, der sich über die vorliegenden Tagesendkurse definiert.

#### Rendite pro Kalenderjahr
Für jedes Kalenderjahr und dem aktuellen Jahr wird die Rendite berechnet.

#### Annualisierte Rendite
Die annualisierte Rendite beinhaltet nur vollständige Kalenderjahre, daher wird das aktuelle Jahr nicht mit eingerechnet.

## Statistische Daten
In dieser Hierarchietabelle werden zurzeit für alle 3 Zeiträume die Standardabweichung dargestellt. Aus den Tagesendkurse wird zusätzlich der und maximale Gewinn bzw. Verlust ausgewiesen. 
- Die Berechnungen berücksichtigen mögliche Benutzer-Einstellungen bezüglich eines Zeitraums.
- Die jährliche Standardabweichung wird aus der Standardabweichung der monatlichen Renditen berechnet.
