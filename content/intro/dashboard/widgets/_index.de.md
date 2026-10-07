---
title: "Widgets"
date: 2026-10-06T15:00:00+01:00
draft: false
weight: 10
archetype: "default"
---
Diese Seite beschreibt die Widgets, die jeder angemeldete Benutzer auf dem [Dashboard]({{% relref "/intro/dashboard" %}}) sehen kann. Die zusätzlichen Karten der Rolle **Administrator** stehen unter [Widgets für Administratoren]({{% relref "/intro/dashboard/adminwidgets" %}}). Die Anlagekarten **Grösste Gewinner**, **Grösste Verlierer**, **Letzte Handelstage** und **Entwicklung des Gesamtwerts** erscheinen erst, wenn ein Klient eingerichtet ist.

Jede Karte hat gespeicherte Einstellungen, die Sie im Entwurfsmodus über **Konfigurieren** ändern. Manche Karten haben zusätzlich eine Einstellung, die nur für das aktuelle Lesen gilt und nicht gespeichert wird.

## Ungelesene Nachrichten
Die Karte zeigt ungelesene Nachrichten aus Ihrem Posteingang. Das Öffnen des Dashboards markiert keine Nachricht als gelesen. Die gespeicherte Einstellung **Maximale Zeilenanzahl** begrenzt die Tabelle, voreingestellt auf fünf Zeilen und zulässig von 1 bis 20. Die Überschrift nennt die Gesamtzahl ungelesener Nachrichten, auch wenn die Tabelle kürzer ist.

Die Tabelle enthält **Spitzname**, **Betreff** und **Erstellungszeit**. Fehlen Einträge, erscheint **Keine offenen Einträge.** **Vollständige Ansicht öffnen** führt zum [Nachrichtensystem]({{% relref "/admindata" %}}), wo Sie lesen, antworten und den Gelesen-Status setzen.

## Offene Datenänderungsanträge
Die Karte zeigt offene [Datenänderungswünsche]({{% relref "/basedata" %}}). Sie hat zwei Listen, **Für Sie** und **Von Ihnen eingereicht**. Ein Antrag kann in beiden Listen erscheinen, etwa wenn Sie selbst Eigentümer der betroffenen Entität sind und den Wunsch gestellt haben. Die **Maximale Zeilenanzahl** gilt für jede Liste getrennt, ebenfalls von 1 bis 20, voreingestellt fünf.

Die Tabellen enthalten **Informationsobjekt**, **Bemerkung Datenänderung** und **Erstellungszeit**. **Vollständige Ansicht öffnen** führt zur Registerkarte **Datenänderungswunsch für Sie** bzw. **Ihre Datenänderungswünsche**.

Welche Anträge unter **Für Sie** stehen, folgt denselben Rechten wie die vollständige Ansicht. Benutzer der Rollen **Benutzer mit Limits** und **Benutzer ohne Limits** sehen die offenen Wünsche zu Entitäten, deren Eigentümer sie sind. Benutzer der Rollen **Privilegierter Benutzer** und **Administrator** sehen alle offenen Datenänderungswünsche der Instanz, weil sie geteilte Daten unabhängig vom Eigentümer bearbeiten dürfen.

## Grösste Gewinner und Grösste Verlierer
Die beiden Karten teilen sich dieselbe Rangliste gehaltener Instrumente und zeigen entgegengesetzte Enden. **Grösste Gewinner** listet die Instrumente mit der stärksten positiven Bewegung, **Grösste Verlierer** jene mit der stärksten negativen. Eine Bewegung von genau null erscheint in keiner der beiden Karten. Die Beträge stehen in der Währung des Mandanten.

Jede Karte hat drei Perioden: **Intraday** für den laufenden Handel, **Letzter Handelstag** und **Gewählter Tag**. Jede Periode ist zweimal gereiht, einmal **Nach Veränderung** als prozentuale Kursbewegung und einmal **Nach Wert der Veränderung** als Wirkung auf den Mandanten. Die Periodenknoten selbst haben keine Summe; addierte Ranglistenzeilen wären keine Periodenkennzahl.

