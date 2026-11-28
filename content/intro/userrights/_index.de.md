---
title: "Daten und Benutzerrechte"
date: 2026-11-19T12:00:00+01:00
draft: false
weight: 20
archetype: "default"
---
## Daten
In GT gibt es **private** und **geteilte Daten**. Bei den **geteilten Daten** wird anhand der **Benutzerrechte** die Sicht- und  Veränderbarkeit geregelt. Daten wie die "Portfolios" und "Transaktionen" sind **private Daten** und für andere Benutzer nicht zugänglich. Die Informationsklassen "Anlageklassen" und "Handelsplatz" sind beispielsweise **geteilte Daten** und für alle Benutzer sichtbar aber die **Autorisierung** des Benutzers bestimmt deren **Veränderbarkeit**.

### Private Daten
Konto, Transaktion usw. sind **private Daten** und können von einem andere Benutzer nicht gesehen oder verändert werden.

### Geteilte Daten
Bei den **geteilten Daten** bestimmen die **Zugriffsrechte** des Benutzers bzw. ob dieser der Besitzer der **Entität** über die **Veränderbarkeit** einer Entität.

#### Besitzer einer Entität
Der **Benutzer** der eine **Entität** einer **Informationsklasse** der geteilten Daten erfasst, wird automatisch zum **Besitzer** dieser. Er kann diese **Entität** unabhängig seiner **Benutzerrechte** bearbeiten. Auch die Benutzer der **Privilegierten- bzw. Administratoren-Gruppe** können uneingeschränkt diese **Entität** ändern, alle anderen Benutzer können über [Datenänderungswunsch](../../basedata/) eine Änderung vorschlagen.

#### Optimistisches Sperren
GT verwendet ein **optimistisches Sperren** das auf der Basis einer Versionsnummer basiert. Mit der **optimistischen Sperre** verfügt jedes Entität über ein Attribut, das als Versionsnummer fungiert. Jede Entität besitzt eine **Versionsnummer** diese wird beim Aktualisieren bzw. beim Speichern mit der Versionsnummer in der Datenbank verglichen. Wenn diese Versionnummern nicht übereinstimmen, bedeutet dies, dass ein anderer Benutzer die Entität vor Ihnen geändert hatte. Die Aktualisierung schlägt fehl, da Sie eine veraltete Version der Entität modifizierten. Wenn dies der Fall ist, wiederholen Sie den Vorgang, indem Sie das Entität erneut abrufen und damit eine neue Version aktualisieren. Die optimistische Sperre hindert Sie daran, versehentlich **Änderungen** anderer **Benutzer** zu **überschreiben**. Sie hindert auch andere Benutzer daran, versehentlich Ihre Änderungen zu überschreiben.

## Benutzerrechte
Mit der **vollständigen Registrierung** eines Benutzers, wird dieser automatisch zur Benutzerrolle **Benutzer mit Limits** zugeordnet. Die Ausnahme bildet der **Hauptadministrator**, siehe dazu "[GT nach der Installation](../installationupdate/installationafter)". GT definiert die folgenden vier **Benutzerrollen**:

