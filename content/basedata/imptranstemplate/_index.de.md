---
title: "Import Vorlagengruppe"
date: 2025-04-22T12:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
Die meisten Handelsplattformen bieten eine Exportfunktion von Transaktionen. Das Resultat dieses Exports kann möglicherweise in GT importiert werden. Da jede Handelsplattform ihre eigenes Design der **Dokumente** implementiert, muss in GT jeweils entsprechend dieser Dokumente eine oder mehrere **Importvorlage** erstellt werden. GT kann die zwei Dokumenttypen **PDF** und **CSV** verarbeiten.

{{% notice note %}}
Das Ziel von GT mit diesen Importvorlagen ist eine möglichst hohe Generalisierung des Transaktionsimports. Damit werden viele spezifische Implementierung pro Handelsplattform vermieden. Zudem führt eine Dokumentänderung der Handelsplattform nur in wenigen Fällen zu einem Update von GT, was dem Benutzer und Softwareentwickler sehr dienlich ist.
{{% /notice %}}

## Zweck der Import Vorlagengruppe
Für eine Handelsplattform kann es eine oder mehrere Importvorlagen geben, daher werden diese in einer **Vorlagengruppe** gehalten.

## Eigenschaften Vorlagengruppe
- **Name Vorlagengruppe**: Name der Vorlagengruppe.
- **Transaktion Implementierung**: In einigen Fällen benötigt die Verarbeitung der **PDF/CSV Dokumente** eine **spezielle Transaktionsimport Implementierung** im **Back-End**. Meistens genügt die allgemeine in GT implementierte Importfunktion mit den entsprechenden Importvorlagen für die Verarbeitung der Transaktionsdokumente einer Handelsplattform. In allen anderen Fällen muss in GT eine Importfunktion implementiert werden.

## Erstellen und bearbeiten Vorlagengruppe
Eine **Import Vorlagengruppe** wird im Hauptbereich in der Ansicht "**Import Vorlagengruppe**" erstellt, bearbeitet und gelöscht.
- **Löschen Import Vorlagengruppe**: Dieser Menüpunkt wird erst aktiv, wenn keine Importvorlagen dieser Vorlagengruppe existiert.

## Funktionen auf markierte Ansicht Import Vorlagengruppe
Es gibt zusätzliche Funktionalität nebst dem Bearbeiten der Vorlagengruppe. Diese Funktionen sind unterstützend beim Erstellen von Importvorlagen.
{{< youtube DXVgkHk1qc4 >}}