Die gespeicherte Einstellung **Instrumente pro Periode** begrenzt, wie viele Instrumente je Reihung und Periode erscheinen, voreingestellt drei und zulässig von 1 bis 5. Den **gewählten Handelstag** stellen Sie auf der Karte selbst ein. Dieser Tag wird nicht gespeichert; **Aktualisieren** kehrt zur Vorgabe zurück, dem Handelstag vor dem letzten abgeschlossenen. Ein Datum, das kein Handelstag ist oder nach dem letzten abgeschlossenen Handelstag liegt, wird auf den nächstliegenden gültigen Handelstag zurückgesetzt. Welche Perioden Sie aufgeklappt lassen, merkt sich dieser Webbrowser je Karte.

{{% notice note %}}
Diese Karten beantworten die Frage, welche gehaltenen Instrumente sich am stärksten bewegt haben. Sie sind kein Periodenertrag und keine Alternative zur Karte **Letzte Handelstage**.
{{% /notice %}}

Instrumente ohne brauchbares Kurspaar fehlen in der Rangliste; die Karte nennt, wie viele gehaltene Instrumente das betrifft. Gehaltene Margin-Positionen werden von der Rangliste nicht erfasst, ebenfalls mit Angabe der Anzahl. Fehlt für die Daten einer Periode der Kurs zum Periodenrand, bleibt das Instrument in der Liste, die Zeile trägt ein Sternchen und im Kurzhinweis die tatsächlich verwendeten Daten, etwa **Bewertet von … bis …, weil für die Daten dieser Periode kein Kurs vorlag.** Beim Intraday kann der letzte Kurs älter als heute sein; der Kurzhinweis lautet dann **Letzter Kurs vom …**.

Eine leere Periode bleibt nicht stumm. Fand heute an keiner Börse ein Handel statt, sagt die Intraday-Periode das. Reicht der Handelskalender nicht weit genug zurück, um die beiden Handelstage einer Schlusskurs-Periode zu bestimmen, erscheint der entsprechende Hinweis statt einer leeren Rangliste.

## Letzte Handelstage
Die Karte zeigt, was der Mandant oder ein einzelnes Depot an den letzten abgeschlossenen Handelstagen gewonnen oder verloren hat, neben der Veränderung des Gesamtwerts und den Ein- und Auszahlungen, welche die Differenz erklären. Die Berechnung ist dieselbe wie im Bericht [Periodenertrag]({{% relref "/reportportfolio/periodperformance" %}}).

Die gespeicherte Einstellung **Angezeigte Handelstage** legt fest, wie viele abgeschlossene Handelstage die Tabelle enthält, voreingestellt fünf und zulässig von 2 bis 10. Reicht die Historie nicht so weit, zeigt die Karte die vorhandenen Tage, ohne stillschweigend einen anderen Zeitraum zu wählen. Über **Depot** beschränken Sie die Karte auf eines der Portfolios des Klienten; die leere Auswahl gilt für den ganzen Mandanten. Diese Auswahl wird nicht gespeichert. **Aktualisieren** kehrt zum ganzen Mandanten zurück.

Über der Tabelle stehen die Summen **Total Gewinn**, **Veränderung Gesamtwert** und **Externe Bargeld Ein-/Auszahlung**. Die Tabelle selbst, neueste Sitzung oben, enthält zusätzlich **Konto- und Deposten real**, **Kontozins real** und **Wertpapiere + Saldo**. Die Tage laufen über dieselben Feiertage und Tage mit fehlenden Kursen, die der Periodenertrag in der Datumsauswahl sperrt.

{{% notice note %}}
Gebühren und Kontozinsen sind im Ergebnis des Tages bereits enthalten und werden gezeigt, um eine Bewegung zu erklären, die kein Markt verursacht hat. Sie dürfen nicht zum **Total Gewinn** dazugezählt werden.
{{% /notice %}}

