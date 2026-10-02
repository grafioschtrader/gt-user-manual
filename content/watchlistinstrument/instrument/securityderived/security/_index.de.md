---
title: "Wertpapier"
date: 2026-09-22T12:00:00+02:00
draft: false
weight: 8
archetype: "default"
---
## Eigenschaften und Tabellenspalten
Die Kombination **ISIN** und **Währung** sind einzigartig. Damit ist es unmöglich dasselbe Wertpapiere, d.h. ISIN/Währung mit unterschiedlichen Handelsplätzen zu erfassen. Zudem können diese nach der erstmaligen Speicherung nicht mehr geändert werden.
- **Name**: Bei **Anleihen** und **Wandelanleihen** sollte der Name mit dem Zinssatz des Kupons beginnen. Zum Beispiel “0.0219 IBK 19-25” für die Anleihe der Industrial Bank of Korea. In GT gibt es eine Funktionalität, die den Zinssatz aus dem Namen liest, siehe dazu [Rendite bis zur Fälligkeit](../../../../basedata/udfmetadata/instruments/). Der Bearbeitungsdialog schlägt diesen Zinssatz zudem im Reiter **Anleihensimulation** als **Jährlicher Coupon (%)** vor.
- **Gehebelt Inverse**: Kennzeichnet ein gehebeltes und/oder inverses Instrument. Eine offene Position erhöht gemäss dem positiven oder negativen Hebel die entsprechende Marktpräsenz in der entsprechenden Anlageklasse. Dies wird nur für die ETF und ENT **Finanzinstrument** unterstützt. Bei einem inversen Wertpapier wird dieser Faktor negativ sein.

### Anlageklasse
+ Mit der Wahl der **Anlageklasse** ändert sich die **Sichtbarkeit** des Reiters "**Wertpapier-Split/s**". Für Anleihen und Wandelanleihen kann kein Split erfasst werden. Zudem muss der gewählte **Handelsplatz** Kursdaten liefern, andernfalls kann das Wertpapier keinen Split haben. In GT kann ein Handelsplatz auch ohne Kursdaten definiert werden.
+ Die Wahl der Eigenschaft **Anlageklasse** beeinflusst auch die Sichtbarkeit von anderen Eigenschaften wie beispielsweise von **Stückelung** und **Emittentenland** sowie des Reiters **Anleihensimulation**.
+ Die Eigenschaften **Anlageklasse** und **Finanzinstrument** können bei einem bestehenden **Wertpapier** mit einer oder mehreren **Transaktion/en** nicht mehr geändert werden. 

### Stückelung 
Bei Anleihen ist dies die kleinste handelbare Einheit, bei Festgeldern der Kurs. Derzeit gibt es keine Validierung bei Käufen und Verkäufen. Es ist daher möglich, dass ein zahlungsunfähiger Gläubiger für die teilweise oder vollständige Rückzahlung andere Stückelungen wählt. Bei Anleihen hat diese Angabe daher vor allem Informationscharakter.

### Emittentenland
Das **Emittentenland** ist optional und wird von den [Simulationssteuermodellen]({{% relref "/algoalert/historicalrun/taxmodel" %}}) verwendet, beispielsweise für eine Quellensteuer, die vom Sitz des Emittenten abhängt. Es wird für jedes Wertpapier mit einem Emittenten angeboten, jedoch nicht für die Finanzinstrumente CFD, Forex und Index nicht investierbar.

Während Sie die **ISIN** erfassen, schlägt der Dialog deren erste zwei Buchstaben als Emittentenland vor. Sobald Sie selbst ein Land wählen, ändert der Vorschlag dieses nicht mehr. Bei bestehenden Wertpapieren wurde das Emittentenland beim Update auf dieselbe Weise befüllt. Prüfen Sie den Wert insbesondere bei Anleihen: Die ersten zwei Buchstaben einer ISIN bezeichnen das Land, das die ISIN vergeben hat, und nicht immer den Sitz des Emittenten. CHF-Anleihen ausländischer Emittenten an der SIX tragen beispielsweise meist eine ISIN, die mit CH beginnt.

