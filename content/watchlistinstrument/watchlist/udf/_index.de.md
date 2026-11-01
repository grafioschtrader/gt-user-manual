---
title: "Zusatzfeld Ansicht"
date: 2026-10-15T12:00:00+02:00
draft: false
weight: 25
archetype: "default"
---
In dieser Ansicht werden die **zusätzlichen Felder** für **Währungspaare** und **Instrumente** in Tabellenform angezeigt.  

## Tabellenansicht
Selbstverständlich können auch in dieser Tabelle die definierten Eigenschaften ein- und ausgeblendet werden. In der aufklappbaren Tabellenzeile sind dann noch immer alle Zusatzfelder entsprechend dem Instrument sichtbar.

### Webseiten-URL
Enthält ein Zusatzfeld eine Webseiten-URL, so wird diese durch eines der folgenden Symbole dargestellt: {{< svg "link0.svg" svg-icon-size >}}, {{< svg "link1.svg" svg-icon-size >}}, {{< svg "link2.svg" svg-icon-size >}}, {{< svg "link3.svg" svg-icon-size >}}.
{{% callout style="info" title="Ermittlung welches Symbol" %}}
Die folgende Information ist für den Benutzer von untergeordneter Bedeutung: Intern verwendet GT für jedes Zusatzfeld einen eindeutigen numerischen Schlüssel. Dieser wird beim Anlegen eines Zusatzfeldes um 1 erhöht. Aus diesem Schlüssel wird mittels Modulo das entsprechende URL-Symbol berechnet. Bei mehreren Zusatzfeldern mit Webseiten-URL muss keine Gleichverteilung der Symbole vorliegen.
{{% /callout %}}

### Expandierende Tabellenzeile
Es kann sein, dass Sie sehr viele Felder definiert haben und nur einige davon in der Tabelle angezeigt werden. Unter der aufklappbaren Tabellenzeile werden alle definierten Felder angezeigt.

## Automatisch gefüllte Werte
Zusatzfelder für Instrumente können ihre Werte [automatisch füllen]({{% relref "/basedata/udfmetadata/instruments" %}}). Beim Öffnen dieser Ansicht werden Felder mit einer Vorlage sofort gefüllt, ausser in einer Simulationsumgebung oder bei Zugriff auf einen fremden Klienten. Ein Symbol vor dem Wert zeigt dessen Herkunft:
- Ein Zauberstab kennzeichnet einen automatisch gefüllten Wert. Der Tooltip nennt die Füllquelle, den Zeitpunkt der letzten Prüfung und allenfalls die Quelle.
- Ein Schloss kennzeichnet eine Zelle, die Sie manuell übernommen haben.
- Eine Glühbirne mit kursivem Wert kennzeichnet einen noch nicht angenommenen Vorschlag eines Sprachmodells. Der Tooltip zeigt den Auszug aus der Quelle.

Ein Wert ohne Symbol ist manuell erfasst. Bei einem Wertpapier mit automatisch gefüllten Feldern enthält das Kontextmenü den Eintrag **Automatisches Füllen**. Darunter finden Sie je Feld **Manuell übernehmen** beziehungsweise **Wieder automatisch füllen** und bei einem Vorschlag **Vorschlag annehmen** und **Vorschlag ablehnen**. Eine manuell übernommene Zelle verändert GT nicht mehr, auch wenn sie leer ist. Mit **Wieder automatisch füllen** wird eine Zelle mit Vorlage sofort neu berechnet.
