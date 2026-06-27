---
title: "HTTP headers"
date: 2026-02-25T12:00:00+01:00
draft: false
weight: 40
archetype: "default"
---
Some APIs require custom HTTP headers for authentication. Headers are defined at the connector level and apply to all endpoints.
- **Header name**: Name of the HTTP header (e.g. `X-Api-Key`, `Authorization`).
- **Header value**: Value of the header. The placeholder `{apiKey}` is automatically replaced with the configured API key.
