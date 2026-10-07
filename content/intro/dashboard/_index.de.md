---
title: "Dashboard"
date: 2026-09-08T15:00:00+01:00
draft: false
weight: 2
archetype: "default"
---
Nach der Anmeldung öffnet GT die Hauptansicht auf dem **Dashboard**. Es steht als erstes **Hauptelement** im **Navigationsbaum**, auf derselben Ebene wie die Portfolios. Über das Fragezeichen in der Menüleiste gelangen Sie auf diese Seite, die beiden folgenden Seiten beschreiben die einzelnen Widgets.

Das Dashboard gibt eine persönliche Übersicht: welche Nachrichten und Anträge Aufmerksamkeit verlangen, und wie sich die gehaltenen Instrumente sowie der Mandant an den letzten Handelstagen entwickelt haben. Jede Karte fasst zusammen; wo eine vollständige Bearbeitung nötig ist, führt **Vollständige Ansicht öffnen** zur bestehenden Ansicht. Die Anordnung der Karten gehört zum Benutzer, nicht zum Klienten, und wird mit den [persönlichen Daten]({{% relref "/intro/settings" %}}) exportiert.

{{% notice info %}}
Ein Administrator kann das Dashboard für die ganze Installation mit `g.use.dashboard=false` in `application.properties` ausschalten. Dann entfällt das Hauptelement im Navigationsbaum, und nach der Anmeldung erscheint wieder die bisherige Hauptansicht.
{{% /notice %}}

## Aufbau der Ansicht
Über den Karten stehen der Titel **Dashboard**, der **Aktive Mandant** als interne Nummer des Klienten sowie die Schaltflächen **Dashboard bearbeiten** und **Alle aktualisieren**. Der Zeitpunkt **Geladen am** gilt für die ganze Ansicht. Jede Karte trägt denselben Zeitpunkt für ihre eigenen Daten, dazu den Titel des Widgets, die gespeicherte Zeilen- oder Instrumentenzahl und eine eigene Schaltfläche **Aktualisieren**.

Der Hinweis **Simulationsumgebung** erscheint, wenn Sie nicht in Ihrem eigenen Klienten arbeiten, etwa nach einem Wechsel zu einem [betreuten Klienten]({{% relref "/tenantportfolio/client/managedclients" %}}) oder in einer Simulationsumgebung der [regelbasierten Strategien]({{% relref "/algoalert" %}}). Die Anlagekarten folgen dann dem **aktiven Mandanten**. Ungelesene Nachrichten und Datenänderungsanträge bleiben beim angemeldeten Benutzer, damit ein Klientenwechsel nicht den Posteingang einer anderen Person auf das Dashboard holt.

{{% notice note %}}
Das Öffnen des Dashboards markiert keine Nachricht als gelesen. Gelesen wird erst in der vollständigen Ansicht des [Nachrichtensystems]({{% relref "/admindata" %}}).
{{% /notice %}}

Ein leeres Dashboard fordert Sie auf, **Dashboard bearbeiten** zu wählen und eines der verfügbaren Widgets hinzuzufügen. Ein Widget, das mit den aktuellen Zugriffsrechten nicht mehr verfügbar ist, bleibt unsichtbar; die Karte selbst kann dennoch in der gespeicherten Anordnung stehen.

## Anordnung bearbeiten
**Dashboard bearbeiten** schaltet in den Entwurfsmodus, denselben Befehl finden Sie im Menü **Bearbeiten**, sobald das Dashboard der aktive Bereich ist. Im Entwurf zeigen die Karten keine aktuellen Zahlen, sondern den Hinweis, die Anordnung zu speichern, damit die Daten geladen werden. Rechts erscheint der **Widget-Katalog** mit den **verfügbaren Widgets**. Jedes Widget darf höchstens einmal auf dem Dashboard stehen; stehen bereits alle zur Verfügung stehenden Karten darauf, meldet der Katalog das.

