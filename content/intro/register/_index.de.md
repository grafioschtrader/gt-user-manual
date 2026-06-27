---
title: "Registrieren und Anmelden"
date: 2021-03-23T11:00:00+01:00
draft: false
weight: 30
archetype: "default"
---
GT ist eine **mandantenfähige Webanwendung**, für deren Nutzung eine Registrierung mit einigen Angaben erforderlich ist. Zusätzlich ist für eine Benutzersitzung eine Anmeldung mit **E-Mail** und **Passwort** erforderlich.

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

{{% notice style="info" title="JWT und Password ändern" %}}
Ein JWT verliert seine Gültigkeit, wenn das Ablaufdatum überschritten wird, daher können mehrere JWTs eines Benutzers gleichzeitig gültig sein. Eine Änderung des Passwortes macht die vorherigen JWT nicht ungültig, d.h. das JWT sollte nicht in "falsche Hände" geraten. Wir glauben, dass das Ungültigmachen von JWTs für GT nicht notwendig ist, da wir keine große Gefahr von Cross-Site-Scripting(XSS)- oder Cross-Site-Request-Forgery (CSRF)-Angriffen sehen.
{{% /notice %}}

### Authentifizierung
Zurzeit verlang GT nur die E-Mail und ein Passwort für die Anmeldung. Der Administrator kann gewisse Vorgaben für die Passwortstärke geben, siehe dazu "[Globale Einstellungen](../../admindata/globalsettings)".

### Brute-Force-Attacke
Nach **5** fehlgeschlagenen Anmeldeversuchen auf derselben IP-Adresse wird das Backend von GT für eine bestimmte Zeitspanne keine Anmeldeversuche für diese gesperrte IP-Adresse durchführen.
