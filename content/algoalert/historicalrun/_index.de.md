---
title: "Historische Wiederholung"
date: 2026-10-01T12:00:00+02:00
draft: false
weight: 15
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="In Bearbeitung - Zielversion V0.38.0" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Eine historische Wiederholung wertet die Strategien einer Simulationsumgebung an jedem Handelstag nach deren Eröffnungsdatum aus und bucht die daraus entstehenden Aufträge als gewöhnliche Transaktionen innerhalb der Simulation. So sehen Sie, wie sich Ihre Aufteilung und Ihre Strategien in der Vergangenheit verhalten hätten, ohne dass Ihr echtes Portfolio berührt wird.

Voraussetzung ist eine bestehende Simulationsumgebung. Wie Sie eine solche eröffnen, beschreibt [Regelbasierter Handel](../algo/).

Regelmässige Einzahlungen, Auszahlungen, Kontozinsen und Gebühren sowie Sparpläne in einzelne Wertpapiere lassen sich mit [Daueraufträgen](./standingorders/) innerhalb der Simulation nachbilden. Welche Courtagen und Depotgebühren anfallen, beschreibt [Gebühren in der Wiederholung](./fees/).

## Wiederholung starten
Wählen Sie im Kontextmenü einer Simulationsumgebung den Eintrag **Wiederholung starten...**. Weil jede Wiederholung alle Buchungen ausserhalb des geschützten Eröffnungsbestandes löscht, auch die von Hand erfassten, fragt GT zuerst nach einer Bestätigung; erst danach öffnet sich der Dialog **Historische Wiederholung**. Der Eintrag erscheint nur im Hauptmandanten und nur bei Schreibberechtigung: Die Wiederholung bucht ihre Ausführungen als Eigentümer der Umgebung, weshalb sie nicht aus einer geöffneten Simulation heraus gestartet werden kann. Sind Sie in eine Simulation gewechselt, kehren Sie zuerst mit **Zum Hauptmandanten wechseln** zurück.

Im Dialog geben Sie das **Enddatum** an; der Beginn ist immer das Eröffnungsdatum der Umgebung und lässt sich nicht verändern. Damit bleibt nachvollziehbar, woraus ein aufgezeichnetes Ergebnis berechnet wurde. Das Enddatum muss nach dem Eröffnungsdatum und vor dem heutigen Tag liegen, denn ein noch laufender Tag hat keinen Schlusskurs, gegen den entschieden werden könnte. Wie viele Handelstage eine Wiederholung höchstens umfassen darf, legt der Administrator in den globalen Einstellungen mit `gt.simulation.max.run.trading.days` fest. Zulässig sind 300 bis 7500 Handelstage, vorgegeben sind 3000, also rund zwölf Jahre.
Zusätzlich können Sie **Simulationssteuermodelle anwenden** einschalten. Dann schätzt GT Transaktionssteuern und Ertragssteuerabzüge anhand der [Simulationssteuermodelle]({{% ref "/algoalert/historicalrun/taxmodel" %}}) der Steuerländer. Die Option ist standardmässig ausgeschaltet. **Regelmässige Anleihencoupons erzeugen** ist davon unabhängig, verwendet die Angaben im Reiter [Anleihensimulation]({{% relref "/watchlistinstrument/instrument/securityderived/security#reiter-anleihensimulation" %}}) des Wertpapiers und erzeugt vertragliche Coupons, wenn für eine direkte Festzinsanleihe keine gespeicherten Ausschüttungen vorliegen.

