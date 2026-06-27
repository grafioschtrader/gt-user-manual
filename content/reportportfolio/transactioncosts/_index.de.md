---
title: "Transaktionskosten"
date: 2026-02-26T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
Die Auswertung **Transaktionskosten** bietet eine umfassende Analyse aller beim Wertpapierhandel angefallenen Kosten und Steuern. Sie steht ausschliesslich auf **Mandantenebene** zur Verfügung und analysiert alle Wertpapiertransaktionen über alle Portfolios und Depots hinweg. Alle Werte werden automatisch in die Hauptwährung umgerechnet, wobei historische Wechselkurse für eine präzise Kostenanalyse verwendet werden.
Der Report ist als Registerkartenmenü mit zwei Registerkarten aufgebaut:
- **Transaktionskosten**: Zeigt die Kostenübersicht und Detailauswertung pro Depot (in den folgenden Abschnitten dokumentiert).
- **Gebührenmodellvergleich**: Vergleicht die geschätzten Kosten des YAML-Gebührenmodells mit den tatsächlich erfassten Transaktionskosten. Siehe [Gebührenmodellvergleich](#gebührenmodellvergleich) am Ende dieser Seite.
## Aufbau der Auswertung
Die Auswertung gliedert sich in zwei Ebenen. Die **Haupttabelle** zeigt eine Zusammenfassung pro Depot mit aggregierten Kostenwerten. Durch Aufklappen einer Zeile werden die einzelnen Transaktionen sichtbar, die zu diesen Kosten geführt haben. Diese hierarchische Struktur ermöglicht sowohl einen schnellen Überblick als auch detaillierte Analysen auf Transaktionsebene.
Die Haupttabelle zeigt für jedes Depot folgende Spalten: **Name** des Depots, **Steuer (jede Art)** für die Summe aller gezahlten Steuern in Hauptwährung, **Bezahlte Transaktionen** für die Anzahl der kostenpflichtigen Transaktionen, **Durchschnitt Transaktionskosten** für die durchschnittlichen Kosten pro Transaktion und **Transaktionskosten** für die Gesamtsumme aller Ordergebühren ohne Steuern. Die Fusszeile zeigt die Gesamtsummen über alle Depots hinweg.
## Detailansicht der Transaktionen
In der Detailansicht werden alle einzelnen Transaktionen eines Depots mit ihren jeweiligen Kosten angezeigt. Die Spalten umfassen **Datum** und **Name** des Wertpapiers, **Handelsplatz** an dem das Wertpapier gehandelt wurde, **Transaktionstyp** für die Art der Transaktion, **Währung Konto** und **Währung Wertpapier** für die verwendeten Währungen, **Transaktionskosten** in der Originalwährung, **Steuer (jede Art)** in Hauptwährung, **Nettopreis** als Transaktionsvolumen in Hauptwährung, **Wechselkurs** für die Währungsumrechnung und **Transaktionskosten** in Hauptwährung. Die Detailtabelle verwendet eine Paginierung und zeigt standardmässig 20 Transaktionen pro Seite.
## Funktionen
Die Haupttabelle kann nach allen Spalten sortiert werden, um beispielsweise die kostenintensivsten Depots zu identifizieren. Die Sortierung nach durchschnittlichen Transaktionskosten zeigt, welche Depots im Verhältnis zur Transaktionsgrösse am günstigsten sind. In der Detailansicht können Transaktionen chronologisch oder nach Kostenhöhe sortiert werden.
Die Währungsumrechnung erfolgt automatisch in die Hauptwährung des Mandanten. Das System verwendet dabei historische Wechselkurse zum jeweiligen Transaktionszeitpunkt. In der Detailansicht wird sowohl der Originalbetrag als auch der verwendete Wechselkurs angezeigt, sodass die Konvertierung jederzeit überprüft werden kann.
### Transaktionen bearbeiten und löschen
In der Detailansicht können einzelne Transaktionen über das **Kontextmenü** oder das Menü **Bearbeiten** bearbeitet oder gelöscht werden. Dies ist nützlich wenn Transaktionskosten nachträglich korrigiert werden müssen. Die verfügbaren Optionen entsprechen denen in der Auswertung [Transaktionen](../transactionlist/). Nach jeder Änderung wird der Bericht automatisch neu geladen um die aktuellen Werte anzuzeigen.
## Diagramm-Darstellung
Über das Menü **Ansicht** und die Option **Zeige Diagramm** wird ein Streudiagramm im Zusatzbereich angezeigt. Jeder Datenpunkt repräsentiert eine einzelne Transaktion. Die X-Achse zeigt den Nettopreis der Transaktion in Hauptwährung, die Y-Achse die angefallenen Transaktionskosten. Jedes Depot wird als separate Datenserie mit eigener Farbe dargestellt. Die Serien sind standardmässig ausgeblendet und können durch Klicken auf die Legende aktiviert werden. Wenn Sie mit der Maus über das Diagramm fahren, werden Ihnen detaillierte Informationen zu den jeweiligen Datenpunkten angezeigt. Durch Klicken auf einen Datenpunkt wird die entsprechende Zeile in der Detailtabelle automatisch selektiert.
## Filterkriterien
Das System wendet automatisch Filterkriterien an um den Bericht auf relevante Transaktionen zu fokussieren. Nur Transaktionen mit tatsächlichen Kosten werden berücksichtigt. Transaktionen bei denen das Feld Transaktionskosten null oder leer ist, werden nicht aufgenommen. Geldmarkt-Direktanlagen werden ausgeschlossen, da diese Anlageklasse typischerweise keine traditionellen Maklergebühren verursacht. Alle anderen Wertpapiertransaktionen werden erfasst, unabhängig von der Anlageklasse oder dem Transaktionstyp.
## Gebührenmodellvergleich
Der Gebührenmodellvergleich überprüft, ob das YAML-basierte Gebührenmodell die tatsächlich angefallenen Transaktionskosten für historische Kauf- und Verkaufstransaktionen korrekt vorhersagt. Voraussetzung ist ein konfiguriertes Gebührenmodell — entweder auf dem [Handelsplattform Plan]({{% ref "/basedata/tradingplatformplan" %}}) oder als depotspezifische Überschreibung auf dem [Depot]({{% ref "/tenantportfolio/securityaccounts" %}}).
{{% notice style="warning" title="Voraussetzung" %}}
Damit der Vergleich Ergebnisse liefert, muss für das gewählte Depot ein Gebührenmodell konfiguriert sein — entweder auf dem zugehörigen Handelsplattform Plan oder als depotspezifisches Gebührenmodell.
{{% /notice %}}
### Eingabesteuerung
Zwei Steuerelemente bestimmen den Umfang der Analyse:
- **Depot**: Wählt das zu analysierende Depot aus. Die Auswahlliste zeigt die Depots im Format «Portfolio / Depot».
- **Nullkosten ausschliessen**: Blendet Transaktionen ohne Transaktionskosten aus (standardmässig aktiviert).
### Zusammenfassung
Nach Auswahl eines Depots erscheinen zwei Zusammenfassungspanels.
Das Panel **Übersicht** zeigt folgende Felder:
| Feld | Beschreibung |
|------|-------------|
| **Plan** | Name des Handelsplattform Plans. Falls das Depot ein eigenes Gebührenmodell hat, wird dies entsprechend gekennzeichnet. |
| **Total** | Gesamtzahl der Kauf-/Verkaufstransaktionen des Depots. |
| **Verglichen** | Anzahl der erfolgreich verglichenen Transaktionen. |
| **Übersprungen** | Anzahl der übersprungenen Transaktionen (Nullkosten). |
| **Fehler** | Anzahl der Transaktionen, bei denen die Schätzung fehlgeschlagen ist (z.B. keine passende Regel). |

Das Panel **Genauigkeit** zeigt statistische Kennzahlen zur Modellqualität:
| Feld | Beschreibung |
|------|-------------|
| **Mittlerer absoluter Fehler** | Durchschnittliche absolute Abweichung zwischen Istkosten und geschätzten Kosten. |
| **Mittlerer relativer Fehler** | Durchschnittliche relative Abweichung in Prozent. |
| **RMSE** | Wurzel des mittleren quadratischen Fehlers — gewichtet grössere Abweichungen stärker. |
### Detailtabelle
Die Detailtabelle zeigt den Vergleich auf Ebene der einzelnen Transaktionen. Folgende Spalten sind verfügbar:
| Spalte | Beschreibung |
|--------|-------------|
| **Transaktionsdatum** | Datum der Transaktion. |
| **Transaktionstyp** | Art der Transaktion (Kauf/Verkauf). |
| **Wertpapier** | Name des gehandelten Instruments. |
| **Anlageklassentyp** | Anlageklassenkategorie (z.B. Aktien, Obligationen). |
| **Finanzinstrument** | Spezieller Anlageinstrumententyp (z.B. ETF, Direktanlage). |
| **MIC** | MIC-Code des Handelsplatzes. |
| **Istkosten** | Tatsächlich erfasste Transaktionskosten. |
| **Geschätzte Kosten** | Vom Gebührenmodell berechnete Kosten. |
| **Relativer Fehler %** | Prozentuale Abweichung zwischen Ist- und Schätzwert. Die Spalte ist farblich kodiert: Grün zeigt geringe Abweichung (~0 %), über Gelb (~25 %) bis Rot (50 %+). |
| **Währung** | Währung der Transaktion. |
| **Handelswert** | Gesamtbetrag des Trades (Kurs × Stückzahl). |
| **Kurs** | Kurs zum Transaktionszeitpunkt. |
| **Stückzahl** | Anzahl der gehandelten Anteile. |
| **Passende Regel** | Name der Gebührenmodellregel, die für diese Transaktion gegriffen hat. |
### Modellhierarchie
Wenn ein Depot ein eigenes Gebührenmodell definiert, hat dieses Vorrang vor dem Modell des Handelsplattform Plans. Das Panel **Übersicht** zeigt an, welches Modell für den Vergleich verwendet wurde. Weitere Informationen zur depotspezifischen Überschreibung finden Sie unter [Depots — Gebührenmodell]({{% ref "/tenantportfolio/securityaccounts" %}}).
