---
title: "Historische Daten"
date: 2026-11-20T22:54:47+01:00
draft: false
weight: 32
archetype: "default"
---
In GT werden **Historische Kursdaten** und **Splits** aus externen Datenquellen bezogen.

## Historische Kursdaten (Tagesendkurse)
Tagesendkurse sind für GT sehr wichtig, aus diesem Grund gibt es für deren Bearbeitung die Ansicht **Historische Kurse** im **Zusatzbereich**. Standardmässig werden diese Kursdaten **täglich** über eine oder mehrere **Hintergrundaufgabe/n** aktualisiert.

GT speichert die historischen Kurse in der Stückelung nach allen Splits des Wertpapiers, also splitbereinigt. Kommt ein neuer Split hinzu, lädt GT die Kurshistorie vollständig neu, sobald die Kursquelle die bereinigten Kurse liefert. Bis dahin prüft eine Hintergrundaufgabe dies während einiger Tage wiederholt, siehe [Ablauf eines Splits]({{% relref "/admindata/taskdatachangemonitor/taskdescription/splitdividend/#SPLITFLOW" %}}). Für eine erfasste [Abspaltung]({{% relref "/basedata/securityaction/spinoff" %}}) gilt eine Ausnahme: Die Kurse des Mutterwertpapiers vor dem Ex-Tag speichert GT unbereinigt, so wie an diesen Tagen gehandelt wurde.

