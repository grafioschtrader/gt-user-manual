---
title: "Field mappings"
date: 2026-02-25T12:00:00+01:00
draft: false
weight: 30
archetype: "default"
---
Each endpoint requires **field mappings** that specify which field in the API response corresponds to which internal GT field. The available target fields depend on the feed type.
## Target fields for historical endpoints
- **`date`**: Trading date
- **`open`**: Opening price
- **`high`**: Daily high price
- **`low`**: Daily low price
- **`close`**: Closing price
- **`volume`**: Trading volume
## Target fields for intraday endpoints
- **`last`**: Last traded price
- **`open`**: Opening price
- **`high`**: Daily high price
- **`low`**: Daily low price
- **`volume`**: Trading volume
- **`prevClose`**: Previous day's closing price
- **`changePercentage`**: Change in percent
- **`timestamp`**: Timestamp of the last quote
## Field mapping fields
- **Target field**: The internal GT field (see lists above).
- **Source expression**: Where to find the value in the API response. The meaning depends on the response format: for JSON, the property name or dot-path; for HTML (regex groups) the capture group number; for HTML (multi selector) a CSS selector.
- **CSV column index**: 0-based column index (CSV format only).
- **Divider expression**: Optional divisor for unit conversion (e.g. `100` if the provider delivers prices in sub-currency units).
- **Required**: Whether this field must be present for a data record to be accepted as valid.
