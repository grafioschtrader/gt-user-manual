---
title: "Admin-Nachrichten"
date: 2026-02-01T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GTNet ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Der Reiter **Admin-Nachrichten** bietet eine dedizierte Ansicht für die Verwaltung administrativer Freitext-Nachrichten zwischen GTNet-Instanzen. Im Gegensatz zu den technischen Systemnachrichten in der Hauptansicht ermöglicht diese Ansicht den Administratoren einen direkten Austausch von Informationen.

### Warum eine separate Ansicht?
Die Trennung der Admin-Nachrichten von der regulären GTNet-Einrichtung erfolgt aus mehreren Gründen:
- **Unterschiedlicher Zweck**: Die reguläre Ansicht zeigt technische Systemnachrichten wie Handshake-Anfragen, Datenanfragen und Statusmeldungen. Admin-Nachrichten hingegen dienen der freien Kommunikation zwischen Administratoren.
- **Konversationen**: Admin-Nachrichten unterstützen Antworten und ermöglichen so fortlaufende Gespräche. Technische Nachrichten haben fixe Antworttypen.
- **Sichtbarkeitssteuerung**: Nur Admin-Nachrichten verfügen über die Option, die Sichtbarkeit einzuschränken.
- **Übersichtlichkeit**: Die Trennung verhindert, dass administrative Kommunikation zwischen technischen Meldungen untergeht.

### Tabellenübersicht
Die Tabelle zeigt alle bekannten GTNet-Instanzen mit folgenden Spalten:
- **URL**: Die Adresse der entfernten Instanz.
- **Zeitzone**: Die Zeitzone des Servers.
- **Admin-Nachrichten**: Gesamtzahl der Admin-Nachrichten mit dieser Instanz.
- **Zu beantworten**: Anzahl eingehender Nachrichten, die noch nicht beantwortet wurden.
- **Antwort erwartet**: Anzahl gesendeter Nachrichten, auf die noch eine Antwort aussteht.

Die Zeilen können über das Pfeilsymbol expandiert werden, um die zugehörigen Nachrichten-Threads anzuzeigen.

### Versenden von Admin-Nachrichten
Admin-Nachrichten können an einen oder mehrere Empfänger gesendet werden. Das System verhält sich dabei unterschiedlich:

{{< mermaid >}}
flowchart TD
    A[Benutzer möchte Admin-Nachricht senden] --> B{Anzahl Empfänger?}
    B -->|Einzelner| C[Antwort im Nachrichten-Thread]
    B -->|Mehrere| D[Domänen per Checkbox auswählen]
    C --> E[Nachricht wird sofort gesendet]
    D --> F[Kontextmenü: Admin-Nachricht senden]
    F --> G[Nachrichten werden in Warteschlange gestellt]
    E --> H[Bestätigung: Nachricht erfolgreich gesendet]
    G --> I[Hintergrund-Task versendet Nachrichten]
    I --> J[Liste aktualisiert sich nach Abschluss]
{{< /mermaid >}}

#### Einzelner Empfänger
Wenn Sie auf eine bestehende Nachricht in einem Thread antworten möchten, klicken Sie mit der rechten Maustaste auf die Nachricht und wählen Sie **Antworten**. Die Nachricht wird synchron versendet. Nach erfolgreichem Versand erscheint die Meldung «Admin-Nachricht erfolgreich gesendet».

#### Mehrere Empfänger
Um eine Admin-Nachricht an mehrere Instanzen gleichzeitig zu senden:
1. Aktivieren Sie die Checkboxen der gewünschten Domänen in der Tabelle.
2. Öffnen Sie das Kontextmenü mit einem Rechtsklick.
3. Wählen Sie **Admin-Nachricht senden**.

Die Nachrichten werden asynchron im Hintergrund verarbeitet. Sie erhalten die Bestätigung «Admin-Nachricht für X Empfänger in die Warteschlange gestellt». Die Liste aktualisiert sich automatisch nach einigen Momenten, sobald die Nachrichten zugestellt wurden. Die Zustellung erfolgt über die [Hintergrundaufgabe 26](../../../taskdatachangemonitor/taskdescription/#job26).

{{% notice info %}}
Bei mehreren Empfängern werden die Nachrichten im Hintergrund zugestellt. Die Liste wird möglicherweise erst nach einigen Momenten aktualisiert.
{{% /notice %}}

### Sichtbarkeit von Nachrichten
Admin-Nachrichten verfügen über eine Sichtbarkeitseinstellung, die bestimmt, wer die Nachricht auf dem Empfängerserver sehen kann:
- **Alle Benutzer**: Die Nachricht ist für alle Benutzer auf dem Empfängerserver sichtbar.
- **Nur Admin**: Die Nachricht ist nur für Administratoren auf dem Empfängerserver sichtbar.

#### Vererbungsregel für die Sichtbarkeit
{{% notice warning %}}
Die Sichtbarkeit kann nur eingeschränkt, aber niemals erweitert werden.
{{% /notice %}}

Wenn Sie auf eine Nachricht antworten, gelten folgende Regeln:

| Ursprüngliche Nachricht | Mögliche Sichtbarkeit der Antwort |
|-------------------------|-----------------------------------|
| Alle Benutzer | Alle Benutzer oder Nur Admin |
| Nur Admin | Nur Admin |

Das bedeutet: Wenn die ursprüngliche Nachricht mit «Nur Admin» markiert war, müssen alle Antworten in diesem Thread ebenfalls auf «Nur Admin» beschränkt bleiben. Eine Nachricht, die als vertraulich begann, kann nicht nachträglich öffentlich gemacht werden.

### Der Bearbeitungsdialog
Beim Erstellen oder Beantworten einer Admin-Nachricht erscheint ein Dialog mit folgenden Feldern:
- **Nachrichtencode**: Der Typ der Nachricht. Für Admin-Nachrichten ist dies «Admin-Nachricht» oder «Admin-Rundnachricht» bei mehreren Empfängern.
- **Nachricht**: Der Freitext-Inhalt der Nachricht.
- **Sichtbarkeit**: Dropdown zur Auswahl zwischen «Alle Benutzer» und «Nur Admin». Dieses Feld ist nur bei Admin-Nachrichten verfügbar.

{{% notice note %}}
Nur Benutzer mit Administratorrechten können Admin-Nachrichten versenden. Normale Benutzer sehen diese Ansicht nicht im Menü.
{{% /notice %}}
