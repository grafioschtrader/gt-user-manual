---
title: "Hintergrundaufgaben"
date: 2026-02-01T22:54:47+01:00
draft: false
weight: 40
archetype: "default"
---
Hier werden die verschiedenen Aufgabentypen der Batchverarbeitung beschrieben. Gewisse Aufgabentypen können auch an anderen Orten dieser Bedienungsanleitung erläutert worden sein. Einige Aufgabentypen können nicht vom Administrator erstellt werden. Diese werden ausschliesslich vom System erstellt.
## 0 - Historische- und Innertag-Kursaktualisierung mit Nachführung der Vollständigkeit und Split Kalender update
Mit diesem Aufgagetyp werden verschiedene Aufgaben in der folgenden Reihenfolge abgearbeitet:
1. Historischen und intraday Kursdaten nachgeführt. 
2. [Vollständigkeit historischer Wertpapier-Kursdaten](../../../admindata/historyquotequality/)
3. Bestandstabelle über Bargeld, mehr dazu unter [Architektur von GT - Bestandstabelle](../../../reportportfolio/periodperformance/holdingtable/) 
4. Für Währungspaare ohne historische Kursdaten wird eine neue Aufgabe mit der ID 10 angelegt
## 1 - Dividende lesen, da Konnektor geändert wurde
Mehr zu diesem Thema, siehe [Dividende](../../../watchlistinstrument/externaldata/historyquote/).

## 2 - Splits neu einlesen, weil Konnektor geändert wurde oder das Instrument einen Split erfahren hat
Wird ausgelöst, wenn der Datenkonnektor des Splits geändert wurde oder wenn ein möglicher neuer Split im Splitkalender entdeckt wurde. Er lädt alle Splits für ein bestimmtes Wertpapier neu. Wenn die Splits des Instruments geändert wurden, werden auch die historischen Kursdaten komplett neu eingelesen.

{{% notice style="info" title="Falsche Split-Detektion" %}}
Die Splits werden teilweise anhand der Namen der Unternehmen ermittelt. Es ist möglich, dass es andere Finanzprodukte mit einem ähnlichen oder demselben Namen gibt. Beispielsweise wurde am 14.11.2025 im Split-Kalender von Yahoo Finance der Split PLTR.NE von Palantir Technologies Inc. erkannt. Dieser Split betraf jedoch nicht die originale Palantir-Aktie, sondern ein Zertifikat. In dieser Grafioschtrader-Instanz war dieses Zertifikat nicht vorhanden. Zudem stimmt das Kürzel PLTR mit der originalen Aktie überein. In der Folge wurde der Job mit der ID 2 für einige Tage immer wieder erstellt und durchgeführt. Sie können einen solchen Job, der noch den Status „Warten” hat, löschen. Damit wird auch die weitere Erstellung dieses Jobs unterbunden. Das System stellt die weitere Erstellung dieses Jobs allerdings auch automatisch nach fünf Tagen ein.
{{% /notice %}}

## 3 - Mögliche Erstellung von Währungen und Wiederherstellung von Bestandstabellen
Wenn sich die Hauptwährung eines Klienten oder eines seiner Portfolios ändert, müssen eventuell fehlende Währungspaare für diese Hauptwährung erstellt werden. Ausserdem müssen die Bestandstabellen neu aufgebaut werden. Siehe mehr zu diesem Thema unter "[Bericht Periodenertrag und redundante Daten](../../../reportportfolio/periodperformance/holdingtable)".

## 4 - Währung des Klienten und der Portfolios geändert, daher Bestandstabellen neu erstellen
Wenn sich die Hauptwährung eines Klienten und seiner Portfolios ändert, müssen eventuell fehlende Währungspaare für diese Hauptwährung angelegt werden. Ausserdem müssen die Bestandstabellen neu aufgebaut werden.

