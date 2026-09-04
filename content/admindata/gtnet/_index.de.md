---
title: "GTNet"
date: 2026-08-31T22:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GTNet ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
GTNet (Grafioschtrader-Netzwerk) ist ein dezentrales Peer-to-Peer-System, das mehreren Grafioschtrader-Instanzen den Austausch von Finanzdaten ermöglicht. Jede Installation kann gleichzeitig als Datenanbieter und Datennutzer im Netzwerk agieren. Diese Seite gibt einen Überblick über den Aufbau des Netzwerks, über die Stellen im Programm, an denen es wirkt, und über seine Grenzen. Die eigentliche Einrichtung beschreibt [Erste Schritte mit GTNet](gettingstarted/), den laufenden Betrieb [GTNet überwachen](monitoring/).

### Überblick
GTNet verbindet verschiedene Grafioschtrader-Instanzen miteinander und ermöglicht den gegenseitigen Austausch von Kursdaten. Dabei kann jede Instanz selbst entscheiden, ob sie Daten bereitstellt, empfängt oder beides gleichzeitig tut. Es gibt keine zentrale Stelle, welche das Netz verwaltet: Jede Instanz kennt nur jene Gegenparts, die ihr Administrator erfasst oder zugelassen hat.

Der Austausch erfolgt automatisiert über eine Maschine-zu-Maschine-Kommunikation (M2M). Sobald sich zwei Instanzen einig sind, läuft er ohne weiteres Zutun eines Benutzers ab; im Programm ist davon nur das Ergebnis zu sehen, nämlich zusätzliche Kurse und ausgefüllte Wertpapier-Stammdaten.

{{< mermaid >}}
graph LR;
    ME["Eigene Instanz"]
    P1["Gegenpart, Modus Offen"]
    P2["Gegenpart, Modus Push Offen"]
    POOL[("Gemeinsamer Kursdatenpool")]
    ME <-->|Intraday-Kurse| P1
    ME <-->|Historische Kurse| P2
    ME -->|Anfrage Wertpapier-Metadaten| P1
    P2 --- POOL
{{< /mermaid >}}

Die Einstellungen zu GTNet tragen zwei Präfixe, welche diesen Aufbau widerspiegeln. Einstellungen mit **g.gnet** betreffen das Netzwerk selbst, also Verbindungen, Nachrichten und Protokollierung. Einstellungen mit **gt.gtnet** betreffen das, was über dieses Netzwerk gehandelt wird, also die Kursdaten von Grafioschtrader. Alle sind unter [Globale Einstellungen GTNet](globalsettings/) beschrieben.

### Die beiden Rollen einer Instanz
Lieferant und Bezüger sind keine Eigenschaften einer Installation, sondern eines einzelnen Austauschs. Dieselbe Instanz kann einem Gegenpart Kurse liefern und von einem anderen welche beziehen, und sie kann mit demselben Gegenpart in der einen Datenart liefern und in der anderen beziehen. Beide Richtungen dürfen gleichzeitig aktiv sein.

Welche Rolle für ein einzelnes Instrument gilt, legen dessen vier Austauschoptionen fest: «Erhalte Intraday Preis» und «Erhalte historische Preisdaten» machen die Instanz zum Bezüger, «Sende Intraday Preise» und «Sende historische Preisdaten» zum Lieferanten. Diese Optionen werden unter [Austausch Preisdaten](exchange/) gepflegt.

### Arten des Datenaustauschs
GTNet unterstützt drei Arten von Daten für den Austausch:
- **Intraday-Kurse**: Aktuelle Tageskurse für Wertpapiere und Währungspaare mit OHLCV-Daten (Eröffnung, Hoch, Tief, Schluss, Volumen).
- **Historische Kurse**: Tägliche Schlusskurse für die Vergangenheit.
- **Wertpapier-Metadaten**: Stammdaten von Wertpapieren wie ISIN, Name, Anlageklasse, Börseninformationen und Konnektoreinstellungen. Dies ermöglicht das Übernehmen von Wertpapierkonfigurationen aus anderen GTNet-Instanzen.

Die beiden Kursarten werden als laufender Austausch vereinbart und danach immer wieder abgerufen. Wertpapier-Metadaten dagegen werden nur dann abgefragt, wenn jemand ein Wertpapier erfassen will; hier gibt es keine dauernde Vereinbarung.

### Wie eine Instanz am Netz teilnimmt
Für jede Datenart legt eine Instanz getrennt fest, in welchem Umfang sie andere bedient. Der gewählte Modus wird den Gegenparts mitgeteilt, damit diese wissen, ob sich eine Anfrage überhaupt lohnt.

