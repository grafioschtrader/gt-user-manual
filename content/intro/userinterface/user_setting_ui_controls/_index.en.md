---
title: "Web Storage, Controls and properties"
date: 2026-11-19T12:00:00+01:00
draft: false
weight: 5
archetype: "default"
---
## Control elements
GT consists of different operating elements. Some **control elements** are more extensive than others and allow settings to be made for the interaction element itself.

### Table
The **Table**, **Tree** and **Hierarchy table** controls are the basic interaction elements of GT.

#### "Web memory" and table
The table is a frequently used **interaction element** in GT for the presentation of data. In some views, the web browser saves certain settings relating to the displayed columns in the **local memory**. This [web storage](https://en.wikipedia.org/wiki/Web_Storage) is not deleted when the web browser is closed and is available again for configuring the table in the next session.

#### Expanding table row
An **expander symbol** is visible in the first column of certain tables. Clicking on it expands the table row vertically, making different details for this table row visible. Clicking on the **expander symbol** again reduces the table row to its original size.

#### Tooltip in header cell
A tooltip or quick info is implemented on some **header cells** of the **table header**. This appears when the mouse pointer hovers over the corresponding header cell for a short time.

### Sorting the columns
In most tables, you can sort the data by one or more columns. Clicking on a **column header** sorts the table by that column. Clicking again reverses the sort order. Many tables support **multi-column sorting**, where you hold down the **Ctrl key** and click on additional column headers. Sorting automatically takes the respective data type into account, for example, numbers are sorted numerically and text alphabetically. For columns with translated values, sorting is based on the displayed text, not the internal values.

### Filter function
Tables can be equipped with a **filter row** located directly below the header row. This allows you to restrict the displayed data according to various criteria. Depending on the data type of the column, different filtering options are available. For **text columns**, you can either enter a search term or select from a list of existing values. For **numbers**, a menu with various comparison operators is available. For **date columns**, a calendar opens where you can select a date. Additionally, for date columns you can specify whether you want to search for an exact date or display dates before or after a specific date. The available options are **No Filter**, **Equals**, **Same or Before**, and **Same or After**. Filters can be set separately in each column and work in combination, meaning only rows that meet all set filter conditions are displayed.

### Show and hide table columns
For many tables, you can individually control the visibility of individual columns. You can access this function via the **View** menu in the menu bar once you have activated the corresponding table. In the submenu you will find the entry **Show columns**, which displays a list of all available columns. Visible columns are marked with a filled square, hidden columns with an empty square. Clicking on a column name toggles the visibility of that column. Your settings are saved in the local storage of the web browser and remain even after closing the browser. The next time you open GT, your preferred column settings will be automatically restored.

### Fixed header row and fixed columns
In many tables the **header row** stays in place when you scroll vertically, so the column headings remain visible even in long lists. Some wide tables, for example those of the **watchlist** and the **batch processing monitor**, additionally allow columns to be fixed at the left edge. You set the number in the **Show columns** dialog with the slider **Columns fixed on the left**. When scrolling horizontally, the name of the instrument, for example, stays in place while the key figures to its right move past. This setting is also kept in the local storage of the web browser.

## Compact presentation of the data
Many modern web applications, especially SaaS offerings, rely on generous spacing, large tiles and few figures per screen. At first glance this looks more appealing. GT, on the other hand, rather gives the impression of accounting software. Behind this lies the principle of **usefulness before beauty**: anyone who manages assets, fills in a tax return or checks price data wants to see and compare as much information as possible at the same time, without constantly changing pages. This fits with GT showing as much data as possible to all user roles, see [Visibility in the user interface]({{% relref "/intro/userrights" %}}#visibility-in-the-user-interface).

A compact presentation puts more rows and columns on the screen. Figures are lined up in tables and can therefore be compared directly. The controls described above support this:
- The **expanding table row** shows details directly below the row. You do not have to leave the current view and therefore lose neither your sorting, your filters nor your position in the list.
- **Multi-column sorting** and the **filter row** narrow down large amounts of data at the important places, instead of you having to scroll through long lists.
- The **fixed header row** and the **columns fixed on the left** keep you oriented even when a table holds more data than the screen can show.
- With **Show columns** you hide columns you do not need. This way you decide yourself how dense a table is.
- The **context menu** offers many commands without buttons taking up space.
- The **additional area** shows a chart below the data, so that table and chart are visible at the same time.
- Entering data in **modal dialogs** leaves the current view in place in the background. After saving you are back exactly where you were.

A pronounced example is the [Dividends/Interest]({{% relref "/reportportfolio/dividends" %}}) report. There the expansion even has three levels: the year row opens the securities and bank accounts of that year, and a security or account row in turn opens the individual transactions. When filling in a tax return it is very practical to see the underlying transactions of a position while keeping the previous tax year expanded for comparison. All this happens without leaving this tab, and the transactions can even be edited directly from the expansion.

## Properties
Some information classes have the general property **Comment**, which is used to add a note. This property is not mentioned further in this documentation.

### Diagram basic functionality
GT offers various options for graphically displaying data. Charts are displayed in the **additional area** below the main area. A chart is usually displayed via the **context menu** or the **View** menu. Frequently used chart types are line charts for price trends and bar charts for comparisons. The **legend** shows the different data series and allows you to show or hide individual series by clicking on them. When you move your mouse over the chart, you will see detailed information about the respective data points. For some charts, you can click on a data point to access further detailed information. The content of the additional area is retained when you switch to other views, so you can view charts and data in parallel.