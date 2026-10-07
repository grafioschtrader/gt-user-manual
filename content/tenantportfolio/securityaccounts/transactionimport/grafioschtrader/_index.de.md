---
title: "Grafioschtrader-Import"
date: 2026-09-28T10:00:00+02:00
draft: false
weight: 25
archetype: "default"
---
GT kann seine **eigenen Exporte** wieder einlesen. Dazu gehören der [CSV-Export der Transaktionen](../../../../reportportfolio/transactionlist/) sowie die [Transaktionsbelege als PDF](../../../../transaction/receipt/). Für diesen Rückimport dient eine besondere, mit GT ausgelieferte **Import Vorlagengruppe** mit dem Namen «**Grafioschtrader**».
{{% notice info %}}
Anders als die Vorlagengruppen für eine Handelsplattform, die Sie selbst erstellen, ist die Vorlagengruppe «Grafioschtrader» **einzigartig** und wird bereits fertig mit GT mitgeliefert. Sie sollte weder bearbeitet noch ein zweites Mal angelegt werden. Eine allgemeine Beschreibung finden Sie unter [Import Vorlagengruppe](../../../../basedata/imptranstemplate/).
{{% /notice %}}
## Voraussetzung
Damit der Rückimport angeboten wird, müssen zwei Dinge zusammentreffen. Ein Administrator muss die mitgelieferte Vorlagengruppe «Grafioschtrader» einmalig für die ganze Installation bestimmt haben, siehe [Import Vorlagengruppe](../../../../basedata/imptranstemplate/), und beim [Klient](../../../client/) muss das Auswahlkästchen «**Grafioschtrader-Importvorlagen freischalten**» gesetzt sein. Erst danach steht die nachfolgend beschriebene Auswahl zur Verfügung.
## Anwendung
Sind beide Voraussetzungen erfüllt, erscheint an den beiden Stellen des Transaktionsimports das Auswahlkästchen «**Grafioschtrader-Importvorlagen verwenden**»: einerseits im Dialog zum Erstellen oder Bearbeiten einer **Importgruppe**, andererseits beim Ablegen eines PDF-Dokuments per **Drag & Drop**. Ist das Kästchen gesetzt, verwendet GT für diesen Import die Vorlagen aus der Vorlagengruppe «Grafioschtrader» statt die Vorlagen der normalen **Handelsplattform** des Depots. Dadurch lässt sich ein GT-Export in ein beliebiges **Depot** einlesen, ohne dessen Zuordnung zur Handelsplattform zu ändern.
## Eine Datei pro Depot
Der CSV-Export erzeugt eine Datei pro **Depot**. Lesen Sie jede Datei in das dazugehörige Depot ein. Damit gelangen die Transaktionen wieder in dasselbe Depot, aus dem sie stammen.
## Kontoüberträge verbinden
Ein **Kontoübertrag** zwischen zwei Portfolios wird beim Export auf zwei Dateien verteilt, weil jedes Portfolio sein eigenes Depot hat. Werden diese Dateien getrennt importiert, entstehen zunächst zwei unverbundene Buchungen, eine Auszahlung und eine Einzahlung. Ein Übertrag zwischen zwei Konten desselben Portfolios steht dagegen in derselben Datei und wird bereits beim Import verbunden. Der Menüpunkt «**Kontoüberträge verbinden**» auf **Mandantenebene** in der Auswertung [Transaktionen](../../../../reportportfolio/transactionlist/) stellt die fehlenden Verbindungen nachträglich her. Er darf beliebig oft ausgeführt werden, denn bereits verbundene Buchungen werden nicht mehr angefasst.

GT betrachtet alle unverbundenen Ein- und Auszahlungen des Mandanten. Eine Auszahlung und eine Einzahlung kommen nur dann als Paar in Frage, wenn sie auf **dieselbe Minute** lauten und auf zwei verschiedenen Konten liegen. Zuerst werden Paare **gleicher Währung** gesucht, deren Beträge übereinstimmen. Danach werden die übrigen Buchungen mit **verschiedenen Währungen** geprüft: Der Umrechnungskurs, der sich aus den beiden Beträgen ergibt, darf höchstens 8 % vom Schlusskurs des Währungspaares an diesem Tag abweichen. Dieselbe Grenze gilt auch, wenn Sie einen Kontoübertrag von Hand erfassen. Eine Buchung, die bereits ein passendes Gegenstück in gleicher Währung hat, wird nicht zusätzlich mit einer Fremdwährung verglichen. Verbunden wird ein Paar nur, wenn die Zuordnung **eindeutig** ist, also keine der beiden Buchungen ein zweites passendes Gegenstück hat. Jedes Paar wird anschliessend gleich geprüft wie ein von Hand erfasster Kontoübertrag.
{{< mermaid >}}
graph TD
    A[Unverbundene Aus- und Einzahlung] --> B{Gleiche Minute und verschiedene Konten?}
    B -- Nein --> X[Kein Paar]
    B -- Ja --> C{Gleiche Währung?}
    C -- Ja --> D{Gleicher Betrag?}
    C -- Nein --> E{Kurs höchstens 8 % vom Schlusskurs entfernt?}
    D -- Nein --> X
    E -- Nein --> X
    D -- Ja --> F{Einziges passendes Gegenstück?}
    E -- Ja --> F
    F -- Nein --> M[Mehrdeutig, wird übersprungen]
    F -- Ja --> G{Prüfung wie bei einem erfassten Kontoübertrag}
    G -- Bestanden --> V[Verbunden]
    G -- Nicht bestanden --> R[Abgewiesen]
{{< /mermaid >}}
### Die Meldung
Nach dem Lauf meldet GT vier Zahlen. Zu beachten ist, dass zwei davon einzelne **Buchungen** und zwei davon **Paare** aus je zwei Buchungen zählen.

