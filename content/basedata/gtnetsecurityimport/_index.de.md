---
title: "GTNet Wertpapierimport"
date: 2026-02-04T12:00:00+01:00
draft: false
weight: 55
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GTNet ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Der **GTNet Wertpapier-Import** ermöglicht es, mehrere Wertpapiere effizient über das GTNet-Peer-Netzwerk anzulegen. Anstatt jedes Wertpapier manuell mit Metadaten, Datenkonnektoren und Anlageklassen zu konfigurieren, können Benutzer die Informationen von anderen GTNet-Instanzen abrufen und automatisch übernehmen.
## Arbeitsablauf
Die Funktion folgt einem Master-Detail-Muster in drei Schritten:
1. **Import-Set erstellen**: Ein benanntes Set für die zu importierenden Wertpapiere anlegen.
2. **Import-Positionen hinzufügen**: Für jedes Wertpapier ISIN, Ticker und Währung angeben.
3. **GTNet-Abfrage starten**: Die Positionen werden im Hintergrund bei erreichbaren GTNet-Peers abgefragt und die Wertpapiere automatisch erstellt oder verknüpft.
## Import-Set verwalten
Ein **Import-Set** dient als Container für zusammengehörige Wertpapiere. Über das Kontextmenü kann ein neues Import-Set erstellt, bearbeitet oder gelöscht werden.
- **Name** (erforderlich): Ein aussagekräftiger Name für das Import-Set, z.B. "Portfolio Migration 2024".
- **Bemerkung** (optional): Zusätzliche Notizen zum Import-Set.
## Positionstabelle
Nach Auswahl eines Import-Sets werden die zugehörigen Positionen in einer Tabelle angezeigt.
**Bearbeitbare Spalten:**
- **ISIN**: Die internationale Wertpapierkennnummer (max. 12 Zeichen).
- **Ticker/Symbol**: Das Börsenkürzel (max. 6 Zeichen).
- **Währung**: Der ISO-4217-Währungscode (z.B. USD, EUR, CHF).
**Schreibgeschützte Spalten (nach Verknüpfung):**
- **Verknüpftes Wertpapier**: Der Name des gefundenen oder erstellten Wertpapiers.
- **Anlageklasse**: Die Kategorie des Wertpapiers.
- **Finanzinstrument**: Der Instrumententyp.
- **Unter Anlageklasse**: Die Unterkategorie.
{{% notice style="info" title="Validierung" %}}
Mindestens ISIN oder Ticker muss angegeben werden. Die Währung ist immer erforderlich.
{{% /notice %}}
## Positionen hinzufügen
Es gibt zwei Möglichkeiten, Positionen hinzuzufügen:
- **Manuell**: Über das Kontextmenü **Erstellen Import-Position** eine neue Zeile anlegen und die Daten eingeben.
- **CSV-Import**: Über **CSV-Datei hochladen** mehrere Positionen auf einmal importieren. Das CSV-Format verwendet Semikolon als Trennzeichen mit den Spaltenüberschriften `isin`, `tickerSymbol` und `currency`.
## GTNet-Abfrage starten
Nachdem Positionen erfasst wurden, kann über das Kontextmenü **Wertpapiere von GTNet importieren** die Abfrage gestartet werden. Ein Hintergrundauftrag fragt die erreichbaren GTNet-Peers ab und verknüpft oder erstellt die Wertpapiere basierend auf dem besten Treffer.
{{% notice style="warning" title="Hintergrundauftrag" %}}
Die GTNet-Abfrage wird als Hintergrundauftrag ausgeführt. Die aktuelle Ansicht wird **nicht automatisch aktualisiert**. Um den Status des Imports zu prüfen, verwenden Sie die **Hintergrund-Jobs** im Systemmenü. Sobald der Auftrag abgeschlossen ist, können Sie die Ansicht manuell aktualisieren, um die verknüpften Wertpapiere zu sehen.
{{% /notice %}}
{{% notice style="info" title="Tageslimit für Benutzer mit Limits-Berechtigung" %}}
Benutzer mit der Berechtigung "Limits" unterliegen einem täglichen Limit für die Anzahl der über GTNet erstellbaren Wertpapiere (standardmässig 150 pro Tag). Werden mehr Positionen angefragt als das Limit erlaubt, stoppt der Import-Vorgang und die verbleibenden Positionen werden nicht verarbeitet. Der Status kann in den **Hintergrund-Jobs** eingesehen werden, wo auch eine entsprechende Fehlermeldung angezeigt wird. Bereits erstellte Wertpapiere bleiben erhalten. Das Verknüpfen mit bereits existierenden Wertpapieren wird nicht auf das Limit angerechnet.
{{% /notice %}}
{{% notice style="info" title="Eigentümer der importierten Wertpapiere" %}}
Wertpapiere, die über GTNet erstellt werden, sind öffentlich zugänglich und gehören keinem bestimmten Mandanten. Alle Benutzer der Instanz können diese Wertpapiere verwenden. Der Benutzer, der den GTNet-Import auslöst, wird als Ersteller des Wertpapiers eingetragen. Dadurch können Benutzer mit der Berechtigung "Limits" die von ihnen importierten Wertpapiere direkt bearbeiten - allerdings unterliegt die Erstellung neuer Wertpapiere dem oben beschriebenen Tageslimit.
{{% /notice %}}
## Import historischer Kursdaten
Nach dem Erstellen der Wertpapiere werden automatisch historische Kursdaten importiert. GTNet ermittelt dabei den optimalen Datenanbieter: Das System fragt bei allen erreichbaren GTNet-Peers die verfügbare Datenabdeckung ab, also das früheste und letzte Datum der vorhandenen Kursdaten. Der Peer mit der längsten historischen Abdeckung und der besten Erfolgsrate wird ausgewählt, und die historischen Kursdaten werden von diesem Peer abgerufen. Falls keine GTNet-Peers historische Daten für das betreffende Wertpapier anbieten, werden die konfigurierten Datenkonnektoren als Fallback verwendet.
{{% notice style="info" title="Nicht alle Peers bieten Kursdaten" %}}
Die Bereitstellung von Wertpapier-Metadaten und historischen Kursdaten sind separate Funktionen in GTNet. Eine Peer-Instanz kann Wertpapierinformationen anbieten, ohne historische Kursdaten bereitzustellen. In solchen Fällen wird automatisch auf die lokalen Datenkonnektoren zurückgegriffen.
{{% /notice %}}
## Verknüpftes Wertpapier bearbeiten
Nach erfolgreicher Verknüpfung kann das importierte Wertpapier über das Kontextmenü **Bearbeiten Wertpapier** geöffnet und angepasst werden. Dies ermöglicht die Korrektur von Metadaten, Anlageklassen oder Datenkonnektoren.
## Positionen und Wertpapiere löschen
Das Kontextmenü bietet zwei Löschoptionen:
- **Datensatz löschen Import-Position**: Löscht nur den Positionseintrag. Das verknüpfte Wertpapier bleibt erhalten und kann weiterhin im System verwendet werden.
- **Verknüpftes Wertpapier löschen**: Entfernt die Verknüpfung zwischen Position und Wertpapier und löscht das Wertpapier aus dem System. Die Position bleibt erhalten und kann erneut über GTNet abgefragt werden. Diese Option ist nützlich, wenn das importierte Wertpapier nicht dem gewünschten Ergebnis entspricht.
## Übertragene Daten prüfen
Nach der GTNet-Abfrage sollten die erstellten Wertpapiere visuell überprüft werden:
- **Konnektorverfügbarkeit**: Die Peer-Instanz kann Wertpapiere mit Datenkonnektoren liefern, die auf der eigenen Instanz nicht verfügbar sind. Dies tritt häufig bei Konnektoren auf, die ein kostenpflichtiges Abonnement erfordern. Prüfen Sie, ob die historischen und Intraday-Konnektoren für Ihre Instanz funktionieren.
- **Anlageklassen-Unterschiede**: Die "Unter Anlageklasse" ist benutzerdefiniert und kann zwischen Instanzen variieren. Auch die Kategorisierung nach Region und Land kann unterschiedlich aktiviert sein. Überprüfen Sie die Anlageklassenzuordnung und passen Sie diese bei Bedarf an.
- **Ticker-Symbol-Einschränkungen**: Bei der Suche nur mit Ticker und Währung (ohne ISIN) können die Ergebnisse ungenau sein. Im Gegensatz zur ISIN, die weltweit eindeutig ist, ist das Ticker-Symbol nicht eindeutig und kann auf verschiedenen Börsen unterschiedliche Wertpapiere bezeichnen. ISIN-basierte Abfragen sind zuverlässiger.
## Erweiterte Details
Zeilen in der Positionstabelle können erweitert werden, um zusätzliche Informationen anzuzeigen. Der Inhalt hängt davon ab, ob ein Wertpapier erfolgreich verknüpft wurde oder nicht.
### Bei verknüpftem Wertpapier
Wenn ein Wertpapier verknüpft ist, zeigt die erweiterte Ansicht die URLs der konfigurierten Datenkonnektoren für historische Kurse, Intraday-Kurse, Dividenden und Splits.
### Bei nicht verknüpftem Wertpapier (Import-Lücken)
Wenn die GTNet-Abfrage ein Ergebnis von einer Peer-Instanz erhalten hat, aber kein Wertpapier erstellt werden konnte, werden die **Import-Lücken** angezeigt. Diese dokumentieren, warum die Übertragung nicht vollständig möglich war.
{{% notice style="info" title="Wann entstehen Import-Lücken?" %}}
Import-Lücken entstehen, wenn die lokale Instanz nicht alle Konfigurationen der Peer-Instanz unterstützt. Die Peer-Instanz sendet Informationen über das Wertpapier, aber bestimmte Elemente können lokal nicht zugeordnet werden. Dies verhindert nicht unbedingt die Erstellung des Wertpapiers, dokumentiert aber die Abweichungen.
{{% /notice %}}
Die Tabelle zeigt für jede Lücke den **Lückentyp**, die **erwartete Konfiguration** (was die Peer-Instanz anbietet) und die **GTNet Domäne** (von welcher Peer-Instanz das Ergebnis stammt). Folgende Lückentypen können auftreten:
- **Anlageklasse**: Die Kombination aus Anlageklasse, Unter Anlageklasse und Finanzinstrument der Peer-Instanz konnte lokal nicht exakt zugeordnet werden. Die erwartete Konfiguration zeigt das Format "Anlageklasse / Unter Anlageklasse / Finanzinstrument" der Peer-Instanz.
- **Intraday-Konnektor**: Die Peer-Instanz verwendet einen Konnektor für Intraday-Kurse, der lokal nicht verfügbar ist. Die erwartete Konfiguration zeigt den Konnektornamen (z.B. "yahoo", "finnhub").
- **Historischer Konnektor**: Die Peer-Instanz verwendet einen Konnektor für historische Kurse, der lokal nicht verfügbar ist.
- **Dividenden-Konnektor**: Die Peer-Instanz verwendet einen Konnektor für Dividendendaten, der lokal nicht verfügbar ist.
- **Split-Konnektor**: Die Peer-Instanz verwendet einen Konnektor für Aktiensplit-Daten, der lokal nicht verfügbar ist.
{{% notice style="warning" title="Konnektoren mit Abonnement" %}}
Manche Konnektoren erfordern ein kostenpflichtiges Abonnement und einen API-Schlüssel. Wenn die Peer-Instanz solche Konnektoren verwendet, Ihre Instanz aber keinen entsprechenden API-Schlüssel konfiguriert hat, wird eine Konnektor-Lücke angezeigt. Das Wertpapier kann dennoch erstellt werden, aber die betroffenen Kursdaten müssen dann über einen anderen Konnektor bezogen werden.
{{% /notice %}}