| Modus | Bedeutung |
|-------|-----------|
| Geschlossen | Für diese Datenart wird nichts geliefert. Anfragen werden abgewiesen. |
| Offen | Anfragen werden beantwortet, soweit die eigenen Instrumente die gewünschten Daten hergeben. |
| Push Offen | Anfragen werden beantwortet, und zusätzlich werden Kurse entgegengenommen, die ein Gegenpart von sich aus schickt. Diese landen in einem gemeinsamen Kursdatenpool. |

Eine Instanz im Modus «Push Offen» kann deshalb auch Kurse zu Instrumenten liefern, die sie selbst gar nicht führt: Sie gibt weiter, was ihr Pool enthält. Sie ist der bevorzugte Gegenpart, weil sie mehr und aktuellere Daten hält. Für Wertpapier-Metadaten steht dieser Modus nicht zur Verfügung, weil Stammdaten nicht zugeschickt, sondern einzeln abgefragt werden. Die Unterschiede der Modi im Betrieb, insbesondere die Reihenfolge, in welcher Lieferanten angefragt werden, sind unter [GTNet und Nachrichten](setup/) beschrieben.

### Wo GTNet in Grafioschtrader wirkt
GTNet ist keine eigene Funktion, die man aufruft, sondern eine zusätzliche Datenquelle, die an mehreren Stellen des Programms einspringt.

{{< mermaid >}}
graph TD;
    W["Watchlist aktualisieren"] --> LP["Intraday-Kurse aus dem Netz"]
    H["Historische Kursdaten laden"] --> HP["Schlusskurse und Auffüllen von Lücken"]
    K["Konnektor liefert nicht mehr"] --> FB["GTNet als Ersatzquelle"]
    I["Einzelnes Instrument erfassen"] --> MD["Wertpapier-Metadaten und Konnektoreinstellungen"]
    B["Wertpapiere in Menge anlegen"] --> MD
    T["Transaktions-Import mit unbekanntem Wertpapier"] --> MD
{{< /mermaid >}}