Wurde mindestens ein gehaltenes Instrument an diesem Tag mit einem gefüllten Kurs bewertet, weil kein eigener Kurs vorlag, erklärt das ein Kurzhinweis auf der Zeile. Lassen sich keine zwei Handelstage mit vollständigen Kursen bestimmen, erscheint **Es gibt keine zwei Handelstage mit vollständigen Kursen, daher lässt sich keine Veränderung berechnen.** Wurde an diesen Handelstagen nichts gehalten, erscheint der entsprechende Hinweis.

## Entwicklung des Gesamtwerts
Die Karte zeichnet den Gesamtwert des Mandanten oder eines einzelnen Depots als Linie über die Zeit und daneben das investierte Kapital. Der Gesamtwert entspricht **Wertpapiere + Saldo** im Bericht [Periodenertrag]({{% relref "/reportportfolio/periodperformance" %}}); an jedem Tag, den beide zeigen, stimmen die Werte überein. Das **Investierte Kapital** umfasst die bis zu diesem Tag aufgelaufenen Einzahlungen abzüglich der Auszahlungen. Der Abstand zwischen den beiden Linien ist damit der Gewinn oder Verlust der Anlagen. Alle Beträge stehen in der Währung des Mandanten oder, wenn ein Depot gewählt ist, in der Währung dieses Depots.

Die gespeicherte Einstellung **Zeitraum** legt fest, mit welchem Zeitraum die Karte öffnet: **1 Monat**, **3 Monate**, **Seit Jahresbeginn**, **1 Jahr**, **3 Jahre**, **5 Jahre** oder **Seit Beginn**, voreingestellt ist **1 Jahr**. Auf der Karte selbst beschränken Sie die Anzeige über **Depot** auf eines der Portfolios des Klienten, die leere Auswahl gilt für den ganzen Mandanten. Dort lässt sich auch der **Zeitraum** wechseln. Beide Auswahlen auf der Karte werden nicht gespeichert, **Aktualisieren** kehrt zum ganzen Mandanten und zum gespeicherten Zeitraum zurück.

Fahren Sie mit der Maus über das Diagramm, zeigt der Kurzhinweis das Datum, **Wertpapiere + Saldo**, **Investiertes Kapital** und deren **Differenz**. Unter dem Diagramm steht der **Neueste Tag**, für den ein Wert vorliegt. Bei langen Zeiträumen wäre ein Punkt pro Handelstag mehr, als ein Diagramm dieser Grösse darstellen kann. Umfasst der Zeitraum mehr als rund 500 Handelstage, zeigt die Karte deshalb den letzten Wert jeder Woche und vermerkt **Es wird ein Wert pro Woche angezeigt.** Reicht auch das nicht, zeigt sie den letzten Wert jedes Monats. Der neueste Tag ist immer enthalten.

