---
title: "Portfolio-Neugewichtung"
date: 2026-09-29T12:00:00+02:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="In Bearbeitung - Zielversion V0.38.0" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Die Portfolio-Neugewichtung (Rebalancing) vergleicht die tatsächliche Aufteilung Ihres Vermögens mit den Zielgewichtungen, die Sie im Baum der regelbasierten Strategie hinterlegt haben. Ist das konfigurierte Intervall abgelaufen und weicht eine Anlageklasse weiter von ihrem Ziel ab als Sie erlauben, erhalten Sie eine Empfehlung, welche Wertpapiere Sie in welchem Umfang kaufen oder verkaufen müssten, um die gewünschte Aufteilung wiederherzustellen.

GT führt diese Empfehlungen niemals selbst aus. Es wird keine Transaktion gebucht und kein Auftrag erteilt; Sie entscheiden, ob und wann Sie handeln.

## Schritt für Schritt
1. Erstellen Sie unter **Regelbasierter Handel** eine Portfoliobasierte Strategie, entweder von Hand, mit **Strategie aus Portfolio erstellen** aus Ihren aktuellen Beständen oder mit **Strategie aus Watchlist erstellen**, siehe [Regelbasierter Handel](../../algo/).
2. Fügen Sie Anlageklassen und Wertpapiere hinzu und legen Sie deren **Gewichtung** fest. Prüfen Sie die Spalte **Summe der Gewichtungen**: Die Anlageklassen und, getrennt davon, die Wertpapiere jeder Anlageklasse müssen 100% ergeben; **Alle Prozentanteile normalisieren** stellt dies her.
3. Wählen Sie auf der Portfoliobasierten Strategie **Erstellen Strategie Definition**, wählen Sie unter **Strategiename** die **Portfolio-Neugewichtung** und erfassen Sie deren Parameter.
4. Stellen Sie sicher, dass die Portfoliobasierte Strategie aktiv ist.
5. Öffnen Sie den Bericht [Anlageklassen mit Cash](../../../reportportfolio/securitycashaccountreport/) und wählen Sie unter **Mit Strategie vergleichen** Ihre Strategie, um Ziele, Abweichungen und vorgeschlagene Geschäfte zu sehen.
6. Ist ein Kontrollzeitpunkt erreicht, lesen Sie die Meldungen in Ihrem Posteingang und buchen die Geschäfte selbst als gewöhnliche Transaktionen.

## Wo die Zielgewichtungen stehen
Die Zielgewichtungen sind keine Einstellung der Strategie, sondern die Spalte **Gewichtung** im Baum der regelbasierten Strategie. Jede Ebene teilt dabei den Betrag ihrer übergeordneten Ebene auf, und die Gewichtungen von Geschwistern müssen zusammen 100% ergeben. Über die Menüpunkte zur Normalisierung der Gewichtungen lässt sich das bei Bedarf automatisch herstellen.

Zuoberst steht der Wert **Maximal investiert**: der Anteil Ihres Nettovermögens, den die Strategie überhaupt investieren darf. Was darunter steht, teilt genau diesen Betrag auf und nicht noch einmal das Gesamtvermögen.

Ein Beispiel mit einem Nettovermögen von 100'000:

| Ebene | Gewichtung | Betrag | Anteil am Gesamtvermögen |
|---|---:|---:|---:|
| Maximal investiert | 48.2% | 48'200 | 48.2% |
| Anlageklassen-Gruppe innerhalb dieses Budgets | 50% | 24'100 | 24.1% |
| Wertpapier innerhalb dieser Gruppe | 60% | 14'460 | 14.46% |

Im Bericht sehen Sie immer die rechte Spalte, also den Anteil am Gesamtvermögen. Nur so lassen sich eine Gruppe und ein einzelnes Wertpapier miteinander vergleichen.

{{< mermaid >}}
graph TD
    T["Portfoliobasierte Strategie<br/>Maximal investiert 48.2% = 48'200"] --> A["Anlageklassen-Gruppe Aktien<br/>50% = 24'100 = 24.1% des Vermögens"]
    T --> B["Anlageklassen-Gruppe Anleihen<br/>50% = 24'100 = 24.1% des Vermögens"]
    A --> A1["Welt-ETF<br/>60% = 14'460 = 14.46% des Vermögens"]
    A --> A2["US-ETF<br/>40% = 9'640 = 9.64% des Vermögens"]
{{< /mermaid >}}