## 5 - Laden oder erneutes Laden von historischen Preisdaten des Instruments
Beim Anlegen eines Wertpapiers werden die Intraday- und historischen Kursdaten gelesen. Wenn der Konnektor für die historischen Kursdaten geändert wird, müssen die Kurse ebenfalls neu eingelesen werden. Beim Anlegen des Wertpapiers kann dies synchron oder asynchron mit dieser Aufgabe erfolgen. Die historischen Kursdaten werden nur bei Unittests synchron oder gemäss den Einstellungen in `application.properties` geladen. Die Kursdaten für Währungspaare werden immer synchron geladen.

## 6 - Bestandsmenge eines Wertpapiers hat durch ein Split geändert, alle abhängigen Wertpapierbestände der Klienten werden neu aufgebaut 
Die Veränderung von Splits kann sich auf die Bestandstabelle der Instrumente auswirken. Siehe mehr zu diesem Thema unter "[Bericht Periodenertrag und redundante Daten](../../../reportportfolio/periodperformance/holdingtable)".

## 7 - Möglicher Neuaufbau von Kontobestand, da sich die historischen Währungskurse geändert haben
Änderungen der historischen Kursdaten von Währungspaaren können sich auf die Bestandstabelle der Geldkonten auswirken. Siehe mehr zu diesem Thema unter "[Bericht Periodenertrag und redundante Daten](../../../reportportfolio/periodperformance/holdingtable)".

## 8 - Prüfe nach einem Split wiederholend ob die historischen Kursdaten dies widerspiegeln und geladen werden können
Wenn für ein Wertpapier ein Split hinzugefügt wird, kann es einige Tage dauern, bis die angepassten historischen Kursdaten beim Datenprovider vorliegen. Daher wird diese Aufgabe so lange wiederholt, bis die historischen Kursdaten des Wertpapiers erfolgreich eingelesen wurden.

## 9 - Bestände für einen oder alle Klienten wiederherstellen, normalerweise nach einem Import von einem Export
Die Bestandstabellen werden nur dann aktualisiert, wenn die Transaktionen auf dem üblichen Weg verarbeitet werden. Beim Datenimport oder beim Kopieren von Demo-Benutzerkonten ist dies möglicherweise nicht der Fall. Daher können die Bestandstabellen mit dieser Aufgabe für einen oder alle Mandanten aktualisiert werden. 

## 10 - Historische Kursdaten eines leeren Währungspaares laden
Dieser Aufgabentype wird in [Währungspaar und Kryptowährungen](../../../watchlistinstrument/instrument/currencypair/) erwähnt.

## 11 - Kopieren des Quellmandanten in die Demo-Konten
Kopiert die Daten eines Mandanten in andere Mandanten. Diese Funktion kann für Demo-Benutzerkonten verwendet werden. Da Besucher die Daten dieser Benutzerkonten ändern können, sollten diese täglich neu kopiert werden. Das Kopieren der Quellkonten in die Zielkonten erfolgt gemäss den Angaben in `application.properties` und den globalen Einstellungen. Zwei unterschiedliche Quellenkontos sind vorgesehen, damit können beispielsweise deutsch- und  englischsprachige Demo-Benutzerkonten bedient werden.
- **gt.demo.account.pattern.de und gt.demo.account.pattern.en in application.properties**: Mit diesem Muster werden die Zielkonten ausgedrückt. Dieses Muster wird für die Suche nach den Zielkonten verwendet. Wenn kein Demo-Benutzerkonto gewünscht wird, darf nur dieses Suchmuster in der E-Mail des Benutzers keinen Treffer ergeben.
- **gt.source.demo.idtenant.de und gt.source.demo.idtenant.de in globale Einstellungen**: Dies sind die IDs der Quellkonten. Wird das entsprechende Quellkonto nicht gefunden, erfolgt keine Kopierung.

