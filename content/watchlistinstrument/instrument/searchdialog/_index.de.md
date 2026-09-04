---
title: "Instrument Suchdialog"
date: 2026-08-13T22:54:47+01:00
draft: false
weight: 45
archetype: "default"
---
Mit diesem Dialog können bestehende Instrumente selektiert werden. Es gibt eine  Einzel- und Mehrfachselektion:
- **Einzelselektion**: Nur ein einzelnes **Instrument** kann aus der Resultatmenge der Suche selektiert und übernommen werden. Dies findet beispielsweise beim **Transaktionsimport** seine Anwendung.
- **Mehrfachselektion**: Es können **mehrere Instrumente** aus dem Resultat der Suche aus einer Tabelle selektiert werden. Diese Mehrfachselektion wird bei der **Watchlist** und  **Korrelationsmatrix** für das Hinzufügen von Instrumenten benutzt.
- Die Suchkriterien sind eine Und-Verknüpfung und somit muss ein gefundenes Instrument alle Kriterien erfüllen.
## Bereits enthaltene Instrumente
Wird der Dialog aus einer **Watchlist** oder einem **Korrelationsset** geöffnet, werden die dort bereits enthaltenen Instrumente in der Resultattabelle weggelassen. Ein Instrument kann somit nicht zweimal hinzugefügt werden, und die Tabelle zeigt nur, was noch fehlt.
Dient der Dialog dagegen der Zuweisung eines **einzelnen** Instruments, so gibt es keine Sammlung, gegen die verglichen werden könnte, und es wird jedes passende Instrument jedes Mal angeboten. Das betrifft das Basis- und die zusätzlichen Instrumente eines [abgeleiteten Instruments](../securityderived/derivedinstrument/), den Transaktionsimport, den Dauerauftrag und die ISIN-Änderung.
## Suchkriterien
Alle ausgefüllten Kriterien werden mit **und** verknüpft, ein gefundenes Instrument muss also jedes davon erfüllen. Solange kein einziges Kriterium erfasst ist, bleibt die Schaltfläche **Suchen** inaktiv. Die Eingabefelder sind nachfolgend nach Themen gruppiert beschrieben; im Dialog selbst erscheinen sie untereinander.
### ISIN
Die **ISIN** identifiziert ein Instrument eindeutig und wird deshalb exakt und nicht als Teilzeichenkette gesucht. Eine unvollständige ISIN führt somit zu keinem Treffer. Beim Öffnen des Dialogs steht der Eingabecursor in diesem Feld, da die Suche über die ISIN die genaueste ist. Wie sich dieses Feld gegenüber den übrigen Kriterien verhält, ist unter [Ein- und Ausblenden der Eingabefelder](#ein--und-ausblenden-der-eingabefelder) beschrieben.
### Name und Ticker/Symbol
Diese beiden Felder suchen nach der Bezeichnung des Instruments, wenn die ISIN nicht bekannt ist.
- **Name**: Gesucht wird nach einem beliebigen Teil des Namens, Gross- und Kleinschreibung spielt dabei keine Rolle. Die Eingabe "gold" findet somit auch "Goldpreis" und "Barrick Gold".
- **Name als regulärer Ausdruck**: Mit dieser Auswahl wird der eingegebene Name nicht mehr als Teilzeichenkette, sondern als **Suchmuster** ausgewertet. Damit lassen sich Kriterien formulieren, die im Namen selbst stecken, siehe [Namenssuche mit einem Suchmuster](#namenssuche-mit-einem-suchmuster).
- **Ticker/Symbol**: Gesucht wird nach einem beliebigen Teil des Symbols, wobei die Eingabe immer in Grossbuchstaben umgewandelt wird.
### Einordnung
Über die Einordnung eines Instruments lässt sich die Treffermenge stark einschränken, auch ohne den Namen zu kennen.
- **Anlageklasse**: Damit wird auf eine Anlageklasse wie beispielsweise Aktien oder festverzinsliche Wertpapiere eingeschränkt. Wird hier **Währungspaar** gewählt, sucht der Dialog nach Währungspaaren statt nach Wertpapieren; in diesem Fall wird der **Name** gegen die beiden Währungscodes und nicht gegen einen Instrumentennamen geprüft.
- **Unter Anlageklasse**: Damit wird innerhalb der Anlageklasse weiter unterschieden. Die Auswahl entspricht den in der [Anlageklasse]({{% relref "/basedata/instrumentbased/assetclass" %}}) erfassten Unterklassen.
- **Finanzinstrument**: Damit wird nach der Art des Instruments unterschieden, also beispielsweise Direktanlage, ETF oder CFD.
### Handelsplatz, Währung und Gültigkeit
Diese Kriterien betreffen den Handel des Instruments.
- **Handelsplatz**: Damit wird auf einen einzelnen [Handelsplatz]({{% relref "/basedata/instrumentbased/stockexchange" %}}) eingeschränkt.
- **Währung**: Damit wird auf die Währung des Instruments eingeschränkt, in welcher dessen Kurse geführt werden.
- **Aktiv am**: Gesucht werden Instrumente, deren Laufzeit das erfasste Datum umfasst. Eine bereits zurückbezahlte Anleihe wird somit nur gefunden, wenn ein Datum innerhalb ihrer Laufzeit gewählt wird.
### Datenquellen
Mit diesen beiden Kriterien lassen sich alle Instrumente einer bestimmten Datenquelle zusammenstellen, was beispielsweise vor dem Ablösen einer nicht mehr funktionierenden Datenquelle hilfreich ist.
- **Historische Datenquelle**: Damit wird auf die Datenquelle der historischen Kursdaten eingeschränkt.
- **Innertag Datenquelle**: Damit wird auf die Datenquelle der Innertagkurse eingeschränkt.
### Bestand und Sichtbarkeit
Diese Kriterien beziehen sich nicht auf das Instrument selbst, sondern auf dessen Verhältnis zu Ihrem Mandanten.
- **Mit aktivem Bestand**: Damit werden nur Instrumente gefunden, für die Ihr Mandant aktuell einen Bestand hält. Das ist die schnellste Art, ein Instrument aus dem eigenen Depot zu finden, ohne dessen Namen genau zu kennen. Diese Auswahl wird beispielsweise bei der [ISIN-Änderung]({{% relref "/basedata/securityaction/securityisinrename" %}}) benötigt.
- **Privates Wertpapier**: Dieses Auswahlfeld kennt drei Zustände. Ist es nicht gesetzt, werden öffentliche Instrumente und Ihre eigenen privaten Instrumente gefunden. Ist es gesetzt, werden ausschliesslich Ihre eigenen privaten Instrumente gefunden, ist es ausdrücklich abgewählt, ausschliesslich die öffentlichen.
- **Gehebelt Inverse**: Damit wird der Hebelfaktor exakt gesucht, was nur bei gehebelten und inversen Produkten sinnvoll ist.
## Anwendungsfall: Performance-Watchlist füllen
Im Navigationsbereich ist eine Ihrer Watchlisten als [Performance-Watchlist]({{% relref "/watchlistinstrument/watchlist" %}}) gekennzeichnet, und diese sollte alle Ihre offenen Positionen enthalten. Der Grund dafür ist die Kursaktualisierung: die [Performanz-Ansicht]({{% relref "/watchlistinstrument/watchlist/performance" %}}) einer Watchlist ist die einzige Stelle, an der GT die Innertag-Kurse nachführt, und mit den Instrumenten werden auch die davon abhängigen Währungspaare aktualisiert. Was nicht in dieser Watchlist liegt, wird untertags nicht nachgeführt.
Dieser Suchdialog ist der schnellste Weg, diese Watchlist zu füllen oder zu vervollständigen. Öffnen Sie die betreffende Watchlist, wählen Sie **Bestehendes Instrument hinzufügen**, setzen Sie einzig das Auswahlfeld **Mit aktivem Bestand**, starten Sie die Suche ohne weiteres Kriterium, markieren Sie alle Zeilen der Resultattabelle und übernehmen Sie diese mit **Hinzufügen**. Sie müssen dazu keinen einzigen Instrumentennamen kennen.
Da bereits enthaltene Instrumente nicht mehr angeboten werden, kann dieselbe Suche später jederzeit wiederholt werden, nachdem neue Positionen eröffnet wurden; angeboten wird dann nur noch das Fehlende.
## Ein- und Ausblenden der Eingabefelder
Der Dialog blendet Eingabefelder aus, sobald diese durch eine andere Eingabe gegenstandslos werden. Ein ausgeblendetes Feld ist nicht verloren, es erscheint wieder, sobald die auslösende Eingabe zurückgenommen wird.
{{% notice note "ISIN schliesst die übrigen Kriterien aus" %}}
Sobald in der **ISIN** etwas erfasst wird, verschwinden alle übrigen Kriterien, denn die ISIN identifiziert das Instrument bereits eindeutig. Umgekehrt verschwindet die **ISIN**, sobald eines der übrigen Kriterien erfasst wird. Wird das entsprechende Feld wieder geleert, erscheinen die ausgeblendeten Felder erneut.
{{% /notice %}}
Wird unter **Anlageklasse** der Eintrag **Währungspaar** gewählt, werden alle Kriterien ausgeblendet, die es bei einem Währungspaar nicht gibt. Dies betrifft **Ticker/Symbol**, **Privates Wertpapier**, **Gehebelt Inverse**, **Aktiv am**, **Handelsplatz**, **Finanzinstrument**, **Unter Anlageklasse** sowie **Name als regulärer Ausdruck**. Wird zusätzlich eine **Währung** gewählt, verschwindet auch der **Name**, weil die Währung allein bereits genügend genau ist.
Die Schaltfläche **Suchen** bleibt inaktiv, solange kein eigentliches Kriterium erfasst ist. Das Auswahlfeld **Name als regulärer Ausdruck** zählt dabei nicht als Kriterium, da es nur bestimmt, wie der **Name** ausgewertet wird.
## Namenssuche mit einem Suchmuster
Ist **Name als regulärer Ausdruck** gesetzt, bedeutet der **Name** nicht mehr "enthält diesen Text", sondern beschreibt ein Muster, dem der Name entsprechen muss. Damit lassen sich Suchen formulieren, die mit einer gewöhnlichen Textsuche nicht möglich sind. Das Muster wird an einer beliebigen Stelle des Namens gesucht, sofern es nicht mit `^` an den Anfang oder mit `$` an das Ende gebunden wird. Gross- und Kleinschreibung wird auch hier nicht unterschieden.
Die folgenden Beispiele zeigen die gebräuchlichsten Fälle:
- `Const.* software` findet **Constellation Software**, auch wenn die genaue Schreibweise nicht bekannt ist. Das Zeichenpaar `.*` steht für beliebige Zeichen dazwischen und ersetzt somit den unsicheren Teil des Namens.
- `^[23]([.,]\d+)?(?=\s)` findet Anleihen, deren Name mit einem Coupon zwischen 2 und 3.99 Prozent beginnt, also beispielsweise **2.75 % Kanton Zürich 2019-2031**. Solche Bereiche lassen sich mit einer Textsuche nicht ausdrücken.
- `^(Apple|Alphabet)` findet Namen, die entweder mit **Apple** oder mit **Alphabet** beginnen. Der senkrechte Strich trennt Alternativen.
- `ETF$` findet Namen, die auf **ETF** enden.
Lässt sich das eingegebene Muster nicht auswerten, weil beispielsweise eine Klammer nicht geschlossen wurde, wird die Suche mit einer Fehlermeldung abgewiesen. Sie erhalten somit keine stillschweigend leere Treffermenge. Die Suche mit einem Muster steht nur für Wertpapiere zur Verfügung und nicht für Währungspaare.
{{< youtube XIL4XNHsJM0 >}}