## Parameter der Strategie
Die Neugewichtung wird ausschliesslich auf der obersten Ebene, also auf der Portfoliobasierten Strategie selbst, hinterlegt. Eine zweite Neugewichtung auf einer Anlageklassen-Gruppe oder einem Wertpapier gibt es nicht, weil dort bereits die Gewichtung des Baums das Ziel vorgibt und zwei Ziele für dieselbe Position sich widersprechen würden.

| Parameter | Beschreibung |
|---|---|
| **Anzahl Umschichtungen pro Jahr (R)** | Legt fest, wie oft pro Jahr die Aufteilung mit ihren Zielen verglichen wird. Der Abstand zweier Kontrollzeitpunkte beträgt 365 Tage geteilt durch diese Zahl. Zulässige Werte: 1 bis 53. |
| **Abweichung der Anlageklasse (Prozentpunkte des Anlagebudgets)** | Die Toleranz einer Anlageklasse. Erst wenn ihr Betrag um mehr als diesen Anteil des Betrags **Maximal investiert** von ihrem Ziel abweicht, wird sie angepasst. Zulässige Werte: 1 bis 49. |
| **Toleranz der Wertpapiergewichtung (Prozentpunkte)** | Die Bandbreite eines Wertpapiers um seine Gewichtung, in Prozentpunkten des Zielbetrags seiner Anlageklasse. Innerhalb dieser Bandbreite darf eine Anpassung ein Wertpapier über oder unter sein genaues Ziel bewegen. Zulässige Werte: 0 bis 100, vorgegeben sind 5. |
| **Maximal gehandelte Wertpapiere je Anlageklasse** | Wie viele unterschiedliche Wertpapiere einer Anlageklasse an einem Kontrollzeitpunkt höchstens gehandelt werden. Zulässig ist jede ganze Zahl ab 1, vorgegeben sind 3. |

