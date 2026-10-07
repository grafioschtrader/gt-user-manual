---
title: "Mehrere Klienten verwalten"
date: 2026-06-15T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
Bisher war ein Benutzer genau einem **Klient** zugeordnet. Neu kann ein Benutzer mehrere **Klienten** betreuen und zwischen ihnen wechseln. Damit lässt sich beispielsweise die Situation eines Vermögensberaters abbilden, der die **Portfolios** mehrerer Kundinnen und Kunden führt. Zusätzlich kann einem **Klient** ein eigener Lesezugang eingerichtet werden, damit die Kundin oder der Kunde die eigenen **Portfolios** ansehen kann, während der Berater sie pflegt. Möchten Sie hingegen einer Person nur Lesezugriff auf Ihr **eigenes** Portfolio geben, ohne einen eigenständigen Klienten anzulegen, verwenden Sie [Lesezugriff auf das eigene Portfolio teilen]({{< relref "/tenantportfolio/client/sharereadaccess" >}}). Die gemeinsamen Grundlagen beider Funktionen sind unter [Geteilter Zugriff – Grundlagen]({{< relref "/tenantportfolio/client/sharedaccess" >}}) beschrieben.

## Die beiden Rollen
Bei dieser Funktion stehen sich zwei Rollen gegenüber. Der **Berater** ist ein gewöhnlicher Benutzer, der mehrere **Klienten** anlegt, pflegt und zwischen ihnen wechselt. Der **Klient mit Lesezugang** ist die Kundin oder der Kunde, die oder der sich anmeldet, um die eigenen **Portfolios** ausschliesslich anzusehen. Ein und derselbe **Klient** wird also vom Berater verwaltet und von der Kundin oder dem Kunden gelesen.

{{< mermaid >}}
graph LR
    B[Berater] -->|verwaltet| K1[Klient A]
    B -->|verwaltet| K2[Klient B]
    B -->|verwaltet| K3[Klient C]
    K1 -->|Lesezugang| C1[Kundin A]
    K2 -->|Lesezugang| C2[Kunde B]
{{< /mermaid >}}

## Aus Sicht des Beraters
Damit ein Benutzer als Berater auftreten kann, muss ihm eine Administratorin oder ein Administrator mindestens die Rolle **Benutzer ohne Limits** zuweisen. Die Tätigkeiten des Beraters sind über das Menü **Klient** erreichbar, dessen Aufbau unter [Geteilter Zugriff – Grundlagen]({{< relref "/tenantportfolio/client/sharedaccess" >}}) beschrieben ist.

### Einen Klienten erstellen
1. Im Menü **Klient** den Eintrag **Klient erstellen** wählen.
2. Im Dialog die **E-Mail-Adresse** der Kundin oder des Kunden eingeben und ein **Passwort** vergeben; das Passwort ist zur Sicherheit zweimal einzutragen.
3. Mit der Schaltfläche **Klient erstellen** bestätigen.

Daraufhin entsteht ein neuer **Klient** in der Währung des Beraters, auf den der Berater sofort vollen Zugriff hat. Die Kundin oder der Kunde erhält die Anmeldedaten – Anmeldename und Passwort – automatisch per E-Mail. Die **Portfolios** und Konten des neuen Klienten richtet der Berater anschliessend wie bei einem eigenen Klient ein, indem er zunächst dorthin wechselt.

### Zu einem Klienten wechseln
1. Im Menü **Klient** den Eintrag **Zu Klient wechseln** wählen.
2. Der Dialog **Meine Klienten** listet die betreuten Klienten mit den Spalten **E-Mail** und **Klientenname** auf. Der Klient, in dem man sich gerade befindet, erscheint nicht in der Liste.
3. In der Zeile des gewünschten Klienten auf das Wechseln-Symbol klicken.

Die Anwendung lädt neu und zeigt nun das Portfolio des gewählten Klienten. Der Berater arbeitet darin mit allen Bearbeitungsrechten, gerade so, als ob er mit dem eigenen **Klient** angemeldet wäre.

### Einen Klienten löschen
1. Im Menü **Klient** den Eintrag **Zu Klient wechseln** wählen, um den Dialog **Meine Klienten** zu öffnen.
2. In der Zeile des betreffenden Klienten auf das Löschen-Symbol – den Papierkorb – klicken.
3. Die Sicherheitsabfrage bestätigen.

Damit werden das gesamte Portfolio, alle Daten und der Lesezugang der Kundin oder des Kunden endgültig entfernt. Der **Klient**, in dem man sich gerade befindet, lässt sich nicht löschen; dazu wechselt man zuvor zu einem anderen Klienten oder zurück zu sich.

### Zurück zum eigenen Klient
Mit dem Eintrag **Zurück zu mir** im Menü **Klient** kehrt der Berater jederzeit aus einem betreuten Klienten zu seinem eigenen **Klient** zurück.

## Aus Sicht des Klienten
Die Kundin oder der Kunde erhält eine E-Mail mit dem Anmeldenamen, der ihrer oder seiner E-Mail-Adresse entspricht, sowie einem Passwort. Mit diesen Angaben erfolgt die Anmeldung über die gewohnte Anmeldung. Danach steht eine reine Lese-Ansicht der eigenen **Portfolios** zur Verfügung. Wie sich der Lese-Modus auswirkt, ist unter [Geteilter Zugriff – Grundlagen]({{< relref "/tenantportfolio/client/sharedaccess" >}}) beschrieben.

{{< mermaid >}}
sequenceDiagram
    participant B as Berater
    participant GT as Grafioschtrader
    participant K as Kundin oder Kunde
    B->>GT: Klient erstellen mit E-Mail und Passwort
    GT->>K: E-Mail mit Anmeldedaten
    GT-->>B: Voller Zugriff auf den Klienten
    K->>GT: Anmeldung mit E-Mail und Passwort
    GT-->>K: Nur-Lese-Ansicht der Portfolios
{{< /mermaid >}}

## Berechtigungen im Detail
Die folgende Übersicht zeigt, welche Tätigkeiten den beiden Rollen offenstehen. Der Berater hat dabei in jedem von ihm betreuten **Klient** dieselben Rechte wie in seinem eigenen.

| Tätigkeit | Berater | Klient mit Lesezugang |
| --- | :---: | :---: |
| Portfolios, Auswertungen, Transaktionen, Depots und Watchlists ansehen | ✓ | ✓ |
| Daten erstellen, ändern oder löschen | ✓ | ✗ |
| Zwischen betreuten Klienten wechseln | ✓ | ✗ |
| Neuen Klienten anlegen | ✓ | ✗ |
| Betreuten Klienten löschen | ✓ | ✗ |
| Eigenes Passwort ändern | ✓ | ✓ |
| Eigene Sprache ändern | ✓ | ✓ |
| Menü **Klient** sichtbar | ✓ | ✗ |
