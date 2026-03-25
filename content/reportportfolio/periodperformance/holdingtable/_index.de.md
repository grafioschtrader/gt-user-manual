---
title: "Architektur von GT - Bestandstabelle"
date: 2024-03-03T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
Grundsätzlich sollte eine relationale Datenbank keine Redundanzen enthalten. Manchmal sind jedoch Redundanzen erforderlich, um eine ausreichende Performance zu gewährleisten.

## Wie GT die meisten Berichte berechnet
GT berechnet die meisten Berichte aus den Transaktionen und den historischen oder aktuellen Kursdaten. Dabei wird jede Transaktion zur Berechnung herangezogen. Wenn eine Transaktion eine Fremdwährung enthält, muss diese natürlich auch berücksichtigt werden. Dieses Verfahren ist derzeit auch für einige tausend Transaktionen ausreichend performant.

## Problematik der Änderung von Transaktionen oder Kursdaten
In GT können Transaktionen oder Kursdaten jederzeit geändert werden. Beispielsweise kann in GT eine Transaktion gelöscht, hinzugefügt oder in der Vergangenheit geändert werden. Dies ist bei einer Handelsplattform nicht der Fall, weshalb diese ihre ungültigen Transaktionen mit Storno behandelt. Dies hat den Vorteil, dass sie für ein Portfolio zu einem beliebigen Zeitpunkt in der Vergangenheit problemlos eine Summenbildung durchführen können. Somit können Berechnungen mit einer Summierung und den Transaktionen ab diesem Zeitpunkt durchgeführt werden. Natürlich könnten solche Berechnungen auch in GT auf Basis solcher Summierungen implementiert werden. Allerdings sind solche Summierungen in GT eher ungültig, da die Transaktionen vor dieser Summierung jederzeit geändert werden können.

## Bericht Periodenertrag und redundante Daten
Der Bericht Periodenertrag berechnet täglich den Wert des Portfolios über einen bestimmten Zeitraum. Die Berechnung würde zu lange dauern, wenn nur die Transaktions- und Kursdaten der Instrumente und Währungen zur Verfügung stünden. Aus diesem Grund gibt es 3 Datenbanktabellen mit redundanten Daten, welche die Berechnung beschleunigen. Die Daten in diesen Tabellen müssen natürlich auch entsprechend den Änderungen bei Transaktionen, Splits etc. aktualisiert werden. Typisch für diese Tabellen sind zwei Felder, die eine Periode definieren. Beispielsweise kann mit einer einfachen SQL-Abfrage der Bestand eines Instruments zu einem beliebigen Zeitpunkt für einen Klienten ermittelt werden. Beim Design dieser Tabellen stand die Optimierung für die SQL-Abfragen des Berichts Periodenertrag im Vordergrund.
