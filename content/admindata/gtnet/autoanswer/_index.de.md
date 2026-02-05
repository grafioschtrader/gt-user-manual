---
title: "Automatische Nachricht"
date: 2025-12-28T22:54:47+01:00
draft: false
weight: 20
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GTNet ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
GTNet ermöglicht die automatische Beantwortung von eingehenden Nachrichten basierend auf konfigurierbaren Regeln. Dadurch können häufige Anfragen ohne manuelles Eingreifen bearbeitet werden.

### Funktionsweise
Für jeden Nachrichtentyp, der eine Antwort erfordert, können mehrere Antwortregeln definiert werden. Diese Regeln werden in der Reihenfolge ihrer Priorität ausgewertet. Sobald eine Bedingung erfüllt ist, wird die entsprechende Antwort gesendet und die Auswertung beendet. Falls keine Regel zutrifft, wartet die Nachricht auf eine manuelle Bearbeitung durch den Administrator.

### Regel-Konfiguration
Jede automatische Antwortregel umfasst folgende Eigenschaften:
- **Anfrage-Nachrichtentyp**: Die Art der eingehenden Nachricht, auf die diese Regel angewendet wird.
- **Antwort-Nachrichtentyp**: Die zu sendende Antwort bei erfüllter Bedingung (z.B. Akzeptieren oder Ablehnen).
- **Priorität**: Die Auswertungsreihenfolge der Regeln. Niedrigere Werte werden zuerst geprüft.
- **Bedingung**: Eine [EvalEx](https://ezylang.github.io/EvalEx/)-Formel, die bestimmt, ob diese Regel angewendet wird. Eine leere Bedingung wird immer erfüllt.
- **Nachrichtentext**: Ein optionaler Text, der mit der Antwort gesendet wird.
- **Wartezeit in Tagen**: Eine Abkühlphase nach einer Ablehnung, bevor der Absender erneut anfragen kann.

### Verfügbare Variablen für Bedingungen
Das Bedingungsfeld unterstützt [EvalEx](https://ezylang.github.io/EvalEx/)-Ausdrücke. Ein Kontextmenü (Rechtsklick) im Bedingungseingabefeld bietet einfachen Zugang zu allen verfügbaren Variablen und Funktionen. Die Variablen sind nach Kategorien gruppiert:

#### Mein Server
Variablen über diese Serverinstanz:
| Variable | Typ | Beschreibung |
|----------|-----|--------------|
| `MyDailyRequestLimit` | NUMBER | Tägliches Anfragelimit, das wir entfernten Servern auferlegen |
| `MyDailyRequestLimitRemote` | NUMBER | Tägliches Limit, das entfernte Server für uns setzen |
| `MyTimezone` | STRING | Zeitzone dieses Servers |
| `MyMaxLimitLastPrice` | NUMBER | Max. Instrumente pro LP-Anfrage von diesem Server |
| `MyMaxLimitHistorical` | NUMBER | Max. Instrumente pro historischer Anfrage von diesem Server |

#### Entfernter Server
Variablen über den anfragenden entfernten Server:
| Variable | Typ | Beschreibung |
|----------|-----|--------------|
| `RemoteDailyRequestLimit` | NUMBER | Tägliches Anfragelimit des entfernten Servers |
| `RemoteDailyRequestLimitRemote` | NUMBER | Tägliches Limit, das andere dem entfernten Server setzen |
| `RemoteTimezone` | STRING | Zeitzone des entfernten Servers |
| `RemoteDomainRemoteName` | STRING | URL des entfernten Servers |
| `RemoteMaxLimitLastPrice` | NUMBER | Max. Instrumente pro LP-Anfrage vom entfernten Server |
| `RemoteMaxLimitHistorical` | NUMBER | Max. Instrumente pro historischer Anfrage vom entfernten Server |

#### Nachricht
| Variable | Typ | Beschreibung |
|----------|-----|--------------|
| `Message` | STRING | Freitext-Nachrichteninhalt aus der Anfrage |

#### Verbindungen
| Variable | Typ | Beschreibung |
|----------|-----|--------------|
| `TotalConnections` | NUMBER | Gesamtzahl der aktiven GTNet-Verbindungen |
| `ConnectionsLastPrice` | NUMBER | Anzahl der Verbindungen für LP-Daten |
| `ConnectionsHistorical` | NUMBER | Anzahl der Verbindungen für historische Daten |

#### Berechnet
| Variable | Typ | Beschreibung |
|----------|-----|--------------|
| `TimezoneOffsetHours` | NUMBER | Stundendifferenz zwischen entfernter und lokaler Zeitzone |
| `hour` | NUMBER | Aktuelle Stunde (0-23) in UTC |
| `dayOfWeek` | NUMBER | Wochentag (1=Montag, 7=Sonntag) |
| `dailyCount` | NUMBER | Anzahl der heute empfangenen Anfragen von diesem entfernten Server |
| `dailyLimit` | NUMBER | Tägliches Anfragelimit für diesen entfernten Server |

### Zeichenkettenfunktionen
Für Bedingungen mit Zeichenkettenvergleichen stehen EvalEx-Zeichenkettenfunktionen zur Verfügung. Diese können über das Kontextmenü unter "Zeichenkettenfunktionen" eingefügt werden:

| Funktion | Beschreibung |
|----------|--------------|
| `STR_STARTS_WITH(string, prefix)` | Gibt true zurück, wenn die Zeichenkette mit dem Präfix beginnt (Gross-/Kleinschreibung beachtet) |
| `STR_ENDS_WITH(string, suffix)` | Gibt true zurück, wenn die Zeichenkette mit dem Suffix endet (Gross-/Kleinschreibung beachtet) |
| `STR_CONTAINS(string, substring)` | Gibt true zurück, wenn die Zeichenkette die Teilzeichenkette enthält (ohne Gross-/Kleinschreibung) |
| `STR_MATCHES(string, pattern)` | Gibt true zurück, wenn die Zeichenkette dem RegEx-Muster entspricht |
| `STR_LENGTH(string)` | Gibt die Länge der Zeichenkette zurück |
| `STR_LOWER(string)` | Wandelt die Zeichenkette in Kleinbuchstaben um |
| `STR_UPPER(string)` | Wandelt die Zeichenkette in Grossbuchstaben um |
| `STR_TRIM(string)` | Entfernt führende und nachfolgende Leerzeichen |
| `STR_LEFT(string, n)` | Gibt die ersten n Zeichen zurück |
| `STR_RIGHT(string, n)` | Gibt die letzten n Zeichen zurück |
| `STR_SUBSTRING(string, start, end)` | Extrahiert eine Teilzeichenkette von der Start- bis zur Endposition |

### Kontextabhängige Eingabehilfe
Bei der Eingabe von Bedingungen filtert das Kontextmenü automatisch die verfügbaren Variablen basierend auf der Cursorposition:
- Innerhalb eines Funktionsparameters, der einen STRING erwartet, werden nur STRING-Variablen angezeigt
- Innerhalb eines Funktionsparameters, der eine NUMBER erwartet, werden nur NUMBER-Variablen angezeigt
- Ausserhalb jeder Funktion sind alle Variablen und Funktionen verfügbar

### Beispiel-Bedingungen

**Anfragen nur während der Geschäftszeiten akzeptieren (UTC):**
```
hour >= 8 && hour <= 18 && dayOfWeek >= 1 && dayOfWeek <= 5
```

**Ablehnen, wenn das Tageslimit überschritten ist:**
```
dailyCount >= dailyLimit
```

**Nur von vertrauenswürdigen Domains akzeptieren:**
```
STR_STARTS_WITH(RemoteDomainRemoteName, "https://trusted-partner")
```

**Nachrichteninhalt auf bestimmte Schlüsselwörter prüfen:**
```
STR_CONTAINS(Message, "dringend") || STR_CONTAINS(Message, "priorität")
```

**Kombinierte Bedingung - von bekannten Domains während der Geschäftszeiten akzeptieren:**
```
STR_STARTS_WITH(RemoteDomainRemoteName, "https://partner") && hour >= 9 && hour <= 17
```

**Basierend auf Zeitzonenkompatibilität akzeptieren:**
```
TimezoneOffsetHours >= -3 && TimezoneOffsetHours <= 3
```

### Wartezeit nach Ablehnung
Um wiederholte Anfragen von abgelehnten Servern zu vermeiden, kann eine Wartezeit in Tagen festgelegt werden. Während dieser Zeit werden erneute Anfragen desselben Typs von derselben Instanz automatisch ignoriert oder verzögert. Dies gibt dem Administrator Zeit für eine manuelle Überprüfung hartnäckiger Anfragen.
