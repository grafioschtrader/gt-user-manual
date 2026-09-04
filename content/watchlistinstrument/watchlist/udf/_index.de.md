---
title: "Zusatzfeld Ansicht"
date: 2024-06-23T20:54:47+01:00
draft: false
weight: 25
archetype: "default"
---
In dieser Ansicht werden die **zusätzlichen Felder** für **Währungspaare** und **Instrumente** in Tabellenform angezeigt.  

## Tabellenansicht
Selbstverständlich können auch in dieser Tabelle die definierten Eigenschaften ein- und ausgeblendet werden. In der aufklappbaren Tabellenzeile sind dann noch immer alle Zusatzfelder entsprechend dem Instrument sichtbar.

### Webseiten-URL
Enthält ein Zusatzfeld eine Webseiten-URL, so wird diese durch eines der folgenden Symbole dargestellt: {{< svg "link0.svg" svg-icon-size >}}, {{< svg "link1.svg" svg-icon-size >}}, {{< svg "link2.svg" svg-icon-size >}}, {{< svg "link3.svg" svg-icon-size >}}.
{{% notice style="info" title="Ermittlung welches Symbol" %}}
Die folgende Information ist für den Benutzer von untergeordneter Bedeutung: Intern verwendet GT für jedes Zusatzfeld einen eindeutigen numerischen Schlüssel. Dieser wird beim Anlegen eines Zusatzfeldes um 1 erhöht. Aus diesem Schlüssel wird mittels Modulo das entsprechende URL-Symbol berechnet. Bei mehreren Zusatzfeldern mit Webseiten-URL muss keine Gleichverteilung der Symbole vorliegen.
{{% /notice %}}

### Expandierende Tabellenzeile
Es kann sein, dass Sie sehr viele Felder definiert haben und nur einige davon in der Tabelle angezeigt werden. Unter der aufklappbaren Tabellenzeile werden alle definierten Felder angezeigt. 
