---
title: "Registrieren und Anmelden"
date: 2026-11-17T12:00:00+01:00
draft: false
weight: 30
archetype: "default"
---
GT ist eine **mandantenfähige Webanwendung**, für deren Nutzung eine Registrierung mit einigen Angaben erforderlich ist. Zusätzlich ist für eine Benutzersitzung eine Anmeldung mit **E-Mail** und **Passwort** oder mit einem **Passkey** erforderlich.

## Registrieren
Für die Registrierung muss der Benutzer die Eingabefelder von zwei Dialogen ausfüllen. Der erste für seine Daten und der zweite für die Daten des Klienten. Dazwischen muss er eine E-Mail bestätigen und sich zum ersten Mal bei GT anmelden. Der Registrierungsprozess endet mit dem Anmelde-Dialog.

{{< mermaid >}}
sequenceDiagram
    autonumber
    participant Benutzer
    participant A as Anmelden-Form
    participant R as Registrien-Form
    participant K as Klient-Form
    participant Backend
    Benutzer->>A: Wählt Registrieren
    A-->>R: Weiterleitung
    Benutzer->>R: Benutzer registrieren
    R-->>Backend: Eingabe prüfen
    Backend-->>Benutzer: Email mit Bestätigungs URI  
    Benutzer->>Backend: Bestätigt URI
    Backend-->>Benutzer: Weiterleitung Anmelden-Form
    Benutzer->>A: Benutzer meldet sich an
    A-->Backend: Authentifizierung prüfen
    Backend-->>Benutzer: Weiterleitung Klient-Form
    Benutzer->>K: Klient eintragen
    K-->>Backend: Eingabe prüfen
    Backend-->>A: Weiterleitung Anmelden-Form
{{< /mermaid >}}
Im Video ist es vielleicht einfacher...
{{< youtube C9MKBfUXLPA >}}

### Eigenschaften
- **Spitzname**: Dieser Spitzname ist pro GT-Instanz einzigartig, andernfalls könnten Sie nicht von den anderen Benutzer unterschieden werden. Zurzeit wird diese zwischen den Benutzer noch nicht genutzt.
- **E-Mail**: Muss eine für die GT-Instanz einzigartig sein, sonst könnte Sie GT nicht von anderen Benutzer unterscheiden.

## Anmelden
Sie können sich mehrfach an GT **anmelden** auch mit demselben Webbrowser. Jedoch sind Sie dafür verantwortlich, dass die entsprechende Ansichten des Benutzeroberfläche den aktuellen Zustand anzeigt. Das **GT-Backend** wird Ihnen nicht die Änderungen der einten **Benutzersitzung** auf der anderen Benutzersitzung propagieren. 

## Authentifizierung und Sicherheit
In "[Unterschiedliche Installationsarten](../../intro/installationupdate/installationbefore)" wird erklärt, warum GT ein JSON Web Token (JWT) verwendet. Dieses JWT hat eine Ablaufzeit, die vom Administrator unter "[Globale Einstellungen](../../admindata/globalsettings)" festgelegt werden kann. Es gibt keine automatische Erneuerung des JWT, d.h. nach der Ablaufzeit muss sich der Benutzer erneut bei GT anmelden. Daher sollte die Ablaufzeit des JWT nicht zu kurz gewählt werden, wir empfehlen 1440 Minuten, was 24 Stunden entspricht. Dieses JWT wird im Sitzungsspeicher des Webbrowsers gespeichert. Dieser Sitzungsspeicher wird beim Abmelden von GT oder beim Schliessen des Browsers gelöscht. Um das JWT vor Diebstahl zu schützen, sollten Sie sich daher von **GT abmelden** oder den Webbrowser schliessen.

Eine Änderung des Passworts oder einer anderen Anmeldemethode beendet alle bisherigen Browsersitzungen beim nächsten Aufruf. In der Sitzung, in der Sie die Änderung vornehmen, bleiben Sie angemeldet. Eine bereits offene WebSocket-Verbindung bleibt bis zum erneuten Verbindungsaufbau bestehen.

## Passwort vergessen
Wählen Sie auf der Anmeldeseite **Passwort vergessen**, geben Sie Ihre E-Mail-Adresse ein und wählen Sie **Reset-Code anfordern**. Falls das Konto berechtigt ist, erhalten Sie eine E-Mail mit einem einmal verwendbaren Code. Die Antwort auf der Seite verrät nicht, ob eine Adresse registriert ist. Nicht bestätigte Konten und Demo-Konten können diese Funktion nicht nutzen.
Die E-Mail enthält immer einen Code zum Kopieren. Hat der Administrator die öffentliche Adresse unter `g.public.base.url` eingerichtet, enthält sie zusätzlich einen Link. Öffnen Sie den Link oder tragen Sie den Code in die Reset-Seite ein. Geben Sie das neue Passwort unter **Neues Passwort** und **Neues Passwort bestätigen** ein und wählen Sie **Passwort zurücksetzen**. Die Passwortregel der Instanz gilt auch hier. Der Code ist 30 Minuten gültig. Eine erneute Anforderung innerhalb von fünf Minuten verschickt keine zweite E-Mail; der erste Code bleibt gültig. Danach ersetzt eine neue Anforderung den alten Code.
Ein erfolgreicher Reset beendet alle Browsersitzungen und widerruft sämtliche persönlichen Zugriffstokens sowie die Autorisierungen Ihrer KI-Assistenten. Melden Sie sich mit dem neuen Passwort an und verbinden Sie benötigte Assistenten erneut. Sie erhalten eine Sicherheitsmitteilung per E-Mail. Der Reset hebt keine Sperre wegen überschrittener Nutzungs- oder Sicherheitsgrenzen auf. In der Telefonanwendung gibt es keine Reset-Seite; verwenden Sie dafür den Browser.

