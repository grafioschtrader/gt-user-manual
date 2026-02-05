---
title: "Import Transaktion Vorlage"
date: 2021-06-27T12:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
Die Importvorlage für **PDF** und **CSV** haben einige Gemeinsamkeiten. Es wird eine Angabe benötigt, wo die relevanten Werte aus dem Dokument ermittelt werden können. Dies geschieht durch die Zuordnung von **Feldern**. Zusätzlich wird eine **Konfiguration** benötigt, damit die gelesenen Werte der **Felder** korrekt interpretiert werden.

## Zuordnung Felder
Die für die Transaktion bestimmten Werte werden aus den Feldern gelesen. Die von der Transaktionsart unabhängigen **obligatorisch** Felder sind **_Fett und kursiv_** dargestellt:
- **ac**: Beim Kauf oder Verkauf einer Anleihe können Marchzinse anfallen.
- **_cac_**: **Währung des Bankkontos**, es wird ein [ISO 4217](//de.wikipedia.org/wiki/ISO_4217) Wert erwartet. Damit wird das Bankkonto bestimmt.
- **cct**: Währung der Transaktionskosten und Steuer, sollte benutzt werden falls die Kosten nicht in der Währung des Instruments anfallen.
- **cex**: Der **Wechselkurs**.
- **cin**: Die **Währung** des **Instruments**.
- **_datetime_**: **Transaktionszeit**, wobei die Zeit optional ist. Statt **datetime** kann auch die Kombination **date** und **time** verwendet werden.
  - **date**: Falls das Datum und die Zeit getrennt sind, kann dieses Feld genutzt werden. Dies ist der Teil des **Datums**.
  - **time**: Falls das Datum und die Zeit getrennt sind, kann dieses Feld genutzt werden. Dies ist Teil der **Zeit**.
- **exdiv**: Das Ex-Tag kann für Dokumente mit Bezug zu Dividende wichtig sein, siehe dazu [Zins/Dividende](../../../transaction/security/cashbased/)
- **isin**: Die ISIN ist wichtig, da damit eine ziemlich exakte Bestimmung des Instrumentes möglich ist.
- **per**: Bei Anleihen wird der **Nennwert** unter **units** aufgeführt und durch 100 geteilt falls mit das Feld **per** gesetzt ist, d.h. irgend einen Zeichen oder Zeichenkette enthält. Beispielsweise beträgt der Nennwert 10'000.00, dann erfolgt eine Teilung durch 100. Der Wert des Feldes **per** selbst wird nicht ausgewertet.  
- **quotation**: Dies ist der **Kurs**, **Dividende pro Einheit** oder der **Zins in Prozenten bei Anleihen**.
- **symbol**: Falls das Dokument keine **ISIN** aufweist, ist das **Symbol** des Instruments die schlechtere Alternative. 
- **sf1**: Für **spezielle Transaktionsimport Implementierung** ist dieses Feld reserviert. Damit wird ein Wert aus dem Dokument gelesen und einer speziellen Implementierung zugeführt.
- **_ta_**: Die **Gesamtsumme**, damit wird durch eine Kalkulation überprüft ob die anderen Angaben zu dieser Summe führen.
- **tc1**: Die Haupttransaktionskosten, d.h. die **Gebühren der Handelsplattform**.
- **tc2**: Manchmal benötigt es zwei Positionen für die Transaktionskosten.
- **reduce**: Eine Handelsplatform kann eine Vergünstigung auf den Transaktionskosten anbieten. Dieser Betrag wird von den Transaktionskosten abgezogen.
- **_transType_**: Mit dieser Angabe wird über eine **entsprechende Konfiguration** die **Transaktionsart** bestimmt.
- **tt1**: Die Hauptsteuerkosten.
- **tt2**: Manchmal benötigt es zwei Positionen für die Steuerkosten.
- **units**: Die Anzahl der Einheiten an der Transaktion und bei den Anleihen der **Nennwert**, siehe dazu **per**.

### Obligatorische Felder in Abhängigkeit der Transaktionsart
Es wird unterschieden zwischen **Kontotransaktion** und **Wertpapiertransaktion**. Hier sind nur die Anforderungen für **Wertpapiertransaktion** beschrieben.
- **Erkennung des Instrumentes**: Dies geschieht bevorzugt über den Wert des Feldes **isin** und notfalls mit dem Wert des Feldes **symbol**.
- **Wieviel zu welchem Preis**: Bei allen **Wertpapiertransaktion** sind Werte der Felder **units** und **quotation** notwendig und werden über den Wert des Feldes **ta** geprüft.

## Konfiguration
Am Ende der Importvorlage folgt die Sektion **[END]**. Dort sind die **Konfiguration** für die Verarbeitung der **Vorlage** bzw. des zu importierenten **Dokumentes** enthalten. Eine obligatorische Konfiguration wird **_Fett und kursiv_** dargestellt.
+ **ignoreTaxOnDivInt**: Diese Dividende unterliegt nicht der Steuer. Danach wird der Text wie bei **transType** erwartet. Beispielsweise wird mit der Konfiguration "ignoreTaxOnDivInt=Capital Gain" bei der **Transaktion** die Eigenschaft **Unterliegt Steuer** mit dieser Konfiguration bei Dividende oder Zins nicht gesetzt, wobei sich dies auf den Text "Capital Gain" bezieht. Die Funktionalität entspricht **NO_TAX_ON_DIVIDEND_INTEREST**, wobei sich diese Konfiguration generell auf eine Importvorlage bezieht.
+ **_transType_**: Diese ist eine Zuweisung des Textes zur einer **Transaktionsart**. Die Konfiguration besteht aus der Transaktionsart getrennt durch "|" einen oder mehrere Texte. Beispielsweise "transType=DIVIDEND|Zins,Dividende" wobei auch dies der zwei Zeilen-Konfiguration "transType=DIVIDEND|Zins" und "transType=DIVIDEND|Dividende" entspricht. Folgende Transaktionen werden in CSV und PDF Vorlagen unterstützt:
   - **ACCUMULATE**: Ist ein Kauf eines Instrumentes.
   - **DIVIDEND**: Ist für den Zins oder Dividende eines Instrumentes.
   - **REDUCE**: Ist ein Verkauf eines Instrumentes.
   
   - **transType nur für CSV gültig**: Folgende **Transaktionsarten** können zusätzlich in einer Importvorlage für **CSV** angewendet werden: 
     - **DEPOSIT**: Ist eine Auszahlung von einem Konto.
     - **FEE**: Kosten für die Kontoführung oder Depotgebühren.
     - **INTEREST**: Ist der Zins für ein Konto.
     - **WITHDRAWAL**: Ist eine Einzahlung von einem Konto.
+ **_dateFormat_**: Das Format wird für das Feld **datetime** genutzt. Zwei mögliche Beispiele sind "dd.MM.yyyy" oder "dd.MM.yyyy HH:mm". Auch das Feld **date** verwendet diese Format, dabei dürfen Zeit spezifischen Formatangaben nicht benutzt werden.
   - **yyyy** oder **yy**: Für zwei- oder vierstellige Jahreszahl.
   - **MMM** oder **MM**: Monat abgekürzt 3 Buchstaben oder als zweistellige Zahl.
   - **dd**: Tag vom Monat als zweistellige Zahl.
   - **HH**: Die Stunden als zweistellige Zahl von 0-23.
   - **hh**: Die Stunden angeben mit 1-12.
   - **mm**: Minute als zweistellige Zahl.
   - **ss**: Sekunden als zweistellige Zahl.
   - **a**: Falls das 12-Stunden-Format benutzt wird, sollte diese Angabe nicht fehlen. Damit die Zuordnung von **AM** und **PM** korrekt funktioniert.
+ **timeFormat**: Manchmal liefern die Dokumente auch eine exakte Transaktionszeit. Das Format entspricht dem für die Zeit oben genannten Format.
+ **overRuleSeparators**: Die erwarteten länderspezifischen **Zifferngruppierung** und **Dezimaltrennzeichen** des Dokumentes werden über die Einstellungen der **Land/Sprache** des Benutzer bestimmt. Es ist möglich, diese mit der Konfiguration "**Sprache-Land<Zifferngruppierung|Dezimaltrennzeichen>**" zu übersteuern. Für die **Ziffergruppierung** können **mehrere Zeichen** angegeben werden, beim **Dezimaltrennzeichen** ist nur ein Zeichen möglich. Beispielsweise wird mit der Konfiguration "**de-CH<'|.>de-DE<,|.>**" die **Land/Sprache** Einstellung der Benutzer mit "de-CH" oder "de-DE" überschrieben. Eine Konfiguration mit **ALL** setzt unabhängig des Benutzers die entsprechende Konfiguration. Beispielsweise "**All<’'|.>**" erwartet als **Zifferngruppierung** das Zeichen "’" oder "'" und als **Dezimaltrennzeichen** den Punkt.
+ **otherFlagOptions**: Angabe von zusätzlichen Konfigurationen, diese werden mit "|" getrennt.
  + **BOND_QUOTATION_CORRECTION**: Anleihen können selten zwei oder mehrere Male im Jahr Zinse bezahlen. Das zu importierende Dokument weist aber nur den jährlichen Zinssatz aus. In diesem Fall darf das System diesen Zinssatz bzw. der daraus abgeleiteten Kurs entsprechend anpassen.
  + **CASH_SECURITY_CURRENCY_MISMATCH_BUT_EXCHANGE_RATE**: Manchmal wird eine Dividende in einer anderen Währung als der Wertpapierwährung gezahlt, aber es gibt keinen Umrechnungskurs bzw. dieser wird nicht eingelesen. Das System setzt das erwartete Währungspaar und errechnet den Umrechnungskurs und passt die Dividende entsprechend der Kontowährung pro Einheit an.
  + **BASE_CURRENCY_MAYBE_INVERSE**: Die **Basiswährung** ist möglicherweise nicht die erwartete Währung des Instruments. Womöglich ist es die Basiswährung des Kontos.
  + **BOND_ADJUST_UNITS_AND_QUOTATION_WHEN_UNITS_EQUAL_ONE**: Bestimmte Handelsplattformen weisen beim CSV-Dokument für **Zins-Transaktion** bei Anleihen im Feld **units** den Wert 1 aus. **GT** will die korrekte **Anzahl**, wie beim Kauf angegeben. Diese Konfiguration erlaubt GT die Anpassung der **Anzahl** und des **Zins** über die Felder **units** und **quotation**. Diese Anpassung erst mit der Erstellung der Transaktion durchgeführt.
  + **NO_TAX_ON_DIVIDEND_INTEREST**: Bei der **Transaktion** wird die Eigenschaft **Unterliegt Steuer** mit dieser Konfiguration bei Dividende oder Zins nicht gesetzt, d.h. der Betrag ist steuerfrei. Andernfalls wird **Unterliegt Steuer** gesetzt.
  
