---
title: "Settings"
date: 2026-06-16T22:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
The settings are available under **Settings** in the **menu bar**.

## Change password
This allows you to change your existing password. After changing the password, you must log in again.

## Change nickname and country/language
You can customize the **nickname** and the "**Language and country**" property.

## Export personal data
You can export your data at any time and then import it into a new MariaDB Grafioschtrader schema. With this procedure you can say goodbye to a shared GT instance and **start** a **new personal instance of GT** or edit or analyze the newly created database yourself with the appropriate tools.

The export is intended as a migration aid and is deliberately kept small: it contains everything that cannot be re-obtained and leaves everything reproducible to the data providers, which supply it again after the new instance is started for the first time.

### Content of the export file
The export is downloaded as the file **`gtPersonalData.zip`** and contains three files.
- The file **`gt_ddl.sql`** creates the empty database schema.
- The file **`gt_data.sql`** contains your actual data.
- The file **`broken_history_connectors.txt`** lists those still-active instruments whose connector for historical prices no longer works. This file is always present; if no such instruments exist, this is noted accordingly inside it.

### What is exported
In addition to your **personal data**, most **shared data** is exported regardless of ownership. Always included are your structure (portfolios, accounts, transactions, watchlists, standing orders, correlations and transaction imports), the instruments you use together with your **private securities**, the currency pairs you need, and the **dividends and splits**. Shared reference data such as stock exchanges and trading calendars, asset classes, tax tables, the data-provider definitions and the global settings is exported as well.

The **historical price data** is exported only when it cannot be re-obtained: for instruments that are no longer active, for currency pairs, for risk-free-rate instruments, for the preserved legacy price archive and — newly — for still-active instruments whose connector no longer delivers prices. You will find the latter in the file `broken_history_connectors.txt`.

After the import, the historical prices of still-active instruments are **re-obtained**; they are loaded by the connectors a few minutes after the back-end first starts. The current daily or intraday prices are fetched live anyway, and your holdings are recalculated from the transactions during the import. Messages and the internal task list are not exported.

The following table summarises what the export contains.

| Data | Included in the export? |
|------|-------------------------|
| Personal structure, transactions, watchlists | Yes |
| Your own and private instruments, currency pairs | Yes |
| Dividends and splits | Yes |
| Shared reference data (exchanges, asset classes, providers, settings) | Yes |
| Historical prices of inactive instruments, currency pairs, legacy archive | Yes |
| Historical prices of active instruments with a broken connector | Yes (new) |
| Historical prices of active instruments | No – reloaded after the import |
| Intraday / daily prices | No – fetched live |
| Holdings / positions | No – recalculated from transactions |

{{% notice note %}}
The historical price data of an instrument is **not exported** if it is still **active** at the time of export plus one month; it is loaded by the corresponding connectors a few minutes after the back-end of the new GT instance is started for the first time. An **exception** are still-active instruments whose connector for historical prices no longer works: their prices are exported as well, because otherwise they could not be restored.
{{% /notice %}}

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
{{% notice warning %}}
The GT instance or source instance of the export must correspond to the same or **lower version** of the new GT instance or target instance. 
{{% /notice %}} 
Unfortunately only in German:
 {{< youtube 2xmkteNucfs >}}

## Delete my data and the user account
You can delete your data together with the user account. After confirming this deletion action, you will be logged out of Grafioschtrader.

As long as you still look after other people or share your portfolio with someone, the deletion is **blocked**. You must dissolve these connections first. If, as an advisor, you look after separate clients, you have to delete them beforehand; the procedure is described under [Managing multiple clients]({{< relref "/tenantportfolio/client/managedclients" >}}). If you have given another person read access to your own portfolio, you have to revoke those shares beforehand; see [Share read access to your portfolio]({{< relref "/tenantportfolio/client/sharereadaccess" >}}). An overview of both functions is given under [Shared access – basics]({{< relref "/tenantportfolio/client/sharedaccess" >}}). If you attempt the deletion anyway, Grafioschtrader tells you with a corresponding message what still needs to be deleted or revoked.

{{% notice warning %}}
This action cannot be undone!!!
{{% /notice %}}