## 12 - Erzeugt den Handelskalender für Börsen durch einen Hauptindex
Für die Nachführung der freien Handelstage an einem Handelsplatz kann ein Index benutzt werden. Damit erübrigt sich eine manuelle Nachführung der Handelstage. Diese Aufgabe sollte möglichst an einem Sonntag durchgeführt werden, damit werden vorübergehende falsche Eintragungen von Feiertagen vermieden. Mehr zu diesem Thema unter [Handelsplatz](../../../basedata/instrumentbased/stockexchange/).

## 13 - Führt mögliche neue Dividenden der Instrumente über die Konnektoren nach
Das Laden von Dividenden für die einzelnen Instrumente über Konnektoren wird derzeit mit zwei verschiedenen Algorithmen durchgeführt. Der eine Algorithmus überwacht einen oder mehrere Dividendenkalender, der andere arbeitet mit der erwarteten Periodizität der Dividendenzahlungen:
- **Dividendenkalender**: Der Dividendenkalender wird täglich an den Handelstagen über den entsprechenden Konnektor geladen. Die Dividenden werden hinzugefügt, wenn die in diesem Kalender aufgeführten Instrumente auch in dieser GT-Instanz enthalten sind.
- **Periodizität**: Damit werden Dividendenerträge in der Entität Dividende nachgetragen. Der folgende Algorithmus wird verwendet, um eventuell fehlende Dividendenerträge in der Entität Dividende für Wertpapiere zu ermitteln. Dies geschieht auf Basis des Datums der letzten Dividendenzahlung und der Periodizität der erwarteten Zahlungen. Zusätzlich wird auch die Dividendenzahlung in der Entität Transaktion berücksichtigt, wenn die Dividendenzahlung jünger ist als das Datum in der Entität Dividende.

## 14 - Prüft alle Klienten auf offene Positionen von inaktiven Instrumenten und ermittelt mögliche fehlende Dividenden oder Zinse
Hierbei handelt es sich um die Überprüfung, ob Dividenden oder Zinsen für gehaltene Positionen in den Transaktionen erfasst werden. Dieser Hintergrundjob sollte täglich ausgeführt werden. Darüber hinaus wird geprüft, ob es offene Positionen für ein Instrument gibt, das nicht mehr gehandelt wird. Es gibt zwei verschiedene Verfahren, um eventuell fehlende Dividenden- oder Zinszahlungen zu ermitteln:
- **Ausschüttungshäufigkeit und letzte Zahlung**: Wann die nächste Dividenden- oder Zinszahlung zu erwarten ist, lässt sich aus der Ausschüttungshäufigkeit und der letzten Zahlung ableiten. Dieses Verfahren wird nur bei Instrumenten angewendet, bei denen die Daten der Dividendentabelle nicht verfügbar sind oder bei denen die einzelnen Einträge kein Zahlungsdatum haben.
- **Dividentabelle**: In GT wird eine Tabelle der Dividendenzahlungen über Konnektoren nachgefühert. Durch die Verknüpfung dieser Tabelle mit den Transaktionen kann geprüft werden, ob bei den Dividendengeschäften eventuell Einträge fehlen. Das Transaktionsdatum kann um einige Tage vom Zahlungsdatum der Dividendentabelle abweichen. Ausserdem prüft dieses Verfahren nur ca. ein Jahr in die Vergangenheit.

## 15 - Nicht abgeschlossene Benutzer-Registrierungen bereinigen  
Bei der Benutzerregistrierung wird ein Verifizierungstoken erstellt. Dieses muss in einem weiteren Schritt vom zukünftigen Benutzer per E-Mail bestätigt werden. Wird diese Verifizierung nicht erfolgreich durchgeführt, wird der Benutzer und das Verifizierungstoken gelöscht.

## 16 - Laden der historischen Wechselkursdaten der ECB
Mehr dazu unter [European Central Bank](../../../watchlistinstrument/externaldata/)

