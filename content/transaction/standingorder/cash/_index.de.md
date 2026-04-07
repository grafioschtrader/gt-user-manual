---
title: "Dauerauftrag Konto"
date: 2026-02-28T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
Der Dauerauftrag für Konten erstellt automatisch wiederkehrende Kontotransaktionen. Er ist die einfachere der beiden Dauerauftragsarten, da lediglich ein fester Betrag und ein Konto konfiguriert werden müssen.

## Transaktionsarten
Für Konto-Daueraufträge stehen zwei Transaktionsarten zur Verfügung:
- **Einzahlung**: Regelmässige Gutschrift auf das Konto. Typische Anwendung ist beispielsweise ein monatlicher Geldtransfer auf das Handelskonto.
- **Auszahlung**: Regelmässige Belastung des Kontos. Kann beispielsweise für wiederkehrende Gebühren oder regelmässige Entnahmen verwendet werden.

## Eingabefelder
Beim Erstellen oder Bearbeiten eines Konto-Dauerauftrags müssen folgende Angaben gemacht werden:
- **Transaktionsart**: Einzahlung oder Auszahlung.
- **Konto**: Das Konto, auf dem die Transaktion gebucht wird. Die Auswahlliste zeigt alle Konten des Mandanten, gruppiert nach Portfolio und Währung.
- **Betrag**: Der feste Betrag in der Währung des gewählten Kontos, der bei jeder Ausführung gebucht wird.
- **Transaktionskosten** (optional): Fixe Transaktionskosten, die bei jeder generierten Transaktion angewendet werden. Dies kann beispielsweise für Bankgebühren bei wiederkehrenden Überweisungen verwendet werden.

Zusätzlich stehen die gemeinsamen Felder der [Wiederholungskonfiguration](../) zur Verfügung: Wiederholungseinheit, Wiederholungsintervall, Tagesposition, Ausführungstag, Ausführungsmonat, Wochenendanpassung, Gültig-von, Gültig-bis und eine optionale Notiz.

Sobald der Dauerauftrag Transaktionen erstellt hat, werden die transaktionsspezifischen Felder (Transaktionsart, Konto, Betrag und Transaktionskosten) gesperrt. Nur die Terminierungsfelder und die Notiz können weiterhin bearbeitet werden. Siehe [Bearbeitungs- und Löscheinschränkungen](../#bearbeitungs--und-löscheinschränkungen) für Details.

## Erstellte Transaktionen
Jede durch den Dauerauftrag erstellte Transaktion entspricht einer normalen [Kontotransaktion](../../account/). Sie kann in den üblichen Transaktionsansichten eingesehen und bei Bedarf bearbeitet werden. Zusätzlich sind die erstellten Transaktionen direkt in der Dauerauftrags-Tabelle über den Zeilenexpander sichtbar, wo sie ebenfalls über das Kontextmenü bearbeitet oder gelöscht werden können. Siehe [Erstellte Transaktionen einsehen](../#erstellte-transaktionen-einsehen) für Details.
