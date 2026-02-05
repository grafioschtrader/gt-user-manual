---
title: "Batch processing monitor"
date: 2025-12-27T22:54:47+01:00
draft: false
weight: 40
archetype: "default"
---
GT is a **client-server application**, the back-end is designed for **24/7 operation**. This enables the **sequential** execution of different **background tasks**. Whether the execution of such a background task was successful can be monitored with the **batch processing monitor**. These **background tasks** can be triggered by two specific **events**:
- **Scheduled**: In the configuration file **`application.properties`**, some timed tasks are defined with an execution time as a property.
- **Data changes**: If certain data changes, this can have an impact on other data. For example, if a possible relevant split is detected via the split calendar, one or more additional background tasks are created to process it.
  - **Data changes through data sources**: Certain **time-controlled** background tasks change data, which in turn create a new background task.
  - **Data changes by the user**: For example, by the user changing the split or dividend connector on a new or existing instrument.

If no background task is executed, the system queries for pending background tasks in a query cycle of 15 seconds.

## Why this monitor?
Unfortunately, almost no software works flawlessly, and **GT** is no different. The batch processing monitor offers the following advantages:
- Easier monitoring of possible errors in a background task via "**Expanding table row** than via the general **log file** of GT.
- Manual creation of background tasks, so that, for example, a **failed background task** can be performed again.
- Check whether a background task has been created at all and view its **runtime**.

{{% notice info %}} 
It is possible that certain **scheduled background tasks** defined in `application.properties` are executed separately from this **batch monitor** because they do not benefit from the advantages mentioned above. 
{{% /notice %}}

## Properties and table columns
Every user can view the batch processing monitor. **Administrator user rights** are required for **editing**. The individual background tasks are only partially described here, as some of them require extensive knowledge of GT's architecture. However, there are background tasks that have an effect on both **private** and **split data**. For example, the processing of a split affects the shared data because the corresponding security must be adjusted. However, this also has a direct impact on the private data of the clients who have a position in this security.
- **Information object** and ID **entity**: Sometimes certain parameters must be passed to the background task; this is done using the two properties.
- **Priority**: If the time of several tasks of the "**Earliest start time**" property is in the past, the order of execution is based on their **priority**.
- **Status**: This is the status of the process, such as "**Waiting**", "**Running**", "**Executed**", etc.

### Expanding table row
If the indicator for an expanding table row is present. The background task could not be executed without errors. This information can be very helpful for the software developer and possibly also for the administrator.

## Creating or copying and editing background tasks
**Creating** a background task is almost never used. Rather, a failed background task is copied and executed again, possibly after the necessary data has been corrected.

## Deleting background tasks
Background tasks that are not running can be deleted manually. The system also deletes background tasks based on the **End time** property. The retention time in days is defined via `gt.task.data.days.preservein` **Global settings**.

## Interrupting a running background task
A background task may need to be interrupted or canceled. Currently, only the running background task with **ID 0** can be canceled by the **administrator**. It is not guaranteed that this will succeed in every case.

{{% notice info %}}
This function was implemented because the daily background task with **ID 0** had not completed itself twice within a year. A timeout was also introduced. The system attempts to complete this job, and if this action fails, the task receives the status **zombie**. This thread continues to run but does not block the other tasks that have not yet been executed. When this GT instance is restarted, such tasks receive the **zombie/cleared** status
{{% /notice %}}

## Filter for task types
As the number of different background tasks increases, it can become difficult to focus on specific task categories. A filter dialog can be opened via the menu, allowing a targeted selection of the task types to be displayed. The selected filter settings are saved in the browser and are retained for future visits. If no filter settings are available, all background tasks are displayed.

Unfortunately only in German:
{{< youtube psAdA4b1bGw >}}