## 17 - Überwachung der Konnektoren für historische Kursdaten
Dieser Aufgabentyp überprüft ob ein Konnector für **historische Kursdaten** möglicherweise nicht mehr funktioniert. Dabei können über die globalen Einstellungen gewisse Parameter der Überprüfung angepasst werden. Im Falle einer möglichen Fehlfunktion eines Konnektors bekommt der **Hautadministrator** eine Nachricht. Diese Aufgabe sollte täglich durchgeführt werden.
- **gt.history.observation.retry.minus** Übertragungsfehler bei maximaler Wiederholung (**gt.history.retry**) minus dieser Anzahl. Falls `gt.history.retry` den Wert 4 enthält, sollte dieser Wert 0 oder 1 sein. So würde ein Instrument bei 4 bzw. 3 Wiederholungen als nicht fuktionierend betrachtet.
- **gt.history.observation.days.back**: Instrument wird nur berücksichtigt, wenn eine erfolgreiche Übertragung innerhalb des aktuellen Datums minus dieser Anzahl von Tagen stattgefunden hat. Auf diese Weise verfälschen eventuell nicht mehr aktive, aber als aktiv geführte Instrumente die Berechnung nicht.
- **gt.history.observation.falling.percentage**: Meldung, wenn mindestens so viel Prozent eines Konnektors ausgefallen sind. Bei 100 % funktioniert der Konnektor wahrscheinlich gar nicht mehr.

## 18 - Überwachung der Konnektoren für Innertag Kursdaten
Dieser Aufgabentyp prüft, ob ein Konnektor für **Intra-day-Kursdaten** möglicherweise nicht mehr funktioniert. Wie bei der Überwachung der historischen Kursdaten erhält der **Hautadministrator** eine Meldung. Mittels globale Einstellungen können folgende Parameter verändert werden:
- **gt.intraday.observation.retry.minus**: Siehe **gt.history.observation.retry.minus** von Aufgabentype 17.
- **gt.intraday.observation.or.days.back**: Ein Konnektor eines Instruments wird als fehlerhaft eingestuft, wenn die Anzahl der Wiederholungen zu hoch ist oder wenn seit dem aktuellen Datum minus diesem Wert als Anzahl der Tage keine Aktualisierung erfolgt ist.
- **gt.intraday.observation.falling.percentage**: Siehe **gt.history.observation.falling.percentage** von Aufgabentype 17.

## 19 - Füllt und speichert die benutzerdefinierten Felder von Benutzer 0 {#JOB19}
Einige globale benutzerdefinierte Felder haben eine längere Gültigkeitsdauer. Dadurch kann der Inhalt persistent werden. Ausserdem ist der Aufwand für die Erstellung ihrer Inhalte manchmal zeitaufwendig, z.B. weil Datenlieferanten kontaktiert werden müssen. Daher ist eine tägliche Aktualisierung sinnvoll.

Die Erstellung der globalen benutzerdefinierten Felder kann mit dem globalen Parameter "gt.udf.general.recreate" beeinflusst werden. Wenn dieser Wert grösser als 0 gesetzt wird, werden die Werte aller globalen Felder neu erstellt. Nach einmaliger Ausführung dieser Hintergrundaufgabe wird der Wert automatisch wieder auf 0 gesetzt.

## 20 - Online-Status Updates der GTNet Instanzen nachführen
Dieser Aufgabentyp prüft und aktualisiert den Online- und Beschäftigt-Status aller konfigurierten GTNet-Server. Für jeden konfigurierten GTNet-Eintrag wird ein Ping gesendet, um die Erreichbarkeit zu ermitteln. Anschliessend werden die Flags für Online- und Beschäftigt-Status entsprechend aktualisiert. Diese Aufgabe wird kurz nach dem Start der Anwendung ausgeführt, um sicherzustellen, dass der Server vollständig initialisiert und für HTTP-Anfragen bereit ist.

