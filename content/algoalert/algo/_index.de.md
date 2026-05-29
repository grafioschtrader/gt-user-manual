---
title: "Regelbasierter Handel"
date: 2026-02-12T12:00:00+01:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Der regelbasierte Handel in GT basiert auf einer hierarchischen Baumstruktur. An der Wurzel steht die Portfolio-Strategie (**AlgoTop**), die eine Verknüpfung zu einer bestehenden Watchlist herstellt. Unter dieser Wurzel werden Anlageklassen mit prozentualen Gewichtungen angelegt, und innerhalb der Anlageklassen werden Wertpapiere aus der verknüpften Watchlist zugewiesen.

## Portfolio-Strategie erstellen und bearbeiten
Eine neue Portfolio-Strategie erstellen Sie über das Kontextmenü in der Baumansicht des **Regelbasierten Handels**. Beim Erstellen einer Portfolio-Strategie werden folgende Eigenschaften konfiguriert:

| Eigenschaft | Beschreibung |
|---|---|
| **Name** | Bezeichnung der Portfolio-Strategie |
| **Watchlist** | Die verknüpfte Watchlist, aus der Wertpapiere ausgewählt werden können |
| **Maximal investiert** | Maximaler Prozentsatz des Portfolios, der investiert werden darf |
| **Primäres Depot** | Das primäre Wertpapierdepot für Transaktionen |
| **Sekundäres Depot** | Ein optionales sekundäres Wertpapierdepot |

Nachdem die Portfolio-Strategie erstellt wurde, können Anlageklassen und Wertpapiere als Kindknoten hinzugefügt werden.

## Anlageklassen hinzufügen
Über das Kontextmenü der Portfolio-Strategie können Sie mit **Hinzufügen Anlageklasse** eine neue Anlageklasse als Kindknoten erstellen. Jede Anlageklasse erhält eine prozentuale **Gewichtung**, die den Zielanteil dieser Anlageklasse am Gesamtportfolio definiert. Die Summe aller Gewichtungen sollte idealerweise 100% ergeben.

## Wertpapiere hinzufügen
Innerhalb einer Anlageklasse können Sie über **Hinzufügen Strategie Wertpapier** einzelne Wertpapiere hinzufügen. Die verfügbaren Wertpapiere stammen aus der Watchlist, die mit der übergeordneten Portfolio-Strategie verknüpft ist.

## Strategien zuweisen
Sowohl auf der Ebene der Anlageklasse als auch auf der Ebene des Wertpapiers können Strategien zugewiesen werden. Über **Erstelle Strategie** im Kontextmenü wird ein Dialog geöffnet, in dem der gewünschte Strategietyp ausgewählt wird. Je nach Strategietyp erscheinen unterschiedliche Konfigurationsfelder. Detaillierte Informationen zu den einzelnen Strategietypen finden Sie unter [Strategien](../strategy/).

{{< mermaid >}}
graph TD
    A["Portfolio-Strategie<br/>(Name, Watchlist, Max. Investiert)"] --> B["Anlageklasse<br/>(Gewichtung %)"]
    A --> C["Anlageklasse<br/>(Gewichtung %)"]
    B --> D["Wertpapier<br/>(aus Watchlist)"]
    B --> E["Wertpapier<br/>(aus Watchlist)"]
    D --> F["Strategie"]
    E --> G["Strategie"]
    C --> H["Wertpapier<br/>(aus Watchlist)"]
    H --> I["Strategie"]
{{< /mermaid >}}

{{% notice style="info" title="Hinweis" %}}
Die Anlageklasse auf der Ebene der Baumstruktur entspricht nicht direkt einer Anlageklasse in GT. Sie dient zur Gruppierung und Gewichtung von Wertpapieren innerhalb der Portfolio-Strategie.
{{% /notice %}}
