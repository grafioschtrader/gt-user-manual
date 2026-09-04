---
title: "Historische Kurse"
date: 2026-08-19T22:54:47+01:00
draft: false
weight: 12
archetype: "default"
---
Wird ein **Instrument** in einer **Watchliste** oder **Depot** selektiert ist die Funktion **Tagesendkurse als Tabelle** verfügbar. Zusätzlich sind sie auch über "**Fehlende Tagesendkurse**" bzw. "**Vollständigkeit historischer Wertpapier-Kursdaten**" erreichbar. Diese zeigt im **Zusatzbereich** eine Tabelle mit den historischen Kursdaten und anderen nützlichen Informationen bezüglich dieser **historischen Kursdaten** an.

## Automatische Aktualisierung nach Börsenschluss
GT bietet zwei Methoden zur automatischen Aktualisierung der historischen Kursdaten. Der Administrator kann über die Einstellung **gt.update.price.by.exchange** in den [globalen Einstellungen](../../../../admindata/globalsettings/) festlegen, welche Methode verwendet wird. Bei einem Wert von **0** wird die zeitgesteuerte Aktualisierung verwendet, bei einem Wert grösser **0** die börsenabhängige Aktualisierung.

### Zeitgesteuerte Aktualisierung (Job 30)
Bei dieser klassischen Methode werden alle Instrumente zu einem festen Zeitpunkt aktualisiert, der in `application.properties` definiert ist. Diese Hintergrundaufgabe führt neben der Kursaktualisierung weitere wichtige Aufgaben aus und kann daher nicht deaktiviert werden. Mehr dazu unter [Hintergrundaufgaben](../../../../admindata/taskdatachangemonitor/taskdescription/).

### Börsenabhängige Aktualisierung
Diese Methode berücksichtigt die individuellen Handelszeiten der verschiedenen Börsen. Die historischen Kursdaten werden für jede Börse separat aktualisiert, sobald eine gewisse Zeit nach deren Handelsschluss vergangen ist. Dies bringt folgende Vorteile:
- **Frühere Datenverfügbarkeit**: Kursdaten von Börsen mit frühem Handelsschluss stehen schneller zur Verfügung, ohne auf später schliessende Börsen warten zu müssen.
- **Bessere Datenqualität**: Die Aktualisierung erfolgt erst, wenn die Kursdaten beim Datenanbieter voraussichtlich vollständig vorliegen.
- **Verteilte Last**: Die Anfragen an die Datenanbieter werden über den Tag verteilt, anstatt alle gleichzeitig zu einem festen Zeitpunkt zu erfolgen.

Das folgende Diagramm zeigt den Ablauf der börsenabhängigen Aktualisierung:
{{< mermaid >}}
flowchart TD
    A[Hintergrundprozess prüft Börsen] --> B{Börse geschlossen + Wartezeit abgelaufen?}
    B -->|Nein| C[Berechne Wartezeit bis nächste Börse bereit]
    C --> D[Warte bis nächste Börse bereit]
    D --> A
    B -->|Ja| E[Hole Kursdaten vom Datenanbieter]
    E --> F{Erfolgreich?}
    F -->|Ja| G[Speichere Kursdaten]
    F -->|Nein| H[Markiere für späteren Wiederholungsversuch]
    G --> A
    H --> A
{{< /mermaid >}}

Instrumente, deren Aktualisierung fehlschlägt, werden für einen späteren Wiederholungsversuch markiert. Dadurch können vorübergehende Probleme beim Datenanbieter automatisch behoben werden.

{{% notice style="info" title="Protokollierung zur Kontrolle" %}}
Für die Überwachung der börsenabhängigen Aktualisierung wird vorübergehend ein Protokoll geführt. Dieses zeigt für jede Börse, wann die Aktualisierung durchgeführt wurde und wie viele Instrumente erfolgreich aktualisiert werden konnten.
{{% /notice %}}

### Zusammenspiel beider Methoden
Auch wenn die börsenabhängige Aktualisierung aktiviert ist, bleibt Job 30 aktiv. Er übernimmt weiterhin wichtige Aufgaben wie die Pflege der Vollständigkeitsdaten, die Aktualisierung der Bestandstabellen und die Erstellung von Aufgaben für Währungspaare ohne historische Kursdaten. Die eigentliche Kursaktualisierung wird bei aktivierter börsenabhängiger Methode jedoch von dieser übernommen.

## Zusätzliche Überwachung historischer Kursdaten
Es gibt einige Funktionen in GT für die Überwachung der **historischen Kursdaten**. Diese finden Sie unter [Vollständigkeit historischer Kursdaten](../../../../admindata/historyquotequality/) oder [Kurs Datenfeed](../../../watchlist/pricefeed/).