Für **Eröffnungsbestände Depotgebühren (YAML)** steht derselbe [YAML-Editor]({{% ref "/intro/userinterface#yaml-editor" %}}) mit Feldvorschlägen, Kurzhilfe und **Prüfen** zur Verfügung. Die Schlüssel der Zuordnung sind die Namen der Depots in der Simulation; die Felder innerhalb jedes Kontoeintrags werden vorgeschlagen. **Prüfen** kontrolliert die Dokumentstruktur, startet aber keine Wiederholung. Beim Start werden zusätzlich die Konten und die Eröffnungsbestände geprüft. Ein Beispiel und die Fälle, in denen Eröffnungsbestände erforderlich sind, finden Sie unter [Gebühren in der Wiederholung](./fees/#eröffnungsbestände-depotgebühren).

Mit **Wiederholung starten** übergeben Sie die Arbeit an den Server. Sie läuft im Hintergrund weiter, auch wenn Sie den Dialog schliessen.

## Welcher Stand der Strategie verwendet wird
Eine Simulationsumgebung enthält keine eigene Kopie der Strategie, sondern verweist auf die Strategie in Ihrem Hauptmandanten. Jedes **Wiederholung starten** liest diese Strategie neu, und zwar in dem Stand, den sie beim Start hat. Die Gewichtungen und die Auswahl der Wertpapiere werden in diesem Moment für den ganzen Lauf festgehalten. Unter **Einstellungen der Wiederholung** sehen Sie genau diesen festgehaltenen Stand.

Solange eine Wiederholung einer Umgebung läuft, lässt sich deren Strategie auch im Hauptmandanten nicht ändern. Eine Änderung mitten im Lauf würde nur noch für die verbleibenden Tage gelten, und das Ergebnis passte nicht mehr zu den festgehaltenen Einstellungen. Ändern Sie die Strategie deshalb vor oder nach einem Lauf.

Ändern Sie die Strategie zwischen zwei Wiederholungen, liefert die zweite Wiederholung ein anderes Ergebnis, sobald die Änderung eine Entscheidung betrifft. Das neue Ergebnis ersetzt das bisherige. Um zwei Stände einer Strategie zu vergleichen, notieren Sie das erste Ergebnis, bevor Sie die Strategie ändern und erneut wiederholen.

{{< mermaid >}}
graph LR
    A["Strategie R"] --> B["Simulationsumgebung erstellen"]
    B --> C["Wiederholung starten: liest R, Ergebnis 1"]
    C --> D["R im Hauptmandanten ändern"]
    D --> E["Wiederholung starten: liest geändertes R, Ergebnis 2 ersetzt Ergebnis 1"]
{{< /mermaid >}}

Ist die Strategie beim Start nicht einsatzbereit, weil etwa die Gewichtungen einer Ebene nicht 100% ergeben, lehnt GT den Start mit dem Grund ab. Welche Befunde einen Start verhindern, steht unter [Einsatzbereitschaft der Strategie](../algo/#einsatzbereitschaft-der-strategie).

## Ablauf einer Wiederholung
Vor dem ersten Handelstag versetzt GT die Umgebung in ihren Eröffnungszustand zurück: Alles, was eine frühere Wiederholung erzeugt hat, wird entfernt, der Eröffnungsbestand bleibt bestehen. Deshalb beginnt eine wiederholte Ausführung immer beim gleichen Ausgangspunkt und baut nicht auf den Positionen des vorherigen Laufs auf.

Anschliessend wird jeder Handelstag der Reihe nach ausgewertet. Zuerst bucht GT fällige Konto- und Wertpapier-Daueraufträge, danach Ausschüttungen und Ereignisse am Instrumentenende. Anschliessend bewertet es die Umgebung zu den Schlusskursen dieses Tages, prüft eine allfällige Umschichtung und wertet die Strategien der einzelnen Wertpapiere aus. Fällig ist eine Umschichtung nur an einem Kontrollzeitpunkt, der sich nach dem aus der Anzahl Umschichtungen pro Jahr abgeleiteten Intervall wiederholt; wie Kontrollzeitpunkt und Toleranz zusammenwirken, beschreibt [Portfolio-Neugewichtung](../strategy/rebalancing/). Ein Auftrag, der am Schluss eines Tages entsteht, wird nie am selben Tag ausgeführt, sondern zum Schlusskurs des nächsten zulässigen Handelstages dieses Wertpapiers. Vor dem Startdatum und nach dem Enddatum eines Instruments findet keine Marktausführung statt; bei einer direkten Anleihe ist bereits das Enddatum der Rückzahlung vorbehalten.

{{< mermaid >}}
graph TD
    A["Eröffnungszustand wiederherstellen"] --> B["Nächster Handelstag"]
    B --> C["Daueraufträge, Ausschüttungen und Instrumentenende verarbeiten"]
    C --> D["Umgebung zum Schlusskurs bewerten"]
    D --> E["Umschichtung fällig?"]
    E --> F["Strategien der Wertpapiere auswerten"]
    F --> G["Aufträge zum nächsten zulässigen Schlusskurs ausführen"]
    G -->|ja| B
    G -->|nein| H["Kennzahlen berechnen und Lauf abschliessen"]
{{< /mermaid >}}

{{% notice style="info" title="Nur vergangene Kurse" %}}
Eine Entscheidung sieht ausschliesslich Kurse bis und mit dem Tag, an dem sie getroffen wird. Spätere Kurse und der aktuelle Tageskurs sind für die Wiederholung nicht erreichbar. Nur deshalb ist ein Ergebnis überhaupt aussagekräftig.
{{% /notice %}}

## Fortschritt verfolgen und abbrechen
Der Dialog zeigt den **Status** des Laufs sowie unter **Handelstage** und **Ausgewertet**, wie weit er gekommen ist. Mit **Aktualisieren** holen Sie den aktuellen Stand; solange ein Lauf arbeitet, geschieht dies auch selbständig. **Wiederholung abbrechen** stoppt den Lauf nach dem Tag, den er gerade auswertet. Die Umgebung bleibt bis zum tatsächlichen Ende des Laufs gesperrt, nicht nur bis zum Abbruchwunsch. Die bis dahin gebuchten Ausführungen bleiben in der Umgebung erhalten, Kennzahlen werden für einen abgebrochenen Lauf jedoch keine ausgewiesen.

| Status | Bedeutung |
|---|---|
| **In Bearbeitung** | Der Lauf ist eingereiht oder arbeitet gerade. |
| **Abgeschlossen** | Der Lauf hat sein Enddatum erreicht; die Kennzahlen sind gültig. |
| **Abgebrochen** | Sie haben den Lauf gestoppt. |
| **Fehlgeschlagen** | Der Lauf wurde durch einen Fehler beendet; der Grund steht im Dialog. |
| **Unterbrochen** | Der Server wurde während des Laufs beendet. Ein solcher Lauf wird nicht fortgesetzt, sondern neu gestartet. |

Es kann pro Simulationsumgebung immer nur eine Wiederholung gleichzeitig laufen, und der Server führt insgesamt nur wenige gleichzeitig aus. Wird dieses Kontingent erschöpft, meldet GT dies und Sie versuchen es später erneut.

## Ergebnis
Der Abschnitt **Durchführung** hält fest, womit der Lauf gerechnet hat: **Status**, **Eröffnungsdatum**, **Enddatum**, die beiden gewählten Optionen, **Gestartet**, **Beendet** sowie die **Ersatzfrist für Dividendenzahlungen (Kalendertage)**, siehe [Ausschüttungen und Instrumentenende](#ausschüttungen-und-instrumentenende). Ein abgeschlossener Lauf weist unter **Ergebnis** die folgenden Kennzahlen aus. Sie beziehen sich auf das Vermögen der Simulationsumgebung, das an jedem ausgewerteten Tag zu dessen Schlusskursen bewertet wird. Der eingebrachte Eröffnungsbestand ist Kapital und kein Gewinn. Einzahlungen und Auszahlungen durch [Konto-Daueraufträge](./standingorders/) werden ebenfalls als externe Kapitalflüsse statt als Ertrag behandelt.

| Kennzahl | Bedeutung |
|---|---|
| **Gesamtrendite** | Veränderung des Vermögens vom Eröffnungstag bis zum Enddatum. |
| **Annualisierte Rendite** | Die Gesamtrendite auf ein Kalenderjahr hochgerechnet. |
| **Maximaler Rückgang** | Der grösste Rückgang gegenüber einem zwischenzeitlichen Höchststand des Vermögens. |
| **Maximale Rückgangsdauer (Tage)** | Die längste Zeit in Kalendertagen, während der das Vermögen unter einem früheren Höchststand lag: vom Höchststand bis zum ersten Tag, an dem es ihn wieder erreicht. Gezählt wird jede solche Phase, nicht nur jene des grössten Rückgangs. Ist der Höchststand am Enddatum noch nicht wieder erreicht, zählt die Phase bis zum Enddatum. Ein- und Auszahlungen beenden oder verkürzen eine Phase nicht. |
| **Sharpe-Ratio** | Mittlere Tagesrendite geteilt durch deren Streuung, auf ein Jahr hochgerechnet. |
| **Gezahlte Bruttodividenden (Mandantenwährung)** | Summe der bis zum Enddatum ausbezahlten Dividenden vor Steuerabzügen. |
| **Offene Bruttodividenden (Mandantenwährung)** | Dividenden, deren Anspruch bis zum Enddatum entstanden ist, deren Zahltag aber erst danach liegt. |
| **Abgeschlossene Geschäfte** | Anzahl vollständig geschlossener Engagements. Eine am Enddatum noch offene Position zählt nicht mit; ihr Wert steckt im Schlussvermögen. |
| **Gewinnbringende Geschäfte** und **Verlustbringende Geschäfte** | Wie viele der abgeschlossenen Geschäfte mehr eingebracht als gekostet haben und wie viele weniger. |

Eine Kennzahl, die sich nicht bestimmen lässt, bleibt leer statt null zu zeigen. Ein Vermögen, das sich nie bewegt hat, ergibt keine Sharpe-Ratio, und ein Lauf mit nur einem ausgewerteten Tag ergibt keine Rendite.

### Equity-Kurve
Ein abgeschlossener Lauf zeigt über den Menüeintrag **Equity-Kurve** den Verlauf des Vermögens unterhalb der Ansicht als Diagramm. Solange ein Lauf noch läuft, abgebrochen wurde oder gescheitert ist, bleibt der Eintrag deaktiviert. Das Diagramm enthält zwei Linien. **Vermögen** ist das Vermögen der Simulationsumgebung zum Schlusskurs jedes ausgewerteten Tages, und zwar dieselbe Reihe, aus der GT Rendite, maximalen Rückgang und Rückgangsdauer berechnet. **Investiertes Kapital** beginnt beim Vermögen am Eröffnungstag und verändert sich nur durch Einzahlungen und Auszahlungen, etwa aus [Konto-Daueraufträgen](./standingorders/). Der Abstand zwischen beiden Linien ist deshalb das, was die Wiederholung bis zu diesem Tag verdient oder verloren hat; eine Einzahlung hebt beide Linien gleichermassen an und erscheint nicht als Gewinn.

Ein Tag, den GT nicht vollständig bewerten konnte, weil etwa ein Schlusskurs oder ein Wechselkurs fehlte, ist kein Punkt der Kurve; der [Verlauf der Wiederholung](#verlauf-der-wiederholung) nennt ihn. Fällt das Enddatum auf keinen Handelstag, kommt seine Bewertung als letzter Punkt hinzu. Mit dem Schieberegler unter dem Diagramm lässt sich ein Zeitabschnitt vergrössern.

## Berechnungsannahmen
Kennzahlen sind nur so aussagekräftig wie die Annahmen, unter denen sie entstanden sind. GT zeichnet diese deshalb mit jedem Lauf auf und führt sie beim Ergebnis auf.

Ausgeführt wird zum Schlusskurs des nächsten zulässigen Handelstages des Wertpapiers und ohne Kursabweichung bei der Ausführung. Ist für das verwendete Depot oder dessen Handelsplattform Plan ein Gebührenmodell konfiguriert, werden dessen Courtagen und allfällige Depotgebühren berücksichtigt; andernfalls fallen keine an. Deckt das Modell ein Ausführungsdatum nicht ab, bricht die Wiederholung ab, statt das Geschäft kostenlos zu buchen. Einzelheiten stehen unter [Gebühren in der Wiederholung](./fees/). Gewinn oder Verlust eines Geschäfts wird in der Währung des Wertpapiers gemessen. Die Sharpe-Ratio rechnet gegenüber einem risikolosen Zins von null; die Rendite wird über Kalendertage annualisiert, die Streuung über 252 Handelstage.

### Geldkonten und Währungen
Ein Auftrag wird immer von genau einem Geldkonto bezahlt und nie auf mehrere Konten aufgeteilt. Ein Wertpapier, das die Umgebung bereits hält, bleibt bei dem Depot und dem Geldkonto, über die es eröffnet oder zuerst gekauft wurde. Für ein neues Wertpapier gilt das **Primäre Depot** beziehungsweise **Sekundäre Depot** des Wertpapiers oder seiner Anlageklasse im regelbasierten Handel, sofern dieses Depot das Wertpapier an diesem Tag handeln darf. Andernfalls wählt GT selbst: Zuerst kommen die Portfolios, die ein Geldkonto in der Währung des Wertpapiers führen. Innerhalb eines Portfolios fragt GT zuerst das Geldkonto in der Währung des Wertpapiers an, dann jenes in der Währung der Umgebung und danach die übrigen. Genommen wird das erste Konto, das den ganzen Auftrag bezahlen kann.

Ein Kauf über ein Geldkonto in einer anderen Währung braucht keinen vorgängigen Devisenwechsel. Die Umrechnung geschieht innerhalb der Kauf- oder Verkaufstransaktion selbst, und zwar zum Schlusskurs des Währungspaars am Ausführungstag, also am gleichen Tag wie der Kurs des Wertpapiers. Enthält das festgehaltene Gebührenmodell einen passenden Währungstarif, wird dessen prozentualer Aufschlag angewendet; andernfalls gilt der Schlusskurs und die Tarifabdeckung wird ausgewiesen. Zeitliche Kursabweichungen innerhalb eines Tages werden nicht modelliert. Siehe [Aufschläge bei der Währungsumrechnung](./fees/#aufschläge-bei-der-währungsumrechnung). Fehlt der Wechselkurs an diesem Tag, wird der Auftrag mit dem Grund «Kein Wechselkurs für die Abrechnungswährung» nicht ausgeführt.

Folgen mehrere Käufe aufeinander, kann ein früherer Kauf das Geldkonto in der Währung des Wertpapiers aufbrauchen. Ein späterer Kauf eines anderen Wertpapiers in derselben Währung wird dann vollständig über ein anderes Konto abgerechnet, etwa über das Konto in der Währung der Umgebung, mit der Umrechnung in der Transaktion. Voraussetzung ist, dass dieses Konto den ganzen Auftrag bezahlen kann.

Kann kein einzelnes Konto den Auftrag bezahlen, obwohl die Umgebung insgesamt genügend Geld hat, bucht GT nur den fehlenden Betrag von anderen Geldkonten der Umgebung um. Ziel ist bei einem neuen Wertpapier das Geldkonto in der Währung der Umgebung desselben Portfolios, bei einem bereits gehaltenen dessen eigenes Geldkonto. Das Geld stammt zuerst aus Konten in der Währung des Ziels, dann aus dem gleichen Portfolio und erst danach aus anderen Portfolios. Die Überweisung wird am Entscheidungstag gebucht, weil Geld, das erst am Ausführungstag eintrifft, den Kauf noch nicht decken würde. Umgerechnet wird deshalb zum Schlusskurs des Entscheidungstages. Im Verlauf erscheint die Umbuchung als **Geld für einen Auftrag umgebucht**. Ab der ersten solchen Umbuchung entspricht die Währungsaufteilung der Umgebung nicht mehr jener bei der Eröffnung. Liegt das fehlende Geld in einer Fremdwährung, kann es zweimal umgerechnet werden: bei der Umbuchung in die Währung der Umgebung und beim Kauf zurück in die Währung des Wertpapiers. Beide Umrechnungen können den jeweils gültigen prozentualen Aufschlag enthalten. Der Finanzierungstransfer verwendet den Tarif des begünstigten Depots; zusätzlich wirkt sich die Veränderung des Mittelkurses zwischen den beiden Tagen aus.

Reicht das Geld auch danach nicht, wird der Kauf auf den bezahlbaren Umfang gekürzt («Auf den vom Geldkonto bezahlbaren Betrag reduziert»). Ist nicht einmal eine handelbare Einheit bezahlbar, unterbleibt er («Die Umgebung hat für diesen Auftrag kein Geld mehr»). Für Aufträge wird nur bei Umschichtung und Ersterwerb umgebucht. Aufträge einer Strategie wie des [Mean Reversion Dip](../strategy/meanreversiondip/) werden nur über das gewählte Konto bezahlt und gegebenenfalls gekürzt. Der Erlös eines Verkaufs ist erst ab dem folgenden Tag verfügbar. Einen Kauf einer Umschichtung, der deshalb zu kurz ausfiel, vervollständigt GT an den folgenden Handelstagen.

{{< mermaid >}}
graph TD
    A["Kaufauftrag"] --> B{"Kann ein einzelnes Geldkonto<br>den ganzen Auftrag bezahlen?"}
    B -->|ja| C["Kauf über dieses Konto,<br>bei Fremdwährung mit Umrechnung<br>zum Schlusskurs des Ausführungstages"]
    B -->|nein| D["Fehlenden Betrag am Entscheidungstag<br>von anderen Geldkonten umbuchen"]
    D --> E{"Jetzt genug Geld?"}
    E -->|ja| C
    E -->|nein| F["Auftrag kürzen oder<br>nicht ausführen"]
{{< /mermaid >}}

### Sollzinsen
Ein Konto der Simulationsumgebung, dessen **Sollzins** über null liegt, wird für jeden Kalendertag verzinst, an dem es am Tagesende im Minus steht. GT rechnet nach der üblichen Konvention für Kontokorrentkredite: negativer Saldo mal Sollzins durch 360. Ein Wochenende oder Feiertag zählt mit dem Saldo des letzten Tages davor. Der aufgelaufene Zins wird am letzten Kalendertag jedes Monats und am Enddatum als Kontozins mit negativem Betrag belastet; ab dem Folgetag ist er selbst Teil des überzogenen Saldos und wird mitverzinst. Der Eröffnungstag gehört zum Ausgangszustand und wird nicht verzinst.

Massgebend ist der Sollzins beim Start der Wiederholung. Ein Konto mit einem Sollzins von null darf überzogen werden, kostet aber nichts. Sollzinsen sind Aufwand und kein Kapitalfluss: Sie senken Vermögen und Rendite, nicht aber das investierte Kapital der [Equity-Kurve](#equity-kurve). Lässt sich eine Zinsbelastung nicht buchen, bricht die Wiederholung ab, weil ihr Ergebnis ohne den Zins falsch wäre.

### Ertragsabstimmung und Hinweise
Wenn Steuermodelle oder erzeugte Anleihencoupons beteiligt sind, zeigt ein abgeschlossener Lauf zusätzlich die **Ertragsabstimmung**. Für Zins/Dividende und Wertpapierzinsen werden **Bruttozahlungen**, **Einbehaltene Steuer** und **Nettozahlungen** getrennt ausgewiesen. Offene Ansprüche erscheinen als **Bruttoforderungen und Couponabgrenzung**, **Geschätzter Steuerabzug** sowie **Nettoforderungen und Couponabgrenzung**. Die **Abstimmung von Wechselkursen und Rundung** macht Differenzen sichtbar, welche bei Umrechnung und Rundung zwischen Brutto-, Steuer- und Nettobeträgen entstehen können.

Unter **Steuer- und Ertragshinweise** fasst GT gleichartige Hinweise zusammen und zeigt deren Anzahl sowie den betroffenen Zeitraum. Fehlt für ein Land ein Modellabschnitt, ein Zeitraum, eine benötigte Angabe oder eine zutreffende Regel, lässt GT nur diesen Länderbeitrag weg; gültige Beiträge anderer Länder bleiben erhalten und die Wiederholung läuft weiter. Eine ausdrückliche Nullregel gilt dagegen als vollständige Abdeckung. Übersteigt ein geschätzter Steuerabzug den Bruttoertrag, wird dieser Abzug nicht gebucht, damit kein negativer Nettoertrag entsteht. Bei ausgeschalteten Steuermodellen werden keine Steuerhinweise erzeugt.

{{% notice style="warning" title="Kein Ersatz für eine echte Handelssimulation" %}}
Kursabweichungen bei der Ausführung werden nicht berücksichtigt. Ohne konfiguriertes Gebührenmodell fehlen zudem die Transaktionskosten. Ein Ergebnis kann deshalb günstiger ausfallen als ein tatsächlicher Handel, besonders bei häufigen Geschäften.
{{% /notice %}}

## Ausschüttungen und Instrumentenende
Eine gespeicherte Dividende begründet am Ex-Tag einen Anspruch für die Stücke, die vor diesem Tag gehalten wurden, und wird am Zahltag als Geld gebucht. Fehlt der Zahltag, verwendet GT den Ex-Tag zuzüglich einer Ersatzfrist in Kalendertagen, die der Administrator mit `gt.simulation.dividend.payment.delay.days` festlegt (1 bis 32, vorgegeben sind 16). Der Lauf hält die verwendete Frist fest. Bei direkten Festzinsanleihen kann GT zusätzlich regelmässige Coupons aus den Anleiheangaben erzeugen, die im Reiter **Anleihensimulation** des [Wertpapiers]({{% relref "/watchlistinstrument/instrument/securityderived/security" %}}) erfasst werden. Ohne gespeicherte Zinstagekonvention gilt die übliche Konvention der Währung. Gespeicherte Ausschüttungen haben Vorrang. Die Erzeugung unterstützt regelmässige Festzinsperioden; abweichende erste oder letzte Zinsperioden, variable Zinssätze und Verschiebungen auf Bankarbeitstage werden nicht nachgebildet.

Eine direkte Anleihe wird an ihrem Enddatum zum Nennwert zurückgezahlt. Fehlt ein Nennwert je Einheit, verwendet GT 100. Diese Kapitalrückzahlung erfolgt unabhängig von **Regelmässige Anleihencoupons erzeugen**. Werden regelmässige Coupons erzeugt und fällt die Rückzahlung auf einen Coupontermin, wird der volle Coupon separat gebucht. Liegt sie zwischen zwei Couponterminen, enthält die Rückzahlung die bis dahin aufgelaufenen Stückzinsen. Für die Rückzahlung selbst fallen keine Depotgebühr und keine Transaktionssteuer an; ein Steuerabzug auf den Coupon bleibt davon unberührt.

Andere Positionen ohne Margin werden am Instrumentenende zum letzten verfügbaren Schlusskurs an oder vor diesem Datum geschlossen. Dafür gelten die normalen Gebühren- und Steuermodelle. Fehlt ein verwendbarer Kurs, bleibt die Position offen und der Verlauf zeigt **Nicht verfügbar**. Liegt das Instrumentenende vor dem ersten Handelstag der Wiederholung, holt GT Rückzahlung oder Schluss am ersten ausgewerteten Tag nach.

Ist ein Wertpapier unter [Instrument ohne Kursdaten]({{% relref "/basedata/bankruptsecurity" %}}) mit **Kein Handel seit** markiert, handelt die Wiederholung es ab diesem Tag nicht mehr: Weder Strategien noch Umschichtungen kaufen oder verkaufen es. Regelmässige Anleihencoupons werden ab diesem Tag nicht mehr erzeugt und keine Stückzinsen mehr abgegrenzt; gespeicherte Ausschüttungen bleiben dagegen erhalten. Liegt das Datum vor dem Instrumentenende, entfallen auch die Rückzahlung bei Fälligkeit und der Schluss bei Instrumentenende. Die Position bleibt gehalten und wird zum letzten Kurs bewertet. Massgebend ist nur **Kein Handel seit**; **Keine Daten seit** hat auf die Wiederholung keinen Einfluss. Der Lauf hält das Datum beim Start fest, eine spätere Änderung wirkt erst auf einen neuen Lauf.

## Verlauf der Wiederholung
Unter dem Ergebnis führt GT auf, was an welchem Tag geschehen ist. Der Verlauf erklärt einen Lauf, statt ihn nur zusammenzufassen: Er hält ebenso fest, weshalb nichts geschehen ist, denn ein Lauf ohne Geschäfte und ein Lauf ohne Kursdaten sähen sonst gleich aus.

| Spalte | Inhalt |
|---|---|
| **Datum** | Der Schlusstag, zu dem der Eintrag gehört. |
| **Ereignis** | Die Art des Eintrags, siehe unten. |
| **Begründung** | Weshalb so entschieden wurde, beispielsweise ein ausgelöster Einstieg oder eine erschöpfte Investitionsgrenze. |
| **Anzahl**, **Kurs**, **Betrag**, **Währung** | Umfang und Preis, soweit für den Eintrag vorhanden. |
| **Einzelheiten** | Ergänzende Angaben, etwa der Grund eines Abbruchs. |

Die Einträge **Wiederholung gestartet** und **Wiederholung beendet** umschliessen den Lauf. Dazwischen steht **Glattstellung nach Eröffnung** für eine CFD-, Forex- oder gehebelte Position des Eröffnungsbestands, die vor dem eigentlichen Simulationshandel geschlossen wird, **Konto-Dauerauftrag** für eine wiederkehrende Kontobuchung, **Wertpapier-Dauerauftrag** für einen wiederkehrenden Kauf oder Verkauf eines Wertpapiers, **Entscheid** für einen ausgelösten Ein- oder Ausstieg und **Ausführung** für dessen Buchung am folgenden Handelstag. **Umschichtung fällig** und **Ausführung der Umschichtung** halten dasselbe für die Angleichung an Ihre Zielgewichtungen fest. **Ersterwerb fällig** und **Ausführung des Ersterwerbs** erscheinen nur bei einer Umgebung, die am Eröffnungsdatum keine Positionen hielt: Sie kauft sich zu Beginn des Laufs einmalig in ihre Zielgewichtungen ein, und erst danach beginnt das Intervall der Umschichtung zu zählen. Eine Umgebung, die mit Positionen eröffnet, geht dagegen direkt zu ihrer ersten Umschichtung über. **Geld für einen Auftrag umgebucht** kennzeichnet eine Überweisung zwischen zwei Geldkonten der Umgebung, damit ein Auftrag bezahlt werden kann; sie nennt das Wertpapier, für das sie erfolgte. **Dividendenanspruch** und **Dividendenzahlung** halten den am Ex-Tag entstandenen Anspruch und dessen Auszahlung fest. **Rückzahlung bei Fälligkeit** kennzeichnet die Rückzahlung einer direkten Anleihe, **Schluss bei Instrumentenende** das Schliessen einer anderen Position. **Depotgebühr** hält eine abgerechnete Depotgebühr fest, **Sollzins** den belasteten Zins eines überzogenen Kontos, **Courtagenguthaben verwendet** eine durch ein Guthaben verminderte Courtage. **Blockiert** bedeutet, dass eine Handlung bewusst unterblieben ist, etwa wegen einer Sperrfrist oder einer ausgeschöpften Investitionsgrenze. **Nicht verfügbar** bedeutet, dass gar nicht entschieden werden konnte, weil etwa Kursdaten oder ein Wechselkurs fehlten.

## Einschränkungen
CFDs, Forex und Instrumente mit einem Hebelfaktor ungleich 1 werden in einer Wiederholung nicht gehandelt. Enthält der Eröffnungsbestand solche Positionen, werden sie vor dem eigentlichen Simulationshandel glattgestellt (**Glattstellung nach Eröffnung**), danach nicht wieder gekauft, und ihre Gewichtung wird für diesen Lauf auf die übrigen Wertpapiere verteilt. Ein Auftrag, für den innerhalb der Wiederholung kein zulässiger Handelstag mehr folgt, bleibt unausgeführt. Fehlen für einen Tag Schlusskurse oder Wechselkurse, wird für das betroffene Wertpapier nicht entschieden; der Lauf setzt seine Arbeit dennoch fort und hält den Grund im Verlauf fest.

Umgebucht werden kann nur, wenn ein Währungspaar in Richtung vom abgebenden zum empfangenden Konto besteht. Üblicherweise führt GT Währungspaare von einer Fremdwährung in die Währung der Umgebung, beispielsweise USD/CHF, aber nicht umgekehrt. Geld lässt sich deshalb in das Konto der Umgebungswährung bringen, kaum aber aus ihm in ein Fremdwährungskonto. Wird ein bereits gehaltenes Wertpapier über ein Fremdwährungskonto abgerechnet, kann dieses Konto in der Regel nur aus Konten derselben Währung aufgefüllt werden. Reicht deren Geld nicht, wird der Kauf gekürzt, auch wenn die Umgebung in ihrer eigenen Währung noch genügend Geld hätte.

Ausführungen einer Umschichtung tragen keine Zuordnung zu einer Strategie, da die Buchhaltung eine solche nur für Strategien der taktischen Handelsebene führt. Die zugehörige Strategie ist stattdessen im Verlauf ersichtlich. Wird ein Wertpapier gleichzeitig von einer Anlageklasse mit Zielgewichtung und von einer eigenen Strategie beansprucht, meldet die Strategie dieses Wertpapier als nicht entscheidbar. Ordnen Sie ein Wertpapier deshalb nur einer Anlageklasse zu.

{{% notice style="info" title="Ergebnisse werden ersetzt" %}}
Pro Simulationsumgebung wird genau ein Lauf aufbewahrt. Eine erneute Wiederholung stellt den Eröffnungszustand wieder her und ersetzt das bisherige Ergebnis samt Verlauf. Möchten Sie zwei Zeiträume nebeneinander betrachten, legen Sie eine zweite Simulationsumgebung an. **Simulation löschen** entfernt auch deren Lauf; solange eine Wiederholung arbeitet, ist das Löschen abgelehnt. Was in einer Umgebung sonst möglich ist und was darin geschützt ist, steht unter [In der Simulationsumgebung arbeiten](./environment/).
{{% /notice %}}