Eine Karte fügen Sie hinzu, indem Sie sie aus dem Katalog auf das Raster ziehen oder **Widget hinzufügen** wählen. Entfernen Sie eine Karte mit **Entfernen** oder ziehen Sie sie in den Katalog zurück; der Katalog merkt sich Breite und Einstellungen bis zum Ende dieser Bearbeitung. Die Reihenfolge ändern Sie am Ziehpunkt, mit **Nach vorne verschieben** oder **Nach hinten verschieben**. Die **Breite** einer Karte ist **Ein Drittel**, **Hälfte**, **Zwei Drittel** oder **Volle Breite**. **Konfigurieren** öffnet den Dialog mit dem Hilfetext des Widgets und den gespeicherten Einstellungen, etwa der maximalen Zeilenanzahl.

**Dashboard speichern** übernimmt den Entwurf. **Bearbeitung abbrechen** verwirft ihn. **Auf Standard zurücksetzen** setzt den Entwurf auf die Standardanordnung der derzeit verfügbaren Widgets; erst das anschliessende Speichern macht sie wirksam, **Bearbeitung abbrechen** behält die bisherige Anordnung. Verlassen Sie die Ansicht mit ungespeicherten Änderungen, fragt GT, ob diese verworfen werden sollen.

{{< mermaid >}}
graph TD
    A[Dashboard ansehen] --> B[Dashboard bearbeiten]
    B --> C[Widgets hinzufügen, entfernen, anordnen oder konfigurieren]
    C --> D{Entwurf übernehmen?}
    D -->|Dashboard speichern| E[Anordnung wird geladen]
    D -->|Bearbeitung abbrechen| A
    C --> F[Auf Standard zurücksetzen]
    F --> D
    E --> A
{{< /mermaid >}}

Es haben höchstens 24 Widgets auf einem Dashboard Platz, und jedes Widget nur einmal. Ein Widget, das Sie wegen geänderter Rechte nicht mehr sehen, bleibt in der gespeicherten Anordnung und zählt weiter zur Kapazität. Ist die Kapazität ausgeschöpft, müssen Sie zuerst Karten entfernen oder auf die Standardanordnung zurücksetzen.

Speichert eine andere Sitzung zuerst, erscheint der Hinweis, die gespeicherte Anordnung neu zu laden; der eigene Entwurf bleibt unter **Vorheriger, nicht gespeicherter Entwurf** zur Einsicht. Eine Anordnung mit einer nicht unterstützten Version lässt sich nicht bearbeiten, damit die gespeicherte Fassung erhalten bleibt.

## Aktualisieren
**Alle aktualisieren** lädt jede Karte neu. **Aktualisieren** auf einer einzelnen Karte lädt nur diese. Schlägt die Aktualisierung fehl, bleiben die zuvor geladenen Daten sichtbar und die Karte vermerkt, dass die Aktualisierung fehlgeschlagen ist.

Einstellungen, die Sie direkt auf der Karte ändern, etwa den gewählten Handelstag oder das Depot, gehören nicht zur gespeicherten Anordnung. Ein gewöhnliches **Aktualisieren** stellt dafür wieder die gespeicherten Vorgaben her.

{{% notice note %}}
Die Karten **Grösste Gewinner** und **Grösste Verlierer** messen die Kursbewegung gehaltener Instrumente. Die Karte **Letzte Handelstage** zeigt den Periodenertrag des Mandanten oder eines Depots. Das sind zwei verschiedene Fragen; die Zahlen müssen sich nicht zu derselben Aussage addieren. Die Unterschiede beschreibt die Seite [Widgets]({{% relref "/intro/dashboard/widgets" %}}).
{{% /notice %}}

## Welche Widgets Sie sehen
Welche Karten der Katalog anbietet, hängt von der [Benutzerrolle]({{% relref "/intro/userrights" %}}) und davon ab, ob bereits ein Klient besteht. Nachrichten und Datenänderungsanträge stehen jedem angemeldeten Benutzer zur Verfügung. Die Anlagekarten erscheinen erst, wenn ein Klient eingerichtet ist. Zusätzliche Karten für die Rolle **Administrator** beschreibt [Widgets für Administratoren]({{% relref "/intro/dashboard/adminwidgets" %}}).
