---
title: "Transaktionskosten"
date: 2026-09-24T12:00:00+02:00
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
Damit der Courtagenvergleich Ergebnisse liefert, muss für das gewählte Depot ein Gebührenmodell konfiguriert sein — entweder auf dem zugehörigen Handelsplattform Plan oder als depotspezifisches Gebührenmodell.
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

## Beobachtete Wechselkursabweichung vom Tagesschlusskurs
Das Register **Gebührenmodellvergleich** zeigt zusätzlich **Beobachtete Abweichung vom Tagesschlusskurs — Wertschriftendepot** für das gewählte Depot und **Beobachtete Abweichung vom Tagesschlusskurs — Mandantentransfers** für die verbundenen Kontotransfers des Mandanten. Die Depottabelle berücksichtigt Käufe, Verkäufe und Erträge mit einem erfassten Währungspaar. Diese Beobachtungen werden unabhängig vom Courtagenvergleich geladen und stehen auch ohne Gebührenmodell zur Verfügung. **Nullkosten ausschliessen** betrifft nur den Courtagenvergleich.
Eine positive Abweichung bedeutet Kosten für den Kunden, unabhängig davon, ob die erste Währung des Paars gekauft oder verkauft wird. GT vergleicht den erfassten Wechselkurs mit dem gespeicherten Schlusskurs desselben Paars am Buchungstag. Ein Kauf der ersten Währung zu einem Kurs 1% über dem Schlusskurs und ein Verkauf zu einem Kurs 1% darunter ergeben beispielsweise beide eine positive Beobachtung von 1%. Ein Kontotransfer wird einmal über seine Belastung gezählt. Er lässt sich keinem Depot zuordnen; sein modellierter Aufschlag bleibt leer.
### Kennzahlen lesen
Die Zeilen sind nach Währungspaar, Umrechnungsart und gültiger Periode des Währungstarifs gruppiert. **Mittlere Abweichung (%)** und **Median der Abweichung (%)** beschreiben die berücksichtigten Beobachtungen. **Stichproben-Standardabweichung (%)** verwendet die Stichprobenformel und bleibt bei einer einzigen Beobachtung leer. **Volumengewichtete Abweichung (%)** gewichtet grössere Umrechnungen stärker. Massgebend ist der umgerechnete Nettobetrag, zum Schlusskurs in der zweiten Währung des Paars bewertet.
**Mittlerer modellierter Aufschlag (%)** mittelt den anwendbaren prozentualen Tarif nur über abgedeckte Beobachtungen. **Beobachtungen mit Tarif** zählt auch einen ausdrücklich auf null gesetzten Aufschlag. Fehlende Tarifabschnitte, nicht abgedeckte Tage, fehlende passende Regeln und ein fehlender historischer Kurs für eine Tarifstufe in einer dritten Währung werden getrennt gezählt, in den Spalten **Kein Währungstarif konfiguriert**, **Kein Währungstarif für dieses Datum**, **Keine Währungsregel für diese Umrechnung** und **Historischer Kurs für die Tarifwährung fehlt**. **Beobachtungen ohne passenden Tarif** ist deren Summe, einschliesslich **Ungültiger Währungstarif oder ungültige Anfrage**. Ein ungültiger Tarif erscheint mit seiner Meldung in der Spalte **Fehler** und niemals als Nullaufschlag. Der Währungstarif des Depots hat unabhängig vom Courtagen- und Depotgebührenmodell Vorrang vor dem entsprechenden Abschnitt des Plans.
Fehlende erfasste Kurse, fehlende Schlusskurse am genauen Buchungstag und absolute Kursabweichungen von mindestens 8% werden ausgeschlossen und getrennt gezählt. Zuerst wird der erfasste Kurs geprüft, sodass jede Zeile nur einmal zählt. Die 8%-Grenze weist auf mögliche Datenfehler hin, etwa einen verkehrt erfassten Kurs oder darin enthaltene Gebühren; sie beweist keinen Buchungsfehler. Bei weniger als fünf berücksichtigten Beobachtungen weist die Spalte **Einordnung der Stichprobe** darauf hin, dass die Statistik nicht aussagekräftig ist.
### Einordnung und Grenzen
Dies ist eine **beobachtete Abweichung vom Tagesschlusskurs**, keine gemessene Brokergebühr. Jede Beobachtung enthält den Aufschlag und die Differenz zwischen dem Kurs bei der Ausführung und dem Schlusskurs. Regelmässige Ausführungszeiten, eine mit Marktbewegungen verbundene Umrechnungsrichtung, korrelierte Wechselkurse und ungleiche Beträge können dauerhafte Verzerrungen verursachen. Mehr Beobachtungen garantieren nicht, dass sich der Mittelwert dem konfigurierten Tarif annähert.
Der Vergleich erfasst prozentuale Aufschläge ab null und unter 5%. Fixe Umrechnungsgebühren und Mindestgebühren werden nicht modelliert. Kaufbeträge enthalten Courtage, Steuern und Marchzinsen; Verkäufe und Erträge verwenden den Nettoerlös. Bei CFDs und anderen Margin-Instrumenten zählt der tatsächlich umgerechnete Betrag: die Kosten beim Eröffnen einer Position und das Ergebnis beim Schliessen, das auch die Richtung der Umrechnung bestimmt. Gestaffelte Tarife verwenden die im Tarif festgelegte Betragswährung, unabhängig von der Mandantenwährung. Die Bewertung in der Berichtswährung und die Darstellung einer deklarierten Dividende in der Wertpapierwährung erzeugen keinen zusätzlichen Aufschlag. Berechnet eine Courtagenregel die Umrechnung bereits anhand der Abrechnungswährung, setzen Sie für diese Umrechnungen einen ausdrücklichen Nullaufschlag im Währungstarif des Depots, damit dieselben Kosten nicht zweimal modelliert werden.
