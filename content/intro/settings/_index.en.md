---
title: "Settings"
date: 2026-10-20T12:00:00+02:00
draft: false
weight: 35
archetype: "default"
---
The settings are available under **Settings** in the **menu bar**.

## Change password
This allows you to change your existing password. First enter your current password under **Previous password**, then the **New password** and once more under **Confirm new password**. If the two entries differ, the dialog cannot be saved. The requirements a password must meet are set by the administrator of the instance; the message below the input field states the rule in force. By default, a password has at least eight characters with at least one letter and one number. If the previous password is wrong, the change is rejected.
After a successful change you are logged out and log in again with the new password. A demo account cannot change its password.
{{% callout note %}}
If the administrator tightens the password rule, your existing password may no longer meet it. In this case the **Change password** dialog opens by itself right after you log in and can only be closed once you have set a new, valid password.
{{% /callout %}}

## Change nickname and country/language
This dialog adjusts three personal properties. The **Nickname** is between 2 and 30 characters long and must be unique on the instance; if it is already taken, the change is rejected. **Language and country** selects the language of the user interface and at the same time the country-specific format of numbers and dates. **Entity ownership visible** highlights in shared data, such as instruments or stock exchanges, the entries you created yourself; in tables they appear in bold, for example.
After saving you are logged out. The changes take effect from your next login.

## Language-model keys
Use **Language-model keys** to manage providers for AI-assisted suggestions for your custom fields. Keys belong to your home tenant. Requests made with these keys are billed by your provider. The administrator determines which providers are available; Ollama uses the instance's hardware and must be explicitly allowed.
Choose **Add provider**, select a provider and optionally enter a model. Leaving the model empty uses the default shown in the table. Lower numbers under **Order of evaluation** are tried first. Each provider can have one entry, with a default limit of four entries overall. Clear **Active** to skip a provider without deleting its key. **Daily query limit** caps the requests per UTC day over this key and thus your costs; empty means no limit. When the limit is reached, GT tries the next provider in the order, and a run stops only when every key has reached its limit. The column **Requests today** shows the use of the current day, failed attempts included. Independently of this, the instance limits the requests of each user per day to protect its computing time.
Keys are stored encrypted. The table shows only their last four characters under **Key ending**. When editing, the key field shows the stored key ending as a placeholder, because the key itself is never returned to the browser; leave the field empty to keep the saved key. **Test key** checks only the selected entry, including inactive entries. The test counts against the query limit of the key and your personal daily limit of the instance and shows provider errors such as an invalid key, missing credit or an unavailable model.
The menu entry is hidden and key management is blocked while using a simulation environment or a managed tenant. AI requests in those contexts still use your home tenant's keys. Keys are excluded from exports and tenant transfers; enter them again on a new instance. Deleting the account removes the home tenant's keys.

## AI assistants (MCP)
With this menu item you allow an AI assistant such as Claude Code or ChatGPT to read your portfolios. It appears only when the administrator has opened the MCP server. The description is under [AI assistants (MCP)]({{% relref "/intro/settings/aiassistant" %}}).
## Export personal data
You can export your data at any time and then import it into a new MariaDB Grafioschtrader schema. With this procedure you can say goodbye to a shared GT instance and **start** a **new personal instance of GT** or edit or analyze the newly created database yourself with the appropriate tools.

The export is intended as a migration aid and is deliberately kept small: it contains everything that cannot be re-obtained and leaves everything reproducible to the data providers, which supply it again after the new instance is started for the first time.

### Content of the export file
The export is downloaded as the file **`gtPersonalData.zip`** and contains three files.
- The file **`gt_ddl.sql`** creates the empty database schema.
- The file **`gt_data.sql`** contains your actual data.
- The file **`broken_history_connectors.txt`** lists those still-active instruments whose connector for historical prices no longer works. This file is always present; if no such instruments exist, this is noted accordingly inside it.

### What is exported
In addition to your **personal data**, most **shared data** is exported regardless of ownership. Always included are your structure (portfolios, accounts, transactions, watchlists, standing orders, correlations and transaction imports), the instruments you use together with your **private securities**, the currency pairs you need, and the **dividends and splits**. The structure also includes [rule based trading]({{% relref "/algoalert/algo" %}}) with its hierarchy, the strategies and their settings, the rebalancing recommendations and the state of running strategies. The instruments used there are treated like those of a watchlist and are exported together with their dividends, splits and derived instruments. Shared reference data such as stock exchanges and trading calendars, asset classes, tax tables, the data-provider definitions and the global settings is exported as well.

The **historical price data** is exported only when it cannot be re-obtained: for instruments that are no longer active, for currency pairs, for risk-free-rate instruments, for the preserved legacy price archive and — newly — for still-active instruments whose connector no longer delivers prices. You will find the latter in the file `broken_history_connectors.txt`.

After the import, the historical prices of still-active instruments are **re-obtained**; they are loaded by the connectors a few minutes after the back-end first starts. The current daily or intraday prices are fetched live anyway, and your holdings are recalculated from the transactions during the import. Messages, the alarm messages of the strategies and the internal task list are not exported. Neither are the [simulation environments]({{% relref "/algoalert/historicalrun/environment" %}}) together with the results and the event log of their historical replays. Grafioschtrader generates these itself; in the new instance you get them back by running the simulation again.

The following table summarises what the export contains.

