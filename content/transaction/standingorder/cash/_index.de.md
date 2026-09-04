---
title: "Dauerauftrag Konto"
date: 2026-08-07T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
Der Dauerauftrag für Konten erstellt automatisch wiederkehrende Kontotransaktionen. Er ist die einfachere der beiden Dauerauftragsarten, da lediglich ein fester Betrag und ein Konto konfiguriert werden müssen.

## Transaktionsarten
Für Konto-Daueraufträge stehen vier Transaktionsarten zur Verfügung:
- **Einzahlung**: Regelmässige Gutschrift auf das Konto. Typische Anwendung ist beispielsweise ein monatlicher Geldtransfer auf das Handelskonto.
- **Auszahlung**: Regelmässige Belastung des Kontos. Sie eignet sich für regelmässige Entnahmen, mit denen Sie dem Portfolio tatsächlich Kapital entziehen.
- **Kontozins**: Regelmässige Zinsgutschrift auf das Konto.
- **Konto- und Depotkosten**: Wiederkehrende Konto- oder Depotgebühr der Bank. Der Betrag wird immer positiv erfasst und als Belastung gebucht.

Für eine wiederkehrende Bankgebühr wählen Sie **Konto- und Depotkosten** und nicht **Auszahlung**. Eine Auszahlung gilt als Kapitalentnahme aus dem Portfolio und verfälscht dadurch die Performance-Berechnung, während Konto- und Depotkosten korrekt als Aufwand behandelt werden.

