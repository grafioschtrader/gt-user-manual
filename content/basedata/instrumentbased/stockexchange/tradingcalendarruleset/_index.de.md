---
title: "Handelskalender-Regeln"
date: 2026-07-19T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
Ein **Regelsatz** beschreibt die Feiertage eines oder mehrerer Handelsplätze als Regeln. Aus diesen Regeln berechnet GT für jedes Jahr die geschlossenen Tage des Handelskalenders. Im Gegensatz zum [Index für Handelskalender](../) funktioniert ein Regelsatz auch für zukünftige Jahre, für die noch keine Kursdaten vorliegen, und benötigt keinen an der Börse gehandelten Index. Ein Handelsplatz verwendet entweder einen Index **oder** einen Regelsatz, nie beides gleichzeitig.

Die Regelsätze sind geteilte Daten: Jeder Benutzer kann sie einsehen und bearbeiten. Verfügt ein Benutzer nur über eingeschränkte Rechte, wird seine Änderung zu einem Änderungsvorschlag, den eine administrative Person bestätigt. Die Anzahl der Erfassungen, Änderungen und Löschungen pro Tag ist begrenzt.

## Ansicht erreichen
Die Regelsätze werden im Hauptbereich der Ansicht **Handelsplatz** verwaltet. Diese Ansicht ist in zwei Register aufgeteilt: den eigentlichen **Handelsplatz** und die **Handelskalender-Regeln**. Über das zweite Register gelangen Sie zur Tabelle der Regelsätze; Erstellen, Bearbeiten und Löschen erfolgen wie gewohnt über das **Kontextmenü**.

## Tabellenspalten
- **Regel**: Der eindeutige Name des Regelsatzes, beispielsweise der Name der Börse.
- **MIC**: Der optionale Market Identifier Code. Wird beim Erstellen eines Handelsplatzes ein MIC gewählt, für den ein Regelsatz mit demselben MIC existiert, weist GT diesen Regelsatz automatisch zu.
- **Erweitert Regelsatz**: Der übergeordnete Regelsatz, dessen Regeln dieser Regelsatz übernimmt.
- **Verwendet von Börsen**: Die Anzahl Handelsplätze, die ihren Handelskalender aus diesem Regelsatz berechnen.

## Erstellen und bearbeiten
Beim Erstellen oder Bearbeiten eines Regelsatzes werden folgende Angaben erfasst:
- **Regel** (Name): Zwingend, zwei bis vierundsechzig Zeichen, innerhalb aller Regelsätze eindeutig.
- **MIC**: Optional, genau vier Zeichen. Ein MIC darf nur einem einzigen Regelsatz zugewiesen sein.
- **Erweitert Regelsatz**: Optional. Ein Regelsatz kann einen anderen erweitern und dessen Regeln erben. So teilen sich verwandte Handelsplätze eine gemeinsame Grundlage: Die deutschen Regionalbörsen erweitern beispielsweise Xetra, die Schweizer Handelsplätze die SIX. Ein Regelsatz darf sich nicht selbst erweitern, auch nicht über Umwege.

Ein Regelsatz, der von mindestens einer Börse verwendet oder von einem anderen Regelsatz erweitert wird, kann nicht gelöscht werden. Ein Regelsatz ohne eigene Regeln muss einen anderen Regelsatz erweitern.

## Regeln im YAML-Editor
Die eigentlichen Feiertagsregeln werden im **YAML-Editor** erfasst. Der Editor schlägt beim Tippen die möglichen Felder und Werte vor und zeigt zu jedem Eintrag eine Kurzhilfe an. Vor dem Speichern prüft die Schaltfläche die Regeln und listet allfällige Fehler gesammelt auf. Da diese Angaben direkt im Editor eingegeben werden, sind die nachfolgenden englischen Schlüsselwörter Teil der Bedienung und werden hier bewusst genannt.

Ein Regelwerk kennt die folgenden übergeordneten Angaben:
- **note**: Freitext, der Quellen und bekannte Einschränkungen dieses Kalenders für die nächste bearbeitende Person festhält. Er hat keinen Einfluss auf die berechneten Tage.
- **authoritativeFrom** und **authoritativeThrough**: Der Zeitraum in Jahren, für den der Regelsatz durch einen verbindlichen Börsenkalender abgesichert ist. Die Abfrage eines früheren oder späteren Jahres wird abgewiesen, statt geraten zu werden.
- **rules**: Die Liste der wiederkehrenden Schliessungen. Erweitert ein Regelsatz einen anderen, führt er hier nur seine Abweichungen auf; die geerbten Regeln werden zuerst ausgewertet.
- **additionalClosures**: Angekündigte oder historische Schliessungen, die sich nicht sicher als wiederkehrende Regel ausdrücken lassen, etwa ein nationaler Trauertag. Diese Daten werden den berechneten Schliessungen hinzugefügt.
- **openDates**: Daten, an denen eine Regel eine Schliessung erzeugt, die Börse aber dennoch gehandelt hat. Diese Daten werden aus den berechneten Schliessungen entfernt und sind die einzige Möglichkeit, ein einzelnes Auftreten einer Regel aufzuheben.

