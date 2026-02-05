---
title: "Settings"
date: 2021-05-02T22:54:47+01:00
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

### What is exported
In addition to **personal data**, most **shared data** is exported, with the exception of instruments. It is exported regardless of the ownership of an entity.
- **Instruments**: The instruments used in your watchlist and in transactions are exported, as well as **private securities**.

{{% notice note %}} 
The historical price data of an instrument is **not exported** if it is still **active** at the time of export plus one month. The historical price data is loaded by the corresponding connectors after a few minutes when the back-end of the new GT instance is started for the first time.
{{% /notice %}}

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
You can evaluate or edit the GT database with [phpMyAdmin](//www.phpmyadmin.net/), for example.

### Use export as new GT instance
As mentioned, you can use your exported data as the basis for a new GT instance. To install GT, follow the [wiki on GitHub](//github.com/grafioschtrader/grafioschtrader/wiki). After installation, the export can be imported.
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
You can delete your data with the user account. After confirming this deletion action, you will be logged out of Grafioschtrader. {{% notice warning %}} 
This action cannot be undone!!! 
{{% /notice %}}
