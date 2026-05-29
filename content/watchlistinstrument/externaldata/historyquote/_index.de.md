---
title: "Historische Daten"
date: 2021-06-10T22:54:47+01:00
draft: false
weight: 32
archetype: "default"
---
In GT werden **Historische Kursdaten** und **Splits** aus externen Datenquellen bezogen.

## Historische Kursdaten (Tagesendkurse)
Tagesendkurse sind für GT sehr wichtig, aus diesem Grund gibt es für deren Bearbeitung die Ansicht **Historische Kurse** im **Zusatzbereich**. Standardmässig werden diese Kursdaten **täglich** über eine oder mehrere **Hintergrundaufgabe/n** aktualisiert.

## Splits
Splits können manuell mit dem Wertpapier erfasst werden, andererseits überwacht GT **täglich** die Splits in den Kalendern von [Invesing.com](//www.investing.com/stock-split-calendar/) und [Yahoo](//finance.yahoo.com/calendar/splits). Die Überwachung führt dazu, dass über den Konnektor des Wertpapieres der vermutliche Split übernommen wird. Hierbei ist das Problem die korrekte Erkennung des Wertpapiers über den Namen des Unternehmens oder des Symbols. Mit dem indirekten Weg über den Konnektor des Wertpapieres wird sicher gestellt, dass keine "falschen" Splits dem Wertpapier zugeordnet werden.
### Die Wahl der Datenquelle
Zurzeit gibt es nur eine kostenlose **Datenquelle** für die **Splits**:
- [Yahoo USA Finance](//finance.yahoo.com/): Die Ermittlung der **URL-Erweiterung** ist entsprechend der Kursdaten.

## Dividende
Dividenden werden für die **annualisierte Rendite** und zukünftig in der noch nicht implementierten historischen Simulation der Wertentwicklung eines oder mehrere Portfolio genutzt. Es wird keine automatische Übernahme von Dividenden in die realen Portfolios geben. Zurzeit können manuel keine Dividenden erfasst werden.

### Die Wahl der Datenquelle
Wir haben zwei kostenlose **Datenquellen** für **Dividenden**:
- [DivvyDiary](//divvydiary.com/): Diese Datenquelle muss sicherlich manuel überprüft werden: 
  + Manchmal ist die Historie nicht vollständig und nur die letzten Jahre sind vorhanden.
  + Die Währung der Dividende entspricht nicht immer der Währung des Instruments. Diese abweichende Währung muss angegeben werden.
  + Der Betrag ist nicht Aktiensplit bereinigt.
- [Yahoo USA Finance](//finance.yahoo.com/): Die Ermittlung der **URL-Erweiterung** ist entsprechend der Kursdaten. 
  + Der Betrag ist gemäss Aktiensplits bereinigt.

### Wie die Dividenden in System kommen
Durch eine **Hintergrundaufgabe** die standardmässig **täglich ausgeführt** wird, können die **möglichen Instrumente** für eine Dividende selektiert werden. Die Selektion basiert auf dem berechneten Datums des letzten **Ex-Tag** und der **Ausschüttungshäufigkeit** plus 10 Tage. Wobei dieses Berechnete Datum jünger als das Datum von "**Dividenden Check**" sein muss, damit wird vermieden, dass über einen bestimmten Zeitraum dasselbe Instrument täglich auf neue Dividenden überprüft wird.

Zusätzlich kann auch die Eintragung einer Transaktion mit Dividende ein Auslöser für die Abfrage einer Datenquelle sein. Falls in den Transaktionen eine neuere Dividende für das Instrument vorliegt als bei den Dividenden des Instruments, wird die entsprechende Datenquelle mit einer Verzögerung von 10 Tagen abgefragt.
