---
title: "Lesezugriff auf das eigene Portfolio teilen"
date: 2026-06-15T22:54:47+01:00
draft: false
weight: 20
archetype: "default"
---
Mit dieser Funktion geben Sie einer anderen Person Lesezugriff auf Ihr **eigenes** Portfolio. Sie behalten Ihren **Klient** und pflegen ihn weiterhin selbst; die andere Person darf ausschliesslich lesen. Das unterscheidet die Funktion von [Mehrere Klienten verwalten]({{< relref "/tenantportfolio/client/managedclients" >}}), wo ein Berater einen **eigenständigen** Klienten anlegt und das Portfolio für die Kundin oder den Kunden aufbaut. Die gemeinsamen Grundlagen – das Menü **Klient**, das Wechseln und der Lese-Modus – sind unter [Geteilter Zugriff – Grundlagen]({{< relref "/tenantportfolio/client/sharedaccess" >}}) beschrieben.

## Wer teilen darf
Jeder registrierte Benutzer kann den Lesezugriff auf das eigene Portfolio teilen, solange er sich im eigenen **Klient** befindet und nicht selbst nur lesenden Zugriff hat. Dazu dient im Menü **Klient** der Eintrag **Lesezugriff teilen**.

## Zwei Möglichkeiten
Je nachdem, ob die Empfängerin oder der Empfänger bereits ein Konto in dieser Grafioschtrader-Installation besitzt, ergeben sich zwei Möglichkeiten:
- **Bereits registriert**: Die Person erhält Lesezugriff auf Ihr Portfolio. Sie wechselt anschliessend über **Zu Klient wechseln** in Ihren **Klient** und sieht Ihr Portfolio im Lese-Modus.
- **Noch nicht registriert**: Es wird ein reiner Lese-Zugang angelegt, und das vergebene Passwort wird der Person automatisch per E-Mail zugestellt. Nach der Anmeldung sieht sie unmittelbar Ihr Portfolio im Lese-Modus, ohne ein eigenes Portfolio zu besitzen.

## Lesezugriff gewähren
1. Im Menü **Klient** den Eintrag **Lesezugriff teilen** wählen.
2. Die **E-Mail-Adresse** der gewünschten Person eingeben und auf **E-Mail prüfen** klicken.
3. Ist die Person bereits registriert, wird kein Passwort verlangt. Ist sie noch nicht registriert, erscheinen die Felder für das **Passwort**, das zur Sicherheit zweimal einzutragen ist.
4. Mit der Schaltfläche **Lesezugriff gewähren** abschliessen.

Die eigene E-Mail-Adresse sowie Personen, die bereits Lesezugriff besitzen, werden abgewiesen; in diesem Fall erscheint ein Hinweis, und die Eingabe beginnt von vorne.

## Geteilte Leser verwalten
Über den Eintrag **Geteilte Leser** im Menü **Klient** öffnet sich der Dialog **Wer mein Portfolio lesen kann**. Er listet alle Personen mit ihrer **E-Mail** und der Art des Zugriffs auf:
- **Registrierter Benutzer**: eine Person mit eigenem Konto, der Lesezugriff gewährt wurde.
- **Nur-Lese-Zugang**: ein eigens angelegter Lese-Zugang ohne eigenes Portfolio.

Mit **Lesezugriff entziehen** wird der Zugriff wieder aufgehoben. Bei einem **registrierten Benutzer** wird nur der Lesezugriff entfernt; das Konto der Person bleibt bestehen. Bei einem eigens angelegten **Nur-Lese-Zugang** wird dieser Zugang vollständig gelöscht.

{{< mermaid >}}
sequenceDiagram
    participant O as Besitzer
    participant GT as Grafioschtrader
    participant L as Leserin oder Leser
    O->>GT: Lesezugriff teilen mit E-Mail
    GT-->>O: E-Mail prüfen – noch nicht registriert
    O->>GT: Passwort vergeben und Lesezugriff gewähren
    GT->>L: E-Mail mit Anmeldedaten
    L->>GT: Anmeldung mit E-Mail und Passwort
    GT-->>L: Nur-Lese-Ansicht des geteilten Portfolios
{{< /mermaid >}}

{{% notice note %}}
Der Eintrag **Lesezugriff teilen** erscheint nur, wenn die Funktion für die Grafioschtrader-Installation freigeschaltet wurde. Ist sie nicht verfügbar, wurde sie vom Betreiber nicht aktiviert.
{{% /notice %}}
