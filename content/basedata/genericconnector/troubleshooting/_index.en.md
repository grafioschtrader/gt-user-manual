---
title: "Testing and troubleshooting"
date: 2026-02-26T12:00:00+01:00
draft: false
weight: 60
archetype: "default"
---
This section covers how to test endpoint configurations and how to diagnose common issues when using generic connectors.

## Testing an endpoint
After saving an endpoint configuration you can verify it without assigning the connector to a real security or currency pair. The test sends a single HTTP request using the saved endpoint settings and displays the result directly in a dialog.

### Two-session workflow
When you edit a connector definition and its endpoints in one GT session, the changes are saved to the database. To test these changes you need a **second GT session** (or reload the page), because the test loads the connector configuration from the database rather than from the in-memory editor state.

### Opening the test dialog
1. Expand the endpoint you want to test in the accordion.
2. Right-click the endpoint row and choose **Test endpoint...** from the context menu.

### Input fields
The dialog shows different input fields depending on the endpoint configuration:
- **Security or `URL_EXTEND` strategy**: **Ticker** (e.g. `AAPL`, `MSFT`)
- **Currency with `CURRENCY_PAIR` strategy**: **From currency** and **To currency** (e.g. `EUR`, `USD`)
- **Historical endpoint (`FS_HISTORY`)**: Additionally **Date from** and **Date to**

### Result display
After clicking **Test**, the dialog shows three sections:

- **Test result** fieldset -- success or failure indicator, HTTP status code, the request URL (with any API key masked), and execution time in milliseconds. If the request failed, an error message is shown.
- **Parsed data** table -- the data rows parsed from the provider response (at most 200 rows). Column headers are derived from the field mappings of the endpoint.
- **Raw response** (collapsible) -- the first 5000 characters of the raw HTTP response body, useful for inspecting the provider format when field mappings do not parse correctly.

### Key behavior
- The dialog **stays open** after each test so you can adjust parameters and re-test without reopening it.
- Only a **single request** is sent (no pagination). If the endpoint uses pagination, only the first page is fetched.
- **Endpoint options** such as `SKIP_WEEKEND_DATA` or `REMOVE_DUPLICATE_DATES` are **not applied** during testing. The raw parsed output is shown exactly as returned by the provider.

## Historical price data is missing after creating a security
You have created a new security with a generic connector assigned, but no historical price data appears.

**First step:** Open the [batch processing monitor]({{< relref "/admindata/taskdatachangemonitor" >}}) and check whether the historical data load completed successfully or produced an error message. The error message usually points to the cause. Below are the most common errors and their solutions.

### Duplicate date entries
**Error message:** `DataIntegrityViolationException` with `Duplicate entry for key 'IHistoryQuote_id_Date'`.

**Cause:** The data provider returns overlapping data across pagination batches or delivers duplicate rows for the same date.

**Solution:** Enable the `REMOVE_DUPLICATE_DATES` [endpoint option](../endpoints/#endpoint-options) on the affected historical endpoint. This ensures that only the first entry per date is kept, preventing duplicate key violations during import.

### Weekend data in historical prices
**Error message:** No explicit error, but unexpected gaps appear in the data or quality warnings are raised for the instrument.

**Cause:** The data provider delivers rows for Saturday and/or Sunday, which are not valid trading days for most exchanges. These weekend rows can interfere with GT's data quality checks.

**Solution:** Enable the `SKIP_WEEKEND_DATA` [endpoint option](../endpoints/#endpoint-options) on the affected historical endpoint. This filters out any data points that fall on Saturday or Sunday before they are stored.
