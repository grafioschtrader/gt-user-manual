---
title: "Transaktionskosten"
date: 2025-10-25T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
Der **Transaktionskosten-Report** bietet eine umfassende Analyse aller beim Wertpapierhandel angefallenen Kosten und Steuern. Dieser Report hilft dabei, die Auswirkungen von Handelskosten auf die Investmentperformance zu verstehen und Optimierungspotenziale zu identifizieren. Alle Werte werden automatisch in die Mandantenwährung konvertiert, wobei historische Wechselkurse für eine präzise Kostenanalyse verwendet werden.

## Aufruf des Reports
Der Transaktionskosten-Report kann ausschliesslich auf Mandantenebene aufgerufen werden. Er analysiert alle Wertpapiertransaktionen über alle Portfolios und Depots hinweg, die Kosten verursacht haben. Der Zugriff erfolgt über das Hauptmenü → **Depots und Konten** → **Portfolios** → **Transaktionskosten**. Das System lädt die Daten asynchron und nutzt dabei parallele Datenabfragen für optimale Performance.

## Was beinhaltet der Report?
Der Report fokussiert sich auf die Kostenanalyse des Wertpapierhandels und erfasst alle Transaktionen, die Gebühren oder Steuern verursacht haben. Dabei werden intelligente Filterkriterien angewendet, um nur relevante Transaktionen einzubeziehen. Geldmarkt-Direktanlagen werden ausgeschlossen, da diese typischerweise keine traditionellen Maklergebühren aufweisen. Die Analyse umfasst Ordergebühren, Steuern aller Art, Börsengebühren und weitere transaktionsbezogene Kosten.

Der Report gliedert sich in zwei Ebenen. Die Haupttabelle zeigt eine Zusammenfassung pro Wertpapierdepot mit aggregierten Kostenwerten. Durch Aufklappen einer Zeile werden die einzelnen Transaktionen sichtbar, die zu diesen Kosten geführt haben. Diese hierarchische Struktur ermöglicht sowohl einen schnellen Überblick als auch detaillierte Analysen auf Transaktionsebene.

## Spaltenübersicht der Haupttabelle
Die Haupttabelle gruppiert alle Transaktionskosten nach Wertpapierdepot und zeigt folgende aggregierte Werte:

| Spalte | Beschreibung |
|--------|-------------|
| **Name** | Name des Wertpapierdepots (z.B. Broker oder Bank) |
| **Steuer (jede Art)** | Summe aller gezahlten Steuern für dieses Depot in Mandantenwährung |
| **Bezahlte Transaktionen** | Anzahl der Transaktionen, die Kosten verursacht haben |
| **Durchschnittliche bezahlte Transaktion** | Durchschnittliche Transaktionskosten pro bezahlter Transaktion in Mandantenwährung |
| **Transaktionskosten** | Summe aller Transaktionskosten (ohne Steuern) für dieses Depot in Mandantenwährung |

Die Fusszeile der Tabelle zeigt die Gesamtsummen über alle Depots hinweg. Dies ermöglicht einen schnellen Überblick über die gesamten Handelskosten des Mandanten und den Vergleich verschiedener Broker bezüglich ihrer Kosteneffizienz.

## Detailansicht der Transaktionen
Durch Klick auf das Aufklapp-Symbol vor einem Depot werden alle einzelnen Transaktionen angezeigt, die zu den aggregierten Kosten beigetragen haben. Diese Detailtabelle zeigt folgende Informationen:

| Spalte | Beschreibung |
|--------|-------------|
| **Datum** | Datum und Uhrzeit der Transaktion |
| **Name** | Name des gehandelten Wertpapiers |
| **Börse** | Name der Börse, an der das Wertpapier gehandelt wurde |
| **Transaktionstyp** | Art der Transaktion (Kauf, Verkauf, Dividende, Finanzierungskosten) |
| **Währung Cashkonto** | Währung des verwendeten Cashkontos |
| **Währung Wertpapier** | Währung des Wertpapiers |
| **Transaktionskosten** | Ordergebühren in der Originalwährung der Transaktion |
| **Steuer** | Gezahlte Steuern in Mandantenwährung |
| **Nettowert Transaktion** | Basispreis (Stückzahl × Kurs + Stückzinsen) in Mandantenwährung |
| **Wechselkurs** | Verwendeter Wechselkurs für die Währungsumrechnung |
| **Transaktionskosten (MC)** | Ordergebühren umgerechnet in Mandantenwährung |

