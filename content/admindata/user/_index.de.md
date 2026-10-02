---
title: "Benutzer Einstellungen"
date: 2026-08-15T22:54:47+01:00
draft: false
weight: 60
archetype: "default"
---
Für die Bearbeitung dieser Daten werden die Rechte des Administratoren benötigt. Die Benutzerverwaltung ermöglicht Administratoren die vollständige Kontrolle über alle Benutzerkonten. Neben der Vergabe der Rolle können hier auch Ausnahmen für die Limiten eines einzelnen Benutzers auf einer bestimmten Informationsklasse festgelegt werden; die Limiten der Installation als Ganzes werden dagegen in der Ansicht [Limite Informationsklasse]({{% relref "/admindata/entitylimit" %}}) verwaltet. Siehe dazu "[Benutzer mit Limits](https://grafioschtrader.github.io/gt-user-manual/de/intro/userrights/index.html#benutzer-mit-limits)". Zudem kann das Problem "[Schutz der Fremddaten und das Anfragelimit](//grafioschtrader.github.io/gt-user-manual/de/intro/userrights/index.html#schutz-der-fremddaten-und-das-anfragelimit)" nur durch die hier angebotene Funktionalität gelöst werden.

{{% notice note %}}
Normale Benutzer können ihre eigenen Einstellungen wie Spitzname, Sprache und Passwort über das Hauptmenü ändern. Diese Funktionen erfordern keine Administratorrechte. Die hier beschriebenen Funktionen sind ausschliesslich für Administratoren verfügbar.
{{% /notice %}}

## Benutzertabelle
Die Benutzertabelle zeigt alle registrierten Benutzer mit ihren wichtigsten Eigenschaften. Die Tabelle kann nach allen Spalten sortiert werden und bietet eine übersichtliche Darstellung aller relevanten Benutzerinformationen.