## Funktionen auf historischen Kursdaten
Zu den üblichen Bearbeitungsfunktionen einer Informationsklasse gibt es zusätzliche Unterstützung für eine möglichst vollständige Historie der Kursdaten.

### Erstellen und bearbeiten historischer Kursdaten
Nur der **Ersteller** des **Wertpapieres** oder ein Benutzer mit ausreichend Rechten kann historische Kursdaten neu erstellen bzw. löschen.
+ **Erstellen** eines **Historischer Kurses** über **Kontextmenü**.
+ **Bearbeiten** eines **Historischer Kurses** auf dem **selektieren historischen Kurs**.
+ **Löschen** eines **Historischer Kurses** auf dem **selektieren historischen Kurs**.

### Exportieren als CSV-Datei
Die historischen Tagesdaten können exportiert werden. Das Exportformat entspricht dem Importformat.

### Importieren historischer Daten
Das Importformat können Sie dem Exportformat entnehmen. Beim Importieren von historischen Tagesdaten muss das Datum und der Schlusskurs vorhanden sein. Aus der ersten Zeile wird die Zuordnung der Spalte zun den Importfelder ermittelt. Im folgenden Beispiel wurde das Datum in der ersten Spalte und der Schlusskurs in der zweiten Spalte erwartet. Als Feldbegrenzer wird ein **Strichpunkt** erwartet. 
```
date;close;volume;open;high;low
18.02.2021;78;;;;
```
Bestehende historische Tagesdaten können mit einem Import nicht überschrieben werden.

### Lineares befüllen fehlender Kursdaten
Beispielsweise ist es möglich das ein Instrument von der Börse genommen wird, um es mit einem anderen Instrument zu verschmelzen, oder dass nach einem Konkurs überhaupt keine Kurse mehr geliefert werden. In dieser Zeit werden keine Kursdaten geliefert, jedoch benötigt GT die für die Berechnung der periodischen Performance möglichst vollständig die historischen Kursdaten. Mit der Funktion **Lineares befüllen fehlender Kursdaten** werden die Kurse der fehlender Handelstage befüllt. Optional können mögliche Wochenende Kurse auf den Freitag derselben Woche verschoben werden, falls dieser Freitag keine Kursdaten hatte; die Option **Verschiebe Wochenende Kurse auf Freitag** ist nur wählbar, wenn überhaupt Kurse an einem Samstag oder Sonntag vorhanden sind.

Bis zu welchem Tag befüllt wird, bestimmen Sie selbst mit dem Feld **Befüllen bis**. Vorgeschlagen wird der heutige Tag, sodass Kurse für noch nicht eingetretene Tage nie versehentlich entstehen. Wählbar ist der Zeitraum, den das **Aktiv bis Datum** des Instruments und der [Handelskalender](../../../../basedata/instrumentbased/stockexchange/) seines Handelsplatzes zulassen; das frühere der beiden Daten begrenzt die Auswahl. Die zu befüllenden Handelstage werden ebenfalls aus diesem Handelskalender abgeleitet.

Dieses späteste Datum liegt durchaus oft in der Zukunft, etwa bei einer Anleihe, deren **Aktiv bis Datum** der Verfall ist. Kurse für künftige Handelstage lassen sich also sehr wohl erzeugen — Sie müssen das Datum dazu aber bewusst nach vorne setzen. Soll die Befüllung über das **Aktiv bis Datum** hinausreichen, ist dieses beim Instrument entsprechend anzupassen.

Ist der Handelskalender der Börse noch nicht bis zum letzten abgeschlossenen Handelstag nachgeführt, erscheint im Dialog ein Hinweis. Er nennt das Datum, bis zu dem der Kalender nachgeführt ist, und die Anzahl der dadurch nicht abgedeckten Handelstage. Weiter als bis zu diesem Datum kann nicht befüllt werden, denn die Börsenfeiertage darüber hinaus sind nicht bekannt und ein Feiertag würde sonst einen Kurs erhalten. Damit auch diese Handelstage befüllt werden können, ist zuerst der Handelskalender des Handelsplatzes nachzuführen.

### Importierte und/oder linear befüllte löschen
Diese Funktion ist das Gegenstück zu **Lineares befüllen fehlender Kursdaten**: Sie entfernt die zuvor linear befüllten Tagesendkurse sowie die von Hand importierten Kursdaten und lässt die vom **Datenanbieter** gelieferten Kurse unverändert. Damit lassen sich vorhandene Befüllungen oder fehlerhafte Importe zurücksetzen.

