---
title: "Generic Connector"
date: 2026-02-26T12:00:00+01:00
draft: false
weight: 60
archetype: "default"
---
The **Generic Connector** allows you to configure price data connectors entirely through the user interface, without writing any Java code. This lets users create connectors for any data provider that delivers historical or intraday prices via HTTP APIs.
Free data APIs frequently change their URL schemes, response formats, or field names. With the generic connector, users can adapt to such changes on their own, without waiting for a software update.
## Data model
A generic connector consists of three levels:
{{< mermaid >}}
erDiagram
    "Connector definition" ||--o{ Endpoint : has
    "Connector definition" ||--o{ "HTTP header" : has
    Endpoint ||--o{ "Field mapping" : has
{{< /mermaid >}}
+ The **connector definition** describes the data provider: base URL, display name, API key requirements, and rate limit settings.
+ An **endpoint** defines how a specific type of price data is fetched — e.g. historical quotes for securities or intraday quotes for currency pairs. Each endpoint contains the URL template, the response format, and the parsing configuration.
+ A **field mapping** maps a field from the API response to an internal GT field (e.g. date, open price, close price, volume).
+ **HTTP headers** allow sending additional headers with API calls (e.g. `X-Api-Key`).
## Management
The generic connector is managed under **Base data** in the navigation area. The view is structured as follows:
+ **Dropdown selector** at the top to choose a connector.
+ **Detail display** below the dropdown, organized into three fieldsets:
  - **Connector settings** — basic identification and capabilities.
  - **Rate limit settings** — request throttling configuration.
  - **Advanced settings** — includes miscellaneous options and, when configured, a read-only display of the **Token configuration (YAML)**.
+ **HTTP headers table** — an editable table for managing custom HTTP headers.
+ **Endpoints accordion** — each endpoint appears as an accordion panel. Within each panel:
  - Endpoint detail fieldsets (settings, format-specific options, ticker, pagination).
  - An embedded **field mapping table** for managing the endpoint's field mappings.
### Creating and editing connectors
The following actions are available via the context menu (right-click):
+ **Create Generic Connector...**: Create a new connector.
+ **Edit Generic Connector...**: Edit the selected connector.
+ **Delete Generic Connector**: Delete the selected connector. This option is disabled when securities or currency pairs still reference the connector as their price data source.
+ **Activate...**: Activate the connector so it becomes available as a price data connector (administrator only).
+ **Reload connectors...**: Reload all generic connectors from the database and re-register them (administrator only).
+ **Create Endpoint...**: Create a new endpoint for the selected connector.
+ **Edit Endpoint...**: Edit the selected endpoint (when an endpoint accordion panel is selected).
+ **Delete Endpoint**: Delete the selected endpoint (when an endpoint accordion panel is selected).
{{% notice style="info" title="Activation by administrator" %}}
A newly created connector is initially inactive. Only after review and **activation** by an administrator will it be registered as a price data connector and become available in the connector selection for securities and currency pairs.
{{% /notice %}}
## User rights
The generic connector uses a usage-based permission model that restricts editing rights once a connector is in productive use:
+ The **administrator** can edit and delete all connector components at any time.
+ The **creator** (the user who created the connector) can freely edit the connector, its HTTP headers, endpoints, and field mappings **as long as no endpoint has been successfully used**.
+ Once an endpoint has successfully delivered price data for the first time, it becomes locked for the creator — endpoint settings and field mappings for that endpoint can then only be changed by the administrator.
+ Once **any** endpoint has been successfully used, the connector definition and HTTP headers also become locked for the creator.
+ Locked menu items (Edit, Delete) are greyed out in the context menu. However, the creator can still create and edit a new endpoint that has not yet been used.
+ Once all endpoints have been successfully used, the connector transfers to system ownership and can only be managed by the administrator.
{{% notice style="note" title="Why does locking happen?" %}}
When an endpoint has successfully delivered price data, securities or currency pairs may already be linked to this connector. Changes to the endpoint or the connector definition could disrupt the price data supply for these instruments. The lock prevents productive connections from being accidentally broken.
{{% /notice %}}
{{% notice style="warning" title="Deletion protection" %}}
A connector cannot be deleted as long as securities or currency pairs use it as their price data connector. The **Delete** menu item is greyed out when the connector is still referenced by instruments. To delete such a connector, first reassign all affected instruments to a different connector.
{{% /notice %}}
## Procedure for creating a generic connector
The following flowchart and step-by-step description show the typical workflow for creating a generic connector by a user without administrator rights.
{{< mermaid >}}
flowchart TD
    subgraph outside["Preparation outside GT"]
        S1["1. Check website as data provider"]
        S2["2. Identify possible API call"]
        S3["3. Test API call with API client"]
    end
    subgraph inside["Inside GT"]
        S4["4. Create generic connector with endpoints and field mappings"]
        S5["5. Test the endpoints"]
        S6["6. Send message to admin for activation"]
        S7["7. Create security or currency pair and assign connector"]
        S8["8. Check batch processing monitor"]
        S9{"9. Price data correct?"}
        S10["10. Notify admin about problems"]
        S11["11. Make further corrections"]
    end
    S1 --> S2 --> S3 --> S4 --> S5 --> S6 --> S7 --> S8 --> S9
    S9 -->|Yes| E(("Done"))
    S9 -->|No| S10 --> S11 --> S6
{{< /mermaid >}}

1. **Check website as data provider** — First, check whether the website can be used as a data provider. An LLM can be very helpful in finding possible Ajax requests. For historical data, care should be taken to ensure that it is available over a long period of time and as OHLC data. The chart is often the only and frequently the better alternative to the historical price data offered.
2. **Identify possible API call** — Find out the possible API call that can be used to retrieve the desired data.
3. **Test API call with API client** — Test this API call with an API client. This will help you identify any obstacles that need to be overcome. It may not work for authentication reasons, or the site may be protected in some other way. The HTTP header is very important here.
4. **Create generic connector** — Create the generic connector with [connector definition](connectordefinition/), [endpoints](endpoints/), and [field mappings](fieldmappings/).
5. **Test the endpoints** — Test the endpoints using the [testing and troubleshooting](troubleshooting/) feature.
6. **Send message to admin** — Send a message to the admin role so that the working connector can be activated.
7. **Create security or currency pair and assign** — After activation, create a security or currency pair and connect it to this generic connector.
8. **Check batch processing monitor** — Check the [batch processing monitor]({{< relref "/admindata/taskdatachangemonitor" >}}) to see if the loading was successful or if an error message was thrown.
9. **Check price data** — View the price data of the security or currency pair as a table and chart.
10. **Notify admin about problems** — If the data is not correct, notify the admin role. They can then fix the problem or deactivate or delete the generic connector.
11. **Make further corrections** — If the connector has not yet been deleted and there is hope of getting it to work, make further corrections and continue with step 6.

## Connector definition
The fields of the connector definition are described on a separate page: [Connector definition](connectordefinition/).
## Example: JSON API connector
The following example shows the configuration for a fictitious JSON-based data provider:
**Connector definition:**
+ Short ID: `mydata`
+ Domain URL: `https://api.mydata.com/`
+ Rate limit type: Token bucket, 5 requests per 60 seconds
**Endpoint (Historical, Security):**
+ URL template: `v1/eod/{ticker}?from={fromDate}&to={toDate}&token={apiKey}`
+ Response format: JSON
+ Data structure: Array of objects
+ Data path: `results`
+ Date format type: ISO_DATE
**Field mappings:**
- **`date`**: `t`
- **`open`**: `o`
- **`high`**: `h`
- **`low`**: `l`
- **`close`**: `c`
- **`volume`**: `v`
