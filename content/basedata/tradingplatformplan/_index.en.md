---
title: "Trading platform plan"
date: 2018-01-13T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
A **securities account** has a **trading platform plan** for the following reasons:
+ It controls the correct assignment of **import templates** to a depot.
+ **There will be several forecasting algorithms** that try to determine the transaction costs as accurately as possible from the transactions made so far. Thus, in future versions of GT, the **hypothetical closing out of** positions can deliver **real results**.

The relationships are shown in the following entity relationship diagram:
{{< mermaid >}} 
erDiagram 
  "Securities account" }o--|| "Trading platform plan" : has 
  "Trading platform plan" }o--o| "Template group" : has
  "Template group" ||--o{ "Import template" : has 
{{< /mermaid >}}