## Eingabefelder
Beim Erstellen oder Bearbeiten eines Konto-Dauerauftrags müssen folgende Angaben gemacht werden:
- **Transaktionsart**: Einzahlung, Auszahlung, Kontozins oder Konto- und Depotkosten.
- **Konto**: Das Konto, auf dem die Transaktion gebucht wird. Die Auswahlliste zeigt alle Konten des Mandanten, gruppiert nach Portfolio und Währung. Ein über das Feld **Aktiv bis oder Fälligkeitstag** stillgelegtes Konto wird abhängig vom **Gültig-von**-Datum nicht zur Auswahl angeboten. Siehe [Stilllegung eines Kontos]({{% ref "/tenantportfolio/cashaccount#stilllegung-eines-kontos" %}}).
- **Betrag**: Der feste Betrag, der bei jeder Ausführung gebucht wird. Ohne Angabe einer Betragswährung ist dies die Währung des gewählten Kontos.
- **Betragswährung** (optional): Die Währung, in der die Bank den Betrag in Rechnung stellt, falls diese von der Kontowährung abweicht. Siehe [Betrag in Fremdwährung](#betrag-in-fremdwährung).
- **Betragsformel** (optional): Formel zur Berechnung des zu buchenden Betrags. Siehe [Betrag in Fremdwährung](#betrag-in-fremdwährung).
- **Transaktionskosten** (optional): Fixe Transaktionskosten, die bei jeder generierten Transaktion angewendet werden. Dies kann beispielsweise für Bankgebühren bei wiederkehrenden Überweisungen verwendet werden.

Zusätzlich stehen die gemeinsamen Felder der [Wiederholungskonfiguration](../) zur Verfügung: Wiederholungseinheit, Wiederholungsintervall, Tagesposition, Ausführungstag, Ausführungsmonat, Anpassung an Handelstage, Kurstoleranz, Gültig-von, Gültig-bis und eine optionale Notiz. Beim Konto-Dauerauftrag werden über die Anpassung an Handelstage nur Wochenenden übersprungen; Börsenfeiertage spielen keine Rolle, da eine Bank auch an diesen Tagen bucht.

Sobald der Dauerauftrag Transaktionen erstellt hat, werden die transaktionsspezifischen Felder (Transaktionsart, Konto, Betrag, Betragswährung, Betragsformel und Transaktionskosten) gesperrt. Nur die Terminierungsfelder und die Notiz können weiterhin bearbeitet werden. Ein einmal verwendeter Margenfaktor lässt sich damit nachträglich nicht mehr anpassen; ändert die Bank ihre Konditionen, beenden Sie den bestehenden Dauerauftrag über das Feld **Gültig-bis** und erfassen einen neuen. Siehe [Bearbeitungs- und Löscheinschränkungen](../#bearbeitungs--und-löscheinschränkungen) für Details.

## Betrag in Fremdwährung
Manche Banken stellen eine wiederkehrende Gebühr in einer Währung in Rechnung, belasten sie aber einem Konto in einer anderen Währung. Ein Beispiel ist eine Depotgebühr von CHF 3.00 pro Monat, die einem Konto in US-Dollar belastet wird. Der belastete Betrag ist dann jeden Monat leicht anders, weil er vom Wechselkurs abhängt.

Für einen solchen Fall erfassen Sie unter **Betrag** den Betrag in der Rechnungswährung, also 3.00, und setzen die **Betragswährung** auf CHF. Bereits beim Speichern des Dauerauftrags legt GT das benötigte [Währungspaar](../../../watchlistinstrument/instrument/currencypair/) von der Betragswährung zur Kontowährung an, falls es noch nicht besteht. Dessen Kursdaten werden damit lange vor der ersten Ausführung geladen. Bei jeder Ausführung ermittelt GT den Wechselkurs des Ausführungstages und rechnet den Betrag in die Kontowährung um. Ist am Ausführungstag kein Kurs vorhanden, weicht GT im Rahmen der [Kurstoleranz](../#kurstoleranz) auf einen benachbarten Tag aus. Da ein Konto-Dauerauftrag durchaus auf einen Börsenfeiertag fallen kann, an dem keine Wechselkurse geliefert werden, sollte die Kurstoleranz hier nicht auf 0 gesetzt werden. Wird auch innerhalb des Fensters kein Kurs gefunden, wird die Ausführung übersprungen und im [Fehlerprotokoll](../#ausführungsfehler) vermerkt.

### Betragsformel
Der reine Wechselkurs entspricht selten dem Kurs, den die Bank tatsächlich verrechnet, da diese eine Marge aufschlägt. Mit der **Betragsformel** bilden Sie diesen Aufschlag ab. In der Formel stehen zwei Variablen zur Verfügung:

| Variable | Bedeutung | Beispiel |
|---|---|---|
| `a` | Betrag wie im Feld **Betrag** erfasst | 3.00 |
| `r` | Wechselkurs am Ausführungstag | 1.2366 |

| Formel | Beschreibung |
|---|---|
| `a * r` | Reine Umrechnung, entspricht dem Verhalten ohne Formel |
| `a * r * 1.019` | Umrechnung mit einem Bankaufschlag von 1.9% |
| `ROUND(a * r * 1.019, 2)` | Wie oben, zusätzlich auf zwei Stellen gerundet |

Die Formel wird beim Speichern des Dauerauftrags geprüft, sodass ein Schreibfehler sofort gemeldet wird. Verwendet die Formel die Variable `r`, muss eine Betragswährung gesetzt sein.

{{% notice style="info" title="Genauigkeit" %}}
Der Betrag ist eine Annäherung und trifft den Betrag der Bank in der Regel nicht auf den Rappen genau. Die Marge einer Bank ist nicht konstant, sondern schwankt von Monat zu Monat. Bei einer realen Kontogebühr von CHF 3.00 lag die Marge über drei Jahre hinweg im Durchschnitt bei rund 1.9%, und der berechnete Betrag wich üblicherweise etwa zwei Rappen von der tatsächlichen Belastung ab. Falls Sie exakte Werte benötigen, korrigieren Sie die erstellte Transaktion nach der Ausführung.
{{% /notice %}}

{{% notice note %}}
Lässt sich die Formel nicht auswerten oder ist kein Wechselkurs verfügbar, wird keine Transaktion gebucht. Die Ausführung wird übersprungen und der Grund in der Fehlerliste des Dauerauftrags festgehalten.
{{% /notice %}}

## Erstellte Transaktionen
Jede durch den Dauerauftrag erstellte Transaktion entspricht einer normalen [Kontotransaktion](../../account/). Sie kann in den üblichen Transaktionsansichten eingesehen und bei Bedarf bearbeitet werden. Zusätzlich sind die erstellten Transaktionen direkt in der Dauerauftrags-Tabelle über den Zeilenexpander sichtbar, wo sie ebenfalls über das Kontextmenü bearbeitet oder gelöscht werden können. Siehe [Erstellte Transaktionen einsehen](../#erstellte-transaktionen-einsehen) für Details.