| Data | Included in the export? |
|------|-------------------------|
| Personal structure, transactions, watchlists | Yes |
| Your own and private instruments, currency pairs | Yes |
| Dividends and splits | Yes |
| Rule based trading: hierarchy, strategies, recommendations and their instruments | Yes |
| Shared reference data (exchanges, asset classes, providers, settings) | Yes |
| Historical prices of inactive instruments, currency pairs, legacy archive | Yes |
| Historical prices of active instruments with a broken connector | Yes (new) |
| Historical prices of active instruments | No – reloaded after the import |
| Intraday / daily prices | No – fetched live |
| Holdings / positions | No – recalculated from transactions |
| Simulation environments, replay results and event log | No – restored by running the simulation again |

{{% callout note %}}
The historical price data of an instrument is **not exported** if it is still **active** at the time of export plus one month; it is loaded by the corresponding connectors a few minutes after the back-end of the new GT instance is started for the first time. An **exception** are still-active instruments whose connector for historical prices no longer works: their prices are exported as well, because otherwise they could not be restored.
{{% /callout %}}

### From export to new instance
The following diagram shows the entire flow from the export to the running new instance.

{{< mermaid >}}
flowchart TD
    A[Trigger export in GT] --> B[gtPersonalData.zip]
    B --> C[gt_ddl.sql – empty schema]
    B --> D[gt_data.sql – your data]
    B --> E[broken_history_connectors.txt – report]
    C --> F[Create schema in new database]
    D --> G[Import data]
    F --> G
    G --> H[Start back-end for the first time]
    H --> I[Connectors load missing historical prices]
    H --> J[Holdings recalculated from transactions]
    I --> K[Running personal GT instance]
    J --> K
{{< /mermaid >}}

Loading the historical prices requires the API keys of the connectors to be entered in the new instance, and it can take a long time. What to observe is described on the page [Historical prices after the import]({{% relref "/intro/settings/historicalpriceload" %}}).

### Import export into database
If you want to process the export further according to your needs, you can proceed as follows. In the following the database **grafioschtrader_s** and not **grafioschtrader** is mentioned.
````
# Unzip file gtPersonalData.zip
unzip gtPersonalData.zip
# Create the database with your desired password for grafioschtrader
mysql -u root -p -e "create database grafioschtrader_s; GRANT ALL PRIVILEGES ON grafioschtrader_s.* TO grafioschtrader@localhost IDENTIFIED BY 'YOUR_DB_PASSWORD'"
# Create GT database schema, the password from above ('YOUR_DB_PASSWORD') is requested
mysql -ugrafioschtrader -p --default-character-set=utf8 grafioschtrader_s < gt_ddl.sql
# Import of personal data, the password from above ('YOUR_DB_PASSWORD') is requested
mysql -ugrafioschtrader -p --default-character-set=utf8 grafioschtrader_s < gt_data.sql 
# MariaDB-InnoDB has an error, see MDEV-28327, hence the following command:
mysqlcheck -p -a grafioschtrader
````
You can evaluate or edit the GT database with [phpMyAdmin](https://www.phpmyadmin.net/), for example.

### Use export as new GT instance
As mentioned, you can use your exported data as the basis for a new GT instance. To install GT, follow the [wiki on GitHub](https://github.com/grafioschtrader/grafioschtrader/wiki). After installation, the export can be imported.
```
# Stop Grafioschtrader service if it is running 
sudo systemctl stop grafioschtrader.service
# Unzip file gtPersonalData.zip
unzip gtPersonalData.zip
# Delete the existing database
mysql -u root -p -D grafioschtrader -e "DROP DATABASE grafioschtrader"
# Create the database with your desired password for grafioschtrader
mysql -u root -p -e "create database grafioschtrader; GRANT ALL PRIVILEGES ON grafioschtrader.* TO grafioschtrader@localhost IDENTIFIED BY 'YOUR_DB_PASSWORD'"
# Create GT database schema, the password from above ('YOUR_DB_PASSWORD') is requested
mysql -ugrafioschtrader -p --default-character-set=utf8 grafioschtrader < gt_ddl.sql
# Import of personal data, the password from above ('YOUR_DB_PASSWORD') is requested
mysql -ugrafioschtrader -p --default-character-set=utf8 grafioschtrader < gt_data.sql 
# MariaDB-InnoDB has an error, see MDEV-28327, hence the following command:
mysqlcheck -p -a grafioschtrader
# Start Grafioschtrader service if it exists
sudo systemctl start grafioschtrader.service
```
{{% callout warning %}}
The GT instance or source instance of the export must correspond to the same or **lower version** of the new GT instance or target instance. 
{{% /callout %}} 
Unfortunately only in German:
 {{< youtube 2xmkteNucfs >}}

## Delete my data and the user account
You can delete your data together with the user account. After confirming this deletion action, you will be logged out of Grafioschtrader. All your [simulation environments]({{% relref "/algoalert/historicalrun/environment" %}}) and their results are deleted together with the user account.

As long as you still look after other people or share your portfolio with someone, the deletion is **blocked**. You must dissolve these connections first. If, as an advisor, you look after separate clients, you have to delete them beforehand; the procedure is described under [Managing multiple clients]({{% relref "/tenantportfolio/client/managedclients" %}}). If you have given another person read access to your own portfolio, you have to revoke those shares beforehand; see [Share read access to your portfolio]({{% relref "/tenantportfolio/client/sharereadaccess" %}}). An overview of both functions is given under [Shared access – basics]({{% relref "/tenantportfolio/client/sharedaccess" %}}). If you attempt the deletion anyway, Grafioschtrader tells you with a corresponding message what still needs to be deleted or revoked.

{{% callout warning %}}
This action cannot be undone!!!
{{% /callout %}}
