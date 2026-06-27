---
title: "Wertpapier"
date: 2026-06-05T10:54:47+01:00
draft: false
weight: 8
archetype: "default"
---
## Eigenschaften und Tabellenspalten
Die Kombination **ISIN** und **Währung** sind einzigartig. Damit ist es unmöglich dasselbe Wertpapiere, d.h. ISIN/Währung mit unterschiedlichen Handelsplätzen zu erfassen. Zudem können diese nach der erstmaligen Speicherung nicht mehr geändert werden.
- **Name**: Bei **Anleihen** und **Wandelanleihen** sollte der Name mit dem Zinssatz des Kupons beginnen. Zum Beispiel “0.0219 IBK 19-25” für die Anleihe der Industrial Bank of Korea. In GT gibt es eine Funktionalität, die den Zinssatz aus dem Namen liest, siehe dazu [Rendite bis zur Fälligkeit](../../../../basedata/udfmetadata/instruments/). Zudem wird der Zinssatz nirgendwo sonst im Wertpapier gespeichert.
- **Gehebelt Inverse**: Kennzeichnet ein gehebeltes und/oder inverses Instrument. Eine offene Position erhöht gemäss dem positiven oder negativen Hebel die entsprechende Marktpräsenz in der entsprechenden Anlageklasse. Dies wird nur für die ETF und ENT **Finanzinstrument** unterstützt. Bei einem inversen Wertpapier wird dieser Faktor negativ sein.

### Anlageklasse
+ Mit der Wahl der **Anlageklasse** ändert sich die **Sichtbarkeit** des Reiters "**Wertpapier-Split/s**". Für Anleihen und Wandelanleihen kann kein Split erfasst werden. Zudem muss der gewählte **Handelsplatz** Kursdaten liefern, andernfalls kann das Wertpapier keinen Split haben. In GT kann ein Handelsplatz auch ohne Kursdaten definiert werden.
+ Die Wahl der Eigenschaft **Anlageklasse** beeinflusst auch die Sichtbarkeit von anderen Eigenschaften wie beispielsweise von **Stückelung**.
+ Die Eigenschaften **Anlageklasse** und **Finanzinstrument** können bei einem bestehenden **Wertpapier** mit einer oder mehreren **Transaktion/en** nicht mehr geändert werden. 

### Stückelung 
Bei Anleihen ist dies die kleinste handelbare Einheit, bei Festgeldern der Kurs. Derzeit gibt es keine Validierung bei Käufen und Verkäufen. Es ist daher möglich, dass ein zahlungsunfähiger Gläubiger für die teilweise oder vollständige Rückzahlung andere Stückelungen wählt. Bei Anleihen hat diese Angabe daher vor allem Informationscharakter.

### Handelsplatz
+ Falls ein **Handelsplatz** ohne Kursdaten gewählt wird, dann erhält der Dialog den Reiter **"Historische Kurse für Periode"**.

## Reiter "Historische Kurse für Periode"
Siehe dazu [Wertpapier ohne Kursdaten](./securitywithoutpricedata).

## Reiter "Wertpapier-Split/s"
Wie im Kapitel [Historische Daten](../../../externaldata/historyquote/) erwähnt, wird ein Split von Datenquellen eingelesen. Es kann vorkommen, dass die Datenquelle den Split nicht zeitlich akkurat nachführt. Aus diesem Grund kann ein **Split** auch manuell erfasst werden. Ein **Split** der manuell erfasst wurde, wird von System nicht überschrieben oder entfernt.

Ein Split wird erfasst, indem das **Split Datum**, die **Aus Anzahl** und die **Wird Anzahl** eingegeben werden und anschliessend **Übernehmen** angeklickt wird. Der neue Eintrag erscheint danach in der Tabelle unterhalb des Formulars. Das Verhältnis *Aus Anzahl : Wird Anzahl* beschreibt den Split: Bei `2 : 1` wird aus einer Aktie deren zwei, `3 : 2` entspricht einer Zusammenlegung. Durch Auswahl einer Zeile in der Tabelle lässt sich der Eintrag bearbeiten oder löschen. Wird ein bereits vorhandenes Datum erneut erfasst, so überschreibt der Eintrag den bestehenden für dieses Datum, anstatt ihn zu verdoppeln. Die Änderungen werden erst gespeichert, wenn das ganze **Wertpapier** gespeichert wird.

{{% notice style="info" title="Maximale Anzahl Splits" %}}
Pro Instrument können standardmässig bis zu 20 Splits erfasst werden. Eine Administratorin oder ein Administrator kann diese Grenze über die globale Einstellung `gt.max.instrument.splits` anpassen. Die Fusszeile der Tabelle zeigt die aktuelle Anzahl und das erlaubte Maximum an.
{{% /notice %}}
