---
title: "Hintergrundaufgaben"
date: 2024-02-27T22:54:47+01:00
draft: false
weight: 40
archetype: "default"
---
Hier werden die verschiedenen Aufgabentypen der Batchverarbeitung beschrieben. Gewisse Aufgabentypen können auch an anderen Orten dieser Bedienungsanleitung erläutert worden sein. Einige Aufgabentypen können nicht vom Administrator erstellt werden. Diese werden ausschliesslich vom System erstellt.
##### 0 - Historische- und Innertag-Kursaktualisierung mit Nachführung der Vollständigkeit und Split Kalender update
Mit diesem Aufgagetyp werden verschiedene Aufgaben in der folgenden Reihenfolge abgearbeitet:
1. Historischen und intraday Kursdaten nachgeführt. 
2. [Vollständigkeit historischer Wertpapier-Kursdaten](../../../admindata/historyquotequality/)
3. Bestandstabelle über Bargeld, mehr dazu unter [Architektur von GT - Bestandstabelle](../../../reportportfolio/periodperformance/holdingtable/) 
4. Für Währungspaare ohne historische Kursdaten wird eine neue Aufgabe mit der ID 10 angelegt
##### 1 - Dividende lesen, da Konnektor geändert wurde
Mehr zu diesem Thema, siehe [Dividende](../../../watchlistinstrument/externaldata/historyquote/).

##### 2 - Splits neu einlesen, weil Konnektor geändert wurde oder das Instrument einen Split erfahren hat
Wird ausgelöst, wenn der Datenkonnektor des Splits geändert wurde oder wenn ein möglicher neuer Split im Splitkalender entdeckt wurde. Er lädt alle Splits für ein bestimmtes Wertpapier neu. Wenn die Splits des Instruments geändert wurden, werden auch die historischen Kursdaten komplett neu eingelesen.

##### 3 - Mögliche Erstellung von Währungen und Wiederherstellung von Bestandstabellen
Wenn sich die Hauptwährung eines Klienten oder eines seiner Portfolios ändert, müssen eventuell fehlende Währungspaare für diese Hauptwährung erstellt werden. Ausserdem müssen die Bestandstabellen neu aufgebaut werden. Siehe mehr zu diesem Thema unter "[Bericht Periodenertrag und redundante Daten](../../../reportportfolio/periodperformance/holdingtable)".

##### 4 - Währung des Klienten und der Portfolios geändert, daher Bestandstabellen neu erstellen
Wenn sich die Hauptwährung eines Klienten und seiner Portfolios ändert, müssen eventuell fehlende Währungspaare für diese Hauptwährung angelegt werden. Ausserdem müssen die Bestandstabellen neu aufgebaut werden.

##### 5 - Laden oder erneutes Laden von historischen Preisdaten des Instruments
Beim Anlegen eines Wertpapiers werden die Intraday- und historischen Kursdaten gelesen. Wenn der Konnektor für die historischen Kursdaten geändert wird, müssen die Kurse ebenfalls neu eingelesen werden. Beim Anlegen des Wertpapiers kann dies synchron oder asynchron mit dieser Aufgabe erfolgen. Die historischen Kursdaten werden nur bei Unittests synchron oder gemäss den Einstellungen in `application.properties` geladen. Die Kursdaten für Währungspaare werden immer synchron geladen.

##### 6 - Bestandsmenge eines Wertpapiers hat durch ein Split geändert, alle abhängigen Wertpapierbestände der Klienten werden neu aufgebaut 
Die Veränderung von Splits kann sich auf die Bestandstabelle der Instrumente auswirken. Siehe mehr zu diesem Thema unter "[Bericht Periodenertrag und redundante Daten](../../../reportportfolio/periodperformance/holdingtable)".

##### 7 - Möglicher Neuaufbau von Kontobestand, da sich die historischen Währungskurse geändert haben
Änderungen der historischen Kursdaten von Währungspaaren können sich auf die Bestandstabelle der Geldkonten auswirken. Siehe mehr zu diesem Thema unter "[Bericht Periodenertrag und redundante Daten](../../../reportportfolio/periodperformance/holdingtable)".

##### 8 - Prüfe nach einem Split wiederholend ob die historischen Kursdaten dies widerspiegeln und geladen werden können
Wenn für ein Wertpapier ein Split hinzugefügt wird, kann es einige Tage dauern, bis die angepassten historischen Kursdaten beim Datenprovider vorliegen. Daher wird diese Aufgabe so lange wiederholt, bis die historischen Kursdaten des Wertpapiers erfolgreich eingelesen wurden.

