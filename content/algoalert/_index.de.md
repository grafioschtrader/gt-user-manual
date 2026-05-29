---
title: "Regelbasierter Handel und Alarme"
date: 2026-02-12T12:00:00+01:00
draft: false
weight: 22
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
GT unterstützt regelbasierten Handel und ein umfassendes Alarmsystem. Diese Funktionalität ist in zwei Ebenen aufgebaut: die strategische Kernallokation für langfristiges Portfolio-Rebalancing und die taktische Handelsebene für spekulative Strategien mit Alarmen.

## Hierarchische Struktur
Das Algo-System in GT ist hierarchisch aufgebaut. An der Spitze steht die Portfolio-Strategie (**AlgoTop**), die mit einer Watchlist verknüpft ist. Darunter befinden sich die Anlageklassen mit ihren prozentualen Gewichtungen, und auf der untersten Ebene die einzelnen Wertpapiere mit ihren zugewiesenen Strategien.

{{< mermaid >}}
graph TD
    A["Portfolio-Strategie (AlgoTop)"] --> B["Anlageklasse 1 (z.B. Aktien 60%)"]
    A --> C["Anlageklasse 2 (z.B. Obligationen 40%)"]
    B --> D["Wertpapier A"]
    B --> E["Wertpapier B"]
    C --> F["Wertpapier C"]
    D --> G["Strategie 1"]
    D --> H["Strategie 2"]
    E --> I["Strategie 3"]
{{< /mermaid >}}

## Zwei Betriebsebenen
GT unterscheidet zwischen zwei Betriebsebenen für den regelbasierten Handel.

Die **Kernallokation** definiert die langfristige Aufteilung des Portfolios auf verschiedene Anlageklassen. Über periodisches Rebalancing werden Abweichungen von den Zielgewichtungen erkannt und Handlungsempfehlungen generiert. Diese Ebene eignet sich für eine strategische, passive Anlagestrategie.

Die **taktische Handelsebene** ermöglicht die Zuweisung von spekulativen Strategien an einzelne Wertpapiere. Dies umfasst einfache Preisalarme, technische Indikatoren und komplexe Handelsstrategien wie das Dip-Buying mit Gewinn- und Verlustmanagement.

## Zwei Ausführungsmodi
Strategien in GT können in zwei Modi betrieben werden. Im **Alarm-Modus** werden die definierten Strategien gegen aktuelle Marktdaten ausgewertet. Werden die konfigurierten Bedingungen erfüllt, generiert das System Alarmmeldungen, die den Benutzer über das interne Nachrichtensystem informieren. Der Benutzer entscheidet dann selbständig über die Ausführung.

{{% notice style="info" title="Simulationsmodus" %}}
Ein **Simulationsmodus** für das Backtesting von Strategien gegen historische Daten ist geplant, aber noch nicht implementiert. In einer zukünftigen Version wird es möglich sein, Strategien mit einer Kopie des Portfolios gegen historische Kursdaten zu testen.
{{% /notice %}}

## Navigation
In GT erreichen Sie den Bereich für den regelbasierten Handel über den Navigationsbereich auf der linken Seite. Klicken Sie auf **Regelbasierter Handel**, um die Baumansicht mit Ihren Portfolio-Strategien zu öffnen. Zusätzlich finden Sie unter **Mandanten-Alarme** eine Übersicht aller aktiven Alarme Ihres Mandanten.
