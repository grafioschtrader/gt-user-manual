---
title: "Strategien"
date: 2026-02-12T12:00:00+01:00
draft: false
weight: 20
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
GT unterstützt verschiedene Strategietypen, von einfachen Preisalarmen bis zu komplexen Handelsstrategien. Jede Strategie wird einem Knoten in der hierarchischen AlgoTop-Baumstruktur zugewiesen und überwacht die zugehörigen Wertpapiere anhand der konfigurierten Bedingungen.

## Einfache Strategien
Einfache Strategien verwenden flache Schlüssel-Wert-Parameter, die über dynamisch generierte Formularfelder konfiguriert werden. Beim Erstellen einer Strategie über **Erstelle Strategie** wird der Strategietyp ausgewählt, und das System zeigt automatisch die passenden Eingabefelder an. Zu den einfachen Strategien gehören die Preisalarme, Indikator-Alarme und das Portfolio-Rebalancing.

## Komplexe Strategien
Komplexe Strategien besitzen eine verschachtelte Konfiguration, die als JSON gespeichert wird. Für die Bearbeitung steht ein YAML-Textfeld zur Verfügung. Bei der Auswahl einer komplexen Strategie erscheint anstelle der dynamischen Formularfelder ein Textbereich mit der Bezeichnung **Strategiekonfiguration (YAML)**, in den die Konfiguration als YAML geschrieben oder eingefügt werden kann. Beim Speichern wird das YAML automatisch in JSON konvertiert.

## Strategietypen im Überblick

{{< mermaid >}}
graph TD
    S["Strategietypen"] --> R["Rebalancing"]
    S --> P["Preisalarme"]
    S --> I["Indikator-Alarme"]
    S --> K["Komplexe Strategien"]
    R --> R1["Portfolio-Neugewichtung"]
    P --> P1["Absolute Gewinn/Verlust Warnung"]
    P --> P2["Preis im Bestand Gewinn/Verlust Warnung"]
    P --> P3["Gewinn/Verlust Warnung in einer Periode"]
    I --> I1["Gleitender Durchschnitt Kreuzungsalarm"]
    I --> I2["RSI Schwellenwert-Alarm"]
    I --> I3["Benutzerdefinierter Ausdrucksalarm"]
    K --> K1["Mean-Reversion-Dip"]
{{< /mermaid >}}

Die folgende Tabelle listet alle verfügbaren Strategietypen mit ihren Einsatzebenen auf. Die Ebene gibt an, ob die Strategie auf der obersten Ebene (Portfolio), auf der Anlageklassen-Ebene oder auf der Wertpapier-Ebene zugewiesen werden kann.

| Strategietyp | Ebenen | Kategorie | Details |
|---|---|---|---|
| [Portfolio-Neugewichtung](./rebalancing/) | Portfolio, Anlageklasse, Wertpapier | Rebalancing | Periodisches Rebalancing der Portfolioallokation |
| [Absolute Gewinn/Verlust Warnung](./pricealerts/) | Wertpapier | Preisalarm | Alarm bei absoluten Preisgrenzen |
| [Preis im Bestand Gewinn/Verlust Warnung](./pricealerts/) | Portfolio, Anlageklasse, Wertpapier | Preisalarm | Alarm bei prozentualer Bestandsveränderung |
| [Gewinn/Verlust Warnung in einer Periode](./pricealerts/) | Portfolio, Anlageklasse, Wertpapier | Preisalarm | Alarm bei Kursveränderung über einen Zeitraum |
| [Gleitender Durchschnitt Kreuzungsalarm](./indicatoralerts/) | Wertpapier | Indikator | Alarm bei Kreuzung eines gleitenden Durchschnitts |
| [RSI Schwellenwert-Alarm](./indicatoralerts/) | Wertpapier | Indikator | Alarm bei RSI-Über-/Unterschreitung |
| [Benutzerdefinierter Ausdrucksalarm](./indicatoralerts/) | Wertpapier | Indikator | Alarm mit benutzerdefiniertem Ausdruck |
| [Mean-Reversion-Dip](./meanreversiondip/) | Wertpapier | Komplex | Dip-Buying mit Gewinn-/Verlustmanagement |

{{% notice style="info" title="Hinweis" %}}
Für komplexe Strategien wird in zukünftigen Versionen ein erweiterter Editor mit Syntaxhervorhebung und visueller Konfiguration bereitgestellt.
{{% /notice %}}