##### 9 - Bestände für einen oder alle Klienten wiederherstellen, normalerweise nach einem Import von einem Export
Die Bestandstabellen werden nur dann aktualisiert, wenn die Transaktionen auf dem üblichen Weg verarbeitet werden. Beim Datenimport oder beim Kopieren von Demo-Benutzerkonten ist dies möglicherweise nicht der Fall. Daher können die Bestandstabellen mit dieser Aufgabe für einen oder alle Mandanten aktualisiert werden. 

##### 10 - Historische Kursdaten eines leeren Währungspaares laden
Dieser Aufgabentype wird in [Währungspaar und Kryptowährungen](../../../watchlistinstrument/instrument/currencypair/) erwähnt.

##### 11 - Kopieren des Quellmandanten in die Demo-Konten
Kopiert die Daten eines Mandanten in andere Mandanten. Diese Funktion kann für Demo-Benutzerkonten verwendet werden. Da Besucher die Daten dieser Benutzerkonten ändern können, sollten diese täglich neu kopiert werden. Das Kopieren der Quellkonten in die Zielkonten erfolgt gemäss den Angaben in `application.properties` und den globalen Einstellungen. Zwei unterschiedliche Quellenkontos sind vorgesehen, damit können beispielsweise deutsch- und  englischsprachige Demo-Benutzerkonten bedient werden.
- **gt.demo.account.pattern.de und gt.demo.account.pattern.en in application.properties**: Mit diesem Muster werden die Zielkonten ausgedrückt. Dieses Muster wird für die Suche nach den Zielkonten verwendet. Wenn kein Demo-Benutzerkonto gewünscht wird, darf nur dieses Suchmuster in der E-Mail des Benutzers keinen Treffer ergeben.
- **gt.source.demo.idtenant.de und gt.source.demo.idtenant.de in globale Einstellungen**: Dies sind die IDs der Quellkonten. Wird das entsprechende Quellkonto nicht gefunden, erfolgt keine Kopierung.

##### 12 - Erzeugt den Handelskalender für Börsen durch einen Hauptindex
Für die Nachführung der freien Handelstage an einem Handelsplatz kann ein Index benutzt werden. Damit erübrigt sich eine manuelle Nachführung der Handelstage. Diese Aufgabe sollte möglichst an einem Sonntag durchgeführt werden, damit werden vorübergehende falsche Eintragungen von Feiertagen vermieden. Mehr zu diesem Thema unter [Handelsplatz](../../../basedata/instrumentbased/stockexchange/).

##### 13 - Führt mögliche neue Dividenden der Instrumente über die Konnektoren nach
Das Laden von Dividenden für die einzelnen Instrumente über Konnektoren wird derzeit mit zwei verschiedenen Algorithmen durchgeführt. Der eine Algorithmus überwacht einen oder mehrere Dividendenkalender, der andere arbeitet mit der erwarteten Periodizität der Dividendenzahlungen:
- **Dividendenkalender**: Der Dividendenkalender wird täglich an den Handelstagen über den entsprechenden Konnektor geladen. Die Dividenden werden hinzugefügt, wenn die in diesem Kalender aufgeführten Instrumente auch in dieser GT-Instanz enthalten sind.
- **Periodizität**: Damit werden Dividendenerträge in der Entität Dividende nachgetragen. Der folgende Algorithmus wird verwendet, um eventuell fehlende Dividendenerträge in der Entität Dividende für Wertpapiere zu ermitteln. Dies geschieht auf Basis des Datums der letzten Dividendenzahlung und der Periodizität der erwarteten Zahlungen. Zusätzlich wird auch die Dividendenzahlung in der Entität Transaktion berücksichtigt, wenn die Dividendenzahlung jünger ist als das Datum in der Entität Dividende.

