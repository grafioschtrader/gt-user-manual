# Allgemein
In diesem Benutzerhandbuch wird das Open-Source-Projekt „Grafioschtrader” erläutert. Dabei wird vor allem eine leicht verständliche Sprache verwendet. Einige Kapitel werden zudem durch ein YouTube-Video ergänzt. In der Applikation „Grafioschtrader” befindet sich ein Fragezeichen. Durch Klicken darauf gelangt man zur entsprechenden Seite dieses Benutzerhandbuchs. Das Deployment dieses Handbuchs erfolgt mit dem Static-Webseiten-Generator Hugo.

# Stilangaben
- Beachte die schweizerische Schreibweise und verwende das „ß” nicht.
- Bei Markdown sollten unnötige Leerzeilen vermieden werden.
- Grundsätzlich haben deutsche Texte Vorrang vor englischen.
- "Title" im Front Matter dürfen nicht geändert werden.
- Im Front Matter sollte das Datum modifiziert werden. Formel: Aktuelles Datum minus 1 Tag.
- Fliesstext wird vor der Bullet Point Schreibweise bevorzugt. Akzepiert sind Bullet Poins mit vollständigen Sätze. Beispiel: "content/watchlistinstrument/instrument".
- Zu technische Angaben sollten vermieden werden. Beispielsweise kann ein Benutzer nichts anfangen mit Bezeichnungen wie FIXED_INCOME, CURRENCY_CASH und andere. Dies sind Bezeichnungen in der Software und nicht von Benunterinterface.
- Da du die Dokumentation aus dem Code generierst, findest du möglicherweise Variablen mit dem Suffix „_MC”. Dies ist ein Hinweis, dass bei der Spaltenüberschrift die Hauptwährung ausgegeben wird.
- Beachte auch das Glossar (Verzeichnis: "content/glossar")

# Referenzierung zu Benutzertexte in Grafioschtrader
- Wenn du in der Dokumentation auf Texte aus dem Programm referenzierst, beispielsweise für Menüpunkte, Spaltenüberschriften usw., müssen diese korrekt sein. Sie können aus Ressourcen-Dateien des Frontends oder Backends entnommen werden. Dabei sollten nicht einfach übersetz werden, sondern in den Ressourcen-Dateien nachgeschaut werden.
- Die Resource-Dateien für Texte sind im Sprint Boot Backend den Dateinamen messages_de.properties, messages.properties in verschiedenen Artifakten zu finden.
- Die Resource-Dateien für Text im Angluar Frontend de.json und en.json zu finden.

# Angaben zum Theme
Dieses Benutzermanual nutzt das Hugo Relearn Theme (https://github.com/McShelby/hugo-theme-relearn).
Ich nutze im Manual oftmals Boxen für Info, Hinweise usw. Siehe https://mcshelby.github.io/hugo-theme-relearn/shortcodes/notice/index.html für deren Anwendung.

# Referenzierung Source-Code zu Benutzermanuall
Um den Text zu generieren, ist eine entsprechende Einstiegsklasse im Front- oder Backend erforderlich. Im Folgenden werden diese aufgezeigt.
- "content/reportportfolio/portfolios" Frontend: TenantSummariesCashaccountComponent
- "content/reportportfolio/periodperformance"  Frontend: PerformancePeriodComponent
- "content/reportportfolio/securityaccountreport" Frontend: TenantSummariesSecurityaccountComponent
- "content/reportportfolio/securitycashaccountreport" Frontend: TenantSummariesAssetclassComponent
- "content/reportportfolio/dividends/" Frontend: TenantDividendsComponent
- "content/reportportfolio/transactioncosts" Frontend: TenantTransactionCostComponent
- "content/reportportfolio/transactionlist" Frontend: TenantTransactionTableComponent

# Angabe zu der beschriebenen Software in GitHub
Dieses Benutzermanual beschreibt die Software Grafioschtrader (https://github.com/grafioschtrader/grafioschtrader).
Es gibt auch Verweise auf Import Vorlagengruppe (https://github.com/grafioschtrader/gt-import-transaction-template).
Es gibt auch eine Beschreibung des Programms GT-PDF-Transform(https://github.com/grafioschtrader/gt-pdf-transform).


