---
title: "Wertpapier"
date: 2022-04-01T10:54:47+01:00
draft: false
weight : 8
chapter: true
---
## Wertpapier

### Eigenschaften und Tabellenspalten
Die Kombination **ISIN** und **Währung** sind einzigartig. Damit ist es unmöglich dasselbe Wertpapiere, d.h. ISIN/Währung mit unterschiedlichen Handelsplätzen zu erfassen. Zudem können diese nach der erstmaligen Speicherung nicht mehr geändert werden.
- **Gehebelt Inverse**: Kennzeichnet ein gehebeltes und/oder inverses Instrument. Eine offene Position erhöht gemäss dem positiven oder negativen Hebel die entsprechende Marktpräsenz in der entsprechenden Anlageklasse. Dies wird nur für die ETF und ENT **Finanzinstrument** unterstützt. Bei einem inversen Wertpapier wird dieser Faktor negativ sein.

#### Anlageklasse
+ Mit der Wahl der **Anlageklasse** ändert sich die **Sichtbarkeit** des Reiters "**Wertpapier-Split/s**". Für Anleihen und Wandelanleihen kann kein Split erfasst werden. Zudem muss der gewählte **Handelsplatz** Kursdaten liefern, andernfalls kann das Wertpapier keinen Split haben.
+ Die Wahl der **Anlageklasse** beeinflusst auch die Sichtbarkeit von anderen Eigenschaften wie beispielsweise von **Stückelung**.

#### Handelsplatz
+ Falls ein **Handelsplatz** ohne Kursdaten gewählt wird, dann erhält der Dialog den Reiter **"Historische Kurse für Periode"**.

### Reiter "Historische Kurse für Periode"
Siehe dazu [Wertpapier ohne Kursdaten](./securitywithoutpricedata).

### Reiter "Wertpapier-Split/s"
Wie im Kapitel [Historische Daten](../../../externaldata/historyquote/) erwähnt, wird ein Split von Datenquellen eingelesen. Es kann vorkommen, dass die Datenquelle den Split nicht zeitlich akkurat nachführt. Aus diesem Grund kann ein **Split** auch manuell erfasst werden. Ein **Split** der manuell erfasst wurde, wird von System nicht überschrieben oder entfernt.
