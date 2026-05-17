# ✅ Chapter 4 – Using Power BI Desktop

---

## ☑️ 1. What Chapter 4 is about?

Chapter 4 shifts the focus from the **Power BI Service (the web portal)** to **Power BI Desktop (the Windows app)**.

In earlier chapters, David mostly:

* Pulled data into **Excel**, then
* Uploaded the Excel file to Power BI, and
* Built dashboards and reports in the service.

Now he wants something more robust and automated:

* Connect directly to **SQL Server** (his real data source),
* Combine it with **Excel-based budget data**, and
* Use **Query Editor** to shape the data properly.

Chapter 4 teaches you how to:

* Connect Power BI Desktop directly to a **database** (SQL Server in the example). 
* Load data from **multiple sources** (SQL + Excel) into one model. 
* Use **Query Editor** to clean, merge, and transform data. 
* Hide or avoid loading tables so your model stays simple. 
* Fix **seasonality and month sorting** problems in the reports. 

By the end, you’re not just a report user—you’re starting to be a **data modeler**. 

---

## ☑️ 2. Connecting Power BI Desktop to a database

First, David installs **Power BI Desktop** and uses it to connect directly to the **Contoso SQL Server database** instead of going through Excel. 

When he chooses **Get Data → SQL Server** and imports a table:

* Power BI Desktop loads data into its **in-memory model**.
* Initially, the table name is long (includes schema), so he **renames** it to something cleaner, like `Sales2015`. 

The book briefly mentions **Import vs DirectQuery**:

* **Import** (used in the chapter):

  * Copies the data into Power BI’s internal storage.
  * Best when you don’t need *constantly* up-to-the-second data.
* **DirectQuery**:

  * Leaves data in the source and queries it on the fly.
  * More advanced, with technical implications (performance, limitations), so the book doesn’t go deep into it. 

Key idea for you:

> Once Power BI Desktop connects directly to SQL Server, **Excel is no longer a required middle step**. Refreshing the model means **going straight back to the database**, not refreshing a manual Excel export. 

---

## ☑️ 3. Loading data from multiple sources

Connecting to SQL Server solves one problem (live data), but creates another:

* Previously, David’s **Excel model** combined:

  * **Sales** (from SQL Server)
  * **Budget forecasts** (stored in an Excel workbook updated by managers)
* The budget file itself does **not** live in SQL Server, and managers still want to update it in Excel. 

To handle this, Power BI Desktop needs to:

1. Load **Sales** from SQL Server.
2. Load **Budget** from the Excel workbook.
3. Combine the two inside one model.

The chapter explains that each Power BI Desktop dataset actually has an **internal query**. For simple imports you don’t see it, but you can edit it via **Query Editor**. 

---

## ☑️ 4. Query Editor – your data shaping playground

David opens **Query Editor** from Power BI Desktop (Home → Edit Queries). 

The Query Editor window has key areas: 

* **Ribbon** – tabs like **Home**, **Transform**, **Add Column**, **View**.
* **Queries pane** (left) – list of all queries (tables) in the model.
* **Data preview** (center) – shows rows of the currently selected query.
* **Query Settings pane** (right) – shows properties and the list of **Applied Steps**.

In this chapter:

* There’s already a query for **Sales2015** (from SQL Server).
* David uses **New Source** to load data from the **Budget** table in Excel, creating a **second query**. 
* When he clicks **Close & Apply**, both queries are loaded into the model, and in the Fields pane he sees both the SQL table and the Excel table. 

Important concept:

> Every transformation you do (rename column, merge tables, add custom column, etc.) becomes a **step** in the query. You can edit these steps later to change how the data is loaded.

---

## ☑️ 5. The “budget looks wrong” problem

After loading Sales2015 and Budget, David creates a report:

* A chart showing **sales and budget by brand**.

But something is clearly wrong:

* The **Budget 2016** value appears **the same for all columns**.
* The total budget is also **too high**. 

Why? Because the budget was modeled as **a single yearly number** per brand/country, in a table and then spread incorrectly when combined with sales.

This exposes several typical issues:

1. **Granularity mismatch**:

   * Sales are at the level of *country, brand, month*.
   * Budget might be at *country, brand, year*.
   * If you simply join them, the yearly budget can get **repeated for every month**, inflating totals.

2. **Missing months**:

   * Some months have no sales data, so they don’t appear as rows.
   * If you naïvely divide the yearly budget by 12 months, but only 10 months have rows, you’ll effectively allocate more than the real budget in the report. 

