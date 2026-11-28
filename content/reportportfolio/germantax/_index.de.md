---
title: "Steuerhilfe Deutschland"
date: 2026-11-20T18:00:00+01:00
draft: false
weight: 56
archetype: "default"
---
Unter der Jahrestabelle der Auswertung [Realisierte Gewinne](../realizedgains/) erscheint für einen Klienten mit der Hauptwährung **Euro** und dem [Land](../../tenantportfolio/client/#eigenschaften) **Deutschland** die **Steuerhilfe Deutschland**. Sie ermittelt für ein Steuerjahr die **Ausgangswerte für die Steuererklärung**: die Abschnitte der Anlage KAP und der Anlage KAP-INV, alles in Euro. Sie berechnet **keine Steuer**. Sparer-Pauschbetrag, Steuersatz, Solidaritätszuschlag, Kirchensteuer, Günstigerprüfung und Verlustvorträge kennt nur die Veranlagung. Die Hilfe rechnet immer den ganzen Klienten, mit FIFO je Depot, wie das Gesetz es verlangt, unabhängig von der Einstandswert-Methode, die Sie sonst verwenden. Das Steuerjahr wählen Sie in der Auswahl **Steuerjahr** zwischen 2018 und dem laufenden Jahr. Das laufende Jahr ist vorläufig.

{{% callout warning %}}
Ausgangswerte ohne Gewähr, keine Steuerberechnung und keine Steuerberatung. Massgebend sind die Bescheinigungen der Banken. Die Vollständigkeit hängt von den erfassten Transaktionen ab.
{{% /callout %}}

## Voraussetzungen
- Weisen Sie jedem Portfolio mit Konten im [Portfolio-Dialog](../../tenantportfolio/portfolio/) den **Steuerabzug** zu. Solange ein Portfolio **Nicht zugewiesen** ist, steht sein Ertrag in einem eigenen Block und in keinem Formular, und die Hilfe fordert Sie zur Zuweisung auf. Ein Portfolio sollte die Konten genau eines Instituts enthalten.
- Weisen Sie jedem ETF, Investment-Fonds und Pensionsfonds die **Fondsart (deutsche Steuer)** zu, siehe [Fondsart](../../watchlistinstrument/instrument/securityderived/security/#fondsart). Ohne Fondsart rechnet die Hilfe den Fonds nicht.
- Für die Vorabpauschale braucht die Hilfe den **Basiszins** jedes Jahres, in dem Sie einen Fonds halten. Eine Administratorin oder ein Administrator pflegt ihn im Baum der [Steuerdaten](../../admindata/taxdata/) beim Land Deutschland. Für die Jahre 2018 bis 2026 ist er hinterlegt.
- Ordnen Sie ADR und GDR als **Aktie** ein, nicht als Zertifikat. Nur so zählen ihre Verluste im Topf der Aktien.

## Die Abschnitte
- **Aktien** (Anlage KAP): Gewinne und Verluste aus Aktienverkäufen, getrennt, dazu die Dividenden. Die Hilfe verrechnet sie nicht. Ob die Beschränkung, dass Aktienverluste nur mit Aktiengewinnen verrechnet werden, verfassungsgemäss ist, ist anhängig.
- **Zinsen und sonstige Kapitalforderungen** (Anlage KAP): Zinsen von Anleihen und Konten, vereinnahmte und gezahlte Stückzinsen sowie Gewinne und Verluste aus dem Verkauf von Anleihen, je getrennt.
- **Investmentfonds** (Anlage KAP-INV): je Fondsart die Ausschüttungen, die Vorabpauschale und das Ergebnis der Verkäufe, jeweils **vor der Teilfreistellung**, denn diesen Wert erwartet das Formular. Daneben steht der Betrag nach Teilfreistellung als Lesehilfe. Das Ergebnis eines Verkaufs ist um die Vorabpauschalen der verkauften Stücke gemindert. Die Vorabpauschale eines Jahres zählt als Ertrag des Folgejahres.
- **Ausländische Steuer** (Anlage KAP): die erfasste Steuer, eine **rechnerische Obergrenze** von 25 % des Ertrags und der Rest darüber. GT kennt weder Steuerart noch Staat noch einen Erstattungsanspruch aus einem Doppelbesteuerungsabkommen. Die Obergrenze ist deshalb kein endgültig anrechenbarer Betrag.
- **Mit inländischem Steuerabzug**: dieselben Werte der Portfolios, bei denen eine inländische Zahlstelle die Steuer einbehält, nur zur Information. Massgebend ist die Bescheinigung der Bank.
- **Portfolios ohne Zuweisung** und **Käufe vor 2009 und vor 2018**: nur zur Information. Aktien und Forderungen, die vor dem 1. Januar 2009 gekauft wurden, und Fondsanteile vor dem 1. Januar 2018 folgen Übergangsregeln. Ihre Verkäufe stehen in keinem Formular.

## Vollständig oder ohne Betrag
Jede Summe ist entweder vollständig hergeleitet oder sie hat **keinen Betrag** und sagt **Nicht berechenbar, siehe Warnungen**. Eine fehlende Zahl erscheint nie als 0. Die **Warnungen** nennen die Ursache, das Jahr, das Wertpapier und die Summen, die deshalb ohne Betrag bleiben. Typische Ursachen sind ein fehlender Wechselkurs, ein fehlender Basiszins, fehlende Preise oder Ausschüttungen eines Fonds, Verkäufe, die die Käufe nicht decken, und ISIN-Wechsel von Fonds. Eine Lücke wirkt weiter: Wer einen Fonds nach Jahren verkauft, dessen Basiszins eines Jahres dazwischen fehlt, erhält für den Verkauf keinen Betrag. Die Tabelle **Herleitung** zeigt jeden Verkauf, jeden Ertrag und jede Vorabpauschale mit den Beträgen in Euro, dem Wechselkurs und seiner Quelle. Die Liste **Instrumente ausserhalb der Summen** nennt Fonds ohne Fondsart, Zertifikate, ETC und CFD mit dem Wert ihrer Buchungen. Diese Buchungen können für die Steuererklärung trotzdem relevant sein, auch Termingeschäfte und Kryptowerte.

## Grenzen
- Die Berechnung wurde von keiner Steuerfachperson geprüft. Sie folgt dem Gesetz und den Schreiben des Bundesfinanzministeriums nach bestem Wissen, Abweichungen sind möglich. Lassen Sie die Werte vor der Abgabe von einer Steuerfachperson prüfen.
- Die Hilfe ersetzt keine Bescheinigung. Depots bei inländischen Banken erscheinen nur zur Information.
- Sie verwendet gebuchte Kurse und Schlusskurse des Währungspaares, keinen Devisenbriefkurs und keinen EZB-Referenzkurs, und für die Vorabpauschale den Börsenkurs statt des Rücknahmepreises. Die Beträge können deshalb von einer Bank-Abrechnung abweichen.
- Die Vorabpauschale rechnet in Euro: Preise und Ausschüttungen werden zum Kurs ihres Tages umgerechnet. Das Gesetz sagt nichts zur Umrechnung von Fonds in Fremdwährung. Ob diese Umrechnung der Auffassung der Finanzverwaltung entspricht, ist nicht geprüft.
- Bei Anleihen und anderen Kapitalforderungen knüpft das Gesetz für die Zeit vor 2009 an den Zufluss des Ertrags an, nicht an das Kaufdatum. Die Hilfe nimmt auch hier das Kaufdatum vor dem 1. Januar 2009. Der Verkauf einer vor 2009 gekauften Anleihe kann deshalb steuerpflichtig sein, obwohl die Hilfe ihn nur zur Information zeigt.
- Eine als Wertpapieraktion angewendete [Abspaltung](../../basedata/securityaction/spinoff/#auswertungen) folgt der dort erfassten deutschen steuerlichen Einordnung. Solange sie **Ungeklärt** ist, bleiben spätere Verkäufe beider Wertpapiere ohne Betrag. Den Barausgleich für Bruchteile gleichen Sie mit der Bescheinigung der Bank ab.
- Kapitalerhöhungen, Gratisaktien und Fondsverschmelzungen sind nur abgebildet, soweit sie als Transaktionen oder ISIN-Wechsel erfasst sind. Ein späterer Verkauf aus einem solchen Kauf hat keinen Betrag, weil sein steuerlicher Einstand nicht feststeht. Das gilt auch für einen Kauf ohne Gegenleistung.
- Ein Wechsel der Fondsart während der Haltedauer wird nicht nach § 22 InvStG behandelt. Ändern Sie die Fondsart eines Fonds, ändern sich auch frühere Jahre.
- Bei einem Teilübertrag zwischen Depots nimmt FIFO die ältesten Stücke, auch wenn die Bescheinigung der Bank andere nennt.
- Nicht abgedeckt sind Verlustbescheinigungen, Betriebsvermögen, Zusammenveranlagung, Termingeschäfte, Kryptowerte und Rohstoffe.
- Die Hilfe speichert keine abgegebenen Stände. Ändern Sie eine Transaktion, die Fondsart oder einen Basiszins, ändern sich auch bereits ausgewertete Jahre.