## 21 - Wiederholungszähler für Konnektor(en) bei aktiven Instrumenten zurücksetzen
Dieser Aufgabentyp setzt die Wiederholungszähler für historische und Intraday-Kursdaten bei aktiven Instrumenten zurück. Er wird verwendet, um Situationen zu beheben, in denen Konnektoren ihre Wiederholungslimits aufgrund vorübergehender Probleme wie Netzwerkausfällen überschritten haben. Die Aufgabe kann entweder für einen einzelnen Konnektor oder für alle Konnektoren ausgeführt werden.

## 22 - GTNet-Austauschprotokolle aggregieren und alte Nachrichten löschen
Dieser Aufgabentyp führt zwei Wartungsaufgaben durch: die Aggregation von Protokolleinträgen und das Löschen alter Preisaustauschnachrichten.

**Protokollaggregation**: Die GTNet-Austauschprotokolleinträge werden rollierend von kürzeren zu längeren Zeiträumen aggregiert. Die Aggregation erfolgt in mehreren Stufen: Einzelne Einträge werden zu täglichen Zusammenfassungen aggregiert, tägliche zu wöchentlichen, wöchentliche zu monatlichen und monatliche zu jährlichen. Der globale Parameter **gt.gtnet.log.aggregate.days** steuert die Schwellenwerte für jede Stufe im Format «D=1,W=7,M=30,Y=365».

**Nachrichtenlöschung**: Alte Preisaustauschnachrichten werden automatisch gelöscht, um den Speicherbedarf zu reduzieren. Der globale Parameter **gt.gtnet.del.message.recv** steuert die Aufbewahrungsdauer im Format «LP=1,HP=5»:
- **LP** (LastPrice): Anzahl Tage, bevor Letztpreis-Austauschnachrichten gelöscht werden
- **HP** (HistoryPrice): Anzahl Tage, bevor historische Preisaustauschnachrichten gelöscht werden

Der Standardzeitplan läuft täglich um 3:00 Uhr UTC. Weitere Informationen zu den Konfigurationsparametern finden Sie unter [Globale Einstellungen GTNet](../../../admindata/gtnet/globalsettings/).

## 23 - GTNet-Austauschkonfigurationen synchronisieren
Dieser Aufgabentyp synchronisiert GTNetExchange-Konfigurationen mit verbundenen GTNet-Peers und aktualisiert die GTNetSupplierDetail-Einträge. Er arbeitet in zwei Modi: Der vollständige Neuerstellungsmodus erstellt alle Lieferantendetaileinträge für jeden Peer neu, während der inkrementelle Modus nur Änderungen seit der letzten Synchronisation synchronisiert. Der Standardzeitplan läuft täglich um 2:00 Uhr UTC im vollständigen Neuerstellungsmodus. Diese Aufgabe kann auch manuell über die [GTNet-Einrichtung](../../../admindata/gtnet/setup/) ausgelöst werden oder automatisch, nachdem eine Datenaustausch-Anforderung akzeptiert wurde.

## 24 - GTNet-Einstellungsänderungen übertragen
Dieser Aufgabentyp überträgt Einstellungsänderungen an alle konfigurierten GTNet-Peers. Wenn sich lokale GTNet-Entitätseinstellungen ändern (wie maxLimit, acceptRequest, serverState oder dailyRequestLimit), benachrichtigt diese Aufgabe alle verbundenen Peers durch Senden einer Einstellungsaktualisierungsnachricht. Diese Aufgabe ist nicht geplant, sondern wird automatisch ausgelöst, wenn GTNet-Einstellungen gespeichert werden. Die Ausführung als Hintergrundaufgabe verhindert, dass die Benutzeroberfläche bei Netzwerkoperationen während der Einstellungsänderung blockiert wird.