### Eigenschaften und Tabellenspalten
- **ID**: Die eindeutige interne Benutzer-ID.
- **Spitzname**: Der vom Benutzer gewählte Spitzname. Dieser wird möglicherweise für die Kennung statt der internen Benutzer-ID zu anderen Benutzern genutzt.
- **E-Mail**: Die E-Mail-Adresse des Benutzers, die auch als Login-Name dient. Diese kann nach der Registrierung nicht mehr geändert werden.
- **U**: Ein Symbol, das anzeigt, ob für diesen Benutzer ein Änderungsvorschlag vorliegt. Siehe Abschnitt [Änderungsvorschläge](#änderungsvorschläge).
- **L**: Ein Symbol, das anzeigt, ob für diesen Benutzer Limit-Änderungsvorschläge vorliegen.
- **Privilegierteste Rolle**: Die höchste Rolle, die dem Benutzer zugewiesen wurde. Mögliche Werte sind:
  - **Administrator**: Vollständige Administratorrechte
  - **Alles bearbeiten**: Kann alle Entitäten bearbeiten
  - **Benutzer**: Standard-Benutzerrechte
  - **Benutzer mit Limits**: Eingeschränkte Rechte mit konfigurierbaren Limits
- **Aktiv**: Zeigt an, ob der Benutzer sich anmelden kann. Deaktivierte Benutzer können sich nicht mehr einloggen.
- **Sprache und Land**: Die Spracheinstellung des Benutzers. Diese bestimmt die Sprache der Applikation und die Darstellung des Datums- und Zahlenformats.
- **Lokalzeit +/- gleich UTC**: Die Zeitzonenabweichung zur UTC-Zeit in Minuten. Diese wird verwendet, um Zeitangaben korrekt für den Benutzer anzuzeigen.
- **Verstoss Fremddaten/Datenlimit**: Zähler für Verstösse gegen den Schutz von Fremddaten. Wenn dieser Zähler einen konfigurierten Schwellenwert überschreitet, wird der Benutzer automatisch gesperrt.
- **Verstoss Anfragelimit**: Zähler für Überschreitungen des täglichen Anfragelimits. Auch hier führt das Überschreiten eines Schwellenwerts zur automatischen Sperrung des Benutzers.

### Expandierbare Zeile
Für einen Benutzer können die eigens für ihn festgelegten Limiten eingesehen werden. Durch das expandieren der Zeile wird eine Detailansicht mit allen für diesen Benutzer erfassten Limiten angezeigt. In aller Regel handelt es sich dabei um Ausnahmen für Benutzer der Rolle "Benutzer mit Limits", welche die Anzahl der Erstellungs-, Änderungs- und Löschoperationen pro Tag auf einer bestimmten Informationsklasse beschränken.

Die Detailansicht zeigt eine Tabelle mit folgenden Spalten:
- **Entität**: Die Informationsklasse, für welche die Limite gilt (z.B. Wertpapier, Watchlist, Konto).
- **Limitenart**: Was begrenzt wird, in aller Regel "Änderungen pro Tag". Siehe [Limite Informationsklasse]({{% relref "/admindata/entitylimit" %}}).
- **L**: Ein Symbol, das anzeigt, ob für diese Limite ein Änderungsvorschlag vorliegt.
- **Limite**: Der Wert, der nicht überschritten werden darf.
- **Gültig bis**: Das Ablaufdatum der Limite. Nach diesem Datum wird die Limite automatisch nicht mehr angewendet. Bleibt das Feld leer, so gilt die Limite unbefristet.

{{% notice note %}}
Diese Ansicht zeigt ausschliesslich die Limiten des ausgewählten Benutzers. Der Standardwert einer Limite sowie Limiten, die für eine ganze Benutzerrolle gelten, werden in der Ansicht [Limite Informationsklasse]({{% relref "/admindata/entitylimit" %}}) verwaltet. Dort finden Sie auch sämtliche Limiten dieser Installation auf einen Blick.
{{% /notice %}}

## Benutzer bearbeiten
Über das Kontextmenü kann ein Benutzer bearbeitet werden. Dabei können folgende Eigenschaften geändert werden:

- **Spitzname**: Der Spitzname des Benutzers. Dieser muss mindestens 2 und maximal 30 Zeichen lang sein und innerhalb von Grafioschtrader eindeutig sein.
- **Privilegierteste Rolle**: Die höchste Rolle des Benutzers kann geändert werden. Die Auswahl erfolgt aus den verfügbaren Rollen (Benutzer mit Limits, Benutzer, Alles bearbeiten, Administrator).
- **Aktiv**: Über diese Checkbox kann ein Benutzer aktiviert oder deaktiviert werden. Deaktivierte Benutzer können sich nicht mehr anmelden.
- **Lokalzeit +/- gleich UTC**: Die Zeitzoneneinstellung in Minuten zur UTC-Zeit kann manuell angepasst werden, falls die automatische Erkennung nicht korrekt funktioniert. Erlaubte Werte liegen zwischen -720 und +720 Minuten.
- **Verstoss Fremddaten/Datenlimit**: Dieser Zähler kann zurückgesetzt werden, um einen gesperrten Benutzer wieder freizugeben. Der Wert kann zwischen 0 und 99 liegen.
- **Verstoss Anfragelimit**: Auch dieser Zähler kann zurückgesetzt werden, um die Sperre aufzuheben. Der Wert kann zwischen 0 und 99 liegen.

{{% notice info %}}
Die E-Mail-Adresse kann nach der Registrierung nicht mehr geändert werden, da sie als Login-Name dient und für die Identifikation des Benutzers verwendet wird.
{{% /notice %}}

## Benutzerbezogene Limiten verwalten
Für einen einzelnen Benutzer können individuelle Limiten definiert werden, welche den für seine Rolle geltenden Wert übersteuern. Meistens beschränken sie die Anzahl der täglichen Erstellungs-, Änderungs- und Löschoperationen auf einer bestimmten Informationsklasse. Dies ermöglicht eine granulare Kontrolle über die Aktivitäten eines Benutzers und ist besonders nützlich für neue Benutzer oder Benutzer mit eingeschränktem Vertrauen.

### Limite erstellen oder bearbeiten
Über das Kontextmenü kann eine neue Limite erstellt oder eine bestehende bearbeitet werden. Es wird derselbe Dialog verwendet wie in der Ansicht [Limite Informationsklasse]({{% relref "/admindata/entitylimit" %}}), jedoch ohne die Auswahl einer Rolle: die Limite gilt hier immer für den ausgewählten Benutzer. Dabei können folgende Eigenschaften festgelegt werden:

- **Entität**: Auswahl der Informationsklasse, für welche die Limite gelten soll, zusammen mit der Limitenart. Die Auswahl wird aus dem System geladen und umfasst praktisch alle Informationsklassen, also Wertpapier, Watchlist, Konto, Portfolio, Transaktion und viele weitere. Nach der Erstellung kann diese Auswahl nicht mehr geändert werden, denn sie identifiziert die Limite.
- **Limitenart** und **Bereich**: Ergeben sich aus der getroffenen Auswahl, werden lediglich angezeigt und können nicht bearbeitet werden.
- **Limite**: Der Höchstwert, im Regelfall die Anzahl an Erstellungs-, Änderungs- und Löschoperationen, die der Benutzer pro Tag auf dieser Informationsklasse durchführen kann. Zulässig sind Werte von 1 bis 1'000'000, wobei für einzelne Limiten eine engere Einschränkung gilt.
- **Gültig bis**: Das Datum, bis zu dem diese Limite gültig ist. Nach Ablauf dieses Datums wird die Limite automatisch nicht mehr angewendet. Dies ermöglicht zeitlich begrenzte Ausnahmen, beispielsweise während einer Testphase. Bleibt das Feld leer, so ist die Limite unbefristet.

Die erfassten Limiten werden in der expandierbaren Tabellenzeile unterhalb des jeweiligen Benutzers angezeigt. Über das Kontextmenü der Limiten-Tabelle können einzelne Limiten bearbeitet oder gelöscht werden.

{{% notice tip %}}
Limiten sind besonders nützlich, um neuen Benutzern zunächst eingeschränkten Zugriff zu gewähren. Nach einer Bewährungsphase können die Limiten erhöht oder ganz entfernt werden, indem die Rolle des Benutzers auf "Benutzer ohne Limits" oder höher geändert wird.
{{% /notice %}}

## Besitzer von Entitäten wechseln
In bestimmten Fällen kann es notwendig sein, den Besitzer von Entitäten auf einen anderen Benutzer zu übertragen. Dies kann beispielsweise der Fall sein, wenn ein Benutzer das System verlässt, wenn Daten konsolidiert werden sollen oder wenn ein Administrator Testdaten von einem Benutzer übernehmen möchte.

Über das Kontextmenü kann die Funktion **Besitzer von Entitäten wechseln** ausgewählt werden. Dabei wird der aktuell in der Tabelle ausgewählte Benutzer als Quellbenutzer betrachtet, und es muss ein Zielbenutzer aus einer Liste ausgewählt werden. Alle Entitäten, die der Quellbenutzer erstellt hat, werden dann dem Zielbenutzer zugeordnet. Dies betrifft alle Arten von Entitäten wie Wertpapiere, Watchlists, Portfolios, Konten, Transaktionen und weitere.

{{% notice warning %}}
Diese Operation kann nicht rückgängig gemacht werden. Die Anzahl der übertragenen Entitäten wird nach Abschluss der Operation angezeigt.
{{% /notice %}}

## Änderungsvorschläge
Grafioschtrader verfügt über ein System für Änderungsvorschläge, das automatisch aktiviert wird, wenn ein Benutzer gegen Daten- oder Anfragelimits verstösst. Jede Rolle kann gegen diese Limits gesperrt werden. Wenn ein Verstoss auftritt, wird dem betroffenen Benutzer automatisch ein Dialog angezeigt, über den er eine Anfrage an die Administratoren stellen kann. Mit dem Absenden dieses Dialogs wird der Änderungsvorschlag ausgelöst.

### Entstehung von Änderungsvorschlägen
Änderungsvorschläge entstehen nicht durch manuelle Anfragen, sondern automatisch aufgrund von Verstössen:

1. Ein Benutzer überschreitet ein konfiguriertes Limit (Daten- oder Anfragelimit)
2. Das System erkennt den Verstoss und erhöht den entsprechenden Verstoss-Zähler
3. Dem Benutzer wird automatisch ein Dialog angezeigt, der den Verstoss erklärt
4. Der Benutzer kann in diesem Dialog eine Begründung eingeben und um Entsperrung oder Limit-Erhöhung bitten
5. Durch Absenden des Dialogs wird der Änderungsvorschlag an die Administratoren übermittelt

### Arten von Änderungsvorschlägen

**Entsperrungsanfrage**: Wenn ein Benutzer aufgrund zu vieler Verstösse gegen Daten- oder Anfragelimits gesperrt wurde, wird ihm automatisch ein Dialog zur Entsperrungsanfrage angezeigt. Der Benutzer kann hier begründen, warum die Sperre aufgehoben werden soll. Der Administrator kann dann die Verstoss-Zähler zurücksetzen und den Benutzer wieder freigeben.

**Limit-Erhöhungsanfrage**: Wenn ein Benutzer mit Bearbeitungslimits das tägliche Limit für ein bestimmtes Informationsobjekt erreicht, wird automatisch ein Dialog angezeigt, über den der Benutzer eine Erhöhung seiner Limits beantragen kann. Der Administrator kann diese Anfrage prüfen und das Limit anpassen oder die Anfrage ablehnen.

### Ablauf einer Limit-Erhöhungsanfrage
Die Limit-Erhöhungsanfrage betrifft zwei Benutzerrollen sowie das Nachrichtensystem. Der folgende Ablauf zeigt das Zusammenspiel:

```mermaid
sequenceDiagram
    participant U as Benutzer mit Limits
    participant GT as Grafioschtrader
    participant A as Administrator
    participant M as Nachrichtensystem
    U->>GT: Tageslimit eines Informationsobjekts erreicht
    GT-->>U: Dialog mit Antragsformular
    U->>GT: Gewünschtes Tageslimit, Bis Datum und Begründung
    GT->>A: Limit-Änderungsvorschlag (Spalte L)
    A->>GT: Anfrage prüfen, anpassen und genehmigen oder ablehnen
    GT->>M: Benachrichtigung über den Entscheid
    M->>U: GT Nachricht bzw. E-Mail
```

Erreicht ein Benutzer mit Limits das Tageslimit einer Informationsklasse, erscheint automatisch ein Dialog. Die betroffene Informationsklasse ist bereits vorgegeben und kann nicht geändert werden. Der Benutzer trägt das gewünschte Tageslimit, ein Bis Datum sowie eine Begründung ein und sendet den Antrag ab. Der Antrag erscheint beim Administrator als Limit-Änderungsvorschlag und wird in der Benutzertabelle mit dem Symbol in der Spalte **L** angezeigt. Der Administrator öffnet den Antrag über die expandierte Zeile des Benutzers, kann den beantragten Wert und das Ablaufdatum vor der Genehmigung anpassen und entscheidet anschliessend über Genehmigung oder Ablehnung; in beiden Fällen wird der Benutzer über das [Nachrichtensystem]({{% relref "/admindata" %}}) informiert.

Pro Benutzer und Informationsklasse gibt es höchstens eine Limite dieser Art. Eine Genehmigung erstellt entweder eine neue Limite oder erneuert die bestehende, indem Wert und Ablaufdatum aktualisiert werden; auf diese Weise wird auch eine bereits abgelaufene Limite wieder in Kraft gesetzt. Solange eine Anfrage für eine Informationsklasse noch offen ist, kann der Benutzer für dieselbe Informationsklasse keine zweite Anfrage stellen und erhält den Hinweis, dass bereits eine offene Anfrage besteht.

### Anzeige in der Benutzertabelle
Wenn für einen Benutzer Änderungsvorschläge vorliegen, werden diese in der Benutzertabelle durch Symbole angezeigt:
- **U** (Spalte): Zeigt an, dass ein Benutzer-Änderungsvorschlag (Entsperrungsanfrage) vorliegt
- **L** (Spalte): Zeigt an, dass ein Limit-Änderungsvorschlag vorliegt

Administratoren können diese Vorschläge über spezielle Funktionen bearbeiten und entscheiden, ob sie genehmigt oder abgelehnt werden. Bei Genehmigung werden die Verstoss-Zähler zurückgesetzt oder die Limits entsprechend angepasst.