Mit den Feldern **Datum von** und **Datum bis** legen Sie fest, für welchen Zeitraum gelöscht wird. Vorgeschlagen wird immer der gesamte Bereich vom ältesten bis zum jüngsten vorhandenen Kurs, sodass die Funktion ohne weitere Eingabe alles Gewählte in einem Schritt zurücksetzt. Reicht eine lineare Befüllung in die Zukunft, so gehören diese künftigen Kurse ebenfalls dazu. Ausserhalb dieses Bereichs lassen sich die beiden Daten nicht setzen, da dort ohnehin keine Kurse vorhanden sind, und die beiden Felder begrenzen sich gegenseitig, sodass kein Zeitraum mit vertauschtem Anfang und Ende entstehen kann.

Der Zeitraum ist vor allem dann nützlich, wenn eine lineare Befüllung nur teilweise zurückgenommen werden soll. Ein Beispiel: Sie haben ein nicht mehr gehandeltes Instrument bis zum heutigen Tag linear befüllt und erfahren danach, dass ab einem bestimmten Datum ein neuer Kurs gilt, der mitten in den bereits erzeugten Bereich fällt. Sie löschen die Kurse ab diesem Datum, erfassen den neuen Kurs von Hand und befüllen anschliessend wieder linear.

Nur der **Ersteller** des **Wertpapieres** oder ein Benutzer mit ausreichend Rechten darf diese Funktion ausführen. Allen anderen Benutzern wird der Menüeintrag weiterhin angezeigt, ist aber deaktiviert; dies entspricht dem Verhalten von **Lineares befüllen fehlender Kursdaten**. Da es sich um eine Massenoperation auf gemeinsam genutzten Kursdaten handelt, belastet das Ausführen das Tageskontingent eines **Limit Benutzers** nicht.

### Archiv der historischen Kursdaten
Wechselt der Datenanbieter eines Wertpapiers oder löst ein Split eine vollständige Neueinlesung aus, werden ältere Kurse, die der neue Anbieter nicht mehr liefert, in einem eigenen Archiv aufbewahrt. Mehr dazu unter [Archiv der historischen Kursdaten]({{< relref "archive" >}}). In der Archivansicht können archivierte Daten eingesehen, einzelne Kurse bearbeitet oder gelöscht, exportiert, importiert, mit einem nachträglich erfassten Split korrigiert oder das gesamte Archiv geleert werden.

## Tageskontingent für den Abruf historischer Kurse
GT soll nicht als kostenloser Datenanbieter missbraucht werden. Deshalb ist pro Benutzer und Tag die Anzahl **unterschiedlicher Instrumente** begrenzt, für die historische Kursdaten abgerufen werden können. Mehrfache Abrufe desselben Instruments am selben Tag zählen nur einmal. Gezählt wird jede Ansicht, welche die Kurshistorie eines Instruments lädt, also **Tagesendkurse als Tabelle**, [Tagesendkurse als Liniengrafik](../../../eodchart/) einschliesslich der technischen Indikatoren sowie das [Archiv der historischen Kursdaten]({{< relref "archive" >}}).

Der Administrator legt das Kontingent in der Ansicht [Limite Informationsklasse]({{% relref "/admindata/entitylimit" %}}) über die Limite **Abruf historischer Kurse (Instrumente pro Tag)** der Limitenart **Abfragen pro Tag** fest (Standardwert 250 Instrumente für die Rolle **Benutzer mit Limits**). Für Benutzer mit erweiterten Rechten ist von Beginn weg keine solche Limite erfasst, womit sie unbeschränkt sind. Zudem kann der Administrator einzelnen Benutzern zeitlich befristet ein abweichendes Tageskontingent gewähren; dies geschieht im selben Dialog, mit dem auch die täglichen Änderungslimiten eines **Limit Benutzers** angepasst werden. Wer das Kontingent überschreitet, erhält eine Fehlermeldung und kann bis zum Folgetag keine weiteren, an diesem Tag noch nicht abgerufenen Instrumente laden. Wiederholte Überschreitungen können zur Sperrung des Kontos führen, die nur der Administrator aufheben kann.

{{% notice style="info" title="Keine Auswirkung im Alltag" %}}
Ein normaler Benutzer bemerkt diese Begrenzung nicht, da das Kontingent weit über der üblichen Nutzung liegt. Bereits abgerufene Instrumente können beliebig oft erneut betrachtet werden.
{{% /notice %}}

## Eigenschaften und Tabellenspalten
Die Eigenschaften werden hier nicht weiter besprochen, da sebsterklärend oder durch Quckinfo unterstützt.
