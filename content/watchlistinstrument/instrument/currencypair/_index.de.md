---
title: "Währungspaar und Kryptowährungen"
date: 2026-08-31T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
GT unterstützt **Portfolios**, **Konten**, **Instrumente** und **Transaktionen** in unterschiedlichen Währungen. Daher ist das **Währungspaar** für GT unverzichtbar. In GT wird versucht, die **Fiatwährungen** und bestimmte **Kryptowährungen** gleichartig zu behandeln.

Ein **Währungspaar** kann **nicht gehandelt** werden, für den Handel muss ein entsprechendes **Wertpapier** oder **abgeleitetes Instrument** vorliegen. Dieses basiert auf einer **Anlageklasse** mit **Forex** als **Finanzinstrument**. Unterstützte **Kryptowährungen** sind BTC, BNB, ETH, ETC, LTC und XRP. Dabei gilt es zu beachten, dass die Datenquellen für Kryptowährungen nur Innertag- bzw. historische Kursdaten zu den wichtigsten Welt-Währungen wie USD, EUR, JPY, GBP und CHF liefern.

## Wann entsteht ein Währungspaar
Ein Währungspaar muss nur selten von Hand erstellt werden. In den allermeisten Fällen legt GT es selbst an, sobald eine Angabe zwei unterschiedliche Währungen miteinander verbindet. Massgebend ist dabei immer eine **Richtung**: GT erzeugt genau dasjenige Paar, das für die anstehende Umrechnung benötigt wird. Existiert das Paar in dieser Richtung bereits, wird es wiederverwendet und es entsteht kein zweites.

| Auslöser | Erzeugtes Währungspaar (von → nach) |
|---|---|
| Ein [Konto](../../../tenantportfolio/cashaccount/) wird mit einer von der Portfoliowährung abweichenden Währung gespeichert | Portfoliowährung → Kontowährung |
| Eine [Transaktion](../../../transaction/) wird erfasst, bei der Instrument- und Kontowährung auseinanderfallen, oder ein Kontoübertrag zwischen zwei Währungen | Instrument- → Kontowährung bzw. Konto- → Kontowährung |
| Beim [Transaktionsimport](../../../tenantportfolio/securityaccounts/transactionimport/) wird eine Position mit Wechselkurs verarbeitet | wie bei der manuellen Transaktion |
| Ein [Konto-Dauerauftrag](../../../transaction/standingorder/cash/) erhält eine von der Kontowährung abweichende **Betragswährung** | Betragswährung → Kontowährung |
| Die **Währung** eines [Portfolios](../../../tenantportfolio/portfolio/) wird geändert | Klientenwährung sowie alle in diesem Portfolio gehandelten Instrumentwährungen → neue Portfoliowährung |
| Die **Währung** des [Klienten](../../../tenantportfolio/client/) wird geändert | alle Portfoliowährungen sowie alle gehandelten Instrumentwährungen → neue Klientenwährung |
| Die Funktion **Währung Klient und Portfolios** wird ausgeführt | wie die beiden vorhergehenden Zeilen zusammen |
| Eine [Auswertung](../../../reportportfolio/) benötigt einen Wechselkurs, der noch fehlt — dies betrifft Kreuzkurse, den Aufbau der Bestandstabellen, die Instrument-Statistik und verschiedene Portfolio-Berichte | Klientenwährung → Instrument- bzw. Kontowährung |
| In der [Watchlist](../../watchlist/) wird über das Kontextmenü **Hinzufügen neu erstelltes Währungspaar** gewählt | frei gewählte Basis- und Kurswährung |

Das folgende Diagramm fasst den Ablauf zusammen. Entscheidend für die Wartezeit ist, wer das Währungspaar ausgelöst hat.

{{< mermaid >}}
graph TD
    A["Auslöser: Konto, Transaktion, Import, Dauerauftrag,<br/>Währungswechsel, Auswertung, manuelle Erstellung"] --> B{"Währungspaar in dieser<br/>Richtung vorhanden?"}
    B -->|ja| C["Vorhandenes Währungspaar wird verwendet"]
    B -->|nein| D["Neues Währungspaar wird erstellt,<br/>Standardkonnektoren werden zugewiesen"]
    D --> E{"Wer hat das Währungspaar ausgelöst?"}
    E -->|Auswertung| F["Kurse werden sofort geladen,<br/>die Auswertung wartet darauf"]
    E -->|Benutzeraktion| G["Kurse werden unmittelbar danach<br/>im Hintergrund geladen"]
    F --> J["Währungspaar ist einsatzbereit"]
    G --> H{"Kursgeschichte leer geblieben?"}
    H -->|nein| J
    H -->|ja| I["Aufgabe 40 lädt die Kurse später nach"]
{{< /mermaid >}}

