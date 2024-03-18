---
title: "Globale Einstellungen"
date: 2021-05-09T22:54:47+01:00
draft: false
weight: 30
archetype: "default"
---
Dieser Tabelle können Sie die **globalen Einstellungen** von GT entnehmen. Sehr viele **Eigenschaften** beziehen sich auf unterschiedliche **Limiten**, wie beispielsweise die Anzahl möglicher Portfolios pro Klient. Die Veränderung der Eigenschaften ist dem Administratoren vorbehalten.

### Bearbeiten globale Einstellungen
Nur der Administrator kann diese bearbeiten, zudem kann nur der **Eigenschaftswert** der vorhandenen Einstellungen geändert werden.

### Eigenschaften und Tabellenspalten
Die Eigenschaften sind selbsterklärend und werden nicht weiter erläutert.

### Besondere Einstellungen
Die Bedeutung gewisser Einstellung wird an anderer Stelle in diesem Dokument beschrieben. An dieser Stelle werden nur die Einstellungen erwähnt, die umfangreicher sind oder nicht an anderer Stelle in dieser Dokumentation erläutert werden.

#### Einstellung Passwort (gt.password.regex.properties)
Mit dieser Eigenschaft kann eine bestimmte Passwortstärke für das Benutzerpasswort festgelegt werden. Diese Einstellung hat in der Ansicht eine aufklappbare Tabellenzeile, aus der die aktuelle Einstellung entnommen werden kann. Mit Hilfe eines regulären Ausdrucks kann eine bestimmte Mindestanforderung für ein gültiges Passwort festgelegt werden. Nachfolgend einige Beispiele:
 
- "^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$": Mindestens acht Zeichen, mindestens ein Buchstabe und eine Zahl.
- "^(?=.*[A-Za-z])(?=.*\d)(?=.*[@$!%*#?&])[A-Za-z\d@$!%*#?&]{8,}$": Mindestens acht Zeichen, mindestens ein Buchstabe, eine Zahl und ein Sonderzeichen.
- "^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$": Mindestens acht Zeichen, mindestens ein Grossbuchstabe, ein Kleinbuchstabe, eine Zahl und ein Sonderzeichen.
- "^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,10}$": Mindestens acht und höchstens 10 Zeichen, mindestens ein Großbuchstabe, ein Kleinbuchstabe, eine Zahl und ein Sonderzeichen.

Standardmäßig ist in GT das erste Beispiel eingestellt. Die Benutzer werden nicht aufgefordert, ein neues Passwort einzugeben.

##### Parameter für die Passworteinstellung
Die nachfolgenden Parameter müssen für die Passworteinstellung vorliegen:
- **regex**: Hier weisen sie den regulären Ausdruck zu.
- **forceRegex**: Entspricht ein Passwort nicht mehr den Anforderungen des **regulären Ausdrucks**, kann der Benutzer gezwungen werden, ein neues, den Anforderungen entsprechendes Passwort einzugeben. Ist "**forceRegex**" auf "**true**" gesetzt, so wird nach der Anmeldung bei Nichterfüllung der **Passwortstärke** der Dialog zur Passwortänderung geöffnet. Nach erfolgreicher Eingabe des neuen Passwortes muss sich der Benutzer erneut anmelden. Ist "**forceRegex**" auf "**false**" gesetzt, so werden die bestehenden Passwörter weiterhin akzeptiert und erst bei einer Passwortänderung wird der reguläre Ausdruck angewendet.
- **de** und **en**: Zur Zeit unterstützt GT diese beiden Sprachen. Daher muss ein Hinweis- bzw. Fehlertext für die entsprechende Sprache und den regulären Ausdruck vorhanden sein. Aus diesem Text muss der Benutzer die Anforderungen des regulären Ausdrucks nachvollziehbar ableiten können.