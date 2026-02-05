---
title: "GTNet Security Import"
date: 2026-02-04T12:00:00+01:00
draft: false
weight: 55
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
The **GTNet Security Import** enables efficient creation of multiple securities via the GTNet peer network. Instead of manually configuring each security with metadata, data connectors, and asset classes, users can retrieve and automatically adopt information from other GTNet instances.
## Workflow
The feature follows a master-detail pattern in three steps:
1. **Create an Import Set**: Create a named set for the securities to be imported.
2. **Add Import Positions**: Specify ISIN, ticker, and currency for each security.
3. **Start GTNet Lookup**: The positions are queried in the background from accessible GTNet peers, and securities are automatically created or linked.
## Managing Import Sets
An **Import Set** serves as a container for related securities. A new import set can be created, edited, or deleted via the context menu.
- **Name** (required): A descriptive name for the import set, e.g., "Portfolio Migration 2024".
- **Note** (optional): Additional notes about the import set.
## Position Table
After selecting an import set, the associated positions are displayed in a table.
**Editable columns:**
- **ISIN**: The International Securities Identification Number (max. 12 characters).
- **Ticker/symbol**: The stock exchange symbol (max. 6 characters).
- **Currency**: The ISO 4217 currency code (e.g., USD, EUR, CHF).
**Read-only columns (after linking):**
- **Linked Security**: The name of the found or created security.
- **Asset Class**: The category of the security.
- **Financial Instrument**: The instrument type.
- **Sub-asset class**: The subcategory.
{{% notice style="info" title="Validation" %}}
At least ISIN or ticker must be provided. Currency is always required.
{{% /notice %}}
## Adding Positions
There are two ways to add positions:
- **Manual**: Use the context menu **Create Import Position** to add a new row and enter the data.
- **CSV Import**: Use **Upload CSV File** to import multiple positions at once. The CSV format uses semicolon as delimiter with column headers `isin`, `tickerSymbol`, and `currency`.
## Starting the GTNet Lookup
After positions have been entered, the lookup can be started via the context menu **Import securities from GTNet**. A background job queries the accessible GTNet peers and links or creates securities based on the best match.
{{% notice style="warning" title="Background Job" %}}
The GTNet lookup runs as a background job. The current view is **not automatically updated**. To check the import status, use the **Background Jobs** in the system menu. Once the job is complete, you can manually refresh the view to see the linked securities.
{{% /notice %}}
{{% notice style="info" title="Daily Limit for Users with Limits Privilege" %}}
Users with the "Limits" privilege are subject to a daily limit on the number of securities that can be created via GTNet (default: 150 per day). If more positions are requested than the limit allows, the import process stops and the remaining positions are not processed. The status can be viewed in the **Background Jobs**, where a corresponding error message is also displayed. Securities already created are retained. Linking to existing securities does not count against the limit.
{{% /notice %}}
{{% notice style="info" title="Ownership of Imported Securities" %}}
Securities created via GTNet are publicly accessible and do not belong to any specific tenant. All users of the instance can use these securities. The user who triggers the GTNet import is recorded as the creator of the security. This allows users with "Limits" privileges to directly edit the securities they imported - however, creating new securities is subject to the daily limit described above.
{{% /notice %}}
## Historical Price Data Import
After creating securities, historical price data is automatically imported. GTNet determines the optimal data provider: The system queries all reachable GTNet peers for their available data coverage, meaning the earliest and latest date of existing price data. The peer with the longest historical coverage and best success rate is selected, and historical price data is fetched from that peer. If no GTNet peers offer historical data for the security in question, the configured data connectors are used as a fallback.
{{% notice style="info" title="Not All Peers Provide Price Data" %}}
Providing security metadata and historical price data are separate functions in GTNet. A peer instance may offer security information without providing historical price data. In such cases, the system automatically falls back to local data connectors.
{{% /notice %}}
## Editing the Linked Security
After successful linking, the imported security can be opened and modified via the context menu **Edit Security**. This allows correction of metadata, asset classes, or data connectors.
## Deleting Positions and Securities
The context menu offers two deletion options:
- **Delete Record Import Position**: Deletes only the position entry. The linked security remains intact and can still be used in the system.
- **Delete Linked Security**: Removes the link between position and security and deletes the security from the system. The position remains and can be queried again via GTNet. This option is useful when the imported security does not match the desired result.
## Verifying Transferred Data
After the GTNet lookup, the created securities should be visually reviewed:
- **Connector Availability**: The peer instance may deliver securities with data connectors that are not available on your own instance. This often occurs with connectors that require a paid subscription. Check whether the historical and intraday connectors work for your instance.
- **Asset Class Differences**: The "Sub-asset class" is user-defined and may vary between instances. Categorization by region and country may also be enabled differently. Review the asset class assignment and adjust if necessary.
- **Ticker Symbol Limitations**: When searching only with ticker and currency (without ISIN), results may be inaccurate. Unlike ISIN, which is globally unique, the ticker symbol is not unique and may refer to different securities on different exchanges. ISIN-based lookups are more reliable.
## Extended Details
Rows in the position table can be expanded to display additional information. The content depends on whether a security was successfully linked or not.
### When a Security is Linked
When a security is linked, the expanded view shows the URLs of the configured data connectors for historical prices, intraday prices, dividends, and splits.
### When No Security is Linked (Import Gaps)
When the GTNet lookup received a result from a peer instance but no security could be created, the **Import Gaps** are displayed. These document why the transfer was not fully possible.
{{% notice style="info" title="When Do Import Gaps Occur?" %}}
Import gaps occur when the local instance does not support all configurations from the peer instance. The peer instance sends information about the security, but certain elements cannot be mapped locally. This does not necessarily prevent the security from being created, but it documents the discrepancies.
{{% /notice %}}
The table shows for each gap the **Gap Type**, the **Expected Configuration** (what the peer instance offers), and the **GTNet Domain** (from which peer instance the result came). The following gap types can occur:
- **Asset Class**: The combination of asset class, sub-asset class, and financial instrument from the peer instance could not be exactly matched locally. The expected configuration shows the format "Asset Class / Sub-asset Class / Financial Instrument" from the peer instance.
- **Intraday Connector**: The peer instance uses a connector for intraday prices that is not available locally. The expected configuration shows the connector name (e.g., "yahoo", "finnhub").
- **History Connector**: The peer instance uses a connector for historical prices that is not available locally.
- **Dividend Connector**: The peer instance uses a connector for dividend data that is not available locally.
- **Split Connector**: The peer instance uses a connector for stock split data that is not available locally.
{{% notice style="warning" title="Connectors Requiring Subscription" %}}
Some connectors require a paid subscription and an API key. If the peer instance uses such connectors but your instance does not have a corresponding API key configured, a connector gap will be displayed. The security can still be created, but the affected price data must then be obtained through a different connector.
{{% /notice %}}