### Export und Import von Importvorlagen
Die **Importvorlagen** können exportiert bzw. importiert werden. Für den Austausch der Importvorlagen ist das Projekt [gt-import-transaction-template](//github.com/grafioschtrader/gt-import-transaction-template) auf **GitHub** gedacht. Dort finden Sie möglicherweise die Importvorlagen für Ihre Handelsplattform. Falls Sie Ihre selbst erstellten Importvorlagen der Öffentlichkeit zur Verfügung stellen wollen, können Sie diese exportieren und in dieses Projekt einbringen.

{{% notice style="info" title="Dateiname und Einzigartigkeit" %}}
Die Eigenschaften Kategorie, Sprache, Vorlage, Dokumentart und Gültig ab werden für den Export und Import von Importvorlagen verwendet. Die Kombination dieser vier Eigenschaften ist einzigartig. Ausserdem wird der Dateiname einer Importvorlage aus diesen Eigenschaften generiert.
{{% /notice %}}

### Transformiere PDF nach Text
Diese Funktion erlaubt die Transformation eines PDF-Dokuments nach Text, damit dies für weitere Funktionen benutzt werden kann.
{{% notice warning %}}
Die Transformation des PDF Dokumentes nach Text erfolgt im **Back-End**. Falls Sie Bedenken bezüglich des Datenschutz Ihrer genutzten GT-Instanz haben, sollten Sie diese Funktion möglicherweise nicht anwenden.
{{% /notice %}}

### Erstellen und bearbeiten Importvorlage
- Erstellen einer **Importvorlage** über das **Kontextmenü**.
- Bearbeiten einer **Importvorlage** über das **Kontextmenü** bei **selektierter Importvorlage**.
- Löschen einer **Importvorlage** über das **Kontextmenü** bei **selektierter Importvorlage**.

### Prüfe Importvorlagen mit PDF als Text
Mit dieser Funktion kann einer mit der Funktion "**Transformiere PDF nach Text**" erstellter Text auf korrekte Erkennung geprüft werden. Dabei gibt es zwei grundsätzlich verschiedene Resultate:
- **Dokument erkannt**: Der Text bzw. das Dokument wurde erkannt, d.h. es konnte eine Importvorlage gefunden werden, dessen obligatorischen Felder mit einem Feldwert besetzt wurden. 
- **Dokument nicht erkannt**: Es gibt eine tabellarische Ansicht mit allen **Importvorlagen** der **Vorlagengruppe**. Dabei erfolgt die Anzeige mit dem letzten erfolgreich erkannten **Feld** pro **Importvorlage**.

## Eigenschaften und Tabellenspalten
Die Eigenschaft "PDF/CSV Vorlage" ist mit Abstand das komplizierteste Eingabefeld in GT und wird im Kapitel [Import Transaktion Vorlage](./createimptranstemplate/) ausführlich beschrieben. Die Eigenschaft **Zweck der Vorlage** ist ein freier Text der den Zweck der Vorlage für den **Benutzer** beschreibt. Die anderen vier Eigenschaften müssen einzigartig pro **Vorlagengruppe** sein, da sie den  Schlüssel beim **Import** einer **Importvorlage** bilden. Bei deckungsgleichen Eigenschaftswerten wird die bestehende Vorlage überschrieben andernfalls entsteht eine neue Importvorlage.
- **Kategorie**: Die Kategorie beschreibt den Zweck der Importvorlage am besten für das **System**.
- **Dokumentart**: Diese Angabe definiert, ob es sich um eine Vorlage für CSV- oder PDF-Dokumente handelt.
- **Gültig ab**: Dies ist ein Hinweis für den Benutzer, damit er die Vorlage der sich ändernden Dokumentendesign der gleichen Art der Transaktion zu ordnen kann.
- **Sprache Vorlage:** Es ist die Sprache der Vorlage. Die Importvorlagen arbeitet mit Ankerpunkten und diese fixieren sich oftmals an **statischen Texten** des **Dokumentes**.

## Vorgehen beim Erstellen einer PDF-Importvorlage
Falls schon **Importvorlagen** in der **Vorlagengruppe** vorhanden sind, sollte zuvor geprüft werden, ob nicht eine bestehende Importvorlage generalisiert werden kann. Oftmals sind PDF Dokumente von Kauf und Verkauf der Instrumente sehr ähnlich und können mit einer einzelnen **Importvorlage** abgehandelt werden. Eine Importvorlage kann interaktiv wie folgt erstellt werden:
1. Zweifache Anmeldung an GT. Die einte (**S1**) Sitzung für die Erstellung der Importvorlage und die zweite (**S2**) für die Überprüfung dieser. Danach in beiden Sitzungen die Ansicht **Import Vorlagengruppe** ansteuern.
2. **S1/S2**: Die entsprechende Vorlagengruppe auswählen oder in einer der Sitzungen die Vorlagengruppe erstellen. In der jeweils anderen Sitzung wird mit einem erneuten ansteuern der **Import Vorlagengruppe** das Laden der eben erstellen Vorlagengruppe erzwungen.
3. **S1**: Das zu verarbeitende **PDF-Dokument** mit der Funktion "**Transformiere PDF nach Text**" in Text umwandeln.
4. **S2**: Die Funktion "**Prüfe Vorlagen mit PDF als Text**" auslösen und das als Text vorliegende Dokument in das Eingabefeld "**PDF Form als Text**" kopieren.
   - Falls Sie die Importvorlage veröffentlichen wollen, sollten zusätzlich bestimmte Textausschnitte im transformierten Text anonymisiert werden. Dazu gehören sicherlich die Anschrift und irgendwelche Nummern oder Text die Ihr Konto und/oder Depot referenzieren.
5. **S1**: Funktion "**Erstellen Importvorlage**" auslösen und das als Text vorliegende Dokument in das Eingabefeld "**PDF/CSV Vorlage**" des Dialoges kopieren.
6. **S1**: Die Eingabefelder **Kategorie** usw. wie oben beschrieben ausfüllen.
7. **S1**: Im Text des Eingabefeldes **PDF/CSV Vorlage** die auszulesenden Werte mit den entsprechenden Felder ersetzen, siehe dazu [Import Transaktion Vorlage](./createimptranstemplate/).
8. **S1**: Importvorlage speichern.
9. **S2**: Schaltfläche "**Prüfe Vorlagen mit PDF als Text**" betätigen. Gemäss dem Resultat muss in S1 die Importvorlage erneut bearbeitet werden, dazu die **Schritte 7-9** wiederholen.
{{< youtube YB8ZcY35tY4 >}}

### Die Importvorlage veröffentlichen
Sie können GT helfen, indem Sie Ihre erstellten Importvorlage über das Projekt GitHub [gt-import-transaction-template](//github.com/grafioschtrader/gt-import-transaction-template) veröffentlichen. Beispielsweise folgen Sie den Instruktionen von ["Send us your PDF or CSV documents with or without import templates"](//github.com/grafioschtrader/gt-import-transaction-template).
{{< youtube MhRXGeNBe1A >}}
