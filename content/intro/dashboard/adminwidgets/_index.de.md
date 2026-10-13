---
title: "Widgets für Administratoren"
date: 2026-09-08T15:00:00+01:00
draft: false
weight: 20
archetype: "default"
---
Zusätzlich zu den [Widgets]({{% relref "/intro/dashboard/widgets" %}}) aller Rollen sieht ein Benutzer, dessen privilegierteste Rolle **Administrator** ist, die folgende Karte. Die Rolle **Privilegierter Benutzer** erhält sie nicht. Fehlt dem Administrator noch ein Klient, entfallen wie bei den anderen Rollen die Anlagekarten; die Administratorkarte bleibt sichtbar.

## Anträge auf Limitenänderung
Die Karte sammelt Anträge, mit denen ein Benutzer die Änderung seiner Tageslimite verlangt und dafür einen Administrator braucht. Den vollständigen Ablauf beschreibt der Abschnitt [Änderungsvorschläge]({{% relref "/admindata/user" %}}#änderungsvorschläge) der Benutzer-Einstellungen. Die gespeicherte Einstellung **Maximale Zeilenanzahl** begrenzt die Tabelle, voreingestellt fünf Zeilen und zulässig von 1 bis 20.

Die Überschrift nennt die Anzahl offener Anträge. Darunter steht, wie viele **betroffene Benutzer** dahinterstehen. Die Tabelle enthält **Spitzname**, **Informationsobjekt**, **Beantragte Tageslimite**, **Gültig bis**, **Bemerkung Datenänderung** und **Erstellungszeit**. Fehlen Einträge, erscheint **Keine offenen Einträge.**

**Vollständige Ansicht öffnen** führt zur [Benutzerverwaltung]({{% relref "/admindata/user" %}}). Dort erscheint ein Limit-Änderungsvorschlag in der Spalte **L** der Benutzertabelle. Ein solcher Antrag ist ein Hinweis, kein Beweis, dass der Benutzer bereits gesperrt ist; die Sperre wegen wiederholter Verstösse ist ein eigener Vorgang derselben Seite.