## Richtung und Umkehrpaar
CHF/USD und USD/CHF sind für GT zwei eigenständige Währungspaare. Beim automatischen Erstellen prüft GT ausschliesslich, ob das Paar in der benötigten Richtung schon vorhanden ist. Besteht bereits USD/CHF und wird CHF/USD benötigt, so wird CHF/USD zusätzlich angelegt. Beide Paare bestehen danach nebeneinander, werden getrennt mit Kursdaten versorgt und erscheinen beide in der Suche.

Davon ausgenommen sind Auswertungen, die einen Kreuzkurs über die Klientenwährung bilden, sowie die Instrument-Statistik mit der Renditeberechnung. Diese suchen bewusst in beiden Richtungen und rechnen mit dem Kehrwert, statt ein zweites Währungspaar zu erzeugen.

{{% notice tip %}}
Prüfen Sie vor der manuellen Erstellung eines Währungspaars, ob die Gegenrichtung bereits in einer Watchlist vorhanden ist. Andernfalls pflegen Sie zwei Paare, deren Kurse doppelt geladen werden, obwohl ein einzelnes für die Umrechnung genügen würde.
{{% /notice %}}

## Konnektoren eines neuen Währungspaars
Ein automatisch erstelltes Währungspaar erhält seine Datenquellen aus den **globalen Einstellungen**. Welche der vier Einstellungen zum Zug kommt, hängt davon ab, ob eine Kryptowährung beteiligt ist. Es genügt, wenn eine der beiden Währungen eine der unterstützten Kryptowährungen ist, unabhängig davon, ob sie als Basis- oder als Kurswährung auftritt.

| Art des Währungspaars | Historische Kursdaten | Innertag-Kursdaten |
|---|---|---|
| Nur Fiatwährungen, beispielsweise EUR/CHF | «gt.currency.history.connector» | «gt.currency.intra.connector» |
| Mindestens eine Kryptowährung, beispielsweise BTC/USD | «gt.cryptocurrency.history.connector» | «gt.cryptocurrency.intra.connector» |

Diese vier Einstellungen kann der Administrator unter [Globale Einstellungen](../../../admindata/globalsettings/) anpassen. Sie wirken ausschliesslich auf **neu** entstehende Währungspaare; bestehende Währungspaare behalten ihren Konnektor. Für ein einzelnes Währungspaar lässt sich der Konnektor jederzeit über das Kontextmenü **Bearbeiten Währungspaar** in der Watchlist überschreiben.

## Laden der Kursdaten
Wie schnell die Kurse eines neu entstandenen Währungspaars zur Verfügung stehen, hängt vom Auslöser ab.

Wird das Währungspaar durch eine **Benutzeraktion** ausgelöst, also durch ein Konto, eine Transaktion, einen Import, einen Dauerauftrag oder die manuelle Erstellung, so wird es zuerst gespeichert und die Kursdaten werden unmittelbar danach im Hintergrund geladen. Die Eingabe wird dadurch nicht verzögert. Es kann jedoch vorkommen, dass die Ansicht das Währungspaar kurzzeitig noch ohne Kurs zeigt; nach einem Neuladen der Ansicht sind die Werte vorhanden.

Entsteht das Währungspaar dagegen im Rahmen einer **Auswertung**, werden die Kursdaten sofort geladen, weil die Auswertung ohne Kurs kein Ergebnis liefern kann. Diese eine Auswertung dauert deshalb spürbar länger als sonst. Beim nächsten Aufruf ist das Währungspaar bereits vorhanden und die Auswertung läuft wieder in gewohnter Geschwindigkeit.

### Wechsel des Konnektors bei einem bestehenden Währungspaar
Wird beim Bearbeiten eines Währungspaars der Konnektor für die historischen Kursdaten oder dessen URL-Erweiterung geändert, verwirft GT die vorhandenen historischen Kurse und liest sie unmittelbar über die neue Datenquelle neu ein. Auch das geschieht im Hintergrund, sodass der Bearbeitungsdialog sofort schliesst.