Die beiden letzten Parameter lassen sich für eine einzelne Anlageklasse im Dialog **Strategie Anlageklasse** überschreiben, siehe [Regelbasierter Handel](../../algo/#anlageklasse-hinzufügen-und-bearbeiten). Bleiben die Felder dort leer, gilt die Einstellung der Portfolio-Neugewichtung.

## Wann eine Empfehlung entsteht
Die Parameter beantworten verschiedene Fragen. Die Anzahl Umschichtungen pro Jahr legt fest, wie oft die Aufteilung überhaupt verglichen wird: Ein Kontrollzeitpunkt wiederholt sich alle 365 Tage geteilt durch diese Zahl, auf ganze Tage gerundet; vier Umschichtungen pro Jahr ergeben also alle 91 Tage einen Kontrollzeitpunkt. Die Abweichung der Anlageklasse entscheidet an einem solchen Kontrollzeitpunkt, welche Anlageklassen angepasst werden: nur jene, deren Betrag um mehr als die Toleranz vom Ziel abweicht. Die Anpassung einer solchen Anlageklasse umfasst dann ihre gesamte Abweichung. Wie ein dafür nötiger Kauf bezahlt wird, beschreibt der Abschnitt [Finanzierung der Käufe](#finanzierung-der-käufe).

Die Wertpapiertoleranz und die Handelsgrenze bestimmen anschliessend, über welche Wertpapiere diese Anpassung erfolgt. GT reiht die Wertpapiere der Anlageklasse: Muss die Anlageklasse wachsen, stehen die am stärksten untergewichteten Wertpapiere zuvorderst, muss sie schrumpfen, die am stärksten übergewichteten; bei Gleichstand entscheidet das höhere Handelsvolumen. Der Reihe nach nimmt jedes Wertpapier so viel der Anpassung auf, wie seine Bandbreite zulässt, bis die Anpassung vollständig verteilt oder die Handelsgrenze erreicht ist. So wird eine Anlageklasse mit wenigen Geschäften zurückgeführt, statt jedes Wertpapier exakt auf sein Ziel zu setzen. Bleibt ein Rest, weil die Handelsgrenze erreicht ist oder die Bandbreiten nicht genügen, weist der Bericht dies mit **Maximale Anzahl gehandelter Wertpapiere erreicht** beziehungsweise **Restanpassung innerhalb der zulässigen Wertpapierbandbreiten nicht möglich** aus. Nicht berücksichtigte Wertpapiere tragen die Begründung **Für diese Anpassung der Anlageklasse nicht ausgewählt**.

Eine Empfehlung entsteht deshalb nur, wenn ein Kontrollzeitpunkt erreicht ist und mindestens eine Anlageklasse ausserhalb ihrer Toleranz liegt. Eine Abweichung zwischen zwei Kontrollzeitpunkten ist im Bericht sichtbar und löst keine Empfehlung aus; ihren Beginn meldet GT aber, siehe [Allokation ausserhalb der Toleranz](#allokation-ausserhalb-der-toleranz). Ein Kontrollzeitpunkt, an dem alle Anlageklassen innerhalb ihrer Toleranz liegen, verstreicht ohne Meldung; das nächste Intervall zählt trotzdem ab ihm. Die allererste Auswertung einer Portfoliobasierten Strategie ist immer ein Kontrollzeitpunkt.

{{< mermaid >}}
graph TD
    A["Tägliche Auswertung"] --> B{"Kontrollzeitpunkt erreicht?"}
    B -->|Nein| C["Bericht aktualisiert,<br/>eine neue Abweichung wird nur gemeldet"]
    B -->|Ja| D{"Anlageklasse ausserhalb<br/>ihrer Toleranz?"}
    D -->|Nein| E["Keine Meldung,<br/>nächstes Intervall beginnt"]
    D -->|Ja| H{"Genügt das Bargeld<br/>für die Käufe?"}
    H -->|Nein| I["Übergewichtete Anlageklassen<br/>anteilig reduzieren"]
    H -->|Ja| G["Wertpapiere innerhalb ihrer Bandbreite<br/>und der Handelsgrenze auswählen"]
    I --> G
    G --> F["Meldung je ausgewähltem Wertpapier,<br/>nächstes Intervall beginnt"]
{{< /mermaid >}}

Entsteht eine Empfehlung, erhalten Sie für jedes ausgewählte Wertpapier eine Meldung im Posteingang, sofern die Zustellung von Alarmen eingeschaltet ist. Die Meldungen entstehen in der Reihenfolge der Abweichung, beginnend mit den Positionen, die am weitesten über ihrem Ziel liegen, damit Sie Positionen zuerst abbauen und erst danach andere aufbauen können. Der Vergleich selbst ist jederzeit im Bericht [Anlageklassen mit Cash](../../../reportportfolio/securitycashaccountreport/) einsehbar, auch wenn nichts fällig ist.

## Allokation ausserhalb der Toleranz
Zwischen zwei Kontrollzeitpunkten schlägt GT keine Geschäfte vor, meldet aber, wenn die Aufteilung ihre Toleranz verlässt. Bei der täglichen Auswertung der Strategie, die der [Portfolioüberwachung](../../algo/#portfolioüberwachung) zugewiesen ist, entsteht eine Meldung **Allokation ausserhalb der Toleranz**, sobald eine Anlageklasse um mehr als ihre **Abweichung der Anlageklasse** von ihrem Ziel abweicht, ein Wertpapier seine **Toleranz der Wertpapiergewichtung** verlässt oder das Bruttoengagement den Betrag **Maximal investiert** überschreitet. Die Meldung nennt die betroffene Anlageklasse, das Wertpapier oder die Portfoliobasierte Strategie mit Soll, Ist und Abweichung.

Gemeldet wird der Beginn einer solchen Lage. Bleibt eine Anlageklasse wochenlang ausserhalb ihrer Toleranz, erhalten Sie eine einzige Meldung; eine weitere folgt erst, wenn sie zwischendurch wieder innerhalb lag. Eine Lage, die bei der ersten Auswertung schon besteht, wird sofort gemeldet. Die Meldung ist nur ein Hinweis: Sie macht keinen Kontrollzeitpunkt fällig, verschiebt keinen und schlägt kein Geschäft vor. Taktische Gruppen melden nichts, weil ihr Ziel eine Obergrenze ist. Die Meldungen folgen dem Schalter **Alarme aktiviert** der Neugewichtung, erscheinen in der **Alarmdiagnose** unter **Benachrichtigungen** und werden auf der Dashboard-Karte **Rebalancing-Überwachung** als **Ausserhalb der Toleranz** gezählt.

## Finanzierung der Käufe
Eine Neugewichtung verteilt das bereits investierte Vermögen um und bringt kein neues Geld ins Portfolio. Muss eine untergewichtete Anlageklasse aufgestockt werden, wird der Kauf deshalb zuerst aus dem vorhandenen Bargeld bezahlt. Reicht dieses nicht, werden die übergewichteten Anlageklassen im Verhältnis zu ihrem Übergewicht reduziert, bis der fehlende Betrag gedeckt ist. Dies gilt auch dann, wenn diese Anlageklassen selbst noch innerhalb ihrer Toleranz liegen. In einem voll investierten Portfolio ist das der Normalfall: Einer grossen Abweichung nach unten stehen meist mehrere kleinere Abweichungen nach oben gegenüber, von denen keine für sich die Toleranz überschreitet. Ohne diese Verkäufe liesse sich der Kauf gar nicht bezahlen.

Eine Anlageklasse wird dabei nie unter ihr eigenes Ziel reduziert. Im Bericht trägt sie die Begründung **Innerhalb der Toleranz, reduziert zur Finanzierung des Kaufs einer untergewichteten Anlageklasse**. Die Auswahl der Wertpapiere folgt denselben Regeln wie bei jeder anderen Anpassung, also der Wertpapiertoleranz und der Handelsgrenze der betroffenen Anlageklasse.

Ein Beispiel mit einem Nettovermögen von 100'000 und einer Abweichung der Anlageklasse von 8 Prozentpunkten:

| Anlageklasse | Ziel | Ist | Abweichung | Anpassung |
|---|---:|---:|---:|---:|
| Anleihen | 30'000 | 20'000 | −10 | Kauf 10'000 |
| Aktien | 30'000 | 36'000 | +6 | Verkauf 6'000 |
| Rohstoffe | 20'000 | 24'000 | +4 | Verkauf 4'000 |
| Gold | 20'000 | 20'000 | 0 | keine |

Nur die Anleihen liegen ausserhalb der Toleranz. Weil kein Bargeld vorhanden ist, bezahlen die Aktien und die Rohstoffe den Kauf im Verhältnis 6 zu 4.

Da der Erlös eines Verkaufs erst nach dessen Abwicklung verfügbar ist, empfiehlt es sich, zuerst die Verkäufe auszuführen und die Käufe danach. In der [historischen Simulation](../../historicalrun/) geschieht dies automatisch: Konnte ein Kauf am Kontrollzeitpunkt mangels verfügbarem Geld nicht vollständig ausgeführt werden, kauft die Simulation an den folgenden Handelstagen nach, bis das Ziel erreicht ist, höchstens aber während fünf Handelstagen. Im Protokoll erscheint ein solcher Tag mit der Begründung **Nachkauf der Käufe des vorangehenden Stichtags**. Der Nachkauf verschiebt den nächsten Kontrollzeitpunkt nicht. Der erste Nachkauftag darf ohne Kauf bleiben, weil der Erlös eines Verkaufs vom Kontrollzeitpunkt oft erst am zweiten Tag verfügbar ist. Bleibt danach ein Tag ohne Kauf, wird der Nachkauf beendet, weil dann auch kein Geld mehr zu erwarten ist.

## Auswertung und Aktivierung
GT wertet die Portfolio-Neugewichtung einmal pro Tag im Hintergrund aus, und zwar zu den Schlusskursen des letzten abgeschlossenen Tages. Ausgewertet wird, solange die Portfoliobasierte Strategie der Portfolioüberwachung zugewiesen ist, siehe [Regelbasierter Handel](../../algo/#portfolioüberwachung); die Portfolio-Neugewichtung selbst ist nach dem Erstellen aktiv. Ist die Strategie nicht einsatzbereit, weil etwa die Gewichtungen einer Ebene nicht 100% ergeben, wird ihre tägliche Auswertung übersprungen. Unter **Mit Strategie vergleichen** lässt sie sich dann nicht auswählen; der Grund erscheint, wenn Sie mit der Maus über den Eintrag fahren. Welche Befunde das bewirken, steht unter [Einsatzbereitschaft der Strategie](../../algo/#einsatzbereitschaft-der-strategie).

Möchten Sie sofort auswerten, beispielsweise nach einer Änderung der Gewichtungen, öffnen Sie im Register **Alarm** des Klienten die **Alarmdiagnose** und wählen **Jetzt auswerten**. Der Plan wird dann auch neu berechnet, wenn er an diesem Tag bereits ausgewertet wurde. Ein Kontrollzeitpunkt wird dadurch jedoch nicht vorgezogen.

## Der Vergleich rechnet mit dem Bruttoengagement
Als Ist-Wert einer Position zählt ihr Bruttoengagement, also der Betrag, mit dem sie am Markt beteiligt ist, ohne Verrechnung von Long- und Short-Positionen. Bei einer gewöhnlichen Kaufposition entspricht das dem Positionswert. Bei einer Short-Position und bei Margin-Produkten ist es mehr als der Betrag, der Ihrem Vermögen zugerechnet wird: Ein Leerverkauf schafft kein zusätzliches Anlagekapital, auch wenn er Geld auf das Konto bringt.

Die empfohlene Aktion ist immer das Geschäft und nicht die Richtung des Engagements. Eine zu grosse Short-Position wird deshalb mit **Kauf** verkleinert und nicht mit **Verkauf**.

Die empfohlene Anzahl ist der Betrag geteilt durch den Preis einer Einheit und wird nicht gerundet. Runden Sie sie selbst auf eine handelbare Menge.

## Taktische Gruppen werden nicht aufgefüllt
Trägt eine Anlageklassen-Gruppe oder eines ihrer Wertpapiere eine eigene Handelsstrategie, so gilt ihre Gewichtung nur als Obergrenze. Die Neugewichtung baut eine solche Position ab, wenn sie zu gross geworden ist, eröffnet oder vergrössert sie aber nie: Wann eingestiegen wird, entscheidet die dortige Strategie. Das nicht genutzte Budget solcher Gruppen wird im Bericht separat ausgewiesen, damit ersichtlich ist, weshalb der Bericht nicht bis zum Wert **Maximal investiert** aufgefüllt wird.

Die Obergrenze gilt für die ganze Gruppe: Trägt nur eines ihrer Wertpapiere einen Mean-Reversion-Dip, füllt die Neugewichtung auch deren übrige Wertpapiere nicht mehr auf. Legen Sie solche Wertpapiere deshalb in eine eigene Gruppe; wie beide zusammenwirken, beschreibt [Neugewichtung und Mean-Reversion-Dip gemeinsam](../../#neugewichtung-und-mean-reversion-dip-gemeinsam). Ein Preis- oder Indikator-Alarm ist keine Handelsstrategie in diesem Sinn und macht eine Gruppe nicht taktisch.

## Wenn die Investitionsgrenze überschritten ist
Steigt das Bruttoengagement über den Wert **Maximal investiert**, etwa weil die Kurse gestiegen sind, meldet der Bericht **Investitionsgrenze überschritten**. In diesem Zustand werden nur noch Empfehlungen ausgegeben, die das Engagement verringern; alle übrigen erscheinen als **Blockiert** mit der entsprechenden Begründung. Dasselbe gilt, wenn das Nettovermögen nicht positiv ist, denn dann lässt sich kein Zielbetrag berechnen.

Ein Portfolio, dessen Bruttoengagement grösser ist als das Nettovermögen, wird weiterhin ausgewertet und dargestellt. Es wird lediglich nicht weiter aufgebaut.

{{% notice style="info" title="Hinweis" %}}
Die Bewertung erfolgt immer auf den Schlusskursen eines abgeschlossenen Handelstages. Der Bericht weist dieses **Bewertungsdatum** aus, damit ersichtlich ist, auf welchen Stand sich der Vergleich bezieht.
{{% /notice %}}
