---
title: "Importvorlage für PDF"
date: 2025-05-04T12:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
Eine **Importvorlage** für **PDF** ist eine Definition für eine einzelne Wertpapier Transaktion pro Dokument. Eine Importvorlage für ein PDF besteht aus einem in Text umgewandelten PDF Dokument der Handelsplattform. Danach werden die für die Transaktion relevanten Werte durch Variablen mit Ankerpunkten ersetzt. Ein Feld mit Ankerpunkte wird mit "**{}**" umschlossen, die Ankerpunkt sind mit einem "**|**" vom Feldnamen und unter sich getrennt. Ein Beispiel für eine gültiges Feld mit Ankerpunkten, auch **Feldposition** genannt, könnte wie folgt aussehen **{datetime|P|N}**.

{{% notice note %}}
GT folgt mit diesen **Importvorlagen für PDF** einem **deklarativen** statt einem imperativen **Weg** für die Abarbeitung der PDF-Dokumente der unterschiedlichen Handelsplattformen. Damit können viele Updates von GT vermieden werden. Zudem wird sich kaum ein ambitionierter Software-Entwickler freiwillig für die Programmierung dieser Vielzahl von Mustererkennungen melden. Diese Arbeit ist mehrheitlich einfältig und widerspricht völlig dem Gedanken der Generalisierung. GT kreiert keinen Bullshit Job, welcher für den Benutzer nützlich aber für den Betreiber einer GT-Instanz und dem Software-Entwickler nur unnötige Arbeit aufbürdet.
{{% /notice %}}

## Ankerpunkte
Die für die Transaktion relevanten Werte sind über das gesamte PDF-Dokument verteilt. Jedes dieser Transaktionsdokumente hat eine feste Struktur die sich für gleiche Art von Transaktion nur geringfügig ändert. Diese gleichartige Struktur kann GT mit Ankerpunkten für die Erkennung von Werten benutzen. Dabei **referenziert** der **Ankerpunkt** einen **statischen Textteil** der sich pro Dokument nicht ändert.
+ Der Wert kann aus einer Dokumentenzeile erkannt werden. Falls die Struktur in einer Zeile sich kaum ändert, können die folgenden Ankerpunkte verwendet werden.
    - **P**: Vorhergehendes Wort. Funktioniert auch wenn sich der Wert am Beginn einer Zeile befindet. Dieser Ankerpunkt kann auf einen Regulären Ausdruck verweisen.
    - **Pc**: Der Wert hat ein Präfix, d.h. zwischen dem Wert und der Präfix hat es kein Leerzeichen.
    - **N**: Nachfolgendes Wort. Funktioniert auch wenn sich der Wert am Ende einer Zeile befindet. Dieser Ankerpunkt kann auf einen Regulären Ausdruck verweisen.
    - **Nc**: Der Wert hat ein Suffix, d.h. zwischen dem Wert und der Suffix hat es kein Leerzeichen.
    - **SL**: Das erste Wort derselben Zeile.
+ **PL** und **NL** kommen zur Anwendung, falls sich der Ankerpunkt nicht auf einen statischen Textteil innerhalb der zu verarbeitende Zeile fixieren lässt. Möglicherweise ist der Zeilenanfang der vorhergehenden oder nachfolgende Zeile statischer Text.
    - **PL**: Das erste Wort des Zeilenanfanges der vorhergehenden Zeile.
    - **NL**: Das erste Wort des Zeilenanfanges der nachfolgenden Zeile.

Die Anzahl der Wörter und Zahlen in der Zeile des Ankerpunktes muss übereinstimmen, wenn **SL**, **PL** und **NL** als einziger Ankerpunkt gesetzt werden.

### Auslesbarer Bereich
Der **auslesbare Bereich** definiert den zusammenhängenden Bereich von Zeilen des Dokuments wo die **relevanten Werte** liegen und welche durch **Ankerpunkte referenziert** werden. Normalerweise sind die paar ersten und letzten Zeile nicht im auslesbaren Bereich enthalten, daher dürfen diese sowohl in der Importvorlage wie auch im transformierten PDF-Dokument fehlen.

### Ankerpunkte referenziert Menge von statischen Wörter am Zeilenanfang
Manchmal möchte man mit einem Ankerpunkt zwei Varianten eines statischen Wortes referenzieren. Diese Flexibilität ist für den Zeilenanfang **derselben**, **vorhergehenden** und **nachfolgenden Zeile** gegeben. Damit funktioniert dieses Konstrukt für **PL**, **SL** und **NL**. Die möglichen Wörter der Zeilenanfänge werden mit Klammer umschlossen und mit "|" **[w1|w2|w...]** getrennt. Im folgenden Beispiel kann der Zeilenanfang von Feld **tt1** mit "Quellensteuer" oder "Verrechnungssteuer" beginnen:
{{< highlight markdown >}}
[Quellensteuer|Verrechnungssteuer] 35% (CH) CHF {tt1|SL|N|O}
{{< / highlight >}}