## Splits
Splits können manuell mit dem Wertpapier erfasst werden, andererseits überwacht GT **täglich** die Splits in den Kalendern von [Invesing.com](https://www.investing.com/stock-split-calendar/) und [Yahoo](https://finance.yahoo.com/calendar/splits). Die Überwachung führt dazu, dass über den Konnektor des Wertpapieres der vermutliche Split übernommen wird. Hierbei ist das Problem die korrekte Erkennung des Wertpapiers über den Namen des Unternehmens oder des Symbols. Mit dem indirekten Weg über den Konnektor des Wertpapieres wird sicher gestellt, dass keine "falschen" Splits dem Wertpapier zugeordnet werden. Den ganzen Ablauf zeigt [Ablauf eines Splits]({{% relref "/admindata/taskdatachangemonitor/taskdescription/splitdividend/#SPLITFLOW" %}}). Ein [von Hand erfasster Split]({{% relref "/watchlistinstrument/instrument/securityderived/security" %}}) wird vom System nicht überschrieben oder entfernt.
### Die Wahl der Datenquelle
Folgende **Datenquellen** liefern **Splits**:
- [Yahoo USA Finance](https://finance.yahoo.com/): Kostenlos. Die Ermittlung der **URL-Erweiterung** ist entsprechend der Kursdaten.
- [EOD Historical Data](https://eodhistoricaldata.com) und [Eulerpool](https://eulerpool.com/financial-data-api): Mit API-Key. Die **URL-Erweiterung** entspricht jener der Kursdaten.
- [DivvyDiary](https://divvydiary.com/): Nur, wenn ein API-Key hinterlegt ist.

Die Splits müssen zur Kurshistorie passen. Die historische Datenquelle muss ihre Kurse um genau die Splits bereinigt liefern, welche die Split-Datenquelle meldet. Am einfachsten ist das, wenn Kurse und Splits von derselben Datenquelle stammen.
### Abspaltung als Split
Manche Datenanbieter, etwa Yahoo und EOD Historical Data, melden auch eine Abspaltung als Split des Mutterwertpapiers, beispielsweise einen Split von 1000 zu 1281 für die Abspaltung von GE HealthCare von General Electric am 04.01.2023. Solange keine Abspaltung erfasst ist, übernimmt GT einen solchen Split wie jeden anderen. Das Mutterwertpapier erhält dann rechnerisch mehr Stück, und das abgespaltene Wertpapier fehlt im Depot. Erfasst ein Administrator die [Abspaltung]({{% relref "/basedata/securityaction/spinoff" %}}), verwirft GT den Split am Ex-Tag und bringt die Kurshistorie des Mutterwertpapiers in die unbereinigte Form, siehe [Split des Datenanbieters und Kurshistorie des Mutterwertpapiers]({{% relref "/basedata/securityaction/spinoff/#HISTORYBASIS" %}}). Erfassen Sie eine Abspaltung nicht als Split von Hand.

## Dividende
Dividenden werden für die **annualisierte Rendite** und zukünftig in der noch nicht implementierten historischen Simulation der Wertentwicklung eines oder mehrere Portfolio genutzt. Es wird keine automatische Übernahme von Dividenden in die realen Portfolios geben. Zurzeit können manuel keine Dividenden erfasst werden.

### Die Wahl der Datenquelle
Wir haben zwei kostenlose **Datenquellen** für **Dividenden**:
- [DivvyDiary](https://divvydiary.com/): Diese Datenquelle muss sicherlich manuel überprüft werden: 
  + Manchmal ist die Historie nicht vollständig und nur die letzten Jahre sind vorhanden.
  + Die Währung der Dividende entspricht nicht immer der Währung des Instruments. Diese abweichende Währung muss angegeben werden.
  + Der Betrag ist nicht Aktiensplit bereinigt.
- [Yahoo USA Finance](https://finance.yahoo.com/): Die Ermittlung der **URL-Erweiterung** ist entsprechend der Kursdaten. 
  + Der Betrag ist gemäss Aktiensplits bereinigt.

DivvyDiary stellt zudem einen **Ex-Dividenden-Kalender** bereit, über den GT erkennt, *wann* eine Ausschüttung stattgefunden hat. Dies ist von der Rolle als Datenquelle für die einzelne Dividendenhistorie zu unterscheiden.

### Wie die Dividenden in System kommen
Eine **Hintergrundaufgabe**, die standardmässig **täglich ausgeführt** wird, ermittelt auf zwei Wegen, für welche Instrumente neue Dividenden abgefragt werden sollen.
```mermaid
graph TD
    A[Tägliche Hintergrundaufgabe] --> B[Ex-Dividenden-Kalender]
    A --> C[Frequenz-Rückfallebene]
    B -->|Treffer über ISIN und Währung| D[Konnektor des Instruments lädt die Dividenden]
    C -->|letzter Ex-Tag und Häufigkeit plus 10 Tage| D
    B -->|kein Dividenden-Konnektor zugeordnet| E[Eigentümer wird benachrichtigt]
```
Der **erste Weg** ist der **Ex-Dividenden-Kalender**. Die Aufgabe durchläuft Tag für Tag den Zeitraum seit ihrer letzten Ausführung und prüft die im Kalender angekündigten Ausschüttungen. Eine angekündigte Dividende wird über die **ISIN und die Währung** einem in GT geführten Instrument zugeordnet. Bei einer Übereinstimmung wird die Dividendenhistorie – wie bereits bei den Splits beschrieben – über den **Konnektor des Instruments** neu geladen; der Kalender liefert also nur den Hinweis auf den Ex-Tag, die eigentlichen Beträge stammen vom Konnektor des Instruments. Wird ein Instrument im Kalender gefunden, dem **kein Dividenden-Konnektor** zugeordnet ist, so wird der Eigentümer des Instruments benachrichtigt, damit ein Konnektor hinterlegt werden kann.

Der **zweite Weg** dient als Rückfallebene für Instrumente, die von keinem Kalender abgedeckt werden. Die Selektion basiert auf dem berechneten Datum des letzten **Ex-Tags** und der **Ausschüttungshäufigkeit** plus 10 Tage. Dieses berechnete Datum muss jünger als das Datum von "**Dividenden Check**" sein, damit über einen bestimmten Zeitraum nicht dasselbe Instrument täglich auf neue Dividenden überprüft wird.

Zusätzlich kann auch die Eintragung einer Transaktion mit Dividende ein Auslöser für die Abfrage einer Datenquelle sein. Falls in den Transaktionen eine neuere Dividende für das Instrument vorliegt als bei den Dividenden des Instruments, wird die entsprechende Datenquelle mit einer Verzögerung von 10 Tagen abgefragt.
