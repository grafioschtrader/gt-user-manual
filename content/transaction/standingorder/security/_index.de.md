---
title: "Dauerauftrag Wertpapier"
date: 2026-02-28T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
Der Dauerauftrag für Wertpapiere erstellt automatisch wiederkehrende Kauf- oder Verkaufstransaktionen. Im Vergleich zum Konto-Dauerauftrag ist er deutlich umfangreicher, da die Stückzahl aus einem Kurs berechnet werden kann, Kosten über Formeln definierbar sind und eine allfällige Währungskonvertierung berücksichtigt wird.

## Transaktionsarten
Für Wertpapier-Daueraufträge stehen zwei Transaktionsarten zur Verfügung:
- **Kaufen**: Regelmässiger Zukauf eines Wertpapiers. Typische Anwendung ist ein monatlicher ETF-Sparplan.
- **Verkaufen**: Regelmässiger Verkauf eines Wertpapiers. Kann beispielsweise für einen schrittweisen Positionsabbau verwendet werden.

## Investitionsmodus
Die zentrale Konfiguration des Wertpapier-Dauerauftrags ist der **Investitionsmodus**. Dieser bestimmt, ob eine feste Stückzahl oder ein fester Betrag pro Ausführung verwendet wird. Es muss genau einer der beiden Modi gewählt werden.

### Feste Stückzahl
Bei diesem Modus wird bei jeder Ausführung die gleiche Anzahl Einheiten gekauft oder verkauft. Der resultierende Betrag variiert je nach aktuellem Kurs des Wertpapiers.

### Fester Betrag
Bei diesem Modus wird bei jeder Ausführung ein fester Geldbetrag investiert. Die Stückzahl wird anhand des aktuellen Kurses berechnet. Dieser Modus eignet sich besonders für einen Sparplan, da bei tiefen Kursen mehr und bei hohen Kursen weniger Anteile erworben werden (Durchschnittskosteneffekt). Zwei zusätzliche Optionen stehen zur Verfügung:
- **Bruchstücke erlaubt**: Wenn aktiviert, können Bruchteile von Anteilen erworben werden (z.B. 5.237 Stück). Wenn deaktiviert, wird die berechnete Stückzahl auf ganze Einheiten abgerundet.
- **Betrag inkl. Kosten**: Wenn aktiviert, ist der **Investitionsbetrag** der Gesamtbetrag inklusive Kosten. Die effektiv investierte Summe ist dann der Betrag abzüglich der Kosten. Wenn deaktiviert, ist der **Investitionsbetrag** der reine Anlagebetrag und die Kosten werden zusätzlich belastet.

