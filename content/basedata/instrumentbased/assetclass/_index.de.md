---
title: "Anlageklasse"
date: 2026-11-22T12:00:00+01:00
draft: false
weight: 5
archetype: "default"
---
Die **Selektion** einer **Anlageklasse** hat bei der Erfassung eines **Instrument** erheblichen Einfluss auf dessen Eigenschaften. Zudem hat sie Einfluss auf das Reporting und die Art der Transaktion die auf ein Instrument angewendet werden können.
+ Solbald ein **Instrument** die Anlageklasse referenziert, kann nur noch die Eigenschaft "Unter Anlageklasse" geändert werden.
+ Eine durch ein **Instrument** referenziert Anlageklasse kann nicht gelöscht werden.

## Erstellen und bearbeiten Anlageklasse
Ein **Anlageklasse** wird im **Hauptbereich** in der **Ansicht Anlageklasse** erstellt, bearbeitet und gelöscht. Diese **Ansicht** erreichen Sie im **Navigationsbereich** auf dem statischen Element "Anlageklasse".
+ **Erstellen** einer **Anlageklasse** über das **Kontextmenü**.
+ **Bearbeiten** einer **Anlageklasse** über das **Kontextmenü** bei **selektierter Anlageklasse**.
+ **Löschen** einer **Anlageklasse** über das **Kontextmenü** bei **selektierter Anlageklasse**.

### Eigenschaften und Tabellenspalten
Wie oben erwähnt sind die Eigenschaften eingeschränkt veränderbar. Die Eigenschaften und die Tabellenspalten sind übereinstimmend und bedürfen daher keiner differenzierten Beschreibung.

#### Anlageklasse
Die Eigenschaft **Anlageklasse** beeinflusst die Auswahl an **Finanzinstrument**, folgende Kombinationen sind möglich:

|Anlageklasse|Direktanlage|ETF|Investment-Fonds|Pensionsfonds|CFD|Forex|Emittentenrisiko Produkt ETC/ETN usw.|Index nicht investierbar|
|----|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|**Aktien**| {{< svg "eq.svg" svg-icon-size >}} | {{< svg "etf_i.svg" svg-icon-size >}} | {{< svg "f.svg" svg-icon-size >}} |{{< svg "f.svg" svg-icon-size >}} | {{< svg "cfd_i.svg" svg-icon-size >}} |   | {{< svg "ir.svg" svg-icon-size >}} | {{< svg "i.svg" svg-icon-size >}} |
|**Anleihen**| {{< svg "bo.svg" svg-icon-size >}} | {{< svg "etf_i.svg" svg-icon-size >}} | {{< svg "f.svg" svg-icon-size >}} | {{< svg "f.svg" svg-icon-size >}} |   |   | {{< svg "ir.svg" svg-icon-size >}} | {{< svg "i.svg" svg-icon-size >}} |
|**Commodites**| {{< svg "co.svg" svg-icon-size >}} | {{< svg "etf_c.svg" svg-icon-size >}} | {{< svg "f.svg" svg-icon-size >}} |  | {{< svg "cfd_c.svg" svg-icon-size >}} |   | {{< svg "ir.svg" svg-icon-size >}} | {{< svg "i.svg" svg-icon-size >}} |
|**Geldmarkt**| {{< svg "m.svg" svg-icon-size >}} |  {{< svg "etf_i.svg" svg-icon-size >}} | {{< svg "f.svg" svg-icon-size >}} |   |   |   |  | |
|**Immobilien**|  | {{< svg "etf_i.svg" svg-icon-size >}} | {{< svg "fr.svg" svg-icon-size >}} |   |  |  | {{< svg "ir.svg" svg-icon-size >}} | {{< svg "i.svg" svg-icon-size >}} |
|**Kreditderivate**|  | {{< svg "etf_i.svg" svg-icon-size >}}  | {{< svg "f.svg" svg-icon-size >}} |  |  |   | {{< svg "ir.svg" svg-icon-size >}} | {{< svg "i.svg" svg-icon-size >}} |
|**Multi asset**|  | {{< svg "etf_i.svg" svg-icon-size >}} | {{< svg "f.svg" svg-icon-size >}} | {{< svg "f.svg" svg-icon-size >}}  |   |   | {{< svg "ir.svg" svg-icon-size >}} | {{< svg "i.svg" svg-icon-size >}} |
|**Währungspaar**|   | {{< svg "etf_crypto.svg" svg-icon-size >}}  |   |   |   | {{< svg "fx.svg" svg-icon-size >}} | {{< svg "ir.svg" svg-icon-size >}} | |
|**Wandelanleihe**| {{< svg "cb.svg" svg-icon-size >}} | {{< svg "etf_i.svg" svg-icon-size >}} | {{< svg "f.svg" svg-icon-size >}} | {{< svg "f.svg" svg-icon-size >}} |   |   |  | {{< svg "i.svg" svg-icon-size >}} |