### Handelsplatz
+ Falls ein **Handelsplatz** ohne Kursdaten gewählt wird, dann erhält der Dialog den Reiter **"Historische Kurse für Periode"**.
+ Sobald für das Wertpapier Kurse vorliegen oder eine Transaktion erfasst wurde, bietet die Auswahlliste nur noch Handelsplätze derselben Art an, siehe [Handelsplatz](../#handelsplatz).

## Reiter "Historische Kurse für Periode"
Siehe dazu [Wertpapier ohne Kursdaten](./securitywithoutpricedata).

## Reiter "Wertpapier-Split/s"
Wie im Kapitel [Historische Daten](../../../externaldata/historyquote/) erwähnt, wird ein Split von Datenquellen eingelesen. Es kann vorkommen, dass die Datenquelle den Split nicht zeitlich akkurat nachführt. Aus diesem Grund kann ein **Split** auch manuell erfasst werden. Ein **Split** der manuell erfasst wurde, wird von System nicht überschrieben oder entfernt.

Ein Split wird erfasst, indem das **Split Datum**, die **Aus Anzahl** und die **Wird Anzahl** eingegeben werden und anschliessend **Übernehmen** angeklickt wird. Der neue Eintrag erscheint danach in der Tabelle unterhalb des Formulars. Das Verhältnis *Aus Anzahl : Wird Anzahl* beschreibt den Split: Bei `2 : 1` wird aus einer Aktie deren zwei, `3 : 2` entspricht einer Zusammenlegung. Durch Auswahl einer Zeile in der Tabelle lässt sich der Eintrag bearbeiten oder löschen. Wird ein bereits vorhandenes Datum erneut erfasst, so überschreibt der Eintrag den bestehenden für dieses Datum, anstatt ihn zu verdoppeln. Die Änderungen werden erst gespeichert, wenn das ganze **Wertpapier** gespeichert wird.

{{% notice style="info" title="Maximale Anzahl Splits" %}}
Pro Instrument können standardmässig bis zu 20 Splits erfasst werden. Eine Administratorin oder ein Administrator kann diese Grenze über die Limite **Wertpapier / Split** der Limitenart **Gesamtanzahl** anpassen, siehe [Limite Informationsklasse]({{% relref "/admindata/entitylimit" %}}). Die Fusszeile der Tabelle zeigt die aktuelle Anzahl und das erlaubte Maximum an.
{{% /notice %}}

## Reiter "Anleihensimulation"
Dieser Reiter erscheint bei Anleihen und Wandelanleihen mit dem Finanzinstrument **Direktanlage**. Seine Angaben werden ausschliesslich von der [historischen Wiederholung]({{% relref "/algoalert/historicalrun" %}}) verwendet, wenn **Regelmässige Anleihencoupons erzeugen** eingeschaltet ist und für die Anleihe keine Ausschüttungen gespeichert sind. Auf Ihre echten Transaktionen haben sie keinen Einfluss; deren Marchzins stammt immer aus der Buchung selbst. Wird die Anlageklasse auf eine andere Art von Instrument geändert, werden die Angaben beim Speichern entfernt.

Die regelmässigen Coupontermine werden vom **Aktiv bis Datum** rückwärts abgeleitet, die Anzahl Coupons pro Jahr ergibt sich aus der **Ausschüttungshäufigkeit**. Der Nominalbetrag beträgt immer 100 je Einheit. Abweichende erste oder letzte Zinsperioden, Kündigungen, variable Zinssätze und Verschiebungen auf Bankarbeitstage werden nicht unterstützt.

- **Jährlicher Coupon (%)**: Der jährliche Zinssatz der Anleihe. Der Dialog schlägt die Zahl am Anfang des Namens vor, solange Sie nicht selbst einen Zinssatz erfasst haben. Ein Zinssatz von 0 beschreibt eine Nullcouponanleihe, für die keine Coupons erzeugt werden.
- **Zinstagekonvention**: Bestimmt, wie der Marchzins berechnet wird, wenn die Simulation die Anleihe zwischen zwei Couponterminen kauft oder verkauft. Die Höhe eines vollen Coupons hängt nicht davon ab. **Effektiv/effektiv ICMA** zählt die tatsächlichen Tage der Couponperiode, **30E/360 (europäisch)** zählt jeden Monat mit 30 Tagen.

Die massgebende Konvention steht im Prospekt der Anleihe. Da sie vom Anleihenmarkt abhängt, an dem die Anleihe begeben wurde, und nicht vom Sitz des Emittenten, schlägt der Dialog sie anhand der **Währung** des Wertpapiers vor: **30E/360 (europäisch)** für CHF, **Effektiv/effektiv ICMA** für alle anderen Währungen. Dies entspricht der üblichen Praxis bei CHF-Anleihen an der SIX sowie bei Staatsanleihen und den meisten Unternehmensanleihen in EUR, GBP und USD. Der Vorschlag füllt nur ein leeres Feld; eine gespeicherte oder selbst gewählte Konvention bleibt unverändert. Bleibt das Feld leer, verwendet die historische Wiederholung denselben Vorschlag. Damit erhalten auch früher gespeicherte oder durch einen Import erstellte Wertpapiere erzeugte Coupons, sobald sie einen Couponsatz haben.

{{% notice style="info" title="Kleine Abweichungen sind möglich" %}}
USD-Unternehmensanleihen verwenden meist 30/360 (US) und JPY-Anleihen Effektiv/365; beide werden nicht angeboten, stattdessen gilt **30E/360 (europäisch)** beziehungsweise **Effektiv/effektiv ICMA**. Die beiden Konventionen unterscheiden sich um höchstens ein bis zwei Zinstage, die Auswirkung auf ein Simulationsergebnis ist daher gering.
{{% /notice %}}
