---
title: "Kursdaten und Konnektoren"
date: 2026-10-19T12:00:00+02:00
draft: false
weight: 20
archetype: "default"
---
Diese Hintergrundaufgaben laden die Kursdaten der Instrumente und überwachen die Konnektoren, über die sie bezogen werden. Die tägliche Aufgabe 30 aktualisiert alle Kurse und erledigt anschliessend eine Reihe weiterer Arbeiten, darunter das Lesen der Split-Kalender. Die Aufgaben 35 und 40 laden die Kurse eines einzelnen Wertpapiers beziehungsweise Währungspaares, die Aufgabe 45 die Wechselkurse der Europäischen Zentralbank. Scheitert ein Konnektor bei einem Instrument wiederholt, steigt dessen Wiederholungszähler. Die Aufgaben 46 und 47 melden dem Hauptadministrator, wenn dies bei einem Konnektor gehäuft auftritt, und die Aufgabe 49 setzt die Zähler nach der Behebung der Störung zurück. Die wöchentliche Aufgabe 56 lädt keine Kurse, sondern die fundamentalen Basiswerte der Aktien, mit denen die Indikatoren einer Watchlist etwa das Kurs-Gewinn-Verhältnis berechnen.

## 30 - Historische- und Innertag-Kursaktualisierung mit Nachführung der Vollständigkeit und Split Kalender update {#JOB30}
Mit diesem Aufgagetyp werden verschiedene Aufgaben in der folgenden Reihenfolge abgearbeitet:
1. Historischen und intraday Kursdaten nachgeführt. 
2. Die Kurslücken der Währungspaare werden geschlossen, damit für jeden Kalendertag ein Wechselkurs vorliegt, siehe [Kurse für jeden Kalendertag](../../../../watchlistinstrument/instrument/currencypair/#kurse-für-jeden-kalendertag)
3. Fehlende Handelstage von Instrumenten, die als [Instrument ohne Kursdaten]({{% relref "/basedata/bankruptsecurity" %}}) markiert sind, werden linear befüllt, damit ein einziger verstummter Emittent nicht ganze Tage aus der periodischen Performance aller Halter entfernt.
4. [Vollständigkeit historischer Wertpapier-Kursdaten](../../../../admindata/historyquotequality/)
5. Die Split-Kalender werden gelesen. Wird darin ein Split eines Wertpapiers dieser GT-Instanz gefunden, wird die [Aufgabe 32](../splitdividend/#JOB32) angelegt.
6. Bestandstabelle über Bargeld, mehr dazu unter [Architektur von GT - Bestandstabelle](../../../../reportportfolio/periodperformance/holdingtable/) 
7. Für Währungspaare ohne historische Kursdaten wird eine neue Aufgabe mit der ID 40 angelegt

## 35 - Laden oder erneutes Laden von historischen Preisdaten des Instruments {#JOB35}
Beim Anlegen eines Wertpapiers werden die Intraday- und historischen Kursdaten gelesen. Wenn der Konnektor für die historischen Kursdaten geändert wird, müssen die Kurse ebenfalls neu eingelesen werden. Beim Anlegen des Wertpapiers kann dies synchron oder asynchron mit dieser Aufgabe erfolgen. Die historischen Kursdaten werden nur bei Unittests synchron oder gemäss den Einstellungen in `application.properties` geladen. Die Kursdaten für Währungspaare werden immer synchron geladen. Wird der Konnektor für die historischen Kursdaten geändert oder diese Aufgabe für ein Wertpapier ausgelöst, werden die bisherigen Kurse zuerst in das [Archiv der historischen Kursdaten]({{% relref "/watchlistinstrument/externaldata/historyquote/pricedata/archive" %}}) kopiert, bevor die Tabelle aus dem neuen Datenanbieter neu aufgebaut wird.

## 40 - Historische Kursdaten eines leeren Währungspaares laden {#JOB40}
Dieser Aufgabentype wird in [Währungspaar und Kryptowährungen](../../../../watchlistinstrument/instrument/currencypair/) erwähnt.

## 45 - Laden der historischen Wechselkursdaten der ECB {#JOB45}
Mehr dazu unter [European Central Bank](../../../../watchlistinstrument/externaldata/)

## 46 - Überwachung der Konnektoren für historische Kursdaten {#JOB46}
Dieser Aufgabentyp überprüft ob ein Konnector für **historische Kursdaten** möglicherweise nicht mehr funktioniert. Dabei können über die globalen Einstellungen gewisse Parameter der Überprüfung angepasst werden. Im Falle einer möglichen Fehlfunktion eines Konnektors bekommt der **Hautadministrator** eine Nachricht. Diese Aufgabe sollte täglich durchgeführt werden.
- **gt.history.observation.retry.minus** Übertragungsfehler bei maximaler Wiederholung (**gt.history.retry**) minus dieser Anzahl. Falls `gt.history.retry` den Wert 4 enthält, sollte dieser Wert 0 oder 1 sein. So würde ein Instrument bei 4 bzw. 3 Wiederholungen als nicht fuktionierend betrachtet.
- **gt.history.observation.days.back**: Instrument wird nur berücksichtigt, wenn eine erfolgreiche Übertragung innerhalb des aktuellen Datums minus dieser Anzahl von Tagen stattgefunden hat. Auf diese Weise verfälschen eventuell nicht mehr aktive, aber als aktiv geführte Instrumente die Berechnung nicht.
- **gt.history.observation.falling.percentage**: Meldung, wenn mindestens so viel Prozent eines Konnektors ausgefallen sind. Bei 100 % funktioniert der Konnektor wahrscheinlich gar nicht mehr.

## 47 - Überwachung der Konnektoren für Innertag Kursdaten {#JOB47}
Dieser Aufgabentyp prüft, ob ein Konnektor für **Intra-day-Kursdaten** möglicherweise nicht mehr funktioniert. Wie bei der Überwachung der historischen Kursdaten erhält der **Hautadministrator** eine Meldung. Mittels globale Einstellungen können folgende Parameter verändert werden:
- **gt.intraday.observation.retry.minus**: Siehe **gt.history.observation.retry.minus** von Aufgabentype 46.
- **gt.intraday.observation.or.days.back**: Ein Konnektor eines Instruments wird als fehlerhaft eingestuft, wenn die Anzahl der Wiederholungen zu hoch ist oder wenn seit dem aktuellen Datum minus diesem Wert als Anzahl der Tage keine Aktualisierung erfolgt ist.
- **gt.intraday.observation.falling.percentage**: Siehe **gt.history.observation.falling.percentage** von Aufgabentype 46.

## 49 - Wiederholungszähler für Konnektor(en) bei aktiven Instrumenten zurücksetzen {#JOB49}
Dieser Aufgabentyp setzt die Wiederholungszähler für historische und Intraday-Kursdaten bei aktiven Instrumenten zurück. Er wird verwendet, um Situationen zu beheben, in denen Konnektoren ihre Wiederholungslimits aufgrund vorübergehender Probleme wie Netzwerkausfällen überschritten haben. Die Aufgabe kann entweder für einen einzelnen Konnektor oder für alle Konnektoren ausgeführt werden.

## 56 - Fundamentale Basiswerte der Aktien laden {#JOB56}
Diese Aufgabe lädt einmal pro Woche den Gewinn je Aktie, den Buchwert und den Umsatz je Aktie, die Anzahl Aktien und die Nettoverschuldung, die in den [Indikatoren einer Watchlist]({{% relref "/watchlistinstrument/watchlist/indicator" %}}) als Fundamentaldaten zur Verfügung stehen. Berücksichtigt werden aktive Aktien in Direktanlage, die auf einer Watchlist eines beliebigen Mandanten stehen oder in einer Transaktion vorkommen. Die Werte von Wertpapieren, die diese Bedingung nicht mehr erfüllen, werden dabei gelöscht. Wird die Aufgabe von Hand mit dem Informationsobjekt **Wertpapier** und dessen ID erstellt, lädt sie nur dieses eine Wertpapier; ohne Informationsobjekt lädt sie alle.
Abgefragt werden Yahoo, Finnhub und Eulerpool. Finnhub liefert im kostenlosen Plan nur Titel an US-Börsen, Eulerpool findet die Titel über die ISIN, und beide werden nur gefragt, wenn ihr [Verbindungs-API-Key]({{% relref "/admindata/connectorapikey" %}}) hinterlegt ist. Jede Quelle wird für sich und mit einer eigenen Pause zwischen zwei Anfragen abgearbeitet, damit ihre kostenlose Abfragegrenze eingehalten wird; für rund 250 Aktien dauert ein Lauf etwa fünf Minuten. Weist eine Quelle den Schlüssel oder den Plan zurück, wird sie für den Rest des Laufs nicht mehr gefragt. Liefert eine Quelle für eine Aktie keine verwertbare Antwort, bleiben deren Werte aus dem letzten Lauf bestehen. Anschliessend prüft GT die neuen Werte gegen den aktuellen Kurs, vergleicht die Quellen untereinander und legt fest, welcher Wert verwendet wird.
Liefert eine Quelle keine einzige verwertbare Antwort oder wurde sie zurückgewiesen, endet die Aufgabe als fehlgeschlagen. Die Fehlermeldung nennt dann für jede Quelle die Anzahl verwertbarer, fehlgeschlagener und nicht unterstützter Aktien sowie den Zeitpunkt der letzten verwertbaren Antwort. Die Werte der übrigen Quellen sind trotzdem gespeichert. Läuft bereits eine solche Aufgabe, beendet sich eine zweite sofort, ohne etwas zu tun.
Der Zeitpunkt wird mit der Eigenschaft `gt.fundamental.load.cron` der Anwendungskonfiguration festgelegt, standardmässig samstags um 04:23 UTC. Bei der Installation wird dieser Zeitpunkt auf eine zufällige Minute zwischen 03:00 und 05:00 UTC verschoben, solange er noch der Vorgabe entspricht, damit nicht alle GT-Instanzen Yahoo zur gleichen Zeit abfragen. Mit `-` entfällt der automatische Lauf, die Aufgabe kann aber weiterhin von Hand erstellt werden. Die Eigenschaft `gt.fundamental.sources` legt fest, welche der Quellen `yahoo`, `finnhub` und `eulerpool` gefragt werden. Bleibt sie leer, verlässt keine Anfrage nach Fundamentaldaten die Instanz, auch nicht bei einer von Hand erstellten Aufgabe.