Die Detailtabelle verwendet Paginierung und zeigt standardmässig 20 Transaktionen pro Seite. Die Sortierung erfolgt nach Transaktionsdatum absteigend, kann aber durch Klick auf die Spaltenüberschriften angepasst werden. Mehrfachsortierung ist durch Halten der Strg/Cmd-Taste möglich.

## Funktionalität
Der Report bietet verschiedene Funktionen zur Analyse und Verwaltung der Transaktionskosten:

### Sortierung und Gruppierung
Die Haupttabelle kann nach allen Spalten sortiert werden, um beispielsweise die kostenintensivsten Depots zu identifizieren. Die Sortierung nach durchschnittlichen Transaktionskosten zeigt, welche Broker im Verhältnis zur Transaktionsgrösse am günstigsten sind. In der Detailansicht können Transaktionen chronologisch oder nach Kostenhöhe sortiert werden.

### Transaktionen bearbeiten und löschen
In der Detailansicht können einzelne Transaktionen über das Kontextmenü (Rechtsklick) bearbeitet oder gelöscht werden. Dies ist nützlich, wenn Transaktionskosten nachträglich korrigiert werden müssen. Die verfügbaren Optionen entsprechen denen im Transaktionen-Report. Nach jeder Änderung wird der Report automatisch neu geladen, um die aktuellen Werte anzuzeigen.

{{% notice tip %}}
Über das Kontextmenü in der Detailansicht können die Transaktionsdaten auch als CSV-Datei exportiert werden. Dies ermöglicht weiterführende Analysen in Tabellenkalkulationsprogrammen oder die Archivierung der Kostendaten.
{{% /notice %}}

### Währungsumrechnung
Alle Kosten und Steuern werden automatisch in die Mandantenwährung konvertiert. Das System verwendet dabei historische Wechselkurse zum jeweiligen Transaktionszeitpunkt, um eine präzise Kostenanalyse unabhängig von den ursprünglich verwendeten Handelswährungen zu gewährleisten. Dies ist besonders wichtig bei internationalen Portfolios mit Transaktionen in verschiedenen Währungen.

Die Umrechnung erfolgt transparent und nachvollziehbar. In der Detailansicht wird sowohl der Originalbetrag als auch der verwendete Wechselkurs angezeigt, sodass die Konvertierung jederzeit überprüft werden kann. Bei Transaktionen in der Mandantenwährung beträgt der Wechselkurs 1,0.

## Chart-Visualisierung
Der Report bietet eine grafische Darstellung der Transaktionskosten als Streudiagramm. Der Chart wird über das Menü mit der Option "Grafik anzeigen" aufgerufen und öffnet sich in einem separaten Bereich unterhalb der Hauptansicht.

Im Streudiagramm repräsentiert jeder Datenpunkt eine einzelne Transaktion. Die X-Achse zeigt den Nettowert der Transaktion (Transaktionsvolumen in Mandantenwährung), die Y-Achse die angefallenen Transaktionskosten. Jedes Depot wird als separate Datenserie mit eigener Farbe dargestellt, wobei die Serien standardmässig ausgeblendet sind und durch Klick auf die Legende aktiviert werden müssen.

Der Chart verwendet Plotly und bietet umfangreiche Interaktionsmöglichkeiten. Sie können in den Chart hineinzoomen, einzelne Datenpunkte per Mauszeiger inspizieren und Datenserien ein- oder ausblenden. Die Hover-Funktion zeigt detaillierte Informationen zu jeder Transaktion an. Durch Klick auf einen Datenpunkt wird die entsprechende Zeile in der Detailtabelle automatisch selektiert und der Tabellenbereich scrollt zur selektierten Transaktion.

{{% notice style="info" title="Kostenanalyse mit dem Chart" %}}
Das Streudiagramm ist besonders nützlich, um Kostenausreisser zu identifizieren. Transaktionen mit ungewöhnlich hohen Kosten im Verhältnis zum Transaktionsvolumen werden sofort sichtbar. Der Vergleich verschiedener Depots im Chart zeigt, welcher Broker konsistent günstigere Konditionen bietet.
{{% /notice %}}

