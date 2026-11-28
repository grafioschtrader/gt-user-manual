---
title: "Abspaltung"
date: 2026-11-20T12:00:00+01:00
lastmod: 2026-11-20T21:00:00+01:00
draft: false
weight: 10
archetype: "default"
---
Bei einer Abspaltung erhalten die Aktionäre des Mutterunternehmens Anteile am neuen Unternehmen. Die Stückzahl des Mutterwertpapiers bleibt gleich. Ein Teil seines Einstands geht auf das abgespaltene Wertpapier über.

Das folgende Diagramm zeigt die Aufgaben von Administrator und Benutzer, von der Erfassung der Abspaltung bis zur Anwendung auf die eigenen Depots:

{{< mermaid >}}
sequenceDiagram
    participant A as Administrator
    participant S as System (GT)
    participant B as Benutzer

    A->>S: Abspaltung erfassen: Mutter, Tochter,<br/>Ex-Tag und Verhältnis
    opt Tochter noch nicht in GT
        S-->>A: Vorschlag aus der Suche bei den Datenanbietern<br/>(Name, Ticker, Konnektoren)
    end
    opt Aufteilung aus Schlusskursen vorschlagen
        S-->>A: Gemeinsamer Kurstag, Schlusskurse,<br/>Wechselkurs und geschätzter Anteil
    end
    A->>S: Speichert Einstandsaufteilung mit Quelle<br/>und deutscher steuerlicher Einordnung
    S->>S: Speichert ein neues Wertpapier und zählt die<br/>Mandanten mit Bestand vor dem Ex-Tag
    S->>S: Hintergrundaufgabe: Splits lesen, Kursbasis bestimmen,<br/>Kurshistorie der Mutter neu laden
    S->>B: Benachrichtigung per interner Mail
    B->>S: Abspaltung anwenden
    S-->>B: Vorschau pro Depot: Stückzahlen, Kurse,<br/>Beträge und vorhandene Buchungen
    B->>S: Wählt pro Depot Buchen oder Überspringen
    S->>S: Erzeugt pro gebuchtem Depot Verkauf und Wiederkauf<br/>der Mutter sowie Kauf der Tochter
    S-->>B: Status: Angewendet
    opt Korrektur
        A->>S: Abspaltung bearbeiten: neue Einstandsaufteilung
        S->>S: Auswertungen rechnen mit dem neuen Anteil,<br/>die Buchungen bleiben unverändert
    end
{{< /mermaid >}}

## Abspaltung erfassen
Ein Administrator öffnet unter **Basisdaten → Wertpapieraktion** die **Systemaktionen** und wählt **Abspaltung erfassen**. Er wählt das Mutterwertpapier und das abgespaltene Wertpapier aus. Ein neues Wertpapier kann er über ISIN und Handelswährung suchen und den Vorschlag mit Name, Ticker und Konnektoren prüfen.

Massgeblich ist der Ex-Tag. Das Verhältnis bezieht sich auf die Stückelung an diesem Tag. Bei einer Abspaltung im Verhältnis 4 zu 1 erhält ein Depot für 100 Stück des Mutterwertpapiers 25 Stück des neuen Wertpapiers.

