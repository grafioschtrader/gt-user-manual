---
title: "HTTP-Header"
date: 2026-02-25T12:00:00+01:00
draft: false
weight: 40
archetype: "default"
---
Manche APIs erfordern benutzerdefinierte HTTP-Header für die Authentifizierung. Header werden auf der Connector-Ebene definiert und gelten für alle Endpunkte.
- **Header-Name**: Name des HTTP-Headers (z.B. `X-Api-Key`, `Authorization`).
- **Header-Wert**: Wert des Headers. Der Platzhalter `{apiKey}` wird automatisch durch den konfigurierten API-Schlüssel ersetzt.
