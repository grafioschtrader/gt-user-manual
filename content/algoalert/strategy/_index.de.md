---
title: "Strategien"
date: 2026-09-26T12:00:00+02:00
draft: false
weight: 20
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="In Bearbeitung - Zielversion V0.40.0" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
GT unterstützt verschiedene Strategietypen, von einfachen Preisalarmen bis zu komplexen Handelsstrategien. Jede Strategie wird einem Knoten in der hierarchischen Baumstruktur der Portfoliobasierten Strategie zugewiesen und überwacht die zugehörigen Wertpapiere anhand der konfigurierten Bedingungen.

## Einfache Strategien
Einfache Strategien verwenden flache Schlüssel-Wert-Parameter, die über dynamisch generierte Formularfelder konfiguriert werden. Beim Erstellen einer Strategie über **Erstellen Strategie Definition** wählen Sie im Dialog **Strategie Definition** unter **Strategiename** den Strategietyp aus, und das System zeigt automatisch die passenden Eingabefelder an. Angeboten werden nur die Typen, die auf der gewählten Ebene zulässig sind und dort nicht bereits in einer einmaligen Ausprägung bestehen. Beim Bearbeiten lassen sich nur die Parameter ändern, nicht aber der Strategietyp. Zu den einfachen Strategien gehören die Preisalarme, Indikator-Alarme und das Portfolio-Rebalancing.

## Komplexe Strategien
Komplexe Strategien besitzen eine verschachtelte Konfiguration. Bei der Auswahl einer komplexen Strategie erscheint anstelle der dynamischen Formularfelder ein YAML-Editor mit Syntaxhervorhebung, automatischer Vervollständigung, laufender Syntaxprüfung und Erläuterungen beim Überfahren eines Schlüssels mit der Maus. Eine neue Strategie ist mit einer Vorlage vorbelegt; **Vorlage laden** stellt diese jederzeit wieder her. Das Kontrollkästchen **Aktiv** erscheint nur bei komplexen Strategien: Ist es ausgeschaltet, lässt sich auch ein noch unvollständiger Entwurf speichern, der nicht ausgewertet wird. Gespeichert wird mit **Übernehmen**.

Die gemeinsame Bedienung beschreibt [YAML bearbeiten und prüfen]({{% ref "/intro/userinterface#yaml-editor" %}}). **Prüfen** berücksichtigt, ob **Aktiv** eingeschaltet ist: Ein inaktiver Entwurf darf unvollständig bleiben, eingegebener YAML-Text muss jedoch syntaktisch gültig sein. Für eine aktive Strategie gelten zusätzlich die Anforderungen an eine ausführbare Konfiguration. **Übernehmen** prüft erneut; bei Fehlern bleibt der Text im Dialog erhalten.

## Strategietypen im Überblick

{{< mermaid >}}
graph TD
    S["Strategietypen"] --> R["Rebalancing"]
    S --> P["Preisalarme"]
    S --> I["Indikator-Alarme"]
    S --> K["Komplexe Strategien"]
    R --> R1["Portfolio-Neugewichtung"]
    P --> P1["Absoluter Gewinn/Verlust"]
    P --> P2["Bestand Gewinn/Verlust"]
    P --> P3["Gewinn/Verlust in einer Periode"]
    I --> I1["Gleitender Durchschnitt Kreuzung"]
    I --> I2["RSI Schwellenwert"]
    I --> I3["Benutzerdefinierter Ausdruck"]
    K --> K1["Mean-Reversion-Dip"]
{{< /mermaid >}}

Die folgende Tabelle listet alle verfügbaren Strategietypen mit ihren Einsatzebenen auf. Die Ebene gibt an, ob die Strategie der Portfoliobasierten Strategie, einer Anlageklasse oder einem Wertpapier zugewiesen werden kann. Die Spalte Wirkung nennt, was die Strategie in der Portfolioüberwachung Ihres eigenen Portfolios und in einer Simulation bewirkt. Alarme werden in einer Simulation nicht ausgewertet, weil sie weder Richtung noch Menge eines Auftrags kennen; die Begründung steht unter [Welche Strategie wo wirkt](../#welche-strategie-wo-wirkt). Die Portfolio-Neugewichtung und der Mean-Reversion-Dip lassen sich je Knoten nur einmal erfassen, alle übrigen Typen mehrfach.

| Strategietyp | Ebenen | Kategorie | Details | Wirkung |
|---|---|---|---|---|
| [Portfolio-Neugewichtung](./rebalancing/) | Portfoliobasierte Strategie | Rebalancing | Vergleich der Allokation mit den Zielgewichtungen des Baums | Überwachung: Empfehlung · Simulation: Käufe und Verkäufe |
| [Absoluter Gewinn/Verlust](./pricealerts/) | Wertpapier | Preisalarm | Alarm bei absoluten Preisgrenzen | Nur Überwachung: Alarm |
| [Bestand Gewinn/Verlust](./pricealerts/) | Portfoliobasierte Strategie, Anlageklasse, Wertpapier | Preisalarm | Alarm bei prozentualer Bestandsveränderung oder Kursgrenzen einer gehaltenen Position | Nur Überwachung: Alarm |
| [Gewinn/Verlust in einer Periode](./pricealerts/) | Wertpapier | Preisalarm | Alarm bei Kursveränderung über einen Zeitraum | Nur Überwachung: Alarm |
| [Gleitender Durchschnitt Kreuzung](./indicatoralerts/) | Wertpapier | Indikator | Alarm bei Kreuzung eines gleitenden Durchschnitts | Nur Überwachung: Alarm |
| [RSI Schwellenwert](./indicatoralerts/) | Wertpapier | Indikator | Alarm bei RSI-Über-/Unterschreitung | Nur Überwachung: Alarm |
| [Benutzerdefinierter Ausdruck](./indicatoralerts/) | Wertpapier | Indikator | Alarm mit benutzerdefiniertem Ausdruck | Nur Überwachung: Alarm |
| [Mean-Reversion-Dip](./meanreversiondip/) | Wertpapier | Komplex | Dip-Buying mit Gewinn-/Verlustmanagement | Überwachung: Empfehlung · Simulation: Käufe und Verkäufe |

{{% notice style="info" title="Hinweis" %}}
Teilgewinnmitnahme, Aufstockung und Stop-Loss sind keine eigenen Strategietypen, sondern Abschnitte der YAML-Konfiguration des [Mean-Reversion-Dip](./meanreversiondip/).
{{% /notice %}}
