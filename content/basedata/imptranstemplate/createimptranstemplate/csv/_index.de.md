---
title: "Importvorlage für CSV"
date: 2026-07-15T12:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
Eine **Importvorlage** für eine CSV-Datei macht die Verbindung zwischen den **Tabellenspalten** der CSV-Datei und den für GT verlangten **Feldern**. Zusätzlich enthält diese auch CSV-Dokument spezifische **Konfigurationen**. Eine CSV-Datei kann gegenüber dem PDF-Dokument **mehrere Transaktionen** enthalten. Pro Tabellenzeile wird maximal eine Transaktion in GT erzeugt. Gewisse Transaktionen gehen über mehrere Zeilen, beispielsweise ein Teilverkauf von einem Wertpapier. Auf Grund der Importvorlage muss der Benutzer keine Anpassungen an den Spalten der CSV-Datei vornehmen.
{{% notice note %}}
**Warum ein allgemeiner CSV-Importer nicht die Lösung ist?** Ähnliche Tools wie GT verfügen nur über einen CSV-Importer, d.h. der Benutzer muss eine CSV-Datei gemäss der Vorgabe dieses Tools erstellen. Bei GT nehmen wir Abstand von dieser fehleranfälligen und unnötigen Arbeit in die der Benutzer dadurch gezwungen wird. GT ist die Vermeidung von Bullshit Jobs und somit gibt es die Lösung mit diesen **CSV-Importvorlagen**.
{{% /notice %}}

## Aufbau der Vorlage
Wie die PDF-Vorlage besteht eine CSV-Importvorlage aus zwei Abschnitten, welche durch die Zeile **[END]** getrennt sind. Vor **[END]** steht pro auszulesendem Wert eine Zuordnungszeile der Form "**feld=Spaltenüberschrift**". Links steht der Feldname von GT, rechts die **exakte Spaltenüberschrift** aus der Kopfzeile der CSV-Datei. Nach **[END]** folgt die **Konfiguration**. Die allgemeinen Felder wie **datetime**, **isin** oder **units** sowie die allgemeinen Konfigurationen wie **transType**, **dateFormat** oder **overRuleSeparators** sind im übergeordneten Kapitel [Import Transaktion Vorlage](../) beschrieben und werden hier nicht wiederholt.

Beim Verarbeiten einer CSV-Datei vergleicht GT deren Kopfzeile mit den Zuordnungszeilen der Vorlage. Dabei gilt:
- Die Spaltenüberschrift muss exakt übereinstimmen, lediglich führende und nachfolgende Leerzeichen werden ignoriert.
- Jede in der Vorlage aufgeführte Spalte muss in der CSV-Datei vorhanden sein, andernfalls kann die Vorlage nicht angewendet werden.
- Spalten der CSV-Datei ohne Zuordnung werden übergangen, ebenso spielt die Reihenfolge der Spalten keine Rolle.

Da eine Vorlagengruppe mehrere CSV-Vorlagen enthalten kann, wählt der Benutzer beim Hochladen der CSV-Datei die passende Vorlage über deren **templateId** aus.

## Zuordnung Felder
Nebst den allgemeinen Feldern gibt es noch zusätzliche Felder die nur sinnvoll von CSV-Dokumenten unterstützt werden können.
- **order**: Dies Feld ist für die Verknüpfung von mehreren Tabellenzeilen vorgesehen oder für den übergreifenden Import mehrere unterschiedliche CSV-Dokumente. Manchmal müssen zwei unterschiedlicher CSV-Dokumente eingelesen werden, damit dies in GT als Transaktion abgebildet werden kann.
- **sn**: Dieses Feld enthält den Namen des Instruments. Zurzeit wird er für die Erkennung des Instruments nicht genutzt.