**Geprüft** ist die Anzahl der unverbundenen Ein- und Auszahlungen, die betrachtet wurden. Darin sind auch Buchungen enthalten, die gar kein Gegenstück haben, beispielsweise eine gewöhnliche Einzahlung von aussen. **Verbunden** ist die Anzahl der Paare, die nun einen Kontoübertrag bilden. **Mehrdeutig** ist die Anzahl der Buchungen, für die es mehr als ein passendes Gegenstück gibt. GT rät in diesem Fall nicht, sondern lässt diese Buchungen unverändert. **Abgewiesen** ist die Anzahl der Paare, die zwar eindeutig zusammenpassen, aber die Prüfung eines Kontoübertrags nicht bestanden haben. Gründe dafür sind etwa ein überzogenes Konto, ein Datum in einer bereits abgeschlossenen Periode oder ein Umrechnungskurs, der zu weit vom Schlusskurs abweicht. Die Meldung nennt zusätzlich die Buchungen **ohne Gegenbuchung**. Sie ergeben sich aus den geprüften Buchungen abzüglich der verbundenen, mehrdeutigen und abgewiesenen Buchungen.

Ein Beispiel: 58 geprüfte Buchungen mit 26 verbundenen Paaren, keiner mehrdeutigen und keinem abgewiesenen Paar bedeuten, dass 52 Buchungen verbunden wurden und 6 Buchungen ohne Gegenbuchung bleiben. Das sind in der Regel echte Ein- oder Auszahlungen, die nie Teil eines Übertrags waren.
### Grenzen der Zuordnung
Der Export speichert den Zeitpunkt einer Buchung auf die Minute genau. Finden in derselben Minute mehrere Überträge in derselben Währung mit gleichem Betrag statt, lassen sie sich nicht unterscheiden und bleiben mehrdeutig. Bei verschiedenen Währungen kann die Kursprüfung nur greifen, wenn für das Währungspaar an diesem Tag ein Schlusskurs vorhanden ist. Fehlt er, passt jede Kombination von Beträgen, und mehrere Überträge am selben Tag bleiben mehrdeutig. Liegen zwei Überträge zwischen denselben Währungen mit ähnlichem Kurs in derselben Minute, kann auch die Kursprüfung sie nicht trennen.

Mehrdeutige oder abgewiesene Buchungen verbinden Sie von Hand. Löschen Sie dazu eine der beiden Buchungen und wandeln Sie die andere mit «**Umwandlung in Kontoübertrag**» in einen Kontoübertrag um. GT erstellt die Gegenbuchung dabei neu.
### Export aus einer Simulationsumgebung
Stammen die Dateien aus einer [Simulationsumgebung](../../../../algoalert/historicalrun/environment/), sind einige Besonderheiten zu beachten. Die Buchungen einer Simulation tragen nur ein Datum und keine Uhrzeit. Damit fallen alle Überträge eines Tages in dieselbe Minute, und die Beträge allein reichen oft nicht mehr für eine eindeutige Zuordnung. Das betrifft vor allem die Umbuchungen «**Geld für einen Auftrag umgebucht**», die GT während einer Historischen Wiederholung zwischen den Portfolios vornimmt. Davon gibt es häufig mehrere am selben Tag, und sie erfolgen meistens in eine andere Währung. In diesen Fällen trennt erst die Kursprüfung die Überträge voneinander. Der in der Simulation eingerechnete prozentuale Aufschlag auf den Wechselkurs liegt üblicherweise weit innerhalb der erlaubten Abweichung.

Die Einzahlungen, mit denen der Eröffnungsbestand der Umgebung beginnt, werden beim Import zu gewöhnlichen Einzahlungen. Sie haben kein Gegenstück und bleiben deshalb richtigerweise unverbunden. In der Meldung erscheinen sie unter den Buchungen ohne Gegenbuchung.

Damit die Kursprüfung wirkt, müssen im Mandanten, in den Sie importieren, für die betroffenen Währungspaare Schlusskurse an den Tagen der Überträge vorhanden sein. Das ist bei den gängigen Währungen der Fall. Bei einem selten verwendeten Währungspaar lohnt es sich, dessen Kursdaten vor dem Verbinden zu prüfen.
## Einschränkung
Eröffnungs- und Schliessungspositionen von **Margin-Geschäften** werden im Export gekennzeichnet und beim Import übersprungen. Solche Positionen müssen von Hand erfasst werden.
{{% notice note %}}
Dieser Weg über Export und Import mit den Grafioschtrader-Vorlagen dient auch **Testzwecken**. Damit stehen für die Prüfung des Transaktionsimports jederzeit nachvollziehbare Testdaten zur Verfügung und es müssen keine echten Exporte von Banken verwendet werden.
{{% /notice %}}