### Mehr Varianz bei vorhergehenden und nachfolgenden Wort
 Für das vorhergehende und nachfolgende Wort kann der Ankerpunkt eine grosse Flexibilität aufweisen. Wobei die Konfiguration ziemlich Anspruchsvoll sein kann, da diese den Einsatz von **Regulärer Ausdrücke** erlaubt. Bei Ankerpunkten **P** und **N** kann **(?:)** angewendet werden. Dieser erlaubt die Anwendung von einem Regulären Ausdruck, der nicht als Gruppe verarbeitet wird. Es ist ein sehr mächtiges Konstrukt, was uns eine gewisse Varianz für das vorhergehende und nachfolgende Wort erlaubt. Dazu zwei Beispiele:
{{< highlight markdown >}}
BETRAG, DEN WIR DEM KONTO (?:GUTSCHREIBEN|BELASTEN) {cac|P|SL} {ta|SL|N}
{units|P|NL} Vanguard FTSE Japan ETF {quotation|NL|N} (?:[A-Z]{3}$)
{{< / highlight >}}
Die erste Zeile erwartet beim Feld **Währung des Bankkontos (cac)** als vorhergehendes Wort entweder "GUTSCHREIBEN" oder "BELASTEN". Die zweite Zeile ist komplizierter und besagt, das nach dem Feld **quotation** eine Zeichenkette bestehend aus drei Grossbuchstaben folgen muss und am Ende der Zeile steht.

## Feldposition Konfiguration
Machmal muss eine Feldposition zusätzlich Konfiguriert werden, diese werden wie die Ankerpunkte in der Feldposition ausgedrückt.
- **O**: Es gibt optionale Werte wie Steuerkosten, die nur in bestimmten Dokumenten vorkommen. Mit der Konfiguration "**O**" lässt sich dies konfigurieren.
- **R**: Der Inhalt dieser Zeile kann sich mehrfach wiederholen. Diese Konfiguration ist hilfreich bei Verkauf und Kauf von einem Wertpapier, da manchmal die Transaktion über mehrere Börsentransaktionen ausgeführt wird. Die Konfiguration muss in der ersten Feldposition stehen. Folgende Wiederholungen kann nicht bewältigt werden, da sich die Anzahl der Spalten in den zwei Zeilen unterscheidet:
```
17 ZUM KURS VON 136,5
53 VON 136,5
```

## Beispiel einer PDF Vorlage
In folgenden ist ein solche Vorlage für den Kauf und Verkauf von Wertpapieren aufgeführt.
{{< highlight markdown "linenos=true, hl_lines=1 2 5 8 10-13" >}}
Gland, {datetime|P|N}
(?:Börsengeschäft:|Börsentransaktion:) {transType|P|N} Unsere Referenz: 12121212 
Gemäss Ihrem Kaufauftrag vom 12.12.2012 haben wir folgende Transaktionen vorgenommen:
Titel Ort der Ausführung
ISHARES $ CORP BND ISIN: {isin|P} SIX Swiss Exchange
NKN: 1613957
Anzahl Preis Betrag
{units|PL|R} {quotation} {cac} 8’000.00
Total USD 8’250.00
Kommission Swissquote Bank AG USD {tc1|SL|N|O}
[Abgabe (Eidg. Stempelsteuer)|Eidgenössische Stempelsteuer] USD {tt1|SL|N}
Börsengebühren USD {tc2|SL|N}
Zu Ihren Lasten USD {ta|SL|N}
[END]
transType=ACCUMULATE|Kauf
transType=REDUCE|Verkauf
dateFormat=dd.MM.yyyy
overRuleThousandSeparators= '’
{{< / highlight >}}
- _Zeile 1_: Das **{datetime|P|N}** bezieht sich auf das Transaktionsdatum, als Ankerpunkt wurde das vorhergehende und nachfolgende Wort gewählt. In diesem Fall bezieht sich das **P** auf "**Gland,**" und **N** auf das **Zeilenende**.
- _Zeile 2_: Diese Zeile enthält einen [Regulären Ausdruck](//de.wikipedia.org/wiki/Regul%C3%A4rer_Ausdruck#), damit dies funktioniert muss der Ausdruck als Rückwärtsreferenz **(?:…)** definiert werden, andernfalls kann die Gruppierung von GT nicht mehr korrekt funktionieren.
- _Zeile 5_: Aus dieser Zeile wird die **ISIN** gelesen, normalerweise sollten mindestens zwei Ankerpunkte gesetzt werden. In diesem Fall ist **{isin|P}** über das gesamte Dokument eindeutig und ein Ankerpunkt genügt. Mit **P** wird hier der statische Text "**ISIN:**" referenziert.
- _Zeile 8_: **{units|PL|R} {quotation} {cac}** Diese Zeile kann sich wiederholen. Die Konfiguration **R** muss in der ersten Feldposition stehen.
- _Zeile 10_: Diese Feldposition der Hauptsteuerkosten **{tc1|SL|N|O}** ist optional. Die Ankerpunkte beziehen sich auf das Wort "**Kommission**" und dem **Zeilenende**.