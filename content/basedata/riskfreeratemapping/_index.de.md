---
title: "Risikofreier-Zinssatz-Zuordnung"
date: 2026-05-13T22:54:47+01:00
draft: false
weight: 65
archetype: "default"
---
Der **risikofreie Zinssatz** ist der theoretische Zinssatz, den eine Anlage ohne Ausfallrisiko erzielen würde. In GT dient er als Referenz für risikoadjustierte Kennzahlen — heute ausschliesslich für die [Sharpe-Ratio in **Rendite und statistische Daten**]({{< relref "/watchlistinstrument/correlation/returnstatdata" >}}). Weitere Auswertungen mit risikofreiem Zinssatz sind möglich, aber noch nicht umgesetzt.

Jede unterstützte **ISO-Währung** wird in dieser Zuordnung mit einem dedizierten Wertpapier verknüpft, dessen Schlusskurse den risikofreien Zinssatz für diese Währung wiedergeben. Die Werte werden als Dezimalbruch geführt — 4,32 % entsprechen also dem Schlusskurs 0,0432.

## Datenmodell
Eine Zuordnungszeile ist die eindeutige Brücke zwischen einer Währung und ihrem risikofreien Wertpapier. Die Kursdaten dieses Wertpapiers werden vom **FRED**-Datenanbieter geliefert und in der gewohnten Tabelle der historischen Kursdaten gespeichert.

{{< mermaid >}}
erDiagram
    Waehrung ||--|| Zuordnung : "ist Schluessel von"
    Zuordnung ||--|| Wertpapier : "zeigt auf"
    Wertpapier ||--o{ Historischer-Kurs : "liefert Zinssatz"
{{< /mermaid >}}

Das Wertpapier liegt in der speziellen Anlageklasse **Risikofreier Zinssatz** am eigens dafür angelegten Handelsplatz **Risk-Free Rate Sources**. Es ist nicht handelbar — es dient nur als Datencontainer für die Zinssatz-Historie.

## Voreingestellte Zuordnungen
Nach der erstmaligen Inbetriebnahme sind fünf Hauptwährungen bereits zugeordnet. Die Schlusskurse stammen von den folgenden FRED-Serien:

| Währung | Wertpapier                              | FRED-Serien-ID            |
|---------|-----------------------------------------|---------------------------|
| USD     | USD 3M Risk-Free Rate (FRED DGS3MO)     | `DGS3MO`                  |
| EUR     | EUR Risk-Free Rate (ESTR)               | `ECBESTRVOLWGTTRMDMNRT`   |
| GBP     | GBP Risk-Free Rate (SONIA)              | `IUDSOIA`                 |
| CHF     | CHF Risk-Free Rate (3M Interbank)       | `IR3TIB01CHM156N`         |
| JPY     | JPY Risk-Free Rate (Call Rate)          | `IRSTCI01JPM156N`         |

{{% notice style="warning" title="FRED-API-Schlüssel notwendig" %}}
Damit GT die Schlusskurse dieser Wertpapiere abrufen kann, muss ein Administrator unter [Verbindungs-API-Key]({{< relref "/admindata/connectorapikey" >}}) einen gültigen API-Schlüssel für den Anbieter **FRED** hinterlegen. Solange das nicht geschehen ist, bleiben die Schlusskurse leer und die Sharpe-Ratio kann nicht berechnet werden. Beim erstmaligen Einrichten plant GT die Ladeaufträge mit einer Stunde Verzögerung, damit der Schlüssel rechtzeitig registriert werden kann.
{{% /notice %}}

## Bedienung
Die Verwaltung ist im **Navigationsbaum** unter **Basisdaten → Risikofreier-Zinssatz-Zuordnung** erreichbar. Die Tabelle besteht aus drei Spalten:

- **Währung** — Auswahl aus den im System erfassten ISO-Währungen. Eine bereits verwendete Währung erscheint in den anderen Zeilen nicht mehr in der Auswahl. Die Währung kann nach dem Speichern nicht mehr geändert werden — eine versehentlich falsche Zeile ist zu löschen und neu anzulegen.
- **Risikofreies Instrument** — Auswahl aus den vorhandenen risikofreien Wertpapieren. Die Liste wird auf jene Wertpapiere beschränkt, deren Währung mit der gewählten Spalte **Währung** übereinstimmt; ein bereits verwendetes Wertpapier wird ausgeblendet. Wechselt man die Währung, leert sich diese Spalte automatisch und ist anschliessend neu zu setzen.
- **FRED-Serien-ID** — abgeleitete Anzeige der Serien-ID des gewählten Wertpapiers; nicht direkt editierbar.

Eine neue Zeile wird über das **Plus-Symbol** in der Tabellenüberschrift angelegt. Eine bestehende Zeile wird über das **Stift-Symbol** am Zeilenende bearbeitet beziehungsweise über das **Mülltonnen-Symbol** gelöscht.

{{% notice style="info" title="Währung und Wertpapier müssen übereinstimmen" %}}
Sowohl die Auswahlliste im Browser als auch eine zusätzliche Server-Prüfung stellen sicher, dass die Zeilen-Währung mit der Währung des gewählten Wertpapiers übereinstimmt. Eine widersprüchliche Zuordnung wird vom Server mit einer Fehlermeldung abgewiesen.
{{% /notice %}}

## Benutzerrechte
Jeder authentifizierte Benutzer darf die Tabelle lesen und neue Zeilen anlegen. Bestehende Zeilen darf nur der **Ersteller** bearbeiten oder löschen; darüber hinaus haben Benutzer mit der Rolle **Administrator** oder **ohne Limits** vollen Zugriff auf alle Zeilen. Benutzer der Rolle **Limits** sind zusätzlich auf zwei Änderungen pro Tag begrenzt — diesen Wert steuert der globale Parameter `gt.limit.day.RiskFreeRateMapping`.

## Eine neue Währung hinzufügen
Soll eine zusätzliche Währung mit einem risikofreien Zinssatz versorgt werden, ist zunächst das **synthetische Wertpapier** anzulegen — über die normale Wertpapierverwaltung mit der Anlageklasse **Risikofreier Zinssatz** und dem Handelsplatz **Risk-Free Rate Sources**. Sobald historische Kursdaten dafür vorliegen, kann das Wertpapier in dieser Tabelle einer Währung zugeordnet werden und erscheint in der Spalte **Risikofreies Instrument** als Auswahl.

{{% notice style="note" title="Kein Direktanlage-Dialog" %}}
Das Anlegen eines neuen risikofreien Wertpapiers direkt aus dieser Tabelle ist derzeit nicht implementiert. Das Wertpapier muss separat angelegt werden, bevor es zugeordnet werden kann.
{{% /notice %}}