{{% notice note %}}
Beim Währungspaar erscheint für dieses erneute Einlesen **kein** Eintrag im [Aufgabenmonitor](../../../admindata/taskdatachangemonitor/), und die bisherigen Kurse werden nicht in das [Archiv der historischen Kursdaten](../../externaldata/historyquote/pricedata/archive/) übernommen. Beim Wertpapier ist beides der Fall, siehe [Konnektorwechsel und erneutes Einlesen der Kursdaten](../securityderived/#konnektorwechsel-und-erneutes-einlesen-der-kursdaten). Wechseln Sie den Konnektor eines Währungspaars deshalb nur, wenn die neue Datenquelle die benötigte Kursgeschichte tatsächlich abdeckt.
{{% /notice %}}

### Wenn ein Währungspaar ohne Kurse bleibt
Liefert die Datenquelle beim ersten Laden nichts, bleibt das Währungspaar ohne historische Kurse zurück. Für diesen Fall gibt es ein Sicherheitsnetz: Zusammen mit der täglichen Kursaktualisierung sucht GT alle Währungspaare ohne historische Kurse und reiht für jedes die Aufgabe **Historische Kursdaten eines leeren Währungspaares laden** ein. Diese ist im [Aufgabenmonitor](../../../admindata/taskdatachangemonitor/) sichtbar und in der [Aufgabenbeschreibung](../../../admindata/taskdatachangemonitor/taskdescription/) unter der Nummer 40 beschrieben.

Das Sicherheitsnetz greift nur bei vollständig leerer Kursgeschichte. Wurden nach einem Konnektorwechsel lediglich einzelne Kurse eingelesen, gilt das Währungspaar als versorgt. Prüfen Sie in diesem Fall die Vollständigkeit in der [Kurs Datenfeed Ansicht](../../watchlist/pricefeed/) der Watchlist und lösen Sie dort bei Bedarf die Reparatur der historischen Daten aus.

## Kurse für jeden Kalendertag
Ein Währungspaar braucht einen Kurs für **jeden Kalendertag** und nicht nur für die Handelstage. Der Grund liegt bei den Konten: Eine Transaktion, ein Kontoübertrag oder ein Dauerauftrag kann auf einen Samstag, einen Sonntag oder einen Feiertag fallen, und für die Umrechnung dieses Tages muss ein Kurs vorliegen. Bei einem Wertpapier stellt sich diese Frage nicht, dort zählen nur die Handelstage seines Handelsplatzes.

Die Datenquellen liefern für ein Währungspaar jedoch meistens nur an Werktagen einen Kurs. GT schliesst diese Lücken deshalb selbst: Fehlen zwischen zwei vorhandenen Kursen einzelne Tage, so wird für jeden dieser Tage der letzte Kurs vor der Lücke übernommen. Das geschieht zusammen mit der täglichen Kursaktualisierung, siehe [Aufgabenbeschreibung](../../../admindata/taskdatachangemonitor/taskdescription/) unter der Nummer 30.

Wie mit einer Lücke verfahren wird, hängt von deren Länge ab. Umfasst sie höchstens so viele Tage, wie in der globalen Einstellung «gt.history.max.filldays.currency» festgelegt sind — voreingestellt sind fünf Tage, womit ein verlängertes Wochenende abgedeckt ist —, so wird der vorhergehende Kurs ohne Umweg übernommen. Ist die Lücke länger, so fragt GT zuerst die Datenquelle für genau diesen Zeitraum erneut an, und erst wenn diese nichts liefert, wird auch hier der vorhergehende Kurs übernommen.

{{< mermaid >}}
graph TD
    A["Lücke zwischen zwei vorhandenen Kursen"] --> B{"Länger als «gt.history.max.filldays.currency»?"}
    B -->|nein| C["Letzter Kurs vor der Lücke wird für jeden fehlenden Tag übernommen"]
    B -->|ja| D["Datenquelle wird für diesen Zeitraum erneut angefragt"]
    D --> E{"Kurse geliefert?"}
    E -->|ja| F["Gelieferte Kurse werden gespeichert,<br/>verbleibende Lücken erneut geprüft"]
    E -->|nein| C
{{< /mermaid >}}

Diese übernommenen Kurse bleiben bewusst unauffällig. Sie erscheinen weder in der Tabelle der [historischen Kursdaten](../../externaldata/historyquote/pricedata/) noch im Chart des Währungspaars, und auch die statistischen Auswertungen lassen sie aussen vor, damit ein über das Wochenende gleichbleibender Kurs die Kennzahlen nicht verfälscht. Gezählt werden sie einzig in der Statistik zur Vollständigkeit desselben Dialogs, unter **Kursdaten Samstag** und **Kursdaten Sonntag**. Korrigieren Sie einen Kurs von Hand, so werden die unmittelbar darauf folgenden übernommenen Tage mitkorrigiert; Sie müssen diese nicht einzeln nachziehen.

{{% notice note %}}
Gefüllt wird nur **zwischen** zwei vorhandenen Kursen. Vor dem ersten und nach dem letzten gelieferten Kurs entsteht kein Eintrag. Liegt eine Transaktion vor dem Beginn der Kursgeschichte eines Währungspaars, so bleibt dieser Tag ohne Wechselkurs und die Auswertungen überspringen ihn, siehe [Periodenertrag](../../../reportportfolio/periodperformance/). Abhilfe schafft dann nur eine Datenquelle, welche die benötigte Kursgeschichte tatsächlich abdeckt.
{{% /notice %}}

{{< youtube ORLgkY4YiX0 >}}