### Regelarten
Jede Regel besitzt zwingend einen **name** und einen **type**. Welche weiteren Felder nötig sind, hängt von der Regelart ab; eine Regel, der ein benötigtes Feld fehlt, wird beim Speichern abgewiesen.

| type | Bedeutung | Benötigte Felder |
|------|-----------|------------------|
| **FIXED** | Gleicher Monat und Tag in jedem Jahr, z.B. der 1. Januar. | month, day |
| **NTH_WEEKDAY** | Der n-te Wochentag eines Monats, z.B. der dritte Montag im Januar. `nth: -1` wählt den letzten. | month, nth, dayOfWeek |
| **WEEKLY** | Jedes Auftreten eines Wochentags, für ein von Montag bis Freitag abweichendes Börsenwochenende. | dayOfWeek |
| **EASTER_RELATIVE** | Ein Abstand in Tagen zum westlichen Ostersonntag: -2 Karfreitag, 1 Ostermontag, 39 Auffahrt, 50 Pfingstmontag, 60 Fronleichnam. | offset |
| **ORTHODOX_EASTER_RELATIVE** | Wie EASTER_RELATIVE, aber bezogen auf das orthodoxe (julianische) Ostern. | offset |
| **HIJRI** | Ein Datum des islamischen Kalenders, in den gregorianischen umgerechnet. | month, day |
| **EXPLICIT_DATES** | Eine feste Liste von Daten, die sich nicht berechnen lässt, z.B. das chinesische Neujahr. | dates |

Ergänzend stehen diese Felder zur Verfügung:
- **validFrom** und **validTo**: Erstes bzw. letztes Jahr, in dem die Schliessung gilt. Damit lassen sich Feiertage abbilden, die erst ab einem Jahr eingeführt oder ab einem Jahr abgeschafft wurden; auch eine geerbte Regel wird so ab einem Jahr abgeschaltet.
- **observance**: Wie eine auf ein Wochenende fallende Schliessung verschoben wird. `NONE` verwirft sie, was für die meisten kontinentaleuropäischen Börsen richtig ist; `NEAREST_WEEKDAY` (US-Börsen), `SUNDAY_TO_MONDAY`, `NEXT_MONDAY` (britische Ersatztage) und `NEXT_WEEKDAY` verschieben auf einen Werktag. Ohne Angabe gilt `NONE`.
- **halfDay**: `true`, wenn die Börse nur verkürzt handelt statt zu schliessen. Ein solcher Tag wird festgehalten, damit der Regelsatz die Börse vollständig beschreibt, erzeugt aber nie eine Schliessung, denn ein halber Tag ist ein Handelstag.
- **offsetTolerance**: Nur bei HIJRI. Die Anzahl Tage, um die das beobachtete islamische Datum von der Umrechnung abweichen darf, da der Monatsbeginn von der Sichtung des Mondes abhängt.

### Beispiel
Das folgende gekürzte Regelwerk zeigt den Aufbau. Jede Regel steht auf einer eigenen Zeile; die Wochenenden müssen nicht aufgeführt werden, da der Handelskalender Samstag und Sonntag ohnehin getrennt behandelt.
```yaml
note: >
  Feiertage der Beispielbörse. Feiertage werden nicht verschoben, wenn sie auf
  ein Wochenende fallen.
authoritativeFrom: 2000
rules:
  - {name: NewYear, type: FIXED, month: 1, day: 1}
  - {name: GoodFriday, type: EASTER_RELATIVE, offset: -2}
  - {name: EasterMonday, type: EASTER_RELATIVE, offset: 1}
  - {name: LabourDay, type: FIXED, month: 5, day: 1}
  - {name: Ascension, type: EASTER_RELATIVE, offset: 39}
  - {name: WhitMonday, type: EASTER_RELATIVE, offset: 50}
  - {name: NationalDay, type: FIXED, month: 8, day: 1}
  - {name: Christmas, type: FIXED, month: 12, day: 25}
  - {name: StStephen, type: FIXED, month: 12, day: 26}
additionalClosures:
  - 2024-04-15
openDates:
  - 2023-12-26
```

## Schliessungstage anzeigen
Über den aufklappbaren Bereich **Schliessungstage anzeigen** lässt sich der Regelsatz vor dem Speichern für ein einzelnes Jahr auflösen. Die Vorschau berücksichtigt auch die geerbten Regeln des erweiterten Regelsatzes und listet zu jedem berechneten Tag die **Regel** auf, die ihn verursacht hat. Mit **Vorheriges Jahr** und **Nächstes Jahr** wechseln Sie zwischen den Jahren. So lässt sich prüfen, ob die Regeln die erwarteten Feiertage ergeben, bevor der Handelskalender neu berechnet wird.

Die Neuberechnung des Handelskalenders erfolgt nicht sofort, sondern über eine Hintergrundaufgabe. Mehr dazu unter [Hintergrundaufgaben](../../../../admindata/taskdatachangemonitor/taskdescription/#JOB53).
