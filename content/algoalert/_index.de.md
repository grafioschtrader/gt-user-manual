---
title: "Regelbasierter Handel und Alarme"
date: 2026-09-26T12:00:00+02:00
draft: false
weight: 22
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="In Bearbeitung - Zielversion V0.38.0" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
GT unterstützt regelbasierten Handel und ein umfassendes Alarmsystem. Die Strategien gehören zu drei Arten: der Kernallokation für die langfristige Neugewichtung des Portfolios, den Handelsstrategien für einzelne Wertpapiere und den Alarmen, die Kurse und Bestände lediglich beobachten. Welche Art wo wirkt, im eigenen Portfolio oder in einer Simulation, erläutert der Abschnitt [Welche Strategie wo wirkt](#welche-strategie-wo-wirkt).

## Hierarchische Struktur
Das Algo-System in GT ist hierarchisch aufgebaut. An der Spitze steht die **Portfoliobasierte Strategie**, die in der Regel mit einer Watchlist verknüpft ist. Darunter befinden sich die Anlageklassen oder benutzerdefinierten Kategorien mit ihren prozentualen Gewichtungen, und auf der untersten Ebene die einzelnen Wertpapiere. Strategien lassen sich jeder dieser Ebenen zuweisen.

{{< mermaid >}}
graph TD
    A["Portfoliobasierte Strategie"] --> B["Anlageklasse 1 (z.B. Aktien 60%)"]
    A --> C["Anlageklasse 2 (z.B. Obligationen 40%)"]
    B --> D["Wertpapier A"]
    B --> E["Wertpapier B"]
    C --> F["Wertpapier C"]
    D --> G["Strategie 1"]
    D --> H["Strategie 2"]
    E --> I["Strategie 3"]
{{< /mermaid >}}

## Drei Arten von Strategien
Die **Kernallokation** legt die langfristige Aufteilung des Portfolios auf Anlageklassen fest. Die [Portfolio-Neugewichtung](./strategy/rebalancing/) vergleicht in festen Abständen den Bestand mit den Zielgewichtungen des Baums und bestimmt die Käufe und Verkäufe, welche die gewünschte Aufteilung wiederherstellen. Diese Art eignet sich für eine strategische, passive Anlagestrategie.

Eine **Handelsstrategie** wird einem einzelnen Wertpapier zugewiesen und entscheidet selbst, wann es gekauft und wann es verkauft wird. Derzeit ist dies der [Mean-Reversion-Dip](./strategy/meanreversiondip/) mit seinem Gewinn- und Verlustmanagement. Eine Handelsstrategie legt alles fest, was ein Auftrag braucht: die Richtung, die Menge beim Einstieg und beim Ausstieg, das Budget innerhalb der Gewichtung des Baums und die Wartefristen zwischen zwei Geschäften.

Ein **Alarm** beobachtet einen Kurs, einen Bestand oder einen technischen Indikator und meldet, sobald eine Bedingung erfüllt ist. Dazu gehören die [Preis-Alarme](./strategy/pricealerts/) und die [Indikator-Alarme](./strategy/indicatoralerts/). Ein Alarm ist eine Beobachtung und kein Auftrag: Er sagt, dass etwas eingetreten ist, aber weder, ob gekauft oder verkauft werden soll, noch wie viel.

## Welche Strategie wo wirkt
GT wertet Strategien an zwei Orten aus. Die [Portfolioüberwachung](./algo/#portfolioüberwachung) beobachtet Ihr eigenes Portfolio laufend im Hintergrund. Eine **Simulationsumgebung** ist eine abgetrennte Kopie Ihrer Portfolios und Konten auf ein Eröffnungsdatum der Vergangenheit; wie Sie eine solche anlegen, beschreibt [Regelbasierter Handel](./algo/#simulationsumgebung-erstellen). Die [Historische Wiederholung](./historicalrun/) wertet die Strategien darin gegen vergangene Kursdaten aus. Die Auswertung künftiger, simulierter Kursverläufe folgt in einer späteren Version. Nicht jede Art von Strategie wirkt an beiden Orten:

| Art | Strategietyp | Portfolioüberwachung | Simulation |
|---|---|---|---|
| Kernallokation | Portfolio-Neugewichtung | Empfehlung als Meldung im Posteingang | Käufe und Verkäufe werden gebucht |
| Handelsstrategie | Mean-Reversion-Dip | Empfehlung und Benachrichtigung | Käufe und Verkäufe werden gebucht |
| Alarm | Preis-Alarme und Indikator-Alarme | Alarm im Posteingang | Wird nicht ausgewertet |

Im eigenen Portfolio bucht GT nie selbst. Auch die Neugewichtung und der Mean-Reversion-Dip liefern dort nur Empfehlungen; ob und wann Sie handeln, entscheiden Sie, und das Geschäft erfassen Sie als gewöhnliche Transaktion. In einer Simulation gibt es dagegen niemanden, der auf eine Meldung reagieren könnte. Die Wiederholung führt deshalb selbst aus, was eine Strategie vorschlägt: Ein Auftrag, der am Schluss eines Handelstages entsteht, wird zum Schlusskurs des nächsten zulässigen Handelstages gebucht.

Das gelingt nur mit Strategien, die einen vollständigen Auftrag beschreiben. Die Neugewichtung berechnet Richtung und Menge aus der Abweichung von den Zielgewichtungen, der Mean-Reversion-Dip aus seiner Konfiguration. Einem Alarm fehlen diese Angaben: Ein unterschrittenes Preislimit kann eine Kaufgelegenheit sein oder ein Anlass zum Verkauf, und wie viel gehandelt werden soll, lässt sich daraus ebenso wenig ablesen. Hinzu kommt, dass Alarme auf laufende Innertag-Kurse reagieren, während die Simulation mit den Tagesschlusskursen der Vergangenheit rechnet. Alarme werden in einer Simulation deshalb nicht ausgewertet, sie gehören ausschliesslich zu Ihrem eigenen Portfolio.

Möchten Sie prüfen, wie eine Kauf- oder Verkaufsregel in der Vergangenheit gewirkt hätte, so beschreiben Sie diese mit dem Mean-Reversion-Dip. Sein Einstieg nach einer Kursbewegung, sein Gewinnziel, sein Stop-Loss und seine Indikatorregeln erfassen ähnliche Bedingungen wie viele Preis- und Indikator-Alarme und legen zusätzlich die Menge fest.

### Neugewichtung und Mean-Reversion-Dip gemeinsam
Beide lassen sich in derselben Portfoliobasierten Strategie verwenden: die Neugewichtung auf der obersten Ebene, der Mean-Reversion-Dip auf einzelnen Wertpapieren. Die Portfolioüberwachung und die Simulation behandeln diese Kombination gleich. Damit sich die beiden nicht gegenseitig aufheben, gilt die Anlageklasse eines Wertpapiers mit Mean-Reversion-Dip als taktisch. Ihre Gewichtung ist dann nur noch eine Obergrenze. Die Neugewichtung baut die Anlageklasse ab, wenn sie diese Grenze überschreitet, kauft darin aber nie nach, auch nicht bei den übrigen Wertpapieren derselben Anlageklasse. Wann gekauft wird, entscheidet allein der Mean-Reversion-Dip, und auch er darf die Obergrenze nicht überschreiten.

Eine kurz zuvor eröffnete Dip-Position wird deshalb am nächsten Kontrollzeitpunkt nicht wieder verkauft, solange sie nicht durch Kursgewinne über ihre Obergrenze gewachsen ist. Ebenso baut die Neugewichtung eine Position, die der Mean-Reversion-Dip geschlossen hat, nicht wieder auf. Legen Sie Wertpapiere mit Mean-Reversion-Dip am besten in eine eigene Anlageklasse, damit die übrigen Anlageklassen weiterhin vollständig neu gewichtet werden. Ein Alarm macht eine Anlageklasse nicht taktisch. Einzelheiten beschreibt [Taktische Gruppen werden nicht aufgefüllt](./strategy/rebalancing/#taktische-gruppen-werden-nicht-aufgefüllt).

{{< mermaid >}}
graph TD
    T["Portfoliobasierte Strategie"] --> A["Anlageklasse Aktien"]
    T --> D["Anlageklasse Dip-Kandidaten<br/>taktisch: Gewichtung ist Obergrenze"]
    A --> A1["Welt-ETF"]
    A --> A2["US-ETF"]
    D --> D1["Aktie X"]
    R["Portfolio-Neugewichtung"] -.->|"kauft und verkauft"| A
    R -.->|"verkauft nur über der Obergrenze"| D
    M["Mean-Reversion-Dip"] -.->|"kauft und verkauft"| D1
{{< /mermaid >}}

## Navigation
In GT erreichen Sie den Bereich für den regelbasierten Handel über den Navigationsbereich auf der linken Seite. Klicken Sie auf **Regelbasierter Handel**, um die Baumansicht mit Ihren Portfolio-Strategien zu öffnen. Eine Übersicht aller Alarme Ihres Mandanten finden Sie beim Klienten im Register **Alarm**.