This is where **Query Editor transformations** and better modeling come in.

---

## ☑️ 6. Hiding or removing tables (and “Enable Load”)

The book explains that not all tables need to be visible in the final model:

* Some tables are **main data tables** you report on directly.
* Others are **helper/intermediate** queries used only to calculate values for those main tables.

Power BI Desktop lets you:

* **Hide** a table:

  * It remains in the model, but is not shown in the Fields pane (useful for simplifying what report authors see).
* **Disable load (“Enable Load” off)**:

  * The table/query is used *inside Query Editor only* and is **not loaded** into the model at all. 

In David’s scenario:

* He eventually shapes the data so that **Sales2015** contains all the columns needed for reporting.
* The original Budget table is only used to compute some fields and doesn’t need to appear as a standalone table.
* So he marks it **“do not load”** to keep the model neat. 

For you as a learner:

> Use **hide** when a table is useful technically but you don’t want people to drag/drop it.
> Use **“Enable Load” off** when the table is only an intermediate step in building other tables or columns.

---

## ☑️ 7. Handling seasonality and fixing the budget distribution

To handle **seasonality** and fix the budget logic, the chapter does some clever modeling with extra queries:

1. Create a query that counts for each combination of **CountryRegion + Brand** how many **months** in 2015 had sales (a “Number Of Months” table). 
2. **Merge** that query into `Sales2015`, so each row knows the number of months with sales for its brand and country. 
3. Adjust the expression for the **Budget 2016** column so that:

   * Instead of dividing the yearly budget by 12,
   * It divides it by **Number Of Months** (the number of months with actual sales in 2015). 

This way:

* Budget is **distributed only across months that actually have sales**, which makes comparisons more meaningful.
* The total budget in the report now matches the expected yearly budget. 

When David updates the query and clicks **OK**, the report refreshes and shows correct numbers, like the fixed budget value of 37,500. 

This teaches a big lesson:

> Sometimes you must **rethink your data** (e.g., how budget is allocated over months) instead of just plotting it. Query Editor lets you embed this logic into the loading process so the model is always correct.

---

## ☑️ 8. Sorting months properly (fixing the “alphabetical month” issue)

Another classic beginner problem: **months out of order**.

* If your Month column contains text like “April”, “August”, etc., Power BI sorts them **alphabetically**, giving an incorrect order.
* Chapter 4’s section on **handling seasonality and sorting months** shows how you can fix that by:

  * Creating or using a **MonthNumber** column (1–12),
  * Then using **Sort By Column** so that the Month name is sorted by MonthNumber instead of alphabetically. 

In practice, this is a pattern you’ll use a lot:

* Any time labels have a natural order that isn’t alphabetical (months, weekdays, etc.), add a numeric column for the correct order and sort by that.

---

## ☑️ 9. Chapter 4 conclusion – what you should now know

The book ends the chapter with a short list of what you’ve learned about Power BI Desktop: 

* **Power BI Desktop can load data from many databases** (SQL Server in the example).
* You can **load data from multiple sources** and combine them (Excel + SQL Server).
* It uses **Query Editor** to load and transform data, including:

  * Merging queries,
  * Adding calculations,
  * Inserting steps at the right place in the query.
* Some queries are loaded into the model; some are **“do not load”** helpers used only in Query Editor.
* You can **publish** a Power BI Desktop model to the Power BI Service, and it retains the same refresh capabilities via the **Personal Gateway** (from Chapter 3).

The chapter also reassures you:

* Query Editor can feel complex at first, but once you get used to it, it gives you **a lot of power** to design your models.
* Finishing this chapter means you can genuinely call yourself a **data modeler** in Power BI. 

---

## ☑️ 10. How to practise as a learner

To make this chapter “real” for you, try:

1. **Connect Power BI Desktop to two sources**:

   * One database (or CSV pretending to be “Sales”),
   * One Excel file (pretending to be “Budget”).
2. Use **Query Editor** to:

   * Rename tables and columns,
   * Merge in a helper table (e.g., number of months),
   * Add a **custom column** that adjusts budget.
3. Mark at least one query as **“Enable Load” off** and check that it disappears from the Fields pane but still feeds the main table.
4. Add Month and MonthNumber and fix **month sorting** in a chart.
5. Build a report with **Sales vs Budget by month and brand**, then publish it to the Power BI Service.