### Benutzer mit Limits
Benutzer mit Limit kann nur eine **bestimmte Anzahl** von **Entitäten** einer **Informationsklasse** bzw. **seiner geteilten Entitäten** pro Tag erstellen bzw. ändern. Dadurch wird verhindert, dass ein Benutzer einen grösseren willentlichen Schaden an den **geteilten Daten** herbei führen kann. Dieses Limit beinhaltet auch den **Datenänderungswunsch**, weitere Informationen siehe "[Limite Informationsklasse]({{% relref "/admindata/entitylimit" %}})". Erreicht ein solcher Benutzer ein Tageslimit, kann er über einen automatisch angezeigten Dialog eine Erhöhung beantragen; den vollständigen Ablauf beschreibt der Abschnitt [Änderungsvorschläge]({{% relref "/admindata/user" %}}#änderungsvorschläge) der Benutzer-Einstellungen.

### Benutzer ohne Limits
Dieser Benutzer unterliegt keiner Einschränkung bezüglich dem Erstellen oder verändern **seiner geteilten Daten**.

{{% callout note %}}
Die täglichen Limiten sind nur für die Rolle **Benutzer mit Limits** vorgegeben, sie sind aber nicht auf diese Rolle beschränkt: der Administrator kann eine tägliche Limite für jede Benutzerrolle und auch für einen einzelnen Benutzer erfassen. Zudem gilt für die **geteilten Daten** neben der Tageslimite eine Gesamtanzahl pro Ersteller. Beides ist unter [Limite Informationsklasse]({{% relref "/admindata/entitylimit" %}}) beschrieben.
{{% /callout %}}

{{< youtube pNwCed37m8Q >}}

### Privilegierter Benutzer
Dieser hat die gleichen Benutzerrechte wie Benutzer ohne Limits plus kann er auch die **geteilten Daten** aller Benutzer verändern. Das Bearbeiten des **Handelskalender global** und der **Globale Einstellungen** ist dem Benutzer dieser Gruppe nicht erlaubt.

### Administrator
Er kann alle **geteilten Daten** verändern. Zusätzlich kann er Globale und einige Einstellungen des Benutzers ändern.

#### Hauptadministrator {#hauptadmin}
Der **Hauptadministrator** wird mit der E-Mail Adresse in `application.properties` unter der Eigenschaft `gt.main.user.admin.mail` definiert. Er erhält die Alarmmeldungen von GT direkt als E-Mail.

{{% callout style="info" title="Benutzer-ID statt Spitzname" %}}
Mit der Registrierung bei GT wird ein Spitzname erfragt. Standardmässig ist er nur für den Administrator sichtbar. Dann wird bei der Referenzierung eines Benutzers statt dem Spitznamen die ID des Benutzers angezeigt. Der Administrator kann den Spitznamen in den [globalen Einstellungen]({{% relref "/admindata/globalsettings" %}}) für alle Benutzer der Instanz freigeben. Diese ID wird z.B. bei Datenänderungswunsch und Nachricht verwendet.
{{% /callout %}}

## Sichtbarkeit im Benutzerinterface
Bei vielen Webanwendungen, insbesondere bei SaaS-Angeboten, sieht ein gewöhnlicher Benutzer nur einen kleinen Teil der Software. Einstellungen, Überwachung und Verwaltung befinden sich dort in einer eigenen Backoffice-Applikation, die nur dem Betreiber zugänglich ist. Es gibt also zwei Applikationen, eine für das Backoffice und eine für die Benutzer. GT geht bewusst einen anderen Weg: Aus Gründen der **Transparenz** werden möglichst viele Daten allen Benutzerrollen gezeigt. Die **Benutzerrolle** bestimmt in GT deshalb in erster Linie, was ein Benutzer **verändern** darf, und nur in wenigen Fällen, was er **sehen** darf.

Für einen neuen Benutzer kann die Fülle an Funktionalität im Benutzerinterface zunächst etwas überwältigend wirken. Vieles unter dem Hauptelement **Administrative Daten - Nachrichten** betrifft den Betrieb der Instanz und wird für die Verwaltung des eigenen Vermögens nicht benötigt. Diese Offenheit hat aber einen Vorteil: Da ein Benutzer unabhängig seiner Rolle eine ähnliche Sicht wie der Administrator hat, kann er Probleme selbst erkennen, beispielsweise unvollständige Kursdaten oder eine fehlgeschlagene Hintergrundaufgabe, und den Administrator mit einer Nachricht gezielt darauf hinweisen.

### Sichtbar für alle, veränderbar durch den Administrator
Die Bereiche [Handelskalender global]({{% relref "/admindata/tradingcalendar" %}}), [Vollständigkeit historischer Wertpapier-Kursdaten]({{% relref "/admindata/historyquotequality" %}}), [Globale Einstellungen]({{% relref "/admindata/globalsettings" %}}), [GTNet und Nachrichten]({{% relref "/admindata/gtnet" %}}), [Stapelverarbeitungs Monitor]({{% relref "/admindata/taskdatachangemonitor" %}}) und [Steuerdaten]({{% relref "/admindata/taxdata" %}}) stehen allen Rollen offen. Den Handelskalender global, die globalen Einstellungen und die Steuerdaten kann nur der Administrator verändern. Im Stapelverarbeitungs Monitor kann nur er Hintergrundaufgaben erfassen, kopieren oder unterbrechen, und bei GTNet nimmt nur er die Einrichtung der Verbindungen zu anderen Instanzen vor. Ebenso erfasst nur der Administrator die systemweiten [Wertpapieraktionen]({{% relref "/basedata/securityaction" %}}) bei einer ISIN-Änderung. Den anderen Rollen fehlen in diesen Bereichen lediglich die entsprechenden Menüpunkte.

### Nur für den Administrator sichtbar
Einige wenige Daten bleiben dem Administrator vorbehalten, weil sie vertraulich sind oder die Benutzer der Instanz betreffen:
- Im Hauptbaum sieht nur der Administrator die Elemente [Verbindungs-API-Key]({{% relref "/admindata/connectorapikey" %}}), [Benutzer Einstellungen]({{% relref "/admindata/user" %}}) und [Limite Informationsklasse]({{% relref "/admindata/entitylimit" %}}) sowie unter GTNet und Nachrichten das Element [Automatische Nachricht]({{% relref "/admindata/gtnet/autoanswer" %}}).
- Admin-Nachrichten im GTNet, die nur für Administratoren bestimmt sind, werden den anderen Rollen nicht angezeigt.
- Die Karte [Anträge auf Limitenänderung]({{% relref "/dashboard/adminwidgets" %}}) im Dashboard erhält nur der Administrator.
- Der Spitzname anderer Benutzer ist ohne Freigabe nur für den Administrator sichtbar, wie oben unter «Benutzer-ID statt Spitzname» beschrieben.
- Nur der Administrator kann eine Nachricht an einen einzelnen Benutzer senden und dafür die Liste aller Benutzer einsehen. Auch die Rollen «Benutzer ohne Limits», «Benutzer mit Limits» und «Alle Benutzer» kann nur er als Empfänger wählen, während jeder Benutzer den Administrator oder die privilegierten Benutzer anschreiben kann.
- Einen Link zu einem Datenlieferanten, dessen Adresse den API-Schlüssel der Instanz enthält, erhält nur der Administrator.

### Datenänderungswünsche
Der Administrator und der **Privilegierte Benutzer** sehen alle offenen [Datenänderungswünsche]({{% relref "/basedata" %}}#datenänderungswunsch), da sie diese annehmen oder ablehnen können. Die übrigen Rollen sehen nur die Wünsche zu Entitäten, deren Besitzer sie sind. Dasselbe gilt für die Karte **Offene Datenänderungsanträge** im Dashboard, die zusätzlich bei jeder Rolle die eigenen Anträge zeigt. Dies ist der einzige Unterschied in der Sichtbarkeit, der nicht allein den Administrator betrifft. Zwischen dem **Benutzer mit Limits** und dem **Benutzer ohne Limits** gibt es keinen Unterschied in der Sichtbarkeit, sie unterscheiden sich nur in den Limiten beim Verändern.

## Schutz der Fremddaten und das Anfragelimit
Der **Client** von GT kommuniziert mittels einem **REST-API** mit dem **Back-End**. Dieses REST-API ist eine allgemeine Schnittstelle und könnte auch von einem anderen Dienst benutzt werden. Die **privaten Daten** müssen aber besonders geschützt bleiben. Gezielte Zugriff auf die privaten Daten anderer Benutzer werden erkannt und bestraft. Bei mehrfacher Widerhandlung wird der Benutzer gesperrt, die Globale Einstellung von **g.max.security.breach.count** bestimmt diesen Schwellenwert.

### Anfragelimit
GT implementiert eine **Anfragelimit** mittels **Token-Bucket** auf Minuten- und Stundenbasis. D.h. es können pro Benutzer nur eine limitierte Anzahl von Anfragen in den genannten Zeiträumen an das **Back-end** erfolgreich erteilt werden. Am häufigsten wird diese Ratenbegrenzung eingesetzt, um einen Ressourcenmangel zu vermeiden und so die Verfügbarkeit von API-basierten Diensten zu verbessern. Im täglichen Gebrauch wird dieses Abfragelimit kaum überschritten, andernfalls erhält der Benutzer ein Hinweis. Bei mehrmaligen Verstoss und mit dem überschreiten der Globalen Einstellung von **g.max.limit.request.exceeded.count** wird dieser Benutzer von GT ausgesperrt.
