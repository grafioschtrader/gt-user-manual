---
title: "Handelsplattform Plan"
date: 2021-03-13T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
Ein **Depot** hat einen **Handelsplattform Plan** aus folgenden Gründen:
+ Er steuert die korrekte Zuordnung der **Importvorlagen** zu einem Depot.
+ Es **wird mehrere Prognosealgorithmen geben**, die versuchen, die Transaktionskosten möglichst genau aus den bisher getätigten Transaktionen zu ermitteln. So kann in zukünftigen Versionen von GT die **hypothetische Glattstellung** von Positionen **reale Ergebnisse** liefern.

Im folgenden Entity-Relationship-Diagramm sind die Beziehungen dargestellt:

{{< mermaid >}}
erDiagram
    Depot }o--|| Handelsplattform-Plan : hat
    Handelsplattform-Plan }o--o| Vorlagengruppe : hat
    Vorlagengruppe ||--o{ Importvorlage :  hat
{{< /mermaid >}}