#### Benutzergesteuerte Verwendung
- **Watchlist-Aktualisierung**: Beim Aktualisieren einer Watchlist können Instrumente, die für den GTNet-Austausch konfiguriert sind, Intraday-Kurse von verbundenen Gegenparts empfangen. Siehe [Austausch letzter Preis](exchange/lastprice/) für Details.
- **Laden historischer Daten**: Beim Laden historischer Kursdaten kann GTNet Lücken aus anderen Instanzen füllen. Siehe [Austausch historische Kurse](exchange/historicalprice/) für Details.
- **Ausgefallener Konnektor**: Erreicht der Wiederholungszähler eines Instruments das Konnektor-Limit, übernimmt GTNet als Ersatzquelle, ohne dass der Ausfall dadurch verdeckt wird. Siehe [Wiederholungszähler und GTNet-Fallback](quoteretry/).
- **Wertpapiererstellung**: Beim Erstellen eines neuen Wertpapiers ermöglicht die [GTNet Wertpapiersuche](../../watchlistinstrument/instrument/securityderived/#gtnet-wertpapiersuche) das Übernehmen von Wertpapier-Metadaten und Konnektoreinstellungen aus anderen GTNet-Instanzen.
- **Wertpapiere in Menge anlegen**: Der [GTNet Wertpapierimport](../../basedata/gtnetsecurityimport/) legt zu einer Liste von Wertpapieren die Stammdaten in einem Durchgang an und hält fest, was dabei nicht zugeordnet werden konnte.
- **Transaktions-Import**: Enthält ein Import ein Wertpapier, das es hier noch nicht gibt, lässt es sich über den [Wertpapierimport für Transaktionen](../../tenantportfolio/securityaccounts/transactionimport/securityimportfortransaction/) aus dem Netz erfassen, statt es von Hand anzulegen.

#### Systemgesteuerte Verwendung
{{< mermaid >}}
graph LR;
    A[Anwendungsstart] -->|Broadcast| B[Server Online Nachricht]
    C[Anwendungsbeendigung] -->|Broadcast| D[Server Offline Nachricht]
    E[Einstellungen geändert] -->|Broadcast| F[Einstellungen aktualisiert Nachricht]
{{< /mermaid >}}

- **Anwendungsstart**: Wenn der GT-Server mit aktiviertem GTNet startet, wird automatisch ein «Online»-Status an alle verbundenen Gegenparts gesendet
- **Anwendungsbeendigung**: Bei ordnungsgemässem Herunterfahren wird ein «Offline»-Status gesendet, um die Gegenparts zu informieren
- **Einstellungsänderungen**: Konfigurationsänderungen werden automatisch mit verbundenen Instanzen synchronisiert

### Verbindungsaufbau und Authentifizierung
Der Verbindungsaufbau zwischen zwei GTNet-Instanzen erfolgt über ein Handshake-Verfahren. Beim ersten Kontakt werden Authentifizierungstoken ausgetauscht, die für die weitere sichere Kommunikation verwendet werden. Jede Instanz kann selbst entscheiden, ob sie unbekannte Server automatisch akzeptiert oder nur vordefinierte Server zulässt.

Die Token können später erneuert werden. Weil beide Seiten das neue Token nicht im selben Augenblick übernehmen, bleibt das abgelöste Token noch eine begrenzte Zeit gültig. Geht die Antwort auf eine Erneuerung unterwegs verloren, bricht die Verbindung deshalb nicht ab, sondern wird beim nächsten Versuch von selbst wieder in Ordnung gebracht.

### Freigabe des Datenaustauschs
Ein abgeschlossener Handshake bedeutet nur, dass sich zwei Instanzen kennen und miteinander sprechen dürfen. Er berechtigt noch zu keinen Kursdaten. Für Intraday-Kurse und historische Kurse muss zusätzlich je Datenart eine Datenanfrage gestellt und von der Gegenseite bewilligt worden sein. Ohne diese Bewilligung weist die angefragte Instanz die Anfrage ab, auch wenn sie die betreffende Datenart grundsätzlich anbietet.

Wertpapier-Metadaten sind davon ausgenommen. Sie werden bei Bedarf einzeln abgefragt und nicht als laufender Austausch vereinbart; hier genügt die eigene Einstellung, ob solche Anfragen angenommen werden.

Wird eine Datenanfrage abgelehnt oder eine bestehende Bewilligung widerrufen, endet der Austausch auf beiden Seiten im gleichen Zustand. Beide Instanzen zeigen die Datenart danach wieder als nicht vereinbart an, und für eine erneute Aufnahme braucht es eine neue Datenanfrage.

{{% notice style="note" title="Bestehende Verbindungen" %}}
Gegenparts, mit denen bereits Kursdaten ausgetauscht wurden, behalten ihre Bewilligung beim Versionswechsel automatisch. Nur wer bisher lediglich einen Handshake, aber nie einen Austausch hatte, muss eine Datenanfrage stellen.
{{% /notice %}}

### Nachrichten und Statusmeldungen
GTNet nutzt ein nachrichtenbasiertes Kommunikationsprotokoll für verschiedene Zwecke:
- **Handshake-Nachrichten**: Zum Herstellen und Bestätigen von Verbindungen zwischen Instanzen.
- **Statusnachrichten**: Zur Mitteilung von Änderungen wie Online/Offline-Status, Wartungsmodus oder Kapazitätsauslastung. Meldet sich ein Gegenpart beim Herunterfahren ab, wird er sofort als offline geführt; früher blieb er bis zum nächsten fehlgeschlagenen Versand als online eingetragen.
- **Datenanfragen**: Zum Anfordern und Bewilligen des Austauschs bestimmter Datenarten.

### Wer darf was
GTNet betrifft die gesamte Instanz und nicht einen einzelnen Mandanten. Deshalb darf nur ein Benutzer mit Administratorrechten daran etwas verändern: Gegenparts erfassen, bearbeiten oder löschen, Nachrichten versenden und beantworten, Nachrichten löschen sowie die Regeln für die automatische Beantwortung pflegen. Alle übrigen angemeldeten Benutzer dürfen die GTNet-Ansichten lesen, also die Liste der Gegenparts mit deren Nachrichten, die angekündigten Wartungsfenster und das Austauschprotokoll.

Was einem Benutzer nicht erlaubt ist, wird ihm auch nicht angeboten: Die entsprechenden Menüpunkte und Dialoge erscheinen gar nicht, anstatt erst beim Speichern abgewiesen zu werden. Eine Ausnahme bilden die Austausch-Optionen einzelner Instrumente unter [Austausch Preisdaten](exchange/). Sie gehören zum Instrument und folgen dessen gewöhnlichen Bearbeitungsrechten: Ein Administrator und ein «Privilegierter Benutzer» dürfen jedes Instrument ändern, alle übrigen Benutzer nur die von ihnen selbst erfassten Instrumente.

Zwei Bereiche bleiben den Administratoren auch beim blossen Lesen vorbehalten. Die [Automatische Nachricht](autoanswer/) legt offen, unter welchen Bedingungen diese Instanz einen Gegenpart aufnimmt, und der Export der GTNet-Daten gibt den gesamten Bestand auf einmal heraus. Admin-Nachrichten, die als «Nur Admin» gekennzeichnet sind, bleiben zudem in sämtlichen Ansichten den Administratoren vorbehalten — auch dort, wo die Nachrichten eines einzelnen Gegenparts aufgeklappt werden.

Ein als «Nur Admin» gekennzeichneter Gesprächsverlauf bleibt es für jede Nachricht darin. Eine Antwort kann ihn nicht
öffnen — weder eine hier verfasste noch eine, die vom Gegenpart eintrifft, der ja nicht wissen kann, wie der von ihm
beantwortete Verlauf eingestuft ist. Massgebend ist die Nachricht, die das Gespräch begonnen hat.

Eine Nachricht, die noch auf eine Antwort wartet, lässt sich nicht löschen, und der zugehörige Gegenpart ebenso wenig,
solange die Antwort aussteht oder die Anfrage nicht abgelehnt wurde. Das gilt für jede Art von Anfrage, auch für jene,
welche die Instanzen unter sich austauschen — etwa die Token-Erneuerung und den Abgleich der Austauscheinstellungen.

### Abfragelimits und Laststeuerung
Um Server vor Überlastung zu schützen, kennt GTNet zwei Begrenzungen. Das **Tägliche Abfragelimit** legt fest, wie viele Anfragen ein einzelner Gegenpart pro UTC-Tag stellen darf; es wirkt in beide Richtungen, denn die eigene Instanz hört von selbst auf zu fragen, sobald das von einem Gegenpart veröffentlichte Kontingent aufgebraucht ist. Das **Abfragelimit** begrenzt je Austauschart, wie viele Instrumente eine einzelne Anfrage umfassen darf. Beide sind unter [Abfragelimits](requestlimits/) beschrieben. Zusätzlich kann eine Instanz als «ausgelastet» markiert werden, wodurch nur noch Statusnachrichten kommuniziert werden.

### Grenzen: die ausgetauschten Daten werden nicht geprüft
{{% notice style="warning" title="Empfangene Kurse werden nicht überprüft" %}}
GTNet prüft, **wer** mit dieser Instanz spricht, aber nicht, **ob das Gelieferte stimmt**. Tauschen Sie deshalb nur mit Gegenparts Daten aus, denen Sie vertrauen.
{{% /notice %}}
Alle Schutzmassnahmen von GTNet richten sich gegen den falschen Absender und gegen Überlastung: Handshake und Token stellen sicher, dass nur zugelassene Instanzen überhaupt gehört werden, die Zulassung entscheidet, wer aufgenommen wird, und die Abfragelimits verhindern, dass ein Gegenpart die eigene Instanz mit Anfragen überzieht. Keine dieser Massnahmen sagt etwas darüber aus, ob ein gelieferter Kurs richtig ist.

Ein empfangener Kurs wird weder mit den eigenen Konnektoren verglichen noch auf Plausibilität geprüft. Er trägt auch keine Unterschrift und keine Prüfsumme, mit der sich nachträglich feststellen liesse, ob er unterwegs verändert wurde.

Ein gespeicherter Kurs hält zudem nicht fest, von welcher Instanz er stammt. Wird er über eine zweite Instanz weitergegeben, ist die ursprüngliche Herkunft nicht mehr erkennbar. Einen netzweiten Ruf, an dem sich die Zuverlässigkeit einer fremden Instanz ablesen liesse, gibt es ebenfalls nicht; jede Instanz beurteilt ihre Gegenparts nur anhand der eigenen Erfahrungen.

Die Erfolgsquote im [Austauschprotokoll](exchangelog/) misst lediglich, ob überhaupt geliefert wurde, nicht, ob richtig geliefert wurde. Dasselbe gilt für die Reihenfolge, in welcher Lieferanten angefragt werden: Sie bevorzugt den Gegenpart, der bisher zuverlässig geantwortet hat, nicht jenen mit den besseren Kursen.

Am wenigsten prüfen kann ausgerechnet jene Instanz, die am meisten weitergibt. Wer vor allem den gemeinsamen Kursdatenpool führt, hält zu diesen Kursen kein eigenes Wertpapier und damit auch keine Konnektoreinstellung, gegen die sich vergleichen liesse. Eine solche Instanz reicht Kurse weiter, die sie selbst nicht nachprüfen kann.

Daraus ergeben sich drei Empfehlungen. Nehmen Sie nur Gegenparts auf, deren Betreiber Sie einschätzen können. Behalten Sie für jedes Instrument einen funktionierenden Konnektor als Hauptquelle; GTNet ist als Ergänzung gedacht und nicht als Ersatz. Und prüfen Sie auffällige Kurse selbst, etwa in der Kursdatenansicht eines Instruments, wo sich einzelne Werte auch korrigieren lassen.

Eine automatische Prüfung der ausgetauschten Daten ist vorgesehen, aber noch nicht umgesetzt.

## Einrichtungsseiten
Die detaillierte Einrichtung erfolgt über die folgenden Unterseiten:
