---
title: "Globale Einstellungen"
date: 2021-05-09T22:54:47+01:00
draft: false
weight: 30
archetype: "default"
---
Dieser Tabelle können Sie die **globalen Einstellungen** von GT entnehmen. Sehr viele **Eigenschaften** beziehen sich auf unterschiedliche **Limiten**, wie beispielsweise die Anzahl möglicher Portfolios pro Klient. Die Veränderung der Eigenschaften ist dem Administratoren vorbehalten.

## Bearbeiten globale Einstellungen
Nur der Administrator kann diese bearbeiten, zudem kann nur der **Eigenschaftswert** der vorhandenen Einstellungen geändert werden.

## Eigenschaften und Tabellenspalten
Die Eigenschaften sind selbsterklärend und werden nicht weiter erläutert.

## Besondere Einstellungen
Die Bedeutung gewisser Einstellung wird an anderer Stelle in diesem Dokument beschrieben. An dieser Stelle werden nur die Einstellungen erwähnt, die umfangreicher sind oder nicht an anderer Stelle in dieser Dokumentation erläutert werden.

### Einstellung Passwort (gt.password.regex.properties)
Mit dieser Eigenschaft kann eine bestimmte Passwortstärke für das Benutzerpasswort festgelegt werden. Diese Einstellung hat in der Ansicht eine aufklappbare Tabellenzeile, aus der die aktuelle Einstellung entnommen werden kann. Mit Hilfe eines regulären Ausdrucks kann eine bestimmte Mindestanforderung für ein gültiges Passwort festgelegt werden. Nachfolgend einige Beispiele:
 
- "^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$": Mindestens acht Zeichen, mindestens ein Buchstabe und eine Zahl.
- "^(?=.*[A-Za-z])(?=.*\d)(?=.*[@$!%*#?&])[A-Za-z\d@$!%*#?&]{8,}$": Mindestens acht Zeichen, mindestens ein Buchstabe, eine Zahl und ein Sonderzeichen.
- "^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$": Mindestens acht Zeichen, mindestens ein Grossbuchstabe, ein Kleinbuchstabe, eine Zahl und ein Sonderzeichen.
- "^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,10}$": Mindestens acht und höchstens 10 Zeichen, mindestens ein Großbuchstabe, ein Kleinbuchstabe, eine Zahl und ein Sonderzeichen.

Standardmässig ist in GT das erste Beispiel eingestellt. Die Benutzer werden nicht aufgefordert, ein neues Passwort einzugeben.

#### Parameter für die Passworteinstellung
Die nachfolgenden Parameter müssen für die Passworteinstellung vorliegen:
- **regex**: Hier weisen sie den regulären Ausdruck zu.
- **forceRegex**: Entspricht ein Passwort nicht mehr den Anforderungen des **regulären Ausdrucks**, kann der Benutzer gezwungen werden, ein neues, den Anforderungen entsprechendes Passwort einzugeben. Ist "**forceRegex**" auf "**true**" gesetzt, so wird nach der Anmeldung bei Nichterfüllung der **Passwortstärke** der Dialog zur Passwortänderung geöffnet. Nach erfolgreicher Eingabe des neuen Passwortes muss sich der Benutzer erneut anmelden. Ist "**forceRegex**" auf "**false**" gesetzt, so werden die bestehenden Passwörter weiterhin akzeptiert und erst bei einer Passwortänderung wird der reguläre Ausdruck angewendet.
- **de** und **en**: Zur Zeit unterstützt GT diese beiden Sprachen. Daher muss ein Hinweis- bzw. Fehlertext für die entsprechende Sprache und den regulären Ausdruck vorhanden sein. Aus diesem Text muss der Benutzer die Anforderungen des regulären Ausdrucks nachvollziehbar ableiten können.

## Validierung der Eingaberegeln für Globalparameters

Damit nicht jeder Unsinn eingegeben werden kann. Es gibt eine Validierung für einige Eigenschaftenwerte.  Diese ist gleichzeitig auch ein Hinweis darauf, welche Eingabewerte akzeptiert werden.

Unterstützte Regeln

| Regel   | Syntax            | Beschreibung                                      | Gilt für       |
|---------|-------------------|---------------------------------------------------|----------------|
| min     | min:N             | Minimal zulässiger Wert                           | propertyInt    |
| max     | max:N             | Maximal zulässiger Wert                           | propertyInt    |
| enum    | enum:N1,N2,N3,... | Wert muss einer der angegebenen Werte sein        | propertyInt    |
| pattern | pattern:REGEX     | Wert muss dem Regex-Muster entsprechen            | propertyString |

Syntax

Regeln werden durch Kommas getrennt: regel1:wert1,regel2:wert2

Hinweis: Bei enum sind die Werte ebenfalls durch Kommas getrennt, aber der Parser behandelt dies korrekt, indem er nur bei `,` trennt, wenn ein Regelname und `:` folgen.

Beispiele

| inputRule            | Beschreibung                        | Gültige Werte | Ungültige Werte |
|----------------------|-------------------------------------|---------------|-----------------|
| min:1,max:99         | Wert zwischen 1 und 99              | 1, 50, 99     | 0, 100, -5      |
| min:0,max:100        | Prozentwert                         | 0, 50, 100    | -1, 101         |
| enum:1,7,30,365      | Bestimmte Tagesintervalle           | 1, 7, 30, 365 | 2, 14, 60       |
| enum:0,1             | Boolean-ähnliche Ganzzahl           | 0, 1          | 2, -1           |
| min:1                | Mindestens 1 (keine Obergrenze)     | 1, 100, 9999  | 0, -1           |
| max:10               | Höchstens 10 (keine Untergrenze)    | -5, 0, 10     | 11, 100         |
| pattern:^[A-Z]{2,3}$ | 2-3 Großbuchstaben                  | "CH", "USD"   | "ch", "EURO"    |
| pattern:^[a-z0-9.]+$ | Kleinbuchstaben, Ziffern und Punkte | "gt.test"     | "GT.Test"       |

Kombinierte Regeln

Mehrere Regeln können kombiniert werden:

| inputRule                         | Beschreibung                                                    |
|-----------------------------------|-----------------------------------------------------------------|
| min:1,max:99                      | Bereichsvalidierung                                             |
| min:0,max:100,enum:0,25,50,75,100 | Bereich + bestimmte erlaubte Werte (enum hat Vorrang)           |