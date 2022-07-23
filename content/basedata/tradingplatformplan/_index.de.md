---
title: "Handelsplattform Plan"
date: 2021-03-13T22:54:47+01:00
draft: false
weight : 15
chapter: true
---
## Handelsplattform Plan
Ein **Depot** hat einen **Handelsplattform Plan** aus folgenden Gründen:
+ Er steuert die korrekte Zuordnung der **Importvorlagen** zu einem Depot.
+ Es gibt mehrere **Prognose-Algorithmen** die versuchen, möglichst exakt die Transaktionskosten aus den bisherigen getätigten Transaktionen zu ermitteln. Damit kann in **zukünftigen GT-Versionen** die hypothetischen Glattstellung von Positionen realer Ergebnisse liefern.

Im folgenden Entity-Relationship-Diagramm sind die Beziehungen dargestellt:

{{< mermaid >}}
erDiagram
    Depot }o--|| Handelsplattform-Plan : hat
    Handelsplattform-Plan }o--o| Vorlagengruppe : hat
    Vorlagengruppe ||--o{ Importvorlage :  hat
{{< /mermaid >}}