## Konfiguration
Nebst den allgemeinen Konfiguration werden auf Grund der Dokumentenart noch zusätzliche Konfigurationen benötigt:
- **bond**: Möglicherweise enthält das CSV-Dokument Anleihen mit einen **Kurswert**. Dieser Kurswert könnte mit ein "%" gekennzeichnet sein. Daher verlangt diese Konfiguration den Feldnamen und die entsprechende Kennzeichnung. Beispielsweise mit "quotation|%" wird erwartet, dass die Werte des Feld "quotation" mit einem "%" ergänzt sind.
- **_delimiterField_**: Das Feld-Trennzeichen, welches zur Trennung von Datenfeldern (Tabellenspalten) innerhalb der Datensätze (Zeile) benutzt wird.
- **_templateId_**: Diese unterstützt die eindeutige Zuordnung von Importvorlage zum CSV-Dokument. Sie muss innerhalb der Vorlagengruppe eindeutig sein und wird beim Hochladen des CSV-Dokuments zur Selektion angeboten.
- **ignoreLineByFieldValue**: Es ist möglich auf Grund eines **Feldwertes** bestimmte Datenzeilen beim Import zu übergehen. Die Konfiguration **Feld||Zeichenkette** enthält das **Feld** und die entsprechende **Zeichenkette** als Ausschluss der Zeile für den Import. Für die Zeichenkette können auch **reguläre Ausdrücke** verwendet werden, wobei der Ausdruck den **gesamten Feldwert** abdecken muss. Ausnahmsweise wird die Zeichenkette "**||**" als Trenner benutzt. Beispielsweise ignoriert die Konfiguration "ignoreLineByFieldValue=ta||-" alle Zeilen die im Feld "ta" genau ein "-" enthalten.

## Beispiel einer CSV-Vorlage
Die folgende Importvorlage verarbeitet den **Transaktionsexport** von **E-Trading** der Handelsplattform **Postfinance**. Sie deckt mit einer einzigen Vorlage sowohl Wertpapiertransaktionen wie auch Kontotransaktionen ab.
{{< highlight markdown "linenos=true, hl_lines=2 3 13 14 15 16 29 32" >}}
datetime=Datum
order=Auftrag #
transType=Transaktionen
symbol=Symbol
sn=Name
isin=ISIN
units=Anzahl
quotation=Stückpreis
tc1=Kosten
ac=Aufgelaufene Zinsen
ta=Nettobetrag in der Währung des Kontos
cac=Währung
[END]
templatePurpose=Transaktions Export
templateId=1
transType=WITHDRAWAL|Auszahlung,Forex-Belastung
transType=WITHDRAWAL|Fx-Belastung Comp.
transType=DEPOSIT|Vergütung
transType=DEPOSIT|Forex-Gutschrift
transType=DEPOSIT|Fx-Gutschrift Comp.
transType=INTEREST_CASHACCOUNT|Zins
transType=FEE|Jahresgebühr
transType=ACCUMULATE|Kauf
transType=REDUCE|Verkauf
transType=REDUCE|Rückzahlung
transType=DIVIDEND|Dividende
transType=DIVIDEND|Coupon
dateFormat=dd.MM.yyyy HH:mm
delimiterField=;
bond=quotation|%
overRuleSeparators=All<’'|.>
ignoreLineByFieldValue=ta||-
otherFlagOptions=BOND_ADJUST_UNITS_AND_QUOTATION_WHEN_UNITS_EQUAL_ONE
{{< / highlight >}}
- _Zeilen 1-12_: Jede Zeile ordnet einem GT-Feld die exakte Spaltenüberschrift der CSV-Datei zu. Beispielsweise wird mit "units=Anzahl" der Wert des Feldes **units** aus der Spalte "Anzahl" gelesen.
- _Zeile 2_: Die Spalte "Auftrag #" liefert das Feld **order**. Damit werden mehrere Zeilen desselben Auftrags, beispielsweise Teilausführungen eines Kaufs, zu einer Transaktion zusammengeführt.
- _Zeile 3_: Der Text der Spalte "Transaktionen" bestimmt über das Feld **transType** die Transaktionsart. Die Zuordnung der möglichen Texte erfolgt in den _Zeilen 16-27_ der Konfiguration.
- _Zeile 14_: Die Zeile "templatePurpose=" wird beim **Export** einer Importvorlage automatisch eingefügt und füllt beim **Import** die Eigenschaft **Zweck der Vorlage** ab. Beim manuellen Erstellen einer Vorlage muss sie nicht erfasst werden.
- _Zeile 15_: Die obligatorische **templateId** dieser Vorlage. Beim Hochladen eines CSV-Dokuments wird die Vorlage über diese Nummer ausgewählt.
- _Zeilen 16-27_: Die Texte der Spalte "Transaktionen" werden den Transaktionsarten zugeordnet. _Zeile 16_ zeigt die Komma-Liste, sowohl "Auszahlung" wie auch "Forex-Belastung" ergeben eine Auszahlung (WITHDRAWAL). Pro Transaktionsart sind mehrere Zeilen erlaubt, beispielsweise führen "Verkauf" und "Rückzahlung" beide zu einem Verkauf (REDUCE). Die kontobezogenen Transaktionsarten WITHDRAWAL, DEPOSIT, INTEREST_CASHACCOUNT und FEE sind nur in CSV-Vorlagen möglich.
- _Zeile 28_: Das Datumsformat enthält auch die Uhrzeit, da die Spalte "Datum" beides liefert.
- _Zeile 29_: Das obligatorische Feld-Trennzeichen, in diesem Export das Semikolon.
- _Zeile 30_: Bei Anleihen kann der Wert der Spalte "Stückpreis" mit einem "%" ergänzt sein.
- _Zeile 31_: Unabhängig von der **Land/Sprache** Einstellung des Benutzers wird als Zifferngruppierung "’" oder "'" und als Dezimaltrennzeichen der Punkt erwartet.
- _Zeile 32_: Zeilen, deren Spalte "Nettobetrag in der Währung des Kontos" nur ein "-" enthält, werden übergangen. Postfinance kennzeichnet so Buchungen ohne Wirkung auf das Konto.
- _Zeile 33_: Diese Option ist im übergeordneten Kapitel [Import Transaktion Vorlage](../) beschrieben.