##### 14 - Prüft alle Klienten auf offene Positionen von inaktiven Instrumenten und ermittelt mögliche fehlende Dividenden oder Zinse
Hierbei handelt es sich um die Überprüfung, ob Dividenden oder Zinsen für gehaltene Positionen in den Transaktionen erfasst werden. Dieser Hintergrundjob sollte täglich ausgeführt werden. Darüber hinaus wird geprüft, ob es offene Positionen für ein Instrument gibt, das nicht mehr gehandelt wird. Es gibt zwei verschiedene Verfahren, um eventuell fehlende Dividenden- oder Zinszahlungen zu ermitteln:
- **Ausschüttungshäufigkeit und letzte Zahlung**: Wann die nächste Dividenden- oder Zinszahlung zu erwarten ist, lässt sich aus der Ausschüttungshäufigkeit und der letzten Zahlung ableiten. Dieses Verfahren wird nur bei Instrumenten angewendet, bei denen die Daten der Dividendentabelle nicht verfügbar sind oder bei denen die einzelnen Einträge kein Zahlungsdatum haben.
- **Dividentabelle**: In GT wird eine Tabelle der Dividendenzahlungen über Konnektoren nachgefühert. Durch die Verknüpfung dieser Tabelle mit den Transaktionen kann geprüft werden, ob bei den Dividendengeschäften eventuell Einträge fehlen. Das Transaktionsdatum kann um einige Tage vom Zahlungsdatum der Dividendentabelle abweichen. Ausserdem prüft dieses Verfahren nur ca. ein Jahr in die Vergangenheit.

##### 15 - Nicht abgeschlossene Benutzer-Registrierungen bereinigen  
Bei der Benutzerregistrierung wird ein Verifizierungstoken erstellt. Dieses muss in einem weiteren Schritt vom zukünftigen Benutzer per E-Mail bestätigt werden. Wird diese Verifizierung nicht erfolgreich durchgeführt, wird der Benutzer und das Verifizierungstoken gelöscht.

##### 16 - Laden der historischen Wechselkursdaten der ECB
Mehr dazu unter [European Central Bank](../../../watchlistinstrument/externaldata/)

##### 17 - Überwachung der Konnektoren für historische Kursdaten
Dieser Aufgabentyp überprüft ob ein Konnector für **historische Kursdaten** möglicherweise nicht mehr funktioniert. Dabei können über die globalen Einstellungen gewisse Parameter der Überprüfung angepasst werden. Im Falle einer möglichen Fehlfunktion eines Konnektors bekommt der **Hautadministrator** eine Nachricht. Diese Aufgabe sollte täglich durchgeführt werden.
- **gt.history.observation.retry.minus** Übertragungsfehler bei maximaler Wiederholung (**gt.history.retry**) minus dieser Anzahl. Falls `gt.history.retry` den Wert 4 enthält, sollte dieser Wert 0 oder 1 sein. So würde ein Instrument bei 4 bzw. 3 Wiederholungen als nicht fuktionierend betrachtet.
- **gt.history.observation.days.back**: Instrument wird nur berücksichtigt, wenn eine erfolgreiche Übertragung innerhalb des aktuellen Datums minus dieser Anzahl von Tagen stattgefunden hat. Auf diese Weise verfälschen eventuell nicht mehr aktive, aber als aktiv geführte Instrumente die Berechnung nicht.
- **gt.history.observation.falling.percentage**: Meldung, wenn mindestens so viel Prozent eines Konnektors ausgefallen sind. Bei 100 % funktioniert der Konnektor wahrscheinlich gar nicht mehr.

##### 18 - Überwachung der Konnektoren für Innertag Kursdaten
Dieser Aufgabentyp prüft, ob ein Konnektor für **Intra-day-Kursdaten** möglicherweise nicht mehr funktioniert. Wie bei der Überwachung der historischen Kursdaten erhält der **Hautadministrator** eine Meldung. Mittels globale Einstellungen können folgende Parameter verändert werden:
- **gt.intraday.observation.retry.minus**: Siehe **gt.history.observation.retry.minus** von Aufgabentype 17.
- **gt.intraday.observation.or.days.back**: Ein Konnektor eines Instruments wird als fehlerhaft eingestuft, wenn die Anzahl der Wiederholungen zu hoch ist oder wenn seit dem aktuellen Datum minus diesem Wert als Anzahl der Tage keine Aktualisierung erfolgt ist.
- **gt.intraday.observation.falling.percentage**: Siehe **gt.history.observation.falling.percentage** von Aufgabentype 17.

##### 19 - Füllt und speichert die benutzerdefinierten Felder von Benutzer 0
Einige globale benutzerdefinierte Felder haben eine längere Gültigkeitsdauer. Dadurch kann der Inhalt persistent werden. Ausserdem ist der Aufwand für die Erstellung ihrer Inhalte manchmal zeitaufwendig, z.B. weil Datenlieferanten kontaktiert werden müssen. Daher ist eine tägliche Aktualisierung sinnvoll.

##### Splits
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

