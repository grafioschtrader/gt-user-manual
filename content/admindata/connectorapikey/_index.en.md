---
title: "Connector-Api-Key"
date: 2022-02-28T01:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
Some external data sources require an **API key**, regardless of the offer. A user with **administrator user rights** can view and edit these **API keys**.

## Create and edit connection API key
An API key and the subscription type can be set for each supported data source. A connection API key is created, edited and deleted in the main area in the Connection API key view. You can access this view in the navigation area on the static sub-element **Connection-API-Key** under **Administrative data**.
- **Create** a connection API key via the context menu.
- **Edit** a connection API key via the context menu with the connection API key selected.
- **Delete** a connection API key via the context menu with the connection API key selected.

## Properties and table columns
The **Data source** property is unique and reflects whether GT has implemented a corresponding connector for this data source.
- **Data source**: Name of the data source.
- **API key**: The API key of the data source.
- **Subscription type**: Most data sources have tiered offerings. GT may use the offer of the data source according to the subscription type.

{{% notice note %}}
**API key is encrypted in the database**\
The **API key** is stored encrypted in the database. GT uses the environment variable `JASYPT_ENCRYPTOR_PASSWORD` as the password. This environment variable is also used by `application.properties` 
{{% /notice %}}