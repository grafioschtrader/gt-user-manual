---
title: "Widgets für Administratoren"
date: 2026-11-13T15:00:00+01:00
draft: false
weight: 20
aliases: ["/intro/dashboard/adminwidgets/"]
archetype: "default"
---
Zusätzlich zu den [Widgets]({{% relref "/dashboard/widgets" %}}) aller Rollen sieht ein Benutzer, dessen privilegierteste Rolle **Administrator** ist, die folgende Karte. Die Rolle **Privilegierter Benutzer** erhält sie nicht. Fehlt dem Administrator noch ein Klient, entfallen wie bei den anderen Rollen die Anlagekarten; die Administratorkarte bleibt sichtbar.

## Anträge auf Limitenänderung
Die Karte sammelt Anträge, die einen Administrator brauchen. Das sind einerseits Anträge, mit denen ein Benutzer die Änderung seiner Tageslimite verlangt. Andererseits sind es Entsperrungsanfragen von Benutzern, die wegen zu vieler Verstösse abgemeldet wurden. Beide Arten stehen gemeinsam in einer Tabelle, die neuesten zuoberst. Den vollständigen Ablauf beschreibt der Abschnitt [Änderungsvorschläge]({{% relref "/admindata/user" %}}#änderungsvorschläge) der Benutzer-Einstellungen. Die gespeicherte Einstellung **Maximale Zeilenanzahl** begrenzt die Tabelle, voreingestellt fünf Zeilen und zulässig von 1 bis 20.

Die Überschrift nennt die Anzahl offener Anträge. Darunter steht, wie viele **betroffene Benutzer** dahinterstehen. Die Tabelle enthält **Spitzname**, **Erstellungszeit**, **Informationsobjekt**, **Beantragte Tageslimite**, **Gültig bis** und **Bemerkung Datenänderung**. Fehlen Einträge, erscheint **Keine offenen Einträge.**

Bei einer Entsperrungsanfrage nennt **Informationsobjekt** den Grund der Abmeldung, etwa **Zu viele Verstösse gegen das Anfragelimit** oder **Zu viele Verstösse gegen Fremddaten/Datenlimit**. **Beantragte Tageslimite** und **Gültig bis** bleiben dabei leer, die Begründung des Benutzers steht in **Bemerkung Datenänderung**.

Die Zelle **Erstellungszeit** ist rot hinterlegt, damit neue Anträge auffallen. Am Tag der Erstellung ist das Rot am stärksten. Es wird mit jedem Tag schwächer und verschwindet nach fünf Tagen ganz; ältere Anträge haben keinen farbigen Hintergrund mehr.

**Vollständige Ansicht öffnen** führt zur [Benutzerverwaltung]({{% relref "/admindata/user" %}}). Dort erscheint ein Limit-Änderungsvorschlag in der Spalte **L** der Benutzertabelle, eine Entsperrungsanfrage in der Spalte **U**. Ein Limit-Änderungsantrag ist ein Hinweis, kein Beweis, dass der Benutzer bereits gesperrt ist; die Sperre wegen wiederholter Verstösse ist ein eigener Vorgang derselben Seite. Eine Entsperrungsanfrage setzt dagegen voraus, dass der Benutzer bereits abgemeldet wurde.