## Typische Anwendungsfälle
Der Transaktionskosten-Report unterstützt verschiedene Analyseansätze für die Optimierung der Handelskosten:

**Broker-Vergleich**: Durch die Gruppierung nach Depot können Sie die Kostenstrukturen verschiedener Broker direkt vergleichen. Die durchschnittlichen Transaktionskosten pro bezahlter Transaktion zeigen, welcher Broker am kosteneffizientesten ist. Besonders bei aktiven Handelsstrategien können diese Unterschiede erheblich sein.

**Kostenausreisser identifizieren**: Die Detailansicht und der Chart helfen dabei, einzelne Transaktionen mit unverhältnismässig hohen Kosten zu finden. Dies können Fehler bei der Erfassung sein oder tatsächlich überhöhte Gebühren, die möglicherweise reklamiert werden sollten.

**Handelsverhalten optimieren**: Die Analyse zeigt, ob bestimmte Transaktionsarten oder -grössen besonders kostenintensiv sind. Möglicherweise lohnt es sich, kleinere Orders zu konsolidieren oder bestimmte Handelsplätze zu bevorzugen.

**Steuerliche Dokumentation**: Die vollständige Aufstellung aller gezahlten Steuern pro Depot erleichtert die Steuererklärung und dient als Nachweis gegenüber den Finanzbehörden.

**Kostenbudgetierung**: Die Gesamtkostenübersicht hilft bei der Planung zukünftiger Handelsaktivitäten und bei der Bewertung, ob die Handelskosten in einem angemessenen Verhältnis zu den erzielten Erträgen stehen.

## Filterkriterien und Ausschlüsse
Das System wendet automatisch intelligente Filterkriterien an, um den Report auf relevante Transaktionen zu fokussieren:

Nur Transaktionen mit tatsächlichen Kosten werden berücksichtigt. Transaktionen, bei denen das Feld "Transaktionskosten" null oder leer ist, werden nicht in den Report aufgenommen. Dies verhindert, dass kostenfreie Transaktionen die Statistiken verfälschen.

Geldmarkt-Direktanlagen werden ausgeschlossen, da diese Anlageklasse typischerweise keine traditionellen Maklergebühren verursacht. Die Kosten bei diesen Instrumenten sind oft bereits im Preis einkalkuliert oder werden anders abgerechnet. Diese Filterung sorgt für eine aussagekräftige Vergleichbarkeit der tatsächlichen Handelskosten.

Alle anderen Wertpapiertransaktionen werden erfasst, unabhängig von der Anlageklasse oder dem Transaktionstyp. Dies umfasst Aktien, Anleihen, ETFs, Fonds, Derivate und Devisen. Auch Dividendentransaktionen werden berücksichtigt, falls bei diesen Steuern angefallen sind.

## Einschränkungen und Hinweise
Bei der Interpretation des Reports sollten folgende Aspekte beachtet werden:

Die Kostenanalyse basiert auf den erfassten Transaktionsdaten. Wenn Transaktionskosten bei der Erfassung nicht korrekt eingegeben wurden, kann dies die Aussagekraft des Reports beeinträchtigen. Es empfiehlt sich daher, die Vollständigkeit und Korrektheit der Kostendaten regelmässig zu überprüfen.

Die Währungsumrechnung verwendet historische Wechselkurse. Bei fehlenden historischen Kursen für bestimmte Währungspaare kann die Konvertierung ungenau sein. In solchen Fällen sollten die entsprechenden Wechselkurse manuell nachgetragen werden.

Der Report zeigt nur direkt erfasste Transaktionskosten. Indirekte Kosten wie Spreads zwischen Kauf- und Verkaufskurs oder Opportunitätskosten durch verzögerte Orderausführung werden nicht berücksichtigt. Auch laufende Depotgebühren, die nicht einzelnen Transaktionen zugeordnet sind, erscheinen separat als Konto-Transaktionen.

Die durchschnittlichen Transaktionskosten sind ein Mittelwert über alle Transaktionsgrössen. Bei stark variierenden Ordervolumen kann dieser Wert irreführend sein. Für eine detaillierte Kostenstrukturanalyse sollte zusätzlich der Chart konsultiert werden.
