---
title: "Background tasks"
date: 2024-02-27T22:54:47+01:00
draft: false
weight: 40
archetype: "default"
---
The various batch processing task types are described here. Certain task types may also have been explained elsewhere in this manual. Some task types cannot be created by the administrator. These are created exclusively by the system.

##### 0 - Historical and intraday course update with completeness tracking and split calendar update
This task type is used to process various tasks in the following order:
1. Historical and intraday price data update.
2. [Completeness of historical securities price data](../../../admindata/historyquotequality/)
3. Cash holdings table, for more information see [Architecture of GT - Cash holdings table](../../../reportportfolio/periodperformance/holdingtable/)
4. A new task with ID 10 is created for currency pairs without historical price data

##### 1 - Read dividend, as connector has been changed
For more on this topic, see [Dividend](../../../watchlistinstrument/externaldata/historyquote/).

##### 2 - Read splits again because the connector has been changed or the instrument has undergone a split
Is triggered if the data connector of the split has been changed or if a possible new split has been detected in the split calendar. It reloads all splits for a specific security. If the instrument's splits have been changed, the historical price data is also completely reloaded.

##### 3 - Possible creation of currencies and restoration of stock tables
If the main currency of a client or one of his portfolios changes, missing currency pairs may have to be created for this main currency. In addition, the position tables must be reconstructed. See more on this topic under"[Period income report and redundant data](../../../reportportfolio/periodperformance/holdingtable)".

##### 4 - Currency of the client and portfolios changed, therefore recreate position tables
If the main currency of a client and its portfolios changes, any missing currency pairs must be created for this main currency. The position tables must also be rebuilt.

##### 5 - Loading or reloading historical price data of the instrument
When a security is created, the intraday and historical price data is read. If the connector for the historical price data is changed, the prices must also be reloaded. When the security is created, this can be done synchronously or asynchronously with this task. The historical price data is only loaded synchronously for unit tests or according to the settings in `application.properties`. The price data for currency pairs is always loaded synchronously.

##### 6 - Position quantity of a security has changed due to a split, all dependent securities positions of the clients are rebuilt
The change of splits can affect the position table of the instruments. See more on this topic under"[Report on periodic income and redundant data](../../../reportportfolio/periodperformance/holdingtable)".

##### 7 - Possible rebuilding of account holdings because the historical exchange rates have changed
Changes to the historical price data of currency pairs can have an impact on the cash accounts balance table. See more on this topic under"[Period yield and redundant data report](../../../reportportfolio/periodperformance/holdingtable)".

##### 8 - Check repeatedly after a split whether the historical price data reflects this and can be loaded
When a split is added for a security, it can take a few days for the adjusted historical price data to be available from the data provider. This task is therefore repeated until the historical price data for the security has been successfully imported.

##### 9 - Restore positions for one or all clients, usually after an import from an export
The position tables are only updated if the transactions are processed in the usual way. This may not be the case when importing data or copying demo user accounts. This task can therefore be used to update the stock tables for one or all clients.

##### 10 - Load historical exchange rate data of an empty currency pair
This task type is mentioned in [Currency pair and cryptocurrencies](../../../watchlistinstrument/instrument/currencypair/).

##### 11 - Copy the source client to the demo accounts
Copies the data of one client to other clients. This function can be used for demo user accounts. As visitors can change the data of these user accounts, they should be recopied daily. The source accounts are copied to the target accounts according to the specifications in `application.properties` and the global settings. Two different source accounts are provided so that, for example, German and English-speaking demo user accounts can be served.
- **gt.demo.account.pattern.de and gt.demo.account.pattern.en in application.properties**: This pattern is used to express the target accounts. This pattern is used to search for the target accounts. If no demo user account is desired, only this search pattern in the user's e-mail may not result in a hit.
- **gt.source.demo.idtenant.de and gt.source.demo.idtenant.de in global settings**: These are the IDs of the source accounts. If the corresponding source account is not found, no copying takes place.

##### 12 - Creates the trading calendar for stock exchanges through a main index
An index can be used to track the free trading days on a stock exchange. This eliminates the need to track the trading days manually. If possible, this task should be carried out on a Sunday to avoid temporary incorrect entries of public holidays. More on this topic under [Trading Center](../../../basedata/instrumentbased/stockexchange/).

##### 13 - Tracks possible new dividends of the instruments via the connectors
The loading of dividends for the individual instruments via connectors is currently carried out using two different algorithms. One algorithm monitors one or more dividend calendars, the other works with the expected periodicity of dividend payments:

