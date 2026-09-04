---
title: "Testen und Fehlerbehebung"
date: 2026-02-26T12:00:00+01:00
draft: false
weight: 60
archetype: "default"
---
Dieser Abschnitt beschreibt, wie Sie Endpunkt-Konfigurationen testen und häufige Probleme bei der Verwendung generischer Connectors diagnostizieren können.

## Endpunkt testen
Nach dem Speichern einer Endpunkt-Konfiguration können Sie diese überprüfen, ohne den Connector einem echten Wertpapier oder Währungspaar zuweisen zu müssen. Der Test sendet eine einzelne HTTP-Anfrage mit den gespeicherten Endpunkt-Einstellungen und zeigt das Ergebnis direkt in einem Dialog an.

### Zwei-Sitzungen-Arbeitsablauf
Wenn Sie eine Connector-Definition und deren Endpunkte in einer GT-Sitzung bearbeiten, werden die Änderungen in der Datenbank gespeichert. Um diese Änderungen zu testen, benötigen Sie eine **zweite GT-Sitzung** (oder laden Sie die Seite neu), da der Test die Connector-Konfiguration aus der Datenbank lädt und nicht aus dem Speicher des Editors.

### Testdialog öffnen
1. Klappen Sie den zu testenden Endpunkt im Akkordeon auf.
2. Klicken Sie mit der rechten Maustaste auf die Endpunkt-Zeile und wählen Sie **Endpunkt testen...** aus dem Kontextmenü.

### Eingabefelder
Der Dialog zeigt je nach Endpunkt-Konfiguration unterschiedliche Eingabefelder:
- **Wertpapier oder `URL_EXTEND`-Strategie**: **Ticker** (z.B. `AAPL`, `MSFT`)
- **Währung mit `CURRENCY_PAIR`-Strategie**: **Quellwährung** und **Zielwährung** (z.B. `EUR`, `USD`)
- **Historischer Endpunkt (`FS_HISTORY`)**: Zusätzlich **Datum von** und **Datum bis**

### Ergebnisanzeige
Nach dem Klicken auf **Testen** zeigt der Dialog drei Bereiche:

- **Testergebnis**-Fieldset -- Erfolgs- oder Fehleranzeige, HTTP-Statuscode, die Anfrage-URL (mit maskiertem API-Schlüssel) und Ausführungszeit in Millisekunden. Bei fehlgeschlagener Anfrage wird eine Fehlermeldung angezeigt.
- **Geparste Daten**-Tabelle -- die aus der Anbieter-Antwort geparsten Datenzeilen (maximal 200 Zeilen). Die Spaltenüberschriften werden aus den Feld-Zuordnungen des Endpunkts abgeleitet.
- **Rohantwort** (einklappbar) -- die ersten 5000 Zeichen des rohen HTTP-Antwortkörpers, nützlich zur Untersuchung des Anbieterformats, wenn Feld-Zuordnungen nicht korrekt parsen.

### Wichtiges Verhalten
- Der Dialog **bleibt nach jedem Test offen**, sodass Sie Parameter anpassen und erneut testen können, ohne ihn neu öffnen zu müssen.
- Es wird nur eine **einzelne Anfrage** gesendet (keine Paginierung). Wenn der Endpunkt Paginierung verwendet, wird nur die erste Seite abgerufen.
- **Endpunkt-Optionen** wie `SKIP_WEEKEND_DATA` oder `REMOVE_DUPLICATE_DATES` werden beim Testen **nicht angewendet**. Die rohe geparste Ausgabe wird exakt so angezeigt, wie sie vom Anbieter zurückgegeben wird.

## Historische Kursdaten fehlen nach dem Erstellen eines Wertpapiers
Sie haben ein neues Wertpapier mit einem zugewiesenen generischen Connector erstellt, aber es werden keine historischen Kursdaten angezeigt.

**Erster Schritt:** Öffnen Sie den [Stapelverarbeitungsmonitor]({{< relref "/admindata/taskdatachangemonitor" >}}) und prüfen Sie, ob der historische Datenladevorgang erfolgreich abgeschlossen wurde oder eine Fehlermeldung erzeugt hat. Die Fehlermeldung weist in der Regel auf die Ursache hin. Nachfolgend die häufigsten Fehler und deren Lösungen.

### Doppelte Datumseinträge
**Fehlermeldung:** `DataIntegrityViolationException` mit `Duplicate entry for key 'IHistoryQuote_id_Date'`.

**Ursache:** Der Datenanbieter liefert überlappende Daten über Paginierungsseiten hinweg oder gibt doppelte Zeilen für dasselbe Datum zurück.

**Lösung:** Aktivieren Sie die [Endpunkt-Option](../endpoints/#endpunkt-optionen) `REMOVE_DUPLICATE_DATES` beim betroffenen historischen Endpunkt. Damit wird sichergestellt, dass nur der erste Eintrag pro Datum beibehalten wird, wodurch Duplikatschlüssel-Verletzungen beim Import verhindert werden.

### Wochenenddaten in historischen Kursen
**Fehlermeldung:** Kein expliziter Fehler, aber es treten unerwartete Lücken in den Daten auf oder es werden Qualitätswarnungen für das Instrument angezeigt.

**Ursache:** Der Datenanbieter liefert Zeilen für Samstag und/oder Sonntag, die für die meisten Börsen keine gültigen Handelstage sind. Diese Wochenend-Zeilen können die Datenqualitätsprüfungen von GT beeinträchtigen.

**Lösung:** Aktivieren Sie die [Endpunkt-Option](../endpoints/#endpunkt-optionen) `SKIP_WEEKEND_DATA` beim betroffenen historischen Endpunkt. Dadurch werden alle Datenpunkte, die auf Samstag oder Sonntag fallen, vor dem Speichern herausgefiltert.