{{% notice info %}}
Die Karte rechnet nicht selbst. Die Tageswerte berechnet die [Aufgabe 55]({{% relref "/admindata/taskdatachangemonitor/taskdescription" %}}#JOB55) im Hintergrund und speichert sie. Nach einer neuen Transaktion oder nachträglich geladenen Kursen werden die betroffenen Tage neu berechnet; bis dahin steht unter dem Diagramm **Die neuesten Tage werden noch berechnet.** Ein Tag, an dem ein Kurs oder Wechselkurs eines gehaltenen Instruments fehlt, hat keinen Wert, und das Diagramm verbindet die benachbarten Tage.
{{% /notice %}}

Ohne Werte bleibt die Karte nicht stumm, sondern nennt den Grund. Hat die Hintergrundaufgabe den Mandanten noch nie berechnet, etwa kurz nach dem Einrichten oder nach einem Import, erscheint **Die Tageswerte wurden noch nicht berechnet. Eine Hintergrundaufgabe berechnet sie, versuchen Sie es später erneut.** Waren im gewählten Zeitraum keine Bestände vorhanden, erscheint **In diesem Zeitraum wurde nichts gehalten.** Für eine Simulationsumgebung werden keine Tageswerte berechnet, was die Karte ebenfalls meldet.

### Maximieren
Diese Karte lässt sich als einzige maximieren. Das Symbol **Maximieren** im Kopf der Karte vergrössert sie auf den ganzen Bereich des Dashboards, ähnlich wie ein maximiertes Fenster; der Navigationsbaum links bleibt sichtbar. Die übrigen Karten werden dabei nur ausgeblendet und behalten ihre Daten. Mit dem Symbol **Wiederherstellen** oder der Taste Esc kehren Sie zur gewohnten Anordnung zurück. Die Maximierung wird nicht gespeichert und endet auch, wenn Sie **Dashboard bearbeiten** wählen oder den Mandanten wechseln. Das Diagramm passt sich der Grösse an, ebenso wenn Sie danach den Trennbalken verschieben oder das Fenster verändern.

## Rebalancing-Überwachung
Die Karte zeigt, ob die Strategie, die Sie der [Portfolioüberwachung]({{% relref "/algoalert/algo" %}}#portfolioüberwachung) zugewiesen haben, Aufmerksamkeit braucht. Sie steht nur zur Verfügung, wenn der regelbasierte Handel auf dieser Instanz eingeschaltet ist, und hat keine Einstellungen. Die Karte rechnet nicht selbst, sondern fasst die letzte tägliche Auswertung zusammen; das Öffnen des Dashboards bewertet also keine Positionen neu. Auch wenn Sie gerade in einer Simulationsumgebung arbeiten, zeigt die Karte die Überwachung Ihres eigenen Portfolios.

Ist keine Strategie zugewiesen, erscheint **Der Überwachung ist keine Strategie-Hierarchie zugewiesen.**, und wurde die zugewiesene Strategie noch nicht ausgewertet, **Die überwachte Strategie-Hierarchie wurde noch nicht ausgewertet.** Andernfalls nennt die Karte den Namen der Strategie und einen von drei Zuständen:

- **Nichts zu tun** bedeutet, dass die letzte Auswertung kein Geschäft vorschlägt.
- **Stichtag fällig** nennt die Anzahl vorgeschlagener Käufe, Verkäufe und blockierter Geschäfte. An einem Stichtag der [Portfolio-Neugewichtung]({{% relref "/algoalert/strategy/rebalancing" %}}) sollten Sie handeln.
- **Abweichung über der Toleranz, gehandelt wird am nächsten Stichtag** bedeutet, dass die Aufteilung zwar von den Zielen abweicht, der nächste Stichtag aber noch nicht erreicht ist. GT meldet die Abweichung, schlägt die Geschäfte aber erst am Stichtag vor.

Darunter folgen die Angaben **Bewertungsdatum**, **Nächster Stichtag** und **Grösste Abweichung einer Klasse (Pp.)** mit dem Namen der betroffenen Gruppe der Strategie, die Summen der vorgeschlagenen **Verkäufe** und **Käufe** in der Währung des Mandanten die Anzahl der **Mean-Reversion-Signale**, die ein Geschäft verlangen, sowie unter **Ausserhalb der Toleranz** die Anzahl der Anlageklassen, Wertpapiere und der Investitionsgrenze, die ihre Toleranz gerade überschreiten, siehe [Allokation ausserhalb der Toleranz]({{% relref "/algoalert/strategy/rebalancing" %}}#allokation-ausserhalb-der-toleranz). Eine Angabe ohne Wert wird weggelassen statt als null angezeigt. Es folgen höchstens drei der grössten vorgeschlagenen Geschäfte, Verkäufe vor Käufen, weil ein Kauf vor dem Verkauf, der ihn finanziert, die Investitionsgrenze überschreiten kann.

**Rebalancing-Bericht öffnen** führt zum Bericht [Anlageklassen mit Cash]({{% relref "/reportportfolio/securitycashaccountreport" %}}) mit der überwachten Strategie im Vergleich. Dort wird der Vergleich mit den aktuellen Beständen neu berechnet und zeigt alle Zeilen.