- **Dividend calendar**: the dividend calendar is loaded daily on the trading days via the corresponding connector. The dividends are added if the instruments listed in this calendar are also contained in this GT instance.
- **Periodicity**: This is used to add dividend income to the dividend entity. The following algorithm is used to determine any missing dividend income in the Dividend entity for securities. This is done on the basis of the date of the last dividend payment and the periodicity of the expected payments. In addition, the dividend payment in the Transaction entity is also taken into account if the dividend payment is more recent than the date in the Dividend entity.

##### 14 - Checks all clients for open positions of inactive instruments and determines possible missing dividends or interest payments.
This is a check to see if dividends or interest for held positions are recorded in the transactions. This background job should be performed daily. It also checks whether there are open positions for an instrument that is no longer traded. There are two different procedures for determining any missing dividend or interest payments:
- **Distribution** frequency**and last payment**: when the next dividend or interest payment can be expected can be derived from the distribution frequency and the last payment. This method is only used for instruments for which the data in the dividend table is not available or for which the individual entries do not have a payment date.
- **Dividend table**: In GT, a table of dividend payments is added via connectors. By linking this table to the transactions, it is possible to check whether any entries are missing from the dividend transactions. The transaction date may differ by a few days from the payment date in the dividend table. In addition, this procedure only checks approximately one year into the past.

##### 15 - Clean up incomplete user registrations
A verification token is created during user registration. In a further step, this must be confirmed by the future user by e-mail. If this verification is not successful, the user and the verification token are deleted.

##### 16 - Loading the ECB's historical exchange rate data
For more information, see [European Central Bank](../../../watchlistinstrument/externaldata/)

##### 17 - Monitoring the connectors for historical exchange rate data
This task type checks whether a connector for **historical exchange rate data** may no longer be functioning. Certain parameters of the check can be adjusted via the global settings. In the event of a possible malfunction of a connector, the **main administrator** receives a message. This task should be performed daily.
- **gt.history.observation.retry.minus** transmission error at maximum retry**(gt.history.retry**) minus this number. If `gt.history.retry` contains the value 4, this value should be 0 or 1. Thus, an instrument with 4 or 3 repetitions would be considered non-functioning.
- **gt.history.observation.days.back**: Instrument is only taken into account if a successful transmission has taken place within the current date minus this number of days. In this way, instruments that are no longer active but are listed as active do not distort the calculation.
- **gt.history.observation.falling.percentage**: Message if at least this percentage of a connector has failed. At 100 %, the connector probably no longer works at all.

##### 18 - Monitoring of connectors for intraday price data
This task type checks whether a connector for **intraday price data** may no longer be working. As with the monitoring of historical price data, the **main administrator** receives a message. The following parameters can be changed using global settings:
- **gt.intraday.observation.retry.minus**: See **gt.history.observation.retry.minus** of task type 17.
- **gt.intraday.observation.or.days.back**: A connector of an instrument is classified as faulty if the number of repetitions is too high or if no update has taken place since the current date minus this value as the number of days.
- **gt.intraday.observation.falling.percentage**: See **gt.history.observation.falling.percentage** of task type 17.

##### 19 - Fills and saves the user-defined fields of user 0
Some global user-defined fields have a longer validity period. This can make the content persistent. In addition, the effort required to create their content is sometimes time-consuming, e.g. because data suppliers have to be contacted. It therefore makes sense to update them daily.

##### Splits
Various **background tasks** are involved in **recognizing (ID-0)** and **updating (ID-2)** **splits** in GT from external data sources. It can take several days until the split read from a split calendar is also correctly mapped in the corresponding security of the external data sources. This process is shown below in a simplified flowchart, where the hexagon node stands for a background task: 
{{< mermaid >}} 
graph TD; 
    A{{Time-controlled daily}} --> B(Read one or more split calendars) 
    B --> |Possible split found for a security| C{{+2 min: Data change}} 
    C --> D(Read split of corresponding security) 
    D --> |Problem name security in calendar and at GT| E{Split found?} 
    E --> |No| G{{+ 1 day: data change}} 
    G --> D 
    E --> |Yes| H{{+ 1 day data change}} 
    H --> I(Read historical price data of security) 
    I --> |GT needs split adjusted price data| K{Does historical price data contain this split?} 
    K --> |No| H 
    K --> |Yes| L{{+0: data change}} 
    L --> M(update position of the relevant security of the affected clients for this split) 
{{< /mermaid >}} 
It is possible that this process incorrectly recognizes the name of the security from the split calendar, i.e. an attempt is made to apply a split to an existing security in GT. As a result, the corresponding security is checked daily for this split without success. In such a case, the corresponding waiting background task should be deleted using this monitor.