### Ausschnitt einer passenden CSV-Datei
Der folgende **erfundene** Ausschnitt zeigt eine Kopfzeile und drei Datenzeilen, wie sie diese Vorlage erwartet:
{{< highlight markdown >}}
Datum;Auftrag #;Transaktionen;Symbol;Name;ISIN;Anzahl;Stückpreis;Kosten;Aufgelaufene Zinsen;Nettobetrag;Nettobetrag in der Währung des Kontos;Saldo;Währung
02.05.2023 09:31;39230910;Kauf;VOO;Vanguard S&P 500 ETF;US9229083632;12;370.85;25.30;;-4'475.50;-4'475.50;12'510.35;USD
15.06.2023 00:00;;Dividende;VOO;Vanguard S&P 500 ETF;US9229083632;12;1.4874;;;17.85;17.85;12'528.20;USD
30.06.2023 00:00;;Titeleingang;VOO;Vanguard S&P 500 ETF;US9229083632;5;;;;-;-;12'528.20;USD
{{< / highlight >}}
Die erste Datenzeile ergibt über den Text "Kauf" der Spalte "Transaktionen" einen Kauf (ACCUMULATE), wobei GT die Rechnung 12 × 370.85 plus Kosten von 25.30 gegen den Wert 4'475.50 der Spalte "Nettobetrag in der Währung des Kontos" prüft. Die Spalten "Nettobetrag" und "Saldo" sind in der Vorlage nicht zugeordnet und werden ignoriert. Die dritte Zeile wird wegen der Konfiguration "ignoreLineByFieldValue=ta||-" übergangen, da die Spalte "Nettobetrag in der Währung des Kontos" nur ein "-" enthält.
