---
title: "Architektur von GT - Bestandstabelle"
date: 2026-11-25T18:00:00+01:00
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

## Täglicher Gesamtwert
Eine vierte Tabelle speichert für jeden Handelstag den Gesamtwert jedes Klienten und jedes Portfolios, also Wertschriften plus Saldo der Geldkonten plus Ergebnis geschlossener Margin-Positionen. Sie wird aus den drei Bestandstabellen mit derselben Abfrage wie der Bericht Periodenertrag berechnet und ist damit ebenfalls redundant. Ändert sich eine Transaktion, ein Bestand oder ein Kurs, gelten die Werte ab dem betroffenen Tag als veraltet und werden von der Hintergrundaufgabe [55]({{% ref "/admindata/taskdatachangemonitor/taskdescription#JOB55" %}}) neu berechnet. Diese Tabelle wird beim Export der eigenen Daten nicht mitgeliefert, nach einem Import wird sie vollständig neu aufgebaut.

Der Bericht Periodenertrag und sein PDF-Bericht lesen ihre Tageswerte bei Zeiträumen von mehr als einem Jahr aus dieser Tabelle. Nur die Tage, die veraltet sein können, werden bei der Anfrage berechnet: die Tage ab der ältesten Änderung einer Transaktion oder eines Bestands, die Tage ab dem ältesten Kurs, der seit dem letzten Lauf der Aufgabe 55 hinzugekommen ist oder korrigiert wurde, und die Tage vor dem ersten gespeicherten Tag. Eine Korrektur erscheint deshalb sofort im Bericht und nicht erst nach dem nächsten Lauf der Aufgabe. Eine Ausnahme ist ein geänderter Kurs einer Kursperiode: Er trägt keinen Änderungszeitpunkt und wird erst berücksichtigt, wenn die betroffenen Tage neu berechnet werden. Kürzere Zeiträume werden bei der Anfrage vollständig berechnet, weil das für sie schneller ist.
