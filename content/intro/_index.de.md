---
title: "Einführung"
date: 2025-07-04T15:00:00+01:00
draft: false
weight: 1
archetype: "default"
---
Ein Video wird folgen...

### Warum gibt es Grafioschtrader (GT)
Einige Jahre nutzte ich eine Depotverwaltungsapplikation die meine anfänglichen Bedürfnisse genügte. Mit der Zeit erkannte ich, dass die Transaktions- und Depotkosten einen erheblichen Einfluss auf die Gesamtrendite des Portfolios haben. So suchte ich unterschiedliche Onlinebörsenbroker die für meine Investment die günstigsten Handelskonditionen boten. Bisher genügt nur eine Kombination von mehreren Handelsplattformen meinen Bedürfnissen. Als schweizer Anleger kommt hinzu, dass früher oder später Investitionen mit Fremdwährungen erfolgen.

Mit der Zeit verliert man die Übersicht wo wie viel in was investiert ist. Ich dachte dies müsse nicht sein und als Softwareentwickler stürzte ich mich in das Abenteuer Grafioschtrader. Mit **GT** habe ich nun einen Überblick über die **gehaltenen Positionen**, **Performance** und **Kontosalden** per Portfolio oder über alle Portfolios kumuliert.

### Was kann GT
+ **Die Software ist kostenlos und Open-Source**: GT ist als Open-Source freigegeben und wird auf [GitHub](//github.com/grafioschtrader) gefunden.
+ **Mandantenfähigkeit**: GT kann für eine Gruppe von Anlegern oder im Einzelmodus betrieben werden.
+ **Webapplikation**: GT ist eine Webapplikation und liefert die übersichtlichsten Ergebnisse in einem Desktop-Webbrowser.
+ **Mehrere Portfolios mit Währungskonten**: Nachbildung mehreren Portfolios mit einem oder mehreren Depots und einem oder mehreren Bankkonten.  
+ **Mehrere Währungen**: Handel von Wertpapieren in unterschiedlichen Währungen. 
+ **Handel ab Jahrtausendwende**: Grundsätzliche Unterstützung von historischen Kursdaten ab dem Jahr 2000. Hierbei ist zu beachten, dass die Beschaffung von Kursdaten von nicht gehandelten Wertpapieren möglicherweise ein Problem darstellt.
+ **Unterschiedliche Finanzinstrumente**: Aktien, Anleihen, ETF, Wertpapiere ohne Kursdaten, Short ETF, CFD, Forex.
+ **Import von Transaktionen**: Ein Import von einzelnen oder mehreren **PDFs** mit Wertpapier-Transaktionen. Über **CSV**-Datei können auch Kontotransaktionen geladen werden.
+ **Auswertungen über Anlageklassen**: Auwertungen nach den gängigen Anlageklassen wie Aktien, Anleihen, Immobilien, Commodities, usw.

### Für wen eignet sich GT
GT ist der Versuch die **Nachbildung der Portfolios** gemäss der **Realität**, daher gibt es Kontos wie bei Ihrem Onlinebörsenbroker. **Jede Buchung** ist mit einem **Geldkonto** verbunden, somit werden die Buchungen gemäss Ihrer Handelsplattform nachgebildet.
+ Der Investor der seine Finanzdaten nicht einer intransparent Platform anvertrauen will. Sie können **GT selber hosten**, dazu genügt ein [Raspberry Pi 4](//www.raspberrypi.org/products/raspberry-pi-4-model-b/) mit 4GB (möglicherweise auch 2GB) Arbeitsspeicher.
+ Ein Shareholder der eine **Historie** über alle seine **Trades** haben will. Sie können alle geschlossenen Positionen jederzeit einsehen.
+ Privatanleger der wissen will, wie sich seine **Währungsgewinne** entwickeln. Es wird laufend der Währungsgewinn auf den Fremdwährungskonten ausgewiesen.
+ **Investoren-Club** der eine eigene GT-Instanz **hosten** will und sich ein Mitglied gerne um die technischen Aspekte kümmert.
+ Anleger mit **mehreren Portfolios** bei unterschiedlichen Handelsplattformen. GT liefert Informationen pro Portfolio und kann diese aggregiert darstellen.

### Für wen ist GT nicht gedacht
Bevor Sie ihre Lebenszeit unnötig mit GT vergeuden sollten Sie folgendes in Betrachtung ziehen:
+ Dem Daytrader fehlen die **Echtzeitkurse**.
+ Investoren die ihren Handel losgelöst von Geldkontos verwalten möchten.
+ Anleger der nur ein Portfolio bei einem Online-Broker hat und die dortigen Auswertungen genügen.
+ Es gibt Investoren die Portfolios nach bestimmten Kriterien wie beispielsweise ein Dividenden-Portfolio erstellen. GT unterstützt eine solches Vorgehen nicht. Im Gegenteil, die aggregierte Auswertung über reale Portfolios bei unterschiedlichen Handelsplattformen ist das Ziel.
+ In GT wird keine Unterstützung für **angebliche reale öffentliche Portfolios** von Benutzer geben. Den Entwickler von GT entzieht sich die Sinnhaftigkeit dieser möglichen Fake-Portfolios. Als wichtigstes, ein Portfolio ist individuell und hoffentlich stark beeinflusst von den Lebensumstände des entsprechenden Investors. Die Reduzierung dieses Ganzes auf ein paar Zahlen und Grafiken ist nichts anderes als **Desinformation**.

### Für wen GT nicht funktioniert
GT ist definitiv keine Eierlegende Wollmilchsau:
+ Für Investoren die Transaktion mit GT **vor** dem **Jahre 2000** verwalten wollen. GT implementiert konsequent zweistellige Jahreszahlen.
+ In GT ist die Kombination der **ISIN** und der **Währung** eines Instrumentes **einzigartig**. Es kann also nicht der gleiche ETF in EUR beispielsweise für den Handelsplatz Frankfurt und Stuttgart erfasst werden.
+ Für Kapitalanleger die gleichzeitig ein Instrumente einer **Branche/Thema** und **Länder/Regionen** zuordnen wollen.
+ Für Anleger aus der Schweiz, Deutschland bzw. USA die eine **Valoren-Nr.**, **Wertpapierkennnummer (WKN)** bzw. **CUSIP-Nummer** benötigen. GT benutzt die Internationale Wertpapierkennnummer **ISIN** und die nicht immer eindeutigen Ticker/Symbole.

{{% notice info %}}
Die Informationen dieser Hilfe bezieht sich auf die **GT** Version **0.33.7**.
{{% /notice %}}