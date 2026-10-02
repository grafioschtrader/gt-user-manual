---
title: "Regelbasierter Handel"
date: 2026-09-26T10:00:00+02:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="In Bearbeitung - Zielversion V0.38.0" %}}
Aufteilungen, Simulationsumgebungen und deren historische Wiederholung stehen zur Verfügung. Die Auswertung künftiger, simulierter Kursverläufe befindet sich noch in Entwicklung.
{{% /notice %}}
Der regelbasierte Handel in GT basiert auf einer hierarchischen Baumstruktur. An der Wurzel steht eine **Portfoliobasierte Strategie**, die den Teil Ihres Vermögens festlegt, der überhaupt investiert werden darf. Darunter werden Anlageklassen mit prozentualen Gewichtungen angelegt und innerhalb der Anlageklassen die einzelnen Wertpapiere, wiederum mit einer Gewichtung. Auf jeder dieser drei Ebenen lassen sich zusätzlich Strategien hinterlegen.

{{< mermaid >}}
graph TD
    A["Portfoliobasierte Strategie<br/>(Name, Maximal investiert, Watchlist)"] --> B["Anlageklasse<br/>(Gewichtung %)"]
    A --> C["Benutzerdefinierte Kategorie<br/>(Gewichtung %)"]
    B --> D["Wertpapier der Anlageklasse<br/>(Gewichtung %)"]
    B --> E["Wertpapier der Anlageklasse<br/>(Gewichtung %)"]
    C --> H["Wertpapier aus der Watchlist<br/>(Gewichtung %)"]
    A --> S1["Strategie"]
    D --> F["Strategie"]
    H --> I["Strategie"]
{{< /mermaid >}}

## Portfoliobasierte Strategie erstellen
Eine neue Strategie erstellen Sie im Navigationsbaum über das Kontextmenü von **Regelbasierter Handel** mit **Erstellen Portfoliobasierte Strategie...**. Der Dialog umfasst folgende Angaben:

| Feld | Beschreibung |
|---|---|
| **Name** | Bezeichnung der Strategie mit höchstens 40 Zeichen. |
| **Maximal investiert** | Der Anteil Ihres Nettovermögens zwischen 2% und 100%, den die Strategie investieren darf. Alle Gewichtungen darunter teilen genau diesen Betrag auf. |
| **Watchlist** | Die verknüpfte Watchlist. Sie bestimmt, welche Anlageklassen im Dialog zur Auswahl stehen, und liefert die Wertpapiere einer benutzerdefinierten Kategorie. |

Bereits in diesem Dialog können Sie Anlageklassen vorgeben. Mit **Hinzufügen Anlageklasse** ergänzen Sie eine weitere Zeile mit einer Anlageklasse und deren **Gewichtung** zwischen 0,5% und 100%, mit **Letzte Anlageklasse entfernen** nehmen Sie die zuletzt erfasste Zeile wieder zurück. Es sind höchstens neun Zeilen möglich. Zur Auswahl stehen nur Anlageklassen, die in der gewählten Watchlist vorkommen, und eine bereits in einer anderen Zeile gewählte Anlageklasse lässt sich kein zweites Mal wählen. Weitere Anlageklassen und alle Wertpapiere fügen Sie danach in der Baumtabelle hinzu.