## 25 - Ausstehende GTNet-Zukunftsnachrichten zustellen
Dieser Aufgabentyp verarbeitet die Zustellung von Broadcast-Nachrichten, die ein geplantes Ausführungsdatum in der Zukunft haben. Dazu gehören Wartungsfenster-Ankündigungen, Hinweise zur Servereinstellung und deren entsprechende Stornierungsnachrichten. Die Aufgabe führt vier Operationen aus: Erstellen von Zustellversuchen für neue Partner, deren Handshake nach Erstellung einer ausstehenden Nachricht abgeschlossen wurde, Verarbeiten von Stornierungsnachrichten, Zustellen ausstehender Nachrichten an ihre Ziele und Bereinigen abgelaufener Nachrichten. Der Standardzeitplan läuft alle 5 Stunden, aber die Aufgabe wird auch sofort ausgelöst, wenn einer dieser Nachrichtentypen gesendet wird. Weitere Informationen zu GTNet-Nachrichtentypen finden Sie unter [GTNet-Einrichtung](../../../admindata/gtnet/setup/).

## 26 - GTNet-Admin-Nachrichten an Ziele zustellen {#JOB26}
Dieser Aufgabentyp stellt ausstehende Admin-Nachrichten an mehrere GTNet-Ziele zu. Die Aufgabe wird automatisch ausgelöst, wenn ein Administrator über die [Admin-Nachrichten](../../../admindata/gtnet/setup/msgadmin/)-Ansicht eine Nachricht an mehrere ausgewählte Domänen sendet. Durch die asynchrone Verarbeitung im Hintergrund reagiert die Benutzeroberfläche sofort, während die Zustellung separat erfolgt.

Die Aufgabe führt folgende Schritte aus:
- Abfrage aller ausstehenden Zustellversuche für Admin-Nachrichten
- Zustellung der Nachrichten an die jeweiligen Ziele über die M2M-Schnittstelle
- Aktualisierung des Zustellstatus nach erfolgreicher Übertragung
- Protokollierung von fehlgeschlagenen Zustellversuchen

{{% notice info %}}
Diese Aufgabe kann nicht manuell erstellt werden. Sie wird ausschliesslich vom System erstellt, wenn Admin-Nachrichten an mehrere Empfänger gesendet werden.
{{% /notice %}}

## Splits
Für die **Erkennung (ID-0)** und **Nachführung (ID-2)** von **Splits** in GT aus externen Datenquellen, sind unterschiedliche **Hintergrundaufgaben** beteiligt. Es kann mehrere Tage dauern, bis der aus einem Split-Kalender gelesene Split auch beim entsprechenden Wertpapier der externen Datenquellen korrekt abgebildet ist. Im folgenden ist dieser Prozess in einem vereinfachten Flowchart dargestellt, wobei der Hexagon-Knoten für eine Hintergrundaufgabe steht:
{{< mermaid >}}
graph TD;
    A{{Zeitgesteuert täglich}} --> B(Ein oder mehrere Split-Kalender lesen)
    B --> |Möglicher Split für ein Wertpapier gefunden| C{{+2 Minuten: Datenveränderung}}
    C --> D(Split des entsprechenden Wertpapiers einlesen)
    D --> |Problematik Name Wertpapiers im Kalender und bei GT| E{Split gefunden?}
    E --> |Nein| G{{+ 1 Tag: Datenveränderung}}
    G --> D
    E --> |Ja| H{{+ 1 Tag Datenveränderung}}
    H --> I(Historische Kursdaten des Wertpapiers einlesen)
    I --> |GT benötigt Split bereinigte Kursdaten| K{Enthalten historische Kursdaten diesen Split?}
    K --> |Nein| H
    K --> |Ja| L{{+0: Datenveränderung}}
    L --> M(Bestand des betreffenden Wertpapiers der betroffenen Klienten für diesen Split nachführen)
{{< /mermaid >}}
Es ist möglich das dieser Prozess den Namen des Wertpapiers aus dem Split-Kalender falsch erkennt, d.h. es wird versucht, einen Split auf ein in GT bestehendes Wertpapier anzuwenden. Damit wird erfolglos täglich das entsprechende Wertpapier nach diesem Split geprüft. In einem solchen Fall sollte die entsprechende wartende Hintergrundaufgabe mittels diesem Monitor gelöscht werden.