Unter **Einstandsaufteilung** erfasst der Administrator den veröffentlichten Anteil, der auf das abgespaltene Wertpapier übergeht, und nennt die Quelle. Mit **Aufteilung aus Schlusskursen vorschlagen** kann er den Anteil schätzen. GT zeigt den gemeinsamen Kurstag, die Schlusskurse und den Wechselkurs. Wie GT mit einem Split des Datenanbieters und mit der Kurshistorie des Mutterwertpapiers umgeht, beschreibt der Abschnitt [Split des Datenanbieters und Kurshistorie des Mutterwertpapiers](#HISTORYBASIS).

## Split des Datenanbieters und Kurshistorie des Mutterwertpapiers {#HISTORYBASIS}
Viele Datenanbieter, etwa Yahoo und EOD Historical Data, melden eine Abspaltung als Split des Mutterwertpapiers am Ex-Tag und rechnen dessen Kurse vor dem Ex-Tag um diesen Faktor herunter. Für General Electric meldet Yahoo am 04.01.2023 (GE HealthCare) einen Split von 1000 zu 1281 und am 02.04.2024 (GE Vernova) einen von 1000 zu 1253. Als Split gespeichert, würde dieser Faktor jede vor dem Ex-Tag gekaufte Aktie vermehren. Aus 300 Aktien würden 384.3, und der Wert der neuen Anteile wäre zusammen mit den Buchungen der Abspaltung doppelt gezählt.

Für ein Mutterwertpapier mit einer erfassten Abspaltung übernimmt GT deshalb den Split nicht, den der Konnektor genau am Ex-Tag liefert. Splits an anderen Tagen und von Hand erfasste Splits bleiben bestehen. Die Kurse vor dem Ex-Tag speichert GT so, wie an diesen Tagen gehandelt wurde: Hat die Kursquelle sie für die Abspaltung heruntergerechnet, rechnet GT sie beim Laden mit dem Kursfaktor wieder hinauf. Die Bereinigung um echte Splits bleibt dabei erhalten. Ohne erfasste Abspaltung bleibt der Split des Datenanbieters stehen, und GT behandelt das Ereignis näherungsweise wie einen Split. Das Löschen einer Abspaltung stellt diesen Zustand samt der gelieferten Kurshistorie wieder her.

### Kursbasis
Ob eine Kurshistorie für eine Abspaltung heruntergerechnet ist, hängt von der Kursquelle und vom einzelnen Ereignis ab, nicht davon, ob Kurse und Splits vom selben Konnektor stammen. So liefert onvista die Kurse von Pfizer für die Abspaltung von Viatris bereinigt, jene von Mercedes-Benz für Daimler Truck dagegen unbereinigt. Yahoo hat die Kurse von Mercedes-Benz bereinigt, ohne einen Split zu melden. EOD Historical Data bereinigt nur den Schlusskurs, Eröffnung, Hoch und Tief bleiben dort unverändert. GT hält deshalb pro Abspaltung eine **Kursbasis** fest: **Unbereinigt**, **Bereinigt** mit einem **Kursfaktor** oder **Ungeklärt**. Sie gilt für die Kursquelle des Mutterwertpapiers, für die sie bestimmt wurde.

GT bestimmt die Kursbasis jedes Mal, bevor es die Kurshistorie des Mutterwertpapiers vollständig neu lädt. Hat ein Administrator die Kursbasis gesetzt und wurde sie noch nicht angewendet, gilt seine Angabe. Sind die gespeicherten Kurse vor dem Ex-Tag sicher unbereinigt, vergleicht GT sie mit den neu gelieferten Kursen vor und nach dem Ex-Tag; das Verhältnis ergibt den Kursfaktor genau. Sicher unbereinigt sind die Kurse, wenn sie geladen wurden, bevor der Ex-Tag eintrat, oder wenn sie bereits unter einer bestimmten Kursbasis gespeichert wurden. Ist kein Vergleich möglich, bleibt eine bereits angewendete Angabe des Administrators bestehen. Andernfalls prüft GT den Kurssprung am Ex-Tag in den gelieferten Kursen. Er entscheidet nur, wenn er deutlich ist: Die verworfene Lesart müsste eine Marktbewegung von mindestens 10 % und dem Dreifachen der anderen bedeuten. Einen Kursfaktor übernimmt GT dabei nur, wenn der Split von derselben Kursquelle gemeldet wurde. In allen übrigen Fällen bleibt die Kursbasis **Ungeklärt**, und GT speichert die Kurse so, wie die Quelle sie liefert.

{{< mermaid >}}
graph TD
    A[Vollständiges Neuladen der Kurshistorie] --> B{Kursbasis vom Administrator<br/>gesetzt, noch nicht angewendet?}
    B -->|Ja| Z1[Angabe des Administrators]
    B -->|Nein| C{Gespeicherte Kurse vor dem<br/>Ex-Tag sicher unbereinigt?}
    C -->|Ja, Vergleich möglich| Z2[Kursfaktor aus dem Vergleich<br/>gespeicherter und gelieferter Kurse]
    C -->|Nein| D{Angewendete Angabe<br/>des Administrators?}
    D -->|Ja| Z1
    D -->|Nein| E{Deutlicher Kurssprung<br/>am Ex-Tag?}
    E -->|Kurse zeigen den Abschlag| Z3[Unbereinigt]
    E -->|Kurse heruntergerechnet,<br/>Split derselben Kursquelle| Z4[Bereinigt mit dem<br/>Faktor des Splits]
    E -->|nicht eindeutig| Z5[Ungeklärt:<br/>Kurse wie geliefert]
{{< /mermaid >}}

Meldet eine Kursquelle den Split, bevor sie ihre Kurshistorie bereinigt hat, sind die gelieferten Kurse noch unbereinigt. GT erkennt das und rechnet nichts hinauf. Beim nächsten vollständigen Neuladen findet der Vergleich mit den gespeicherten Kursen den Faktor der inzwischen bereinigten Reihe.

### Ablauf nach dem Erfassen
Nach dem Erfassen oder Löschen einer Abspaltung und nach jeder Änderung der Kursbasis stellt GT die [Hintergrundaufgabe 81]({{% relref "/admindata/taskdatachangemonitor/taskdescription/splitdividend/#JOB81" %}}) in die Warteschlange. Sie liest die Splits, bestimmt die Kursbasis, lädt die Kurshistorie des Mutterwertpapiers neu und baut danach dessen Bestände auf. Solange sie wartet oder läuft, lehnt GT Vorschau und Anwenden mit einem Hinweis ab. Versuchen Sie es nach einigen Minuten erneut.

Die Buchungen einer Abspaltung verwenden Schlusskurse ab ihrem Ex-Tag. Diese können für eine spätere Abspaltung desselben Mutterwertpapiers oder des abgespaltenen Wertpapiers heruntergerechnet sein. Deshalb lehnt GT Vorschau, Anwenden und den Vorschlag der Einstandsaufteilung ab, solange eine solche spätere Abspaltung die Kursbasis **Ungeklärt** hat. Eine ungeklärte Kursbasis der Abspaltung selbst sperrt nichts. Sie kann aber die Kurse des Mutterwertpapiers vor dem Ex-Tag zu tief belassen, und der Ertrag je Wertpapier zeigt dann über den Ex-Tag einen Gewinn in der Grössenordnung des Werts der neuen Anteile.

### Kursbasis prüfen und setzen
Die Spalte **Kursbasis** in den Systemaktionen zeigt dem Administrator, wo eine Klärung nötig ist. Im Dialog **Abspaltung bearbeiten** zeigt die Schaltfläche **Splits des Anbieters und Kursbasis** die Splits, die der Split-Konnektor innerhalb von zehn Tagen um den Ex-Tag meldet, den **Splitfaktor des Anbieters** mit seinem **Anteil der Abspaltung am Faktor des Anbieters**, die aktuelle Kursbasis mit ihrer **Kursquelle der Entscheidung** sowie den **Kurssprung der gespeicherten Historie** und den **Kurssprung der gelieferten Historie**. Ein Kurssprung erscheint mit der Marktbewegung, die er bedeutet, falls die Kurse bereinigt beziehungsweise unbereinigt sind. Wurde die Einstandsaufteilung zuvor aus Schlusskursen vorgeschlagen, erscheint zusätzlich der **Aus den Schlusskursen abgeleitete Faktor**. Dieselbe Schaltfläche steht schon beim Erfassen zur Verfügung, sobald Mutterwertpapier und Ex-Tag gewählt sind.

Unter **Kursbasis** wählt der Administrator **Automatisch**, **Unbereinigt** oder **Bereinigt** mit dem **Kursfaktor**, durch den die Kursquelle die Kurse vor dem Ex-Tag geteilt hat. Der Faktor des Splits eines anderen Konnektors kann vom Faktor der Kursquelle abweichen. Das Speichern lädt die Kurshistorie des Mutterwertpapiers neu, und die Notiz der Abspaltung hält die Änderung fest. Eine gesetzte Kursbasis gilt nur für die Kursquelle, für die sie gesetzt wurde. Wechselt die Kursquelle des Mutterwertpapiers, entscheidet GT wieder selbst. **Automatisch** gibt die Entscheidung an GT zurück.

### Beispiel General Electric
General Electric bezieht Kurse und Splits von Yahoo. Die folgende Tabelle zeigt die Schlusskurse, wie Yahoo sie heute liefert, und wie GT sie mit den Kursfaktoren 1.281 und 1.253 speichert:

| Datum | Schlusskurs von Yahoo | Kursfaktor | Gespeicherter Schlusskurs |
|-------|----------------------:|------------|--------------------------:|
| 03.01.2023 | 52.94 | 1.281 × 1.253 | 84.98 |
| 04.01.2023 | 56.03 | 1.253 | 70.20 |
| 01.04.2024 | 139.95 | 1.253 | 175.36 |
| 02.04.2024 | 136.47 | 1 | 136.47 |

Wurde die Kurshistorie vor den beiden Ex-Tagen geladen, ergibt der Vergleich der gespeicherten mit den gelieferten Kursen die beiden Faktoren genau. Wurde sie erst danach geladen, entscheidet der Kurssprung. Ein Depot mit 300 Aktien, die am 01.12.2022 gekauft wurden, erhält 100 Aktien von GE HealthCare und 75 Aktien von GE Vernova und behält seine 300 Aktien von General Electric.

### Echter Split am Ex-Tag
Fällt ein echter Split auf den Ex-Tag, liefert der Anbieter beide Ereignisse als eine einzige Zahl. Erfassen Sie den echten Split dann von Hand beim Mutterwertpapier. GT teilt den Faktor des Anbieters durch diesen Split und verwendet nur den Rest als Kursfaktor der Abspaltung. Bei einem Split von 1 zu 2 hat ein Depot mit 100 Aktien damit Anspruch auf die neuen Anteile für 200 Aktien. Der Dialog zeigt den Splitfaktor des Anbieters neben dem aus den Schlusskursen abgeleiteten Faktor, sodass eine solche kombinierte Zahl auffällt. Erfassen Sie für die Abspaltung selbst keinen Split von Hand, denn einen von Hand erfassten Split verwirft GT nie.

## Vorschau prüfen und anwenden
Wählen Sie die Abspaltung und öffnen Sie **Abspaltung anwenden**. Prüfen Sie pro Depot die berechtigte Stückzahl, die neuen Anteile, die Kurse und die Beträge in der Kontowährung. Die Vorschau zeigt vorhandene Buchungen in der Nähe des Ex-Tags. Erkennt GT die vollständige Folge aus Verkauf, Wiederkauf und Kauf, schlägt es **Überspringen** vor. Wählen Sie **Buchen**, wenn GT die Abspaltung für das Depot erfassen soll. **Überspringen** belässt Ihre vorhandenen Buchungen.

GT erzeugt für jedes gebuchte Depot einen Verkauf und Wiederkauf des Mutterwertpapiers sowie einen Kauf des abgespaltenen Wertpapiers. Der Saldo des Geldkontos ändert sich dadurch insgesamt nicht. Erfassen Sie den Verkauf allfälliger Bruchteile separat, sobald er auf der Abrechnung erscheint.

## Einstand und Korrektur
GT teilt den Einstand nach dem erfassten Anteil auf. Bei FIFO oder LIFO übernimmt jedes abgespaltene Los das ursprüngliche Kaufdatum. Bei Durchschnittseinstand wird der auf das betroffene Depot entfallende Anteil am Einstand der ausgewählten Bestände aufgeteilt. Die Abspaltung selbst erzeugt keinen realisierten Gewinn.

Ein Administrator kann die Einstandsaufteilung mit **Abspaltung bearbeiten** korrigieren. Die gespeicherten Buchungen behalten ihre Kurse. Spätere Auswertungen verwenden den geänderten Einstand, auch für Verkäufe aus früheren Jahren.

## Rückgängig machen
Mit **Abspaltung rückgängig machen** entfernt GT die zugehörigen Systembuchungen. Abhängige Buchungen und weitere angewendete Wertpapieraktionen für das abgespaltene Wertpapier müssen zuerst rückgängig gemacht werden. Korrigieren Sie danach bei Bedarf Ihre ursprünglichen Buchungen und wenden Sie die Abspaltung erneut an.

## Auswertungen
Im [Positionschart]({{% ref "/watchlistinstrument/positionchart" %}}) bleibt die Stückzahl des Mutterwertpapiers über den Ex-Tag gleich. Das investierte Kapital geht zum historischen Einstand auf das abgespaltene Wertpapier über, auch in der Hauptwährung, und erzeugt dabei keinen realisierten Erfolg. Beim abgespaltenen Wertpapier zählt dieser Einstand als Kauf. Der Marktwert der neuen Anteile verlässt das Mutterwertpapier als ein einziger Abfluss, ebenso im [Ertrag je Wertpapier]({{% ref "/reportportfolio/securityperiodperformance" %}}). Der Kursrückgang des Mutterwertpapiers am Ex-Tag erscheint deshalb nicht als Verlust.

Die [Steuerhilfe Deutschland]({{% ref "/reportportfolio/germantax" %}}) folgt der **Deutschen steuerlichen Einordnung**, die der Administrator bei der Abspaltung erfasst. Bei **Aufteilung des Einstands** geht der Anteil des Euro-Einstands jedes Kaufs auf das abgespaltene Wertpapier über, das Kaufdatum bleibt. Ein Bestand aus der Zeit vor 2009 bleibt damit auch beim abgespaltenen Wertpapier Altbestand. Weicht der für die deutsche Steuer massgebliche Anteil vom veröffentlichten Anteil ab, erfasst der Administrator ihn unter **Einstandsaufteilung für die deutsche Steuer**. Bei **Steuerpflichtige Ausschüttung** zählt der Marktwert der neuen Anteile als Dividende, und das abgespaltene Wertpapier gilt als am Ex-Tag gekauft. Solange die Einordnung **Ungeklärt** ist, rechnet die Steuerhilfe wie bei einer Aufteilung des Einstands, weist aber spätere Verkäufe beider Wertpapiere ohne Betrag aus. Gleichen Sie die Einordnung mit der Abrechnung Ihrer Bank ab.

Eine Simulation übernimmt die Abspaltung mit dem Portfolio. Der Einstand beider Wertpapiere ist in der Simulation derselbe wie im Mandanten, aus dem sie erstellt wurde. Rückgängig machen lässt sich die übernommene Abspaltung in der Simulation nicht.

## Warum drei Buchungen?
Eine Abspaltung verschiebt Einstand vom Mutterwertpapier auf das abgespaltene Wertpapier, ohne dass Geld fliesst. GT kennt dafür keinen eigenen Transaktionstyp. Ein solcher Typ müsste von jedem Teil der Anwendung verstanden werden: von der Prüfung der Stückzahlen, den Beständen und Kontoständen, dem Import und Export von Transaktionen, der Transaktionsliste und allen Auswertungen. Stattdessen bucht GT pro Depot drei gewöhnliche Transaktionen, die zusammen den Kontostand nicht verändern. Alles, was mit Käufen und Verkäufen rechnet, bleibt damit ohne Anpassung richtig.

Die drei Buchungen sind zu Marktwerten des Ex-Tags bewertet. Der Verkauf des Mutterwertpapiers erfolgt zum Kurs vor der Abspaltung, also zum Schlusskurs des Ex-Tags plus dem Wert der neuen Anteile, der Wiederkauf zum Schlusskurs des Ex-Tags. So verlässt genau der Marktwert der neuen Anteile das Mutterwertpapier und kommt beim abgespaltenen Wertpapier an. Den Einstand teilen die Berechnungen erst bei jeder Auswertung nach dem erfassten Anteil auf. Sie erkennen die drei Buchungen an ihrer Rolle und behandeln sie nicht als Verkauf und Käufe. Deshalb muss nichts umgebucht werden, wenn ein Administrator den Anteil korrigiert oder Sie die Methode für den Einstand wechseln.

Ein Beispiel: Ein Depot hält 100 Aktien des Mutterwertpapiers mit einem Einstand von 5800. Das Verhältnis ist 4 zu 1, die Einstandsaufteilung 20 %, und die Schlusskurse am Ex-Tag betragen 64 für das Mutterwertpapier und 48 für das abgespaltene Wertpapier. Der Verkaufskurs von 76 ist der Schlusskurs von 64 plus ein Viertel von 48. Die drei Buchungen heben sich auf dem Geldkonto auf, während 20 % des Einstands zum abgespaltenen Wertpapier wandern:

{{< mermaid >}}
graph LR
    M1["Mutter vor dem Ex-Tag<br/>100 Stück, Einstand 5800"] -->|"Verkauf 100 × 76 = +7600"| K["Geldkonto<br/>Saldo unverändert"]
    K -->|"Wiederkauf 100 × 64 = −6400"| M2["Mutter nach dem Ex-Tag<br/>100 Stück, Einstand 4640"]
    K -->|"Kauf 25 × 48 = −1200"| T["Abgespaltenes Wertpapier<br/>25 Stück, Einstand 1160"]
    M1 -.->|"20 % des Einstands, Kaufdaten bleiben"| T
{{< /mermaid >}}

Der Preis für diesen Weg: Die drei Buchungen erscheinen in der Transaktionsliste. Sie lassen sich nicht einzeln ändern oder löschen, sondern nur gemeinsam mit **Abspaltung rückgängig machen** entfernen. Ebenso lehnt GT eine Änderung an einer früheren Buchung des Mutterwertpapiers ab, wenn sie die Stückzahl am Ex-Tag verändern würde. Machen Sie in diesem Fall die Abspaltung rückgängig, korrigieren Sie die Buchung und wenden Sie die Abspaltung erneut an.

## Einschränkungen
- Eine Abspaltung mit mehreren neuen Gesellschaften lässt sich nicht erfassen. Pro Mutterwertpapier und Ex-Tag ist nur eine Abspaltung möglich.
- Eine Aufspaltung, bei der das Mutterunternehmen in mehrere Nachfolger aufgeht, und ein Umtausch von Anteilen des Mutterwertpapiers gegen neue Anteile sind keine Abspaltung im Sinne dieser Aktion.
- Ein Depot mit einem Leerverkauf des Mutterwertpapiers erhält keine neuen Anteile.
- Hat das abgespaltene Wertpapier am Ex-Tag noch keinen Schlusskurs, verwendet GT den ersten Schlusskurs der folgenden zehn Tage. An den Tagen ohne Kurs fehlt der Wert des abgespaltenen Wertpapiers in den Tageswerten des Portfolios.
- Solange die Kursbasis einer Abspaltung **Ungeklärt** ist, speichert GT die Kurse des Mutterwertpapiers vor dem Ex-Tag so, wie die Kursquelle sie liefert. Eine Abspaltung, deren neue Anteile weniger als etwa 10 % des Werts des Mutterwertpapiers ausmachen, entscheidet GT nur über den Vergleich mit gespeicherten Kursen oder über die Angabe des Administrators.
- Den Split eines Datenanbieters, der nicht genau auf den Ex-Tag fällt, übernimmt GT als Split. Der Dialog **Abspaltung bearbeiten** listet die Splits um den Ex-Tag auf.
- Ein echter Split am Ex-Tag muss von Hand erfasst werden. Fehlt er, fehlt er auch in der berechtigten Stückzahl.
- Kurse aus dem Archiv, die nach einem Wechsel der Kursquelle ergänzt werden, behalten die Kursbasis, unter der sie archiviert wurden.
- Kurse, die ein anderer GTNet-Teilnehmer für ein Mutterwertpapier vor dem letzten Ex-Tag liefert, übernimmt GT nicht, weil deren Kursbasis unbekannt ist. Diese Tage füllt nur der Konnektor.
- Das Handelsvolumen vor dem Ex-Tag bleibt so, wie die Kursquelle es liefert. Bei EOD Historical Data und Alpha Vantage bleibt im Schlusskurs die Bereinigung um Dividenden enthalten, GT entfernt nur den Kursfaktor der Abspaltung.
- Eine Korrektur der Einstandsaufteilung wirkt rückwirkend, auch auf realisierte Gewinne und die Steuerhilfe früherer Jahre.
- Den Barausgleich für Bruchteile erfassen Sie selbst als Verkauf.