Einen Dialog zum nachträglichen Bearbeiten der Portfoliobasierten Strategie gibt es nicht. Ihre **Gewichtung**, also der Wert **Maximal investiert**, lässt sich direkt in der Baumtabelle ändern. Ob sie im Hintergrund ausgewertet wird, bestimmt allein die Zuweisung zur [Portfolioüberwachung](#portfolioüberwachung); einen eigenen Schalter zum Aktivieren oder Deaktivieren besitzt die Portfoliobasierte Strategie nicht.

## Die Baumtabelle
Ein Klick auf die Strategie im Navigationsbaum öffnet die Baumtabelle **Regelbasierter Handel**. Sie zeigt die Portfoliobasierte Strategie mit ihren Anlageklassen, Wertpapieren und Strategien. Anlageklassen erscheinen fett.

| Spalte | Inhalt |
|---|---|
| **Name** | Bei einer Anlageklasse Kategorie, Unterkategorie und Instrumenttyp oder der Name der benutzerdefinierten Kategorie, bei einem Wertpapier dessen Name und Währung. |
| **Gewichtung** | Der Anteil am übergeordneten Knoten. Der Wert lässt sich direkt in der Zelle bearbeiten. |
| **Summe der Gewichtungen** | Fasst die Gewichtungen der direkt darunter liegenden Knoten zusammen, damit Sie sofort erkennen, ob eine Ebene auf 100% kommt. |
| **Handel ab** und **Aktiv bis Datum** | Der Handelszeitraum eines Wertpapiers. |

Die Tabelle macht Mängel der Aufteilung farblich sichtbar. Rot erscheint eine **Summe der Gewichtungen**, die nicht 100% ergibt. Ebenfalls rot erscheint der Name einer Anlageklasse, die eine positive Gewichtung trägt, aber kein aktives, positiv gewichtetes und handelbares Wertpapier enthält, sowie der Name eines Wertpapiers, das für eine Strategie ungeeignet ist. Ein **Aktiv bis Datum** in der Vergangenheit wird gelb hinterlegt. Der Hinweis unter der Tabelle fasst diese Regeln zusammen.

### Einsatzbereitschaft der Strategie
Unter der Tabelle zeigt GT, ob die Strategie so, wie sie gerade ist, verwendet werden kann. **Bereit für Simulation und Wiederholung.** bedeutet, dass eine Simulationsumgebung erstellt und eine Wiederholung gestartet werden kann. Ist zusätzlich eine Portfolio-Neugewichtung vorhanden, lässt sich auch der Rebalancing-Vergleich berechnen; fehlt sie, meldet GT **Ohne Portfolio-Neugewichtung gibt es keinen Rebalancing-Vergleich.** Darunter folgen die einzelnen Befunde.

Die Bereitschaft wird bei jedem Anzeigen neu bestimmt und nicht gespeichert. Während Sie eine Strategie aufbauen, ergeben die Gewichtungen zwangsläufig noch nicht 100%; zudem können sich Befunde ändern, ohne dass Sie die Strategie bearbeiten, etwa wenn das Handelsende eines Wertpapiers verstreicht oder die verknüpfte Watchlist ein Wertpapier verliert.

Rot aufgeführte Befunde verhindern die Verwendung. Solange einer davon besteht, lehnt GT **Simulationsumgebung erstellen...**, **Wiederholung starten...** und den Rebalancing-Vergleich mit dem genannten Grund ab:
- Die Gewichtungen der Anlageklassen oder die Gewichtungen der Wertpapiere einer Anlageklasse ergeben nicht 100%.
- Eine Gewichtung fehlt oder liegt ausserhalb von 0% bis 100%, oder **Maximal investiert** liegt ausserhalb dieses Bereichs.
- Die Portfolio-Neugewichtung ist unvollständig, oder das Wertpapierband beziehungsweise die maximale Anzahl gehandelter Wertpapiere einer Anlageklasse ist ungültig.
- Ein aktiver Mean Reversion Dip ist nicht ausführbar, oder sein Wertpapier gehört nicht zur verknüpften Watchlist.
- Ein Wertpapier mit aktivem Mean Reversion Dip ist mehr als einer Anlageklasse zugeordnet.

Die übrigen Befunde werden nur gemeldet, weil GT damit umgehen kann. Ein für Strategien ungeeignetes Wertpapier wird in einer Wiederholung nicht gehandelt, und seine Gewichtung wird auf die übrigen Wertpapiere der Anlageklasse verteilt. Der Anteil einer Anlageklasse ohne handelbares Wertpapier bleibt uninvestiert. Ein Wertpapier ohne Mean Reversion Dip in zwei Anlageklassen sowie eine leere Anlageklasse werden ebenfalls nur angezeigt.

Weicht eine Summe ab, weil Sie einzelne Gewichtungen von Hand geändert haben, rechnet **Prozentanteile normalisieren** die Werte der Knoten direkt unterhalb des gewählten Knotens auf 100% um; allfällige Rundungsdifferenzen trägt der letzte Knoten. **Alle Prozentanteile normalisieren** auf der Portfoliobasierten Strategie tut dies für die Anlageklassen und für die Wertpapiere jeder Anlageklasse.

Wertpapierzeilen besitzen ein Kontrollkästchen. Sind Wertpapiere angekreuzt, erscheint zuoberst im Kontextmenü **Ausgewählte Wertpapiere löschen**, womit Sie mehrere Wertpapiere in einem Schritt entfernen. Das Kontextmenü bietet je nach Knoten folgende Einträge:

| Knoten | Einträge im Kontextmenü |
|---|---|
| Portfoliobasierte Strategie | **Hinzufügen Anlageklasse**, **Prozentanteile normalisieren**, **Alle Prozentanteile normalisieren**, **Erstellen Strategie Definition** |
| Anlageklasse | **Bearbeiten Anlageklasse**, **Löschen Anlageklasse**, **Hinzufügen Wertpapier**, **Prozentanteile normalisieren**, **Erstellen Strategie Definition** |
| Wertpapier | **Bearbeiten Wertpapier**, **Löschen Wertpapier**, **Erstellen Strategie Definition** |

Eine Anlageklasse lässt sich erst löschen, wenn sie weder Wertpapiere noch Strategien enthält. **Erstellen Strategie Definition** ist ausgegraut, wenn für diesen Knoten keine weitere Strategie mehr zur Verfügung steht. In einer Simulationsumgebung ist die Baumtabelle schreibgeschützt; sie bietet dort weder ein Kontextmenü noch bearbeitbare Zellen.

## Anlageklasse hinzufügen und bearbeiten
**Hinzufügen Anlageklasse** und **Bearbeiten Anlageklasse** öffnen den Dialog **Strategie Anlageklasse**. Eine Anlageklasse des Baums ist entweder eine Anlageklasse von GT oder eine frei benannte Gruppe, die **Benutzerdefinierte Kategorie**.

| Feld | Beschreibung |
|---|---|
| **Benutzerdefinierte Kategorie** | Angekreuzt entsteht eine frei benannte Gruppe statt einer Anlageklasse von GT. Diese Wahl ist nur beim Erstellen möglich. |
| **Name** | Name der benutzerdefinierten Kategorie mit höchstens 40 Zeichen. Nur bei angekreuzter **Benutzerdefinierte Kategorie** eingebbar und dann Pflicht. |
| **Anlageklasse** | Die Anlageklasse von GT, angezeigt als Kategorie, Unterkategorie und Instrumenttyp. Nur ohne **Benutzerdefinierte Kategorie** eingebbar und dann Pflicht. Beim Bearbeiten lässt sie sich nicht mehr ändern. |
| **Toleranz der Wertpapiergewichtung (Prozentpunkte)** | Freiwillig, 0 bis 100. Überschreibt für diese Anlageklasse die gleichnamige Einstellung der Portfolio-Neugewichtung. |
| **Maximal gehandelte Wertpapiere je Anlageklasse** | Freiwillig, ab 1. Überschreibt für diese Anlageklasse die gleichnamige Einstellung der Portfolio-Neugewichtung. |
| **Primäres Depot** | Freiwillig. Das Depot, in dem eine Simulation die Geschäfte dieses Knotens in erster Linie bucht. Zur Auswahl stehen alle Depots Ihrer Portfolios. |
| **Sekundäres Depot** | Freiwillig. Das Depot, das verwendet wird, wenn das primäre Depot ein Geschäft nicht ausführen kann. Es lässt sich erst wählen, wenn ein primäres Depot gewählt ist. |
| **Gewichtung** | Pflicht, 0,1% bis 100%. Der Anteil dieser Anlageklasse am Betrag, den **Maximal investiert** freigibt. |

Jede Anlageklasse von GT kann in einer Strategie nur einmal vorkommen; bereits verwendete Anlageklassen sowie jene der CFDs werden deshalb nicht angeboten. Benutzerdefinierte Kategorien unterliegen dieser Einschränkung nicht. Ob eine Anlageklasse von GT oder eine benutzerdefinierte Kategorie gewählt wird, bestimmt, welche Wertpapiere später zur Auswahl stehen: Eine Anlageklasse von GT bietet deren Wertpapiere an, eine benutzerdefinierte Kategorie die Wertpapiere der Watchlist der Strategie.

Die beiden Felder zur Toleranz und zur Anzahl gehandelter Wertpapiere bleiben normalerweise leer; dann gilt die Einstellung der [Portfolio-Neugewichtung](../strategy/rebalancing/). Die Wertpapiertoleranz bezieht sich in Prozentpunkten auf den Zielbetrag der Anlageklasse, die Handelsgrenze zählt unterschiedliche Wertpapiere je Kontrollzeitpunkt.

## Wertpapier hinzufügen und bearbeiten
**Hinzufügen Wertpapier** und **Bearbeiten Wertpapier** auf einer Anlageklasse öffnen den Dialog **Strategie Wertpapier**.

| Feld | Beschreibung |
|---|---|
| **Wertpapier** | Pflicht. Eine Auswahlliste mit Suchfeld. Jeder Eintrag zeigt Name und Währung sowie den Handelszeitraum von **Handel ab** bis **Aktiv bis Datum**. |
| **Primäres Depot** und **Sekundäres Depot** | Freiwillig, mit derselben Bedeutung wie bei der Anlageklasse. |
| **Gewichtung** | Pflicht, 0,1% bis 100%. Der Anteil dieses Wertpapiers am Betrag seiner Anlageklasse. |

Welche Wertpapiere zur Auswahl stehen, hängt von der übergeordneten Anlageklasse ab. Unter einer Anlageklasse von GT erscheinen alle für Sie sichtbaren Wertpapiere dieser Anlageklasse, unabhängig von einer Watchlist. Unter einer benutzerdefinierten Kategorie erscheinen die Wertpapiere der Watchlist, die mit der Portfoliobasierten Strategie verknüpft ist. In beiden Fällen fehlen Wertpapiere, die bereits unter derselben Anlageklasse stehen.

Nicht angeboten und auch nicht gespeichert werden CFDs, Forex und Instrumente mit einem Hebelfaktor ungleich 1. Wurde die Strategie aus historischen Beständen erstellt, gilt der Tag nach ihrem Referenzdatum als Eröffnungsdatum; ein Wertpapier, dessen Handel vorher endete, lässt sich dann ebenfalls nicht hinzufügen. Wertpapiere, deren Handel erst nach diesem Stichtag beginnt, bleiben wählbar. In der Auswahlliste kennzeichnet ein rotes Datum einen Handelsbeginn nach dem Stichtag der Strategie oder ein Handelsende vor heute.

Beim Bearbeiten lassen sich auch das Wertpapier selbst, die Depots und die Gewichtung ändern.

{{% notice style="info" title="Ein Wertpapier gehört in genau eine Anlageklasse" %}}
Da jede Anlageklasse von GT nur einmal vorkommt, steht ein Wertpapier dort höchstens einmal. Zusätzlich kann dasselbe Wertpapier aber in beliebigen benutzerdefinierten Kategorien erscheinen, denn GT prüft dies beim Speichern nicht. Strategien auf der Ebene eines Wertpapiers, etwa der [Mean-Reversion-Dip](../strategy/meanreversiondip/), setzen jedoch voraus, dass das Instrument genau einer Anlageklasse zugeordnet ist. Ordnen Sie jedes Wertpapier deshalb nur einmal zu.
{{% /notice %}}

## Strategien zuweisen
Strategien lassen sich auf allen drei Ebenen zuweisen: der Portfoliobasierten Strategie selbst, einer Anlageklasse und einem Wertpapier. Über **Erstellen Strategie Definition** im Kontextmenü öffnet sich der Dialog **Strategie Definition**, in dem Sie unter **Strategiename** den gewünschten Strategietyp auswählen. Angeboten werden nur die für die Ebene zulässigen Typen. Die **Portfolio-Neugewichtung** steht nur auf der Portfoliobasierten Strategie zur Verfügung, weil darunter bereits die Gewichtungen des Baums die Ziele vorgeben. Detaillierte Informationen zu den einzelnen Strategietypen finden Sie unter [Strategien](../strategy/).

## Portfolioüberwachung
Im Hintergrund ausgewertet wird nur die Portfoliobasierte Strategie, die Sie der Portfolioüberwachung zugewiesen haben: ihre Portfolio-Neugewichtung sowie alle Alarme und Strategien darunter. Zugewiesen wird sie im Navigationsbaum über das Kontextmenü der Strategie mit **Zur Portfolioüberwachung verwenden**, aufgehoben mit **Portfolioüberwachung beenden**. Es ist höchstens eine Strategie zugewiesen; sie trägt im Navigationsbaum ein Auge und den Zusatz «Portfolioüberwachung». Eine Strategie ohne Zuweisung ruht im Hintergrund und behält ihre gesamte Konfiguration, sodass Sie sie in Ruhe aufbauen können.

Simulationen und historische Wiederholungen hängen nicht von dieser Zuweisung ab. Sie werten jede Portfoliobasierte Strategie so aus, wie sie konfiguriert ist.

Anlageklassen und Wertpapiere besitzen in der Baumtabelle keinen eigenen Schalter. Ein einzelnes Wertpapier und eine einzelne Strategie legen Sie über das Kontrollkästchen **Aktiv** im Register **Alarm** des Klienten still, siehe [Alarme](../alert/).

## Aufteilung aus einer Watchlist erstellen
Wählen Sie **Strategie aus Watchlist erstellen** im Kontextmenü von **Regelbasierter Handel** und geben Sie einen **Name** und eine **Watchlist** an. GT legt für jede Anlageklasse der Instrumente in der Watchlist eine Anlageklasse an und ordnet jedes Instrument seiner Anlageklasse zu. Da eine Watchlist keine Beträge kennt, werden beide Ebenen gleich gewichtet: die Anlageklassen untereinander und die Instrumente innerhalb ihrer Anlageklasse. **Maximal investiert** wird auf 100% gesetzt; Sie passen den Wert danach zusammen mit den Gewichtungen an. Währungspaare der Watchlist werden nicht übernommen; jedes Instrument benötigt eine Anlageklasse, und die Watchlist muss mindestens ein Instrument enthalten.

## Aufteilung aus historischen Beständen erstellen
Wählen Sie **Strategie aus Portfolio erstellen** im Kontextmenü von **Regelbasierter Handel** und geben Sie einen **Name** und ein **Referenzdatum** an. Das Datum muss gestern oder früher liegen; der erste Tag mit einer Transaktion ist zulässig. GT berücksichtigt Transaktionen bis zum Ende dieses Tages und historische Schlusskurse sowie Wechselkurse, die an diesem Tag oder davor vorliegen.

Die Aufteilung entsteht allein aus den Beständen dieses Tages, eine Watchlist wird dafür weder benötigt noch verknüpft. Fehlende Kurse oder Wechselkurse verhindern die Erstellung und werden in der Fehlermeldung aufgeführt. Jedes gehaltene Wertpapier benötigt eine Anlageklasse. GT erstellt die gesamte Aufteilung zusammen. Da keine Watchlist verknüpft ist, bieten benutzerdefinierte Kategorien einer solchen Strategie keine Wertpapiere an, und der [Mean-Reversion-Dip](../strategy/meanreversiondip/) findet kein zulässiges Instrument.

**Maximal investiert** ergibt sich aus dem Bruttoengagement geteilt durch das Nettovermögen. Das Bruttoengagement summiert die absoluten Engagements von Long- und Short-Positionen vor einer gegenseitigen Verrechnung. Das Nettovermögen enthält vorzeichenrichtige Positionswerte, Geldbestände und Verbindlichkeiten; Margin-Positionen tragen ihren Abrechnungswert bei einer Schliessung bei. Vermögen und Engagement müssen positiv sein, und das Engagement darf das Vermögen nicht übersteigen.

Bei einem Vermögen von 100'000 und einem Engagement von 48'200 beträgt **Maximal investiert** 48,2%. Eine Anlageklasse mit 50% **Gewichtung** erhält davon 24'100. Ein Wertpapier mit 60% Gewichtung innerhalb dieser Klasse erhält 14'460. Die erzeugten Gewichtungen entsprechen den Engagements am Referenzdatum und ergeben unter den Anlageklassen und getrennt unter den Wertpapieren jeder Klasse jeweils 100%. Geld bleibt auf den Geldkonten.

## Simulationsumgebung erstellen
Wählen Sie **Simulationsumgebung erstellen...** im Kontextmenü einer Portfoliobasierten Strategie. Eine Simulationsumgebung ist eine abgetrennte Kopie Ihrer Portfolios und Konten auf einen Tag der Vergangenheit, in der Sie Ihre Strategien gegen vergangene Kursdaten prüfen können, ohne Ihr echtes Portfolio zu berühren.

### Voraussetzungen
Vorausgesetzt ist einzig eine Portfoliobasierte Strategie. Anlageklassen und Wertpapiere müssen zu diesem Zeitpunkt weder vorhanden noch vollständig gewichtet sein; die Umgebung entsteht auch zu einer noch leeren Strategie. Ob sie danach etwas auswertet, ist eine andere Frage, die der letzte Abschnitt dieser Seite beantwortet.

Die beiden Initialisierungsarten, die Ihr bestehendes Portfolio auslesen, benötigen zusätzlich ein Eröffnungsdatum, das Ihr Portfolio auch erreicht hat: Es muss auf oder nach der ersten Transaktion liegen, wobei der Tag der ersten Transaktion selbst zulässig ist. Vor diesem Tag gibt es weder Bestände noch Geld zum Auslesen, und die Umgebung würde ungewollt leer eröffnet. **Manuelle Geldbestände** übernimmt weder Bestände noch Geld aus dem Portfolio und ist deshalb an diese Untergrenze nicht gebunden. Standardmässig können bis zu fünf Umgebungen gleichzeitig bestehen; die Administration kann diese Limite ändern.

### Die Felder des Dialogs
| Feld | Bedeutung |
|---|---|
| **Name** | Bezeichnung der Umgebung mit höchstens 40 Zeichen; vorbelegt ist der Name der Strategie. |
| **Eröffnungsdatum** | Der abgeschlossene Handelstag, mit dessen Schlussbestand die Umgebung eröffnet wird. Gestern ist der späteste zulässige Tag. |
| **Initialisierungsart** | Bestimmt, woraus dieser Eröffnungsbestand gebildet wird. |
| Betrag je Geldkonto | Erscheint nur bei **Manuelle Geldbestände**, mit einem Feld für jedes Geldkonto Ihrer Portfolios. Ein Konto, dessen Feld leer bleibt, wird nicht in die Umgebung übernommen; mit 0 übernehmen Sie ein Konto ohne Geld. Mindestens ein Feld muss ausgefüllt sein. |

### Wofür das Eröffnungsdatum steht
Das Eröffnungsdatum ist mehr als eine Beschriftung, denn es legt vier Dinge gleichzeitig fest. Es ist die Grenze dessen, was übernommen wird: Transaktionen bis und mit diesem Tag werden kopiert, spätere nicht. Es ist das Buchungsdatum der Eröffnungsbuchungen, mit denen Geld in die Umgebung gelangt. Beim Auflösen des Quellportfolios ist es zugleich der Bewertungstag, denn die Positionen werden zu den Schlusskursen und Wechselkursen dieses Tages bewertet. Und es ist der unverrückbare Anfang jeder historischen Wiederholung: Ausgewertet wird ab dem ersten Handelstag danach, weshalb eine Wiederholung später nur noch nach einem Enddatum fragt.

Nach der Erstellung lässt sich das Datum nicht mehr ändern, denn der Eröffnungsbestand wurde daraus bereits gebucht; ein nachträglich verschobenes Datum würde die vorhandenen Buchungen nicht mehr beschreiben. Für einen anderen Ausgangspunkt erstellen Sie deshalb eine weitere Umgebung.

### Initialisierungsart wählen
{{< mermaid >}}
graph TD
    A["Womit soll die Umgebung eröffnet werden?"] --> B["Mit den bestehenden<br/>Positionen weiterrechnen"]
    A --> C["Mit reinem Geld beginnen"]
    B --> D["Quellportfolio übernehmen"]
    C --> E["Beträge selbst festlegen"]
    C --> F["Wert der Positionen am<br/>Eröffnungsdatum verwenden"]
    E --> G["Manuelle Geldbestände"]
    F --> H["Quellportfolio in Geld auflösen"]
{{< /mermaid >}}

| Initialisierungsart | Eröffnungsbestand |
|---|---|
| **Quellportfolio übernehmen** | Kopiert Transaktionen bis zum Eröffnungsdatum einschliesslich Überträgen und ihren internen Verknüpfungen und berechnet daraus Positionen und Geldbestände. Spätere Transaktionen bleiben ausgeschlossen. |
| **Manuelle Geldbestände** | Übernimmt nur die Geldkonten, für die Sie einen Betrag eingegeben haben, die Portfolios, zu denen sie gehören, und die Depots dieser Portfolios. Ein Portfolio ohne gewähltes Geldkonto entfällt vollständig. Jeder positive Betrag wird zur Eröffnungseinzahlung in der Kontowährung; ein Betrag von 0 erzeugt keine Transaktion. Ursprüngliche Wertpapiergeschäfte werden nicht übernommen. |
| **Quellportfolio in Geld auflösen** | Bewertet Positionen zum historischen Schlusskurs und addiert ihre vorzeichenrichtigen Schliessungserlöse zu den ursprünglichen Geldbeständen. Erstellt nur Eröffnungseinzahlungen oder -auszahlungen, ohne Positionen, fiktive Verkäufe, Gebühren oder Steuern. |

Bei **Quellportfolio in Geld auflösen** speichern Sie zunächst, um die berechneten Eröffnungsbestände nach **Bankkonto**, **Währung** und **Saldo** zu prüfen. Erlöse bleiben dem ursprünglichen Geldkonto zugeordnet, soweit dies eindeutig ist. Andernfalls wählen Sie für jede aufgeführte Position unter **Geldkonto auswählen** ein Zielkonto und speichern erneut, um die Vorschau zu aktualisieren. Ein weiteres **Speichern** der vollständig zugeordneten Vorschau erstellt die Umgebung. Erscheint in der Vorschau eine Zeile mit einem blossen Instrumentnamen oder einem Währungspaar, so fehlt dafür auf dieses Datum ein brauchbarer Schlusskurs; er muss zuerst ergänzt werden. Die Schliessung einer Short- oder Margin-Position kann den Geldbestand vermindern; ein negativer Eröffnungsbestand wird als Auszahlung erfasst.

### Nach der Erstellung
Die Umgebung kopiert Portfolios, Konten und die Watchlist der Strategie. Die Strategie selbst wird gemeinsam verwendet, ist in einer Umgebung aber nur lesbar: Ändern Sie sie in Ihrem eigenen Portfolio und wiederholen Sie die Simulation danach erneut. Transaktionen und Bestände gehören dagegen allein zur Simulation. Mit **Zur Simulation wechseln** öffnen Sie diese, mit **Zum Hauptmandanten wechseln** kehren Sie zurück. Der Navigationsbaum führt jede Umgebung mit ihrem Eröffnungsdatum und ihrer Initialisierungsart auf. Eine historische Wiederholung starten Sie im Kontextmenü der Umgebung mit **Wiederholung starten...**.

**Simulation löschen** entfernt die privaten Daten und ausstehenden Arbeiten der Simulation und erhält die gemeinsame Strategie sowie das Hauptportfolio. Ältere Umgebungen ohne bekanntes Eröffnungsdatum bleiben einsehbar und löschbar; der Navigationsbaum kennzeichnet sie mit **Eröffnungsdefinition fehlt, neu erstellen**, denn ihr Eröffnungsbestand lässt sich nicht mehr nachvollziehen. Eine historische Wiederholung ist für sie nicht mehr möglich.

### Was eine Simulation tatsächlich auswertet
Die Erstellung einer Umgebung erteilt für sich noch keine Handelsaufträge; diese entstehen erst mit einer [Historischen Wiederholung](../historicalrun/). Eine Wiederholung über eine leere Strategie endet mit **Abgeschlossen** und ohne ein einziges Geschäft. Damit tatsächlich gehandelt wird, muss die Hierarchie vollständig sein.

Die **Portfolio-Neugewichtung** handelt ausschliesslich auf der Ebene der Wertpapiere. Eine Anlageklasse ohne zugeordnete Wertpapiere erhält deshalb zwar einen Zielbetrag, es wird aber nichts gekauft. Zusätzlich müssen sich die Gewichtungen der Anlageklassen und, getrennt davon, jene der Wertpapiere innerhalb jeder Anlageklasse auf 100% summieren. Ist das nicht der Fall, ist die Strategie nicht einsatzbereit: Weder lässt sich eine Umgebung erstellen noch eine Wiederholung starten, siehe [Einsatzbereitschaft der Strategie](#einsatzbereitschaft-der-strategie).

Strategien auf der Ebene eines Wertpapiers verlangen zusätzlich, dass das Instrument genau einer Anlageklasse zugeordnet ist. Andernfalls erscheint im Verlauf der Wiederholung das Ereignis **Nicht verfügbar** mit der Begründung **Aktive Allokation und ihre Gewichtungen vervollständigen** oder **Instrument ist mehreren Allokationen zugeordnet**.

CFD-, Forex- und gehebelte Positionen, die mit dem Eröffnungsbestand in die Umgebung gelangen, werden nach der Eröffnung glattgestellt und vom weiteren Simulationshandel ausgeschlossen.

Unabhängig davon begrenzen die vorhandenen Kursdaten, was überhaupt ausgewertet werden kann. Das Eröffnungsdatum bezieht sich auf Ihr Portfolio und nicht auf die Instrumente; es darf deshalb vor dem ersten Kurs eines Instruments liegen. Vor Beginn der Kursdaten entscheidet eine Wiederholung jedoch nichts, und Tage, an denen sich der Schlussbestand nicht vollständig bewerten lässt, weist der Verlauf mit **Nicht verfügbar** aus und lässt sie bei den Kennzahlen ausser Betracht. Beim Erstellen nennt Ihnen der Dialog, ab wann für die Instrumente der Strategie Kursdaten bestehen.

{{% notice style="info" title="Hinweis" %}}
Eine historische Wiederholung wertet die **Portfolio-Neugewichtung** und den [Mean-Reversion-Dip](../strategy/meanreversiondip/) unabhängig von der [Portfolioüberwachung](#portfolioüberwachung) aus und bucht deren Käufe und Verkäufe. Preis- und Indikator-Alarme werden in einer Simulation nie ausgewertet, siehe [Welche Strategie wo wirkt](../#welche-strategie-wo-wirkt).
{{% /notice %}}