## Berechnung bei Ausführung
Bei der Ausführung eines Wertpapier-Dauerauftrags wird der historische Schlusskurs des Wertpapiers am Ausführungsdatum benötigt. Falls kein Kurs verfügbar ist, wird die Ausführung für diesen Termin übersprungen und ein Eintrag im [Fehlerprotokoll](../#ausführungsfehler) erstellt.

### Berechnungsablauf
Der folgende Ablauf zeigt, wie der Hintergrundprozess die Wertpapiertransaktion berechnet:
{{< mermaid >}}
graph TD
    A[Historischen Schlusskurs ermitteln] --> B{Kurs verfügbar?}
    B -- Nein --> C[Fehler protokollieren]
    B -- Ja --> D{Investitionsmodus?}
    D -- Feste Stückzahl --> E["Stückzahl = konfigurierter Wert"]
    D -- Fester Betrag --> F["Stückzahl = Betrag / Kurs"]
    F --> G{Bruchstücke erlaubt?}
    G -- Nein --> H[Stückzahl abrunden]
    G -- Ja --> I[Stückzahl beibehalten]
    H --> J{Stückzahl > 0?}
    I --> J
    J -- Nein --> C[Fehler protokollieren]
    J -- Ja --> K[Kosten berechnen]
    E --> K
    K --> L[Kontobuchung berechnen]
    L --> M[Transaktion erstellen]
{{< /mermaid >}}

### Formel für die Kontobuchung
Die Kontobuchung berechnet sich wie folgt, wobei der Bruttobetrag dem Produkt aus Stückzahl und Kurs entspricht:
- **Kaufen**: Kontobuchung = −(Bruttobetrag + Steuer + Transaktionskosten)
- **Verkaufen**: Kontobuchung = +(Bruttobetrag − Steuer − Transaktionskosten)

Falls eine Währungskonvertierung konfiguriert ist, wird die Kontobuchung anschliessend mit dem historischen Wechselkurs des Ausführungsdatums umgerechnet.

## Kostenbehandlung
Für die automatische Berücksichtigung von Steuern und Transaktionskosten stehen zwei Konfigurationsansätze zur Verfügung, die pro Kostenart unabhängig gewählt werden können.

### Fester Betrag
Ein fixer Kostenwert wird bei jeder Ausführung verwendet. Dies ist sinnvoll bei Brokern mit pauschalen Transaktionsgebühren oder bekannten festen Steuerbeträgen.

### Formel
Alternativ kann eine Formel eingegeben werden, die bei jeder Ausführung mit den aktuellen Werten ausgewertet wird. Falls ein fester Betrag und eine Formel gleichzeitig angegeben sind, hat der feste Betrag Vorrang. In der Formel stehen drei Variablen zur Verfügung:

| Variable | Bedeutung | Beispiel |
|---|---|---|
| `u` | Stückzahl | 10.5 |
| `q` | Kurs des Wertpapiers | 48.30 |
| `a` | Bruttobetrag (u × q) | 507.15 |

#### Formelbeispiele
| Formel | Beschreibung |
|---|---|
| `a * 0.01` | 1% des Bruttobetrags |
| `a * 0.0015` | 0.15% des Bruttobetrags (Stempelsteuer) |
| `u * 0.50` | 0.50 pro Anteil |
| `25` | Pauschale von 25 |
| `MAX(a * 0.005, 5)` | 0.5% des Bruttobetrags, mindestens 5 |

{{% notice style="info" title="Formelauswertung" %}}
Die Formeln werden mit der Bibliothek EvalEx ausgewertet, die mathematische Standardfunktionen wie `MIN`, `MAX`, `ABS`, `ROUND` und weitere unterstützt. Falls eine Formel einen Fehler enthält, wird der Kostenwert auf 0 gesetzt und eine Warnung protokolliert.
{{% /notice %}}

## Wechselkurs
Falls sich die Währung des Wertpapiers von der Währung des Kontos unterscheidet, muss ein **Währungspaar** angegeben werden. Bei der Ausführung wird der historische Wechselkurs am Ausführungsdatum herangezogen, um die Kontobuchung in die Kontowährung umzurechnen. Dieses Verhalten entspricht der Währungsbehandlung bei manuellen [Wertpapiertransaktionen](../../security/).

{{% notice note %}}
Falls am Ausführungsdatum kein historischer Wechselkurs verfügbar ist, wird der nächstältere verfügbare Kurs verwendet.
{{% /notice %}}

## Eingabefelder
Beim Erstellen oder Bearbeiten eines Wertpapier-Dauerauftrags müssen folgende Angaben gemacht werden:
- **Transaktionsart**: Kaufen oder Verkaufen.
- **Konto**: Das Konto, über das die Transaktion abgerechnet wird.
- **Wertpapier**: Das Wertpapier, welches gehandelt wird. Dieses Feld wird beim Bearbeiten oder beim Erstellen aus einer Transaktion vorbelegt.
- **Investitionsmodus**: Feste Stückzahl oder Fester Betrag.
- **Stückzahl** (bei fester Stückzahl): Die Anzahl Einheiten pro Ausführung.
- **Investitionsbetrag** (bei festem Betrag): Der Geldbetrag pro Ausführung.
- **Bruchstücke erlaubt** (bei festem Betrag): Ob Teilanteile erworben werden dürfen.
- **Betrag inkl. Kosten** (bei festem Betrag): Ob die Kosten im Investitionsbetrag enthalten sind.
- **Steuer (jede Art)**: Fester Steuerbetrag pro Ausführung (optional).
- **Steuerkosten-Formel**: Formel zur dynamischen Berechnung der Steuer (optional).
- **Transaktionskosten**: Fester Gebührenbetrag pro Ausführung (optional).
- **Transaktionskosten-Formel**: Formel zur dynamischen Berechnung der Gebühr (optional).

Zusätzlich stehen die gemeinsamen Felder der [Wiederholungskonfiguration](../) zur Verfügung.

Sobald der Dauerauftrag Transaktionen erstellt hat, werden die transaktionsspezifischen Felder (Transaktionsart, Konto, Wertpapier, Währungspaar, Investitionsmodus-Einstellungen und alle Kostenfelder) gesperrt. Nur die Terminierungsfelder und die Notiz können weiterhin bearbeitet werden. Siehe [Bearbeitungs- und Löscheinschränkungen](../#bearbeitungs--und-löscheinschränkungen) für Details.

## Einschränkungen
- **Kursabhängigkeit**: Die Ausführung setzt voraus, dass historische Kursdaten für das Wertpapier am Ausführungsdatum vorhanden sind. Ist kein Kurs verfügbar, wird der Termin übersprungen und ein [Ausführungsfehler](../#ausführungsfehler) protokolliert.
- **Wechselkursabhängigkeit**: Ist ein Währungspaar konfiguriert, muss ebenfalls ein historischer Wechselkurs am Ausführungsdatum vorliegen.
- **Mindest-Stückzahl**: Ergibt die Berechnung eine Stückzahl kleiner oder gleich null (z.B. weil die Kosten den Investitionsbetrag übersteigen), wird die Ausführung übersprungen und ein [Ausführungsfehler](../#ausführungsfehler) protokolliert.
- **Kosten sind Schätzungen**: Die berechneten Kosten entsprechen nicht zwingend den tatsächlichen Kosten Ihrer Handelsplattform. Es ist empfehlenswert, die erstellten Transaktionen periodisch mit den realen Abrechnungen zu vergleichen und bei Bedarf anzupassen.

## Erstellte Transaktionen
Jede durch den Dauerauftrag erstellte Transaktion entspricht einer normalen [Wertpapiertransaktion](../../security/). Sie kann in den üblichen Transaktionsansichten eingesehen und bei Bedarf bearbeitet werden. Zusätzlich sind die erstellten Transaktionen direkt in der Dauerauftrags-Tabelle über den Zeilenexpander sichtbar, wo sie ebenfalls über das Kontextmenü bearbeitet oder gelöscht werden können. Siehe [Erstellte Transaktionen einsehen](../#erstellte-transaktionen-einsehen) für Details.