Der Reset ändert nur das Passwort. Eine aktivierte Authenticator-App, die Wiederherstellungscodes und Ihre Passkeys bleiben bestehen; nach dem Reset fragt die Anmeldung deshalb weiterhin nach dem Code der App.

## Authentifizierung
Jedes Konto hat ein Passwort, mit dem Sie sich zusammen mit Ihrer E-Mail-Adresse anmelden. Der Administrator kann gewisse Vorgaben für die Passwortstärke geben, siehe dazu "[Globale Einstellungen](../../admindata/globalsettings)". Im Dialog [Anmeldesicherheit]({{% relref "/intro/settings#anmeldesicherheit" %}}) unter **Einstellungen** können Sie zusätzlich Passkeys registrieren und eine Authenticator-App als zweiten Faktor aktivieren. Das folgende Diagramm zeigt, was bei der Anmeldung verlangt wird.
{{< mermaid >}}
flowchart TD
    A[Anmeldeseite] --> B{Anmeldemethode}
    B -->|Mit Passkey anmelden| C[Gerät bestätigt per Fingerabdruck, Gesicht oder PIN]
    C --> Z[Angemeldet]
    B -->|E-Mail und Passwort| D{Authenticator-App aktiviert?}
    D -->|Nein| Z
    D -->|Ja| E[Code der App oder Wiederherstellungscode eingeben]
    E --> Z
{{< /mermaid >}}

### Anmelden mit Passkey
Ist ein Passkey möglich, zeigt die Anmeldeseite die Schaltfläche **Mit Passkey anmelden**. Sie geben dabei keine E-Mail-Adresse ein: Der Browser bietet die Passkeys an, die Sie für diese Instanz gespeichert haben, und Ihr Gerät bestätigt Sie per Fingerabdruck, Gesicht oder PIN. Damit ist die Anmeldung abgeschlossen; ein Passwort oder ein Code der Authenticator-App wird nicht verlangt. Brechen Sie den Dialog des Browsers ab, erscheint keine Fehlermeldung. Die Telefonanwendung bietet dieselbe Schaltfläche unter denselben Voraussetzungen an. Auf dem Telefon steht ein Passkey zur Verfügung, wenn er dort entstanden ist oder ein Passwortmanager ihn von einem anderen Gerät synchronisiert hat.
Die Schaltfläche fehlt, wenn der Browser keine Passkeys unterstützt oder wenn GT nicht unter seiner öffentlichen Adresse mit Hostnamen geöffnet ist, etwa über eine IP-Adresse. Eine Anmeldung mit Passkey prüft nicht, ob Ihr Passwort der aktuellen Passwortregel noch entspricht; das geschieht erst bei der nächsten Anmeldung mit dem Passwort. Ist Ihr Konto wegen überschrittener Limiten gesperrt, melden Sie sich mit E-Mail und Passwort an; nur dort können Sie die Freigabe beantragen.

### Zweiter Faktor
Ist eine Authenticator-App aktiviert, erscheint nach E-Mail und Passwort das Feld **Authenticator-Code oder Wiederherstellungscode**. E-Mail und Passwort bleiben stehen; Sie ergänzen den sechsstelligen Code der App oder einen Ihrer Wiederherstellungscodes und wählen erneut **Anmelden**. Leerzeichen und Bindestriche im Code spielen keine Rolle. Jeder Code gilt nur einmal. Muss die Anmeldung mit einer Notiz wiederholt werden, etwa für einen Antrag auf Freigabe, geben Sie dort einen neuen Code ein, weil der erste bereits verbraucht ist. Auch die Telefonanwendung fragt nach dem Code.

### Brute-Force-Attacke
Nach **5** fehlgeschlagenen Anmeldeversuchen auf derselben IP-Adresse wird das Backend von GT für eine bestimmte Zeitspanne keine Anmeldeversuche für diese gesperrte IP-Adresse durchführen.
Falsche Codes des zweiten Faktors zählen dagegen pro Konto und sperren nicht die IP-Adresse. Nach fünf falschen Codes innert 15 Minuten nimmt GT für dieses Konto 15 Minuten lang keinen Code mehr an. Ein richtiger Code setzt den Zähler zurück.