Do that once or twice and Chapter 4 will stop feeling theoretical and start feeling like muscle memory.

---

# ✅ Question & Answer on Chapter 4 (Using Power BI Desktop):

---

### 🔴 1. What is Power BI Desktop, and why do we use it instead of only the Power BI service?

**✴️ Answer:**
Power BI Desktop is a Windows application that lets you connect to data sources, clean and transform data, and build data models and reports on your own machine. You then typically publish those models and reports to the Power BI service for sharing and collaboration. Desktop is where most of the *data preparation and modeling work* happens before reports go online. 

---

### 🔴 2. How do you connect Power BI Desktop to a database such as SQL Server?

**✴️ Answer:**
In Power BI Desktop, you click **Home → Get Data → SQL Server**, enter the **server name** and (optionally) the **database name**, choose the tables or views you want, and then load or transform the data. This is how David connects to the Contoso sales database in the chapter before shaping his data further. 

---

### 🔴 3. Why would you load data from more than one source into the same Power BI Desktop file?

**✴️ Answer:**
Real-world reports often combine information from different systems—for example, **actual sales data** from a SQL Server database and **budget data** from an Excel workbook. In David’s case, he loads sales from the Contoso database and the budget from an Excel file and then combines them so he can compare sales vs. budget in one report. 

---

### 🔴 4. What is the Query Editor in Power BI Desktop, and what are its main areas?

**✴️ Answer:**
Query Editor is a dedicated window where you **clean, combine, and transform** your data before it’s loaded into the data model. Its main areas are:

* **Ribbon** (Home, Transform, Add Column, View)
* **Queries pane** (list of all queries/tables)
* **Data preview** (shows sample rows)
* **Query Settings pane** (properties and step list)

These areas together let you see your data, define transformation steps, and control how data is loaded. 

---

### 🔴 5. How do you load an additional table (like a Budget table in Excel) using Query Editor?

**✴️ Answer:**
Inside Query Editor, you use **Home → New Source**, pick the source type (e.g., Excel), and select the table or range you want (like the **Budget** table). That creates a new query in the **Queries pane**, so you now have multiple queries—Sales2015 from SQL Server and Budget from Excel—in the same model to combine later. 

---

### 🔴 6. What is the difference between **hiding a table** and **disabling its load**?

**✴️ Answer:**

* **Hide a table**: The table stays in the data model, but it’s not shown in the Fields pane. Users can still make it visible again, so it’s only a *usability* feature, not a security or performance feature. 
* **Disable load (turn off “Enable Load”)**: The table is used only inside Query Editor and is **not loaded into the data model** at all. It’s useful for intermediate tables that you use to build other tables but don’t want to clutter the model with. 

---

### 🔴 7. In the chapter scenario, why does David decide to *avoid loading* the Budget table?

**✴️ Answer:**
David merges the Budget data into the **Sales2015** table using Query Editor, so all required columns for reporting are inside Sales2015. The original Budget table then becomes **redundant**—it’s only needed as an intermediate step. To keep the model clean and simpler for other users, he turns off **Enable Load** for the Budget query so it doesn’t appear as a separate table in the Fields pane. 

---

### 🔴 8. Why is seasonality a problem when the budget is simply divided by 12?

**✴️ Answer:**
Some brands have **strong seasonal patterns** (for example, higher sales in specific months), so splitting the annual budget evenly by 12 ignores those patterns. Also, if certain months have no sales rows at all, that month is missing from the report—so you might end up distributing a yearly budget across only 10 months instead of 12, making total budget numbers in the report **incorrect**. 

---

### 🔴 9. Why do months sometimes appear in the wrong order (e.g., April, August, December, February) in visuals?

**✴️ Answer:**
By default, Power BI sorts a text column **alphabetically**, not chronologically. If your Month column contains names like “April”, “August”, etc., they’ll be sorted A–Z instead of January–December. To fix this, you usually add or use a **MonthNumber** column (1–12) and then set **Sort by Column** so that the Month name is sorted using MonthNumber. 

---

### 🔴 10. When should you consider building a single-table model from multiple sources?

**✴️ Answer:**
You should consider building a **single, wide table** when users mainly need a straightforward view of the data (e.g., sales and budget together by month, brand, country) and don’t need complex relationships between many tables. In the chapter, David merges sales and budget into one Sales2015 table, which makes the model easier for non-technical users like Wendy to browse and reduces confusion in the Fields pane. 

---