#### Unter Anlageklasse

GT gliedert die mitgelieferten Anlageklassen vorwiegend nach Ländern und Regionen. Die geografische Angabe steht in der Unteranlageklasse, etwa bei «Aktien USA». Sie können die Bezeichnungen auf Deutsch und Englisch frei wählen. Verwenden Sie für dieselbe Gruppe eine einheitliche Bezeichnung.

Eine spätere Umstellung auf Sektoren wäre aufwendig. Für jedes betroffene Instrument müssten Sie die Zuordnung überprüfen und bei Bedarf ändern. Zudem gehört jedes Instrument in GT zu genau einer Anlageklasse. Eine Gliederung nach Sektoren wurde in GT bisher nicht getestet.

{{% callout style="note" title="Wie lassen sich Einzelaktien und ETFs gemeinsam nach Sektoren auswerten?" %}}
Halten Sie beispielsweise für CHF 2’000 Aktien eines Technologieunternehmens und für CHF 10’000 einen ETF mit 30 % Technologieanteil, entfallen insgesamt CHF 5’000 auf Technologie. Die Einzelaktie zählt mit ihrem ganzen Wert, der ETF mit CHF 3’000. Für diese Rechnung braucht es die Sektorgewichte des ETF zum Auswertungsstichtag. Ein Beispiel für solche Angaben veröffentlicht [iShares für den Core S&P 500 ETF](https://www.ishares.com/us/products/239726/ishares-core-sp-500-etf).

Für eine künftige Sektorauswertung in GT bietet sich der *Global Industry Classification Standard* (GICS) von MSCI und S&P Dow Jones Indices an. Er ordnet Unternehmen nach ihrer Hauptgeschäftstätigkeit ein. Damit die Summe aussagekräftig ist, muss «Technologie» bei der Einzelaktie und im ETF dieselben Unternehmen umfassen. Verwendet ein Anbieter einen anderen Standard wie den *Industry Classification Benchmark* (ICB), können die Sektorgrenzen abweichen. [GICS bei MSCI](https://www.msci.com/indexes/index-resources/gics) · [ICB bei FTSE Russell](https://www.lseg.com/en/ftse-russell/industry-classification-benchmark-icb)

Eine Hürde für GT ist die Datenbeschaffung. Die GICS-Methodik ist öffentlich, die Nutzung der Unternehmenszuordnungen unterliegt Lizenzbedingungen. Eine gemeinsame Auswertung wäre möglich, wenn GT für Einzelaktien und ETFs ausreichend vollständige, aktuelle und einheitliche Daten beziehen und verwenden dürfte. Solche Daten sind kostenlos schwer zu beschaffen. [Nutzungsbedingungen von MSCI](https://www.msci.com/legal/terms-of-use)
{{% /callout %}}

#### Finanzinstrument
Wie oben dargestellt, ist die Auswahl des Finanzinstruments abhängig von der Selektion der Anlageklasse.
- **CFD** und **Forex**: Ein solches Instrument wird mit **Margin** gehandelt. Eine Position wird geöffnet bzw. geschlossen und mit dem Schlissen wird der **Gewinn/Verlust** kontorelevant. Weiter Informationen zu diesem Thema bei [Wertpapiertransaktion](../../../transaction/security)
- **Emittentenrisiko Produkt ETC/ETN usw.**: Falls für das **Instrument** keinem anderen **Finanzinstrument** zugeordnet werden kann, so wird dieses verwendet. Zu dieser Instrumenten gehören **ETC**, **ETN** und **Strukturierte Produkte (Zertifikate)** usw., das Emittentenrisiko ist ihre Gemeinsamkeit.
