---
title: "Token-Konfiguration (YAML)"
date: 2026-02-25T12:00:00+01:00
draft: false
weight: 55
archetype: "default"
---
Manche Datenanbieter verwenden SAML/SSO oder ähnliche Authentifizierungs-Flows, die das Beschaffen eines kurzlebigen JWT-Tokens erfordern, bevor Daten abgerufen werden können. Anstatt API-Schlüssel manuell zu verwalten, ermöglicht die **Token-Konfiguration** dem Connector, automatisch einen mehrstufigen Authentifizierungs-Flow durchzuführen und den Token bei Bedarf zu erneuern.
{{% notice style="info" title="API-Schlüssel vs. Auto-Token" %}}
Ein Connector mit Token-Konfiguration **benötigt keinen statischen API-Schlüssel** in der Konnektor-API-Schlüssel-Tabelle. Der Connector beschafft und erneuert seinen Token selbständig. Wenn das YAML-Feld leer bleibt, fällt der Connector auf den klassischen API-Schlüssel-Modus zurück.
{{% /notice %}}
## Authentifizierungs-Flow
Die automatische Token-Beschaffung folgt einem dreistufigen Ablauf:
{{< mermaid >}}
sequenceDiagram
    participant GT as Grafioschtrader
    participant Seed as Seed-URL
    participant Login as Login-URL
    participant Refresh as Refresh-URL
    GT->>Seed: 1. GET Seed-Seite
    Seed-->>GT: HTML mit eingebettetem Wert (z.B. SAML-Ticket)
    GT->>GT: Wert per Regex extrahieren
    GT->>Login: 2. POST Seed-Wert
    Login-->>GT: JWT-Token + optionale Session-ID
    Note over GT: Token gültig für ttlSeconds
    GT->>Refresh: 3. POST mit Session-ID (optional)
    Refresh-->>GT: Neuer JWT-Token
{{< /mermaid >}}
1. **Seed**: Eine GET-Anfrage ruft eine HTML-Seite ab und extrahiert einen Wert (z.B. ein SAML-Ticket) mittels regulärem Ausdruck.
2. **Login**: Der extrahierte Wert wird per POST gesendet, um einen JWT-Token und eine optionale Session-ID zu erhalten.
3. **Refresh** (optional): Wenn der Token abläuft, wird per POST mit der gecachten Session-ID ein neuer Token beschafft, ohne den Seed-Schritt zu wiederholen. Falls Refresh nicht konfiguriert ist oder fehlschlägt, wird der vollständige Seed + Login-Flow wiederholt.
## YAML-Feldreferenz
### Felder auf oberster Ebene
- **`seed`** (Pflicht): Konfiguration des Seed-Schritts (siehe unten).
- **`login`** (Pflicht): Konfiguration des Login-Schritts (siehe unten).
- **`refresh`** (Optional): Konfiguration des Refresh-Schritts (siehe unten).
- **`ttlSeconds`** (Pflicht): Token-Lebensdauer in Sekunden. Verwenden Sie einen Wert etwas unter der tatsächlichen Token-Laufzeit, um Race-Conditions zu vermeiden. Minimum: 10.
### Seed-Schritt
- **`url`** (Pflicht): URL für die GET-Anfrage der Seed-HTML-Seite.
- **`regex`** (Pflicht): Regulärer Ausdruck mit Capture-Gruppe `(1)` zum Extrahieren eines Werts aus der HTML-Antwort (z.B. ein SAML-Ticket).
### Login-Schritt
- **`url`** (Pflicht): Vollständige URL für die POST-Anfrage beim Login.
- **`body`** (Pflicht): POST-Body-Vorlage. Der Platzhalter `{seedValue}` wird zur Laufzeit durch den extrahierten Seed-Wert ersetzt.
- **`contentType`** (Optional): Content-Type für die POST-Anfrage. Standard: `application/json`. Setzen Sie `application/x-www-form-urlencoded` für formcodierte SAML-Flows.
- **`base64EncodeSeed`** (Optional): Wenn `true`, wird der Seed-Wert vor der Template-Ersetzung Base64-kodiert. Erforderlich für SAML-Flows, bei denen der Seed rohes XML ist. Standard: `false`.
- **`jwtPath`** (Pflicht): JSON-Punkt-Pfad zum JWT-Token in der Login-Antwort (z.B. `data.token`).
- **`sessionPath`** (Optional): JSON-Punkt-Pfad zur Session-ID in der Login-Antwort. Erforderlich, wenn der Refresh-Schritt konfiguriert ist.
### Refresh-Schritt
- **`url`** (Pflicht): URL für die POST-Anfrage zur Token-Erneuerung.
- **`sidHeader`** (Pflicht): Name des HTTP-Headers, über den die gecachte Session-ID bei der Refresh-Anfrage gesendet wird.
## YAML-Beispiel
```yaml
seed:
  url: https://provider.example.com/auth/saml
  regex: 'name="ticket" value="([^"]+)"'
login:
  url: https://api.provider.example.com/auth/login
  body: '{"ticket": "{seedValue}"}'
  contentType: application/json
  base64EncodeSeed: false
  jwtPath: data.accessToken
  sessionPath: data.sessionId
refresh:
  url: https://api.provider.example.com/auth/refresh
  sidHeader: X-Session-Id
ttlSeconds: 270
```
## Verhaltenshinweise
+ Der YAML-Editor im Connector-Bearbeitungsdialog validiert anhand eines JSON-Schemas und bietet Autovervollständigung.
+ Wenn der Token abläuft (nach `ttlSeconds`), versucht der Connector zuerst den **Refresh**-Schritt. Falls Refresh fehlschlägt oder nicht konfiguriert ist, wird der vollständige **Seed + Login**-Flow wiederholt.
+ Erhält eine Datenanfrage eine HTTP-401-Antwort, beschafft der Connector automatisch einen neuen Token und wiederholt die Anfrage einmal.
+ Die Token-Beschaffung ist **thread-sicher** — gleichzeitige Datenanfragen teilen sich denselben Token und nur ein Thread führt die Erneuerung durch.
+ Wenn das YAML-Feld **leer** bleibt, verwendet der Connector den klassischen API-Schlüssel-Modus.
