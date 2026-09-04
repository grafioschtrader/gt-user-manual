---
title: "Geteilter Zugriff – Grundlagen"
date: 2026-06-15T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
Bisher war ein Benutzer genau einem **Klient** zugeordnet und sah ausschliesslich die eigenen **Portfolios**. Darüber hinaus bietet Grafioschtrader zwei verwandte Funktionen, die auf demselben Zugriffsmodell beruhen und beide über das Menü **Klient** sowie über einen Lese-Modus arbeiten:
- [Mehrere Klienten verwalten]({{< relref "/tenantportfolio/client/managedclients" >}}): Ein Berater legt mehrere **eigenständige** Klienten an, pflegt sie und wechselt zwischen ihnen.
- [Lesezugriff auf das eigene Portfolio teilen]({{< relref "/tenantportfolio/client/sharereadaccess" >}}): Eine Besitzerin oder ein Besitzer gibt das **eigene** Portfolio einer anderen Person ausschliesslich zum Lesen frei.

Diese Seite beschreibt, was beiden Funktionen gemeinsam ist. Die Besonderheiten finden Sie auf den beiden verlinkten Seiten.

## Das Menü **Klient**
Sobald die Funktion für die Grafioschtrader-Installation freigeschaltet ist, erscheint in der oberen Menüleiste nach dem Menü **Einstellungen** das zusätzliche Menü **Klient**. Welche Einträge es enthält, hängt von der Rolle und davon ab, ob man sich gerade im eigenen oder in einem fremden Klient befindet.

| Eintrag | Sichtbar für |
| --- | --- |
| **Klient erstellen** | Berater (mindestens **Benutzer ohne Limits**) |
| **Zu Klient wechseln** | jeden nicht lesenden Benutzer, der irgendwohin wechseln kann |
| **Lesezugriff teilen** | jeden nicht lesenden Benutzer, im eigenen Klient |
| **Geteilte Leser** | jeden nicht lesenden Benutzer, im eigenen Klient |
| **Zurück zu mir** | solange man sich in einem fremden Klient befindet (auch im Lese-Modus) |

## Wechseln und zurückkehren
Mit **Zu Klient wechseln** öffnet sich der Dialog **Meine Klienten**, der alle Klienten auflistet, auf die man zugreifen darf – sowohl die selbst betreuten als auch die einem freigegebenen. Der Klient, in dem man sich gerade befindet, erscheint nicht in der Liste. Nach einem Klick auf das Wechseln-Symbol lädt die Anwendung neu und zeigt das Portfolio des gewählten Klienten. Mit **Zurück zu mir** kehrt man jederzeit zum eigenen **Klient** zurück.

## Lese-Modus
Ob jemand einen **Klient** nur lesen darf, entscheidet sich bei jeder Anfrage neu, sodass ein Entzug des Zugriffs sofort wirkt. Im Lese-Modus werden die Aktionen zum Erstellen, Bearbeiten und Löschen in den Menüs gar nicht erst angezeigt, und das Menü **Klient** bleibt bis auf den Eintrag **Zurück zu mir** verborgen. Unabhängig von der Oberfläche weist der Server zusätzlich jeden Änderungsversuch zurück, sodass die Daten in jedem Fall geschützt bleiben. Das eigene Konto bleibt dagegen in der eigenen Hand: Über das Menü **Einstellungen** lassen sich jederzeit das Passwort und die Sprache ändern.

{{< mermaid >}}
graph LR
    A[Berater] -->|verwaltet eigenständige| K1[Klient A]
    A -->|verwaltet eigenständige| K2[Klient B]
    O[Besitzer] -->|teilt das eigene| P[Portfolio]
    P -->|Lesezugriff| R[Leserin oder Leser]
{{< /mermaid >}}

{{% notice note %}}
Das Menü **Klient** erscheint nur, wenn die Funktion für die Grafioschtrader-Installation freigeschaltet wurde. Ist sie nicht verfügbar, wurde sie vom Betreiber nicht aktiviert.
{{% /notice %}}
