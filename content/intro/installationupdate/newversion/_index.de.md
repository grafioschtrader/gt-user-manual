---
title: "Update Grafioschtrader"
date: 2026-09-26T12:00:00+02:00
draft: false
weight: 70
archetype: "default"
---
Aufgrund der Architektur von GT ist die Installation zeitlich etwas aufwändig. Dafür ist das Einspielen von neuen Versionen sehr einfach. Dabei werden die bestehenden Daten automatisch in die neue Version migriert. Weitere Informationen werden unter "[Updating GT](https://github.com/grafioschtrader/grafioschtrader/wiki/Updating-GT)" gefunden.
Durchführung und Anmerkungen zum Update
- Wie mit einen Shell-Script GT einen Update erhält
- Wie es GT mit der Versionierung hält
- Sind Ihre bisherigen Daten sicher?

{{< youtube l2etk4rNcfk >}}

## Bestehende Daten nach der Zeitzonenkorrektur
Die Zeitzonenkorrektur verhindert neue Datumsverschiebungen. Bereits früher falsch gespeicherte Angaben werden nicht automatisch anhand Ihrer Zeitzone berichtigt. Vergleichen Sie betroffene Transaktionen, Transfers und Kapitalmassnahmen mit Ihren Abrechnungen. Falls ein Transfer oder eine Kapitalmassnahme am falschen Tag verbucht wurde, machen Sie die Ausführung vor einer erneuten Erfassung rückgängig, damit keine doppelte Buchung entsteht.

Bereits durch eine frühere Split-Anwendung veränderte historische Kurse werden nicht automatisch wiederhergestellt. Wenden Sie den Split nicht einfach nochmals an; die Kursreihe muss zuerst geprüft und gegebenenfalls wiederhergestellt werden. Bereits überschriebene Erstellungszeitpunkte lassen sich ebenfalls nicht zuverlässig wiederherstellen. Ältere Systemzeitpunkte können aus unterschiedlichen bisherigen Server- und Datenbankeinstellungen stammen und werden nicht pauschal nachträglich verschoben.

Prüfen Sie ausserdem [im Voraus erfasste Daueraufträge]({{% relref "/transaction/standingorder" %}}), bei denen der nächste Termin noch fehlt.
