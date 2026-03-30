---
title: "Token configuration (YAML)"
date: 2026-02-25T12:00:00+01:00
draft: false
weight: 55
archetype: "default"
---
Some data providers use SAML/SSO or similar authentication flows that require acquiring a short-lived JWT token before data can be fetched. Instead of manually managing API keys, the **Token configuration** allows the connector to automatically perform a multi-step authentication flow and refresh the token as needed.
{{% notice style="info" title="API key vs. auto-token" %}}
A connector with a token configuration **does not need a static API key** in the connector API key table. The connector acquires and refreshes its own token automatically. If the YAML field is left empty, the connector falls back to classic API key mode.
{{% /notice %}}
## Authentication flow
The automatic token acquisition follows a three-step flow:
{{< mermaid >}}
sequenceDiagram
    participant GT as Grafioschtrader
    participant Seed as Seed URL
    participant Login as Login URL
    participant Refresh as Refresh URL
    GT->>Seed: 1. GET seed page
    Seed-->>GT: HTML with embedded value (e.g. SAML ticket)
    GT->>GT: Extract value via regex
    GT->>Login: 2. POST seed value
    Login-->>GT: JWT token + optional session ID
    Note over GT: Token valid for ttlSeconds
    GT->>Refresh: 3. POST with session ID (optional)
    Refresh-->>GT: Fresh JWT token
{{< /mermaid >}}
1. **Seed**: A GET request fetches an HTML page and extracts a value (e.g. a SAML ticket) using a regular expression.
2. **Login**: The extracted value is POSTed to obtain a JWT token and an optional session ID.
3. **Refresh** (optional): When the token expires, a POST with the cached session ID obtains a fresh token without repeating the seed step. If refresh is not configured or fails, the full seed + login flow is repeated.
## YAML field reference
### Top-level fields
- **`seed`** (Required): Seed step configuration (see below).
- **`login`** (Required): Login step configuration (see below).
- **`refresh`** (Optional): Refresh step configuration (see below).
- **`ttlSeconds`** (Required): Token lifetime in seconds. Use a value slightly less than the actual token expiry to avoid race conditions. Minimum: 10.
### Seed step
- **`url`** (Required): URL to GET for the seed HTML page.
- **`regex`** (Required): Regular expression with a capture group `(1)` to extract a value from the HTML response (e.g. a SAML ticket).
### Login step
- **`url`** (Required): Full URL to POST for the initial login.
- **`body`** (Required): POST body template. The placeholder `{seedValue}` is replaced with the extracted seed value at runtime.
- **`contentType`** (Optional): Content-Type for the POST request. Default: `application/json`. Set to `application/x-www-form-urlencoded` for form-encoded SAML flows.
- **`base64EncodeSeed`** (Optional): When `true`, the seed value is Base64-encoded before template substitution. Required for SAML flows where the seed is raw XML. Default: `false`.
- **`jwtPath`** (Required): JSON dot-path to the JWT token in the login response (e.g. `data.token`).
- **`sessionPath`** (Optional): JSON dot-path to the session ID in the login response. Required if the refresh step is configured.
### Refresh step
- **`url`** (Required): URL to POST for token refresh.
- **`sidHeader`** (Required): HTTP header name used to send the cached session ID in the refresh request.
## YAML example
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
## Behavior notes
+ The YAML editor in the connector edit dialog validates against a JSON schema and provides autocomplete.
+ When the token expires (after `ttlSeconds`), the connector first attempts the **refresh** step. If refresh fails or is not configured, the full **seed + login** flow is repeated.
+ If a data request receives an HTTP 401 response, the connector automatically re-acquires the token and retries the request once.
+ Token acquisition is **thread-safe** — concurrent data requests share the same token and only one thread performs the renewal.
+ Leaving the YAML field **empty** means the connector uses classic API key mode.
