# Allgemein
In diesem Benutzerhandbuch wird das Open-Source-Projekt �Grafioschtrader� erl�utert. Dabei wird vor allem eine leicht verst�ndliche Sprache verwendet. Einige Kapitel werden zudem durch ein YouTube-Video erg�nzt. In der Applikation �Grafioschtrader� befindet sich ein Fragezeichen. Durch Klicken darauf gelangt man zur entsprechenden Seite dieses Benutzerhandbuchs. Das Deployment dieses Handbuchs erfolgt mit dem Static-Webseiten-Generator Hugo.

# Stilangaben
- Beachte die schweizerische Schreibweise und verwende das �ߔ nicht.
- Bei Markdown sollten unn�tige Leerzeilen vermieden werden.
- Grunds�tzlich haben deutsche Texte Vorrang vor englischen.
- "Title" im Front Matter d�rfen nicht ge�ndert werden.
- Im Front Matter sollte das Datum modifiziert werden. Formel: Aktuelles Datum minus 1 Tag.
- Fliesstext wird vor der Bullet Point Schreibweise bevorzugt. Akzepiert sind Bullet Poins mit vollst�ndigen S�tze. Beispiel: "content/watchlistinstrument/instrument".
- Zu technische Angaben sollten vermieden werden. Beispielsweise kann ein Benutzer nichts anfangen mit Bezeichnungen wie FIXED_INCOME, CURRENCY_CASH und andere. Dies sind Bezeichnungen in der Software und nicht von Benunterinterface.
- Im Benutzerhandbuch dürfen weder JPA-Eigenschaftsnamen (z.B. «gtNetHistoricalRecv», «retryHistoryLoad») noch Datenbank-Spaltennamen (z.B. «retry_history_load», «retry_intra_load») erscheinen. Diese Bezeichnungen sind dem Benutzer unbekannt. Stattdessen ist immer der tatsächlich im Benutzerinterface angezeigte Text zu verwenden, der in den unter «Referenzierung zu Benutzertexte in Grafioschtrader» aufgeführten Ressourcen-Dateien steht. Beispiele: das Datenbankfeld «retry_history_load» heisst im UI «Wiederholungszähler historisch» bzw. «Historical retries counter»; die JPA-Eigenschaft «gtNetHistoricalRecv» entspricht der Option «Erhalte historische Preisdaten» bzw. «Receive historical price data». Globalparameter-Namen mit Punkten (z.B. «gt.history.retry», «gt.gtnet.quote.retry») dürfen verwendet werden, da Administratoren diese in der Tabelle der globalen Einstellungen genau so sehen.
- Da du die Dokumentation aus dem Code generierst, findest du m�glicherweise Variablen mit dem Suffix �_MC�. Dies ist ein Hinweis, dass bei der Spalten�berschrift die Hauptw�hrung ausgegeben wird.
- Beachte auch das Glossar (Verzeichnis: "content/glossar")

# Referenzierung zu Benutzertexte in Grafioschtrader
- Wenn du in der Dokumentation auf Texte aus dem Programm referenzierst, beispielsweise f�r Men�punkte, Spalten�berschriften usw., m�ssen diese korrekt sein. Sie k�nnen aus Ressourcen-Dateien des Frontends oder Backends entnommen werden. Dabei sollten nicht einfach �bersetz werden, sondern in den Ressourcen-Dateien nachgeschaut werden.
- Die Resource-Dateien f�r Texte sind im Sprint Boot Backend den Dateinamen messages_de.properties, messages.properties in verschiedenen Artifakten zu finden.
- Im Angular-Frontend gibt es keine Dateien de.json und en.json mehr. Das Frontend lädt sämtliche Texte über den Endpunkt /api/globalparameters/properties/ aus den oben genannten Backend-Dateien. Ein Schlüssel darf dort klein und mit Punkten geschrieben sein: simulation.start.date erreicht das Frontend als SIMULATION_START_DATE. Massgebend ist immer der Wert in messages_de.properties beziehungsweise messages.properties.

# Angaben zum Theme
Dieses Benutzermanual nutzt das Hugo Relearn Theme (https://github.com/McShelby/hugo-theme-relearn).
Ich nutze im Manual oftmals Boxen f�r Info, Hinweise usw. Siehe https://mcshelby.github.io/hugo-theme-relearn/shortcodes/notice/index.html f�r deren Anwendung.

# Referenzierung Source-Code zu Benutzermanuall
Um den Text zu generieren, ist eine entsprechende Einstiegsklasse im Front- oder Backend erforderlich. Im Folgenden werden diese aufgezeigt.
- "content/reportportfolio/portfolios" Frontend: TenantSummariesCashaccountComponent
- "content/reportportfolio/periodperformance"  Frontend: PerformancePeriodComponent
- "content/reportportfolio/periodperformance/pdfreport" Frontend: PerformanceReportDialogComponent, PerformanceReportDialogService; Backend: PerformanceReportPdfService, section renderers in grafioschtrader.report.pdf
- "content/reportportfolio/securityaccountreport" Frontend: TenantSummariesSecurityaccountComponent
- "content/reportportfolio/securitycashaccountreport" Frontend: TenantSummariesAssetclassComponent
- "content/reportportfolio/dividends/" Frontend: TenantDividendsComponent
- "content/reportportfolio/transactioncosts" Frontend: TenantTransactionCostTabMenuComponent, TenantTransactionCostComponent, FeeModelComparisonComponent
- "content/reportportfolio/transactionlist" Frontend: TenantTransactionTableComponent
- "content/basedata/bankruptsecurity" Frontend: BankruptSecurityTableComponent, BankruptSecurityEditComponent
- "content/intro/dashboard" Frontend: DashboardComponent
- "content/intro/dashboard/widgets" Frontend: DashboardSummaryComponent, HoldingMoversWidgetComponent, LastSessionsPerformanceWidgetComponent
- "content/intro/dashboard/adminwidgets" Frontend: DashboardSummaryComponent
- "content/algoalert/historicalrun" Frontend: AlgoSimulationRunComponent, AlgoSimulationRunTableComponent; Backend: AlgoHistoricalReplayService, AlgoReplayMetrics
- "content/algoalert/algo" Frontend: AlgoTopDataViewComponent, AlgoCreateFromPortfolioDynamicComponent, AlgoSimulationCreateDynamicComponent; Backend: SimulationTenantService, AlgoHistoricalValuationService
- "content/admindata/taxdata" Frontend: TaxDataTreetableComponent
- "content/algoalert/historicalrun/taxmodel" Frontend: TaxModelEditComponent; Backend: TaxEvalExEstimator

# Angabe zu der beschriebenen Software in GitHub
Dieses Benutzermanual beschreibt die Software Grafioschtrader (https://github.com/grafioschtrader/grafioschtrader).
Es gibt auch Verweise auf Import Vorlagengruppe (https://github.com/grafioschtrader/gt-import-transaction-template).
Es gibt auch eine Beschreibung des Programms GT-PDF-Transform(https://github.com/grafioschtrader/gt-pdf-transform).


