# ✅ Chapter 8 – Using Microsoft Power BI in your company

---

## ☑️ 1. What Chapter 8 is about?

This chapter, *“Using Microsoft Power BI in your company”*, shows how Power BI connects to **real corporate data systems**, how it works with **Office / Excel**, and how you can **secure and extend** your BI solution. Key sections are: getting data from existing systems, refresh vs live connections, on-prem vs cloud databases, Analysis Services, Office integration, security & row-level security, custom visuals, REST API, real-time dashboards, and embedding. 

As a learner, you should come out of this chapter knowing:

* Where your company’s data can sit (on-prem, cloud, semantic models).
* When to **import** data vs use **DirectQuery / live connection**.
* How Power BI and **Excel** cooperate.
* How **security** actually works (including row-level security).
* How developers can extend Power BI with **APIs, real-time streams, custom visuals, and embedding**.

---

## ☑️ 2. Getting data from existing systems

The chapter starts by categorizing the main types of systems you’ll see in a typical company: 

* **Cloud sources** – e.g., Azure SQL Database, Azure SQL Data Warehouse.
* **Relational databases** – SQL Server, Oracle, DB2, MySQL, PostgreSQL, etc.
* **Rich semantic models** – systems like **SQL Server Analysis Services**, SAP HANA, SAP BW that already contain calculations, KPIs, hierarchies, etc.
* **Gateways** – components that allow the Power BI service (in the cloud) to talk securely to **on-premises data**.

Two gateway types are introduced: 

1. **Power BI Personal Gateway**

   * Installed on your own machine.
   * Used mainly for **personal** models.
   * Works only when your computer is on.

2. **Power BI Enterprise Gateway**

   * Installed on a **server** by IT.
   * Shared by many users.
   * Usually online 24/7; ideal for production reports.

💡 *Learner takeaway*: When you connect to a corporate database from Power BI Desktop and publish to the service, you often need a **gateway** so scheduled refresh or DirectQuery can work against on-prem data.

---

## ☑️ 3. Data refresh vs live connections

A crucial concept in this chapter is the difference between **imported data** (needing refresh) and **live connections**. 

When you browse data in Power BI, there are two main modes: 

1. **Import (data refresh mode)**

   * Power BI keeps a **copy** of the data inside the dataset.
   * You must **refresh** it (manual or scheduled) to keep it up to date.
   * In Power BI Desktop, this is the default for most sources.
   * Gives you **full modeling capabilities**: relationships, calculated columns, measures, many DAX features, multiple sources in one model.

2. **Live / DirectQuery**

   * No copy is stored (or only minimal caching).
   * Every interaction triggers a **query to the source system** (e.g., SQL Server, Analysis Services).
   * In relational databases, this is exposed as **DirectQuery**.
   * In Analysis Services, you see **Connect Live** vs **Import Data** options. 

Key constraints of live connections:

* A **live model** can connect to **only one data source**, so a single report cannot mix multiple databases in the model; if you want to blend sources, you must use **Import**. 
* However, a **dashboard** can pin tiles from **different reports**, each based on different live connections – so a dashboard can show KPIs from various systems together. 

💡 *When should you use what?*

* Use **Import** when:

  * You need to **combine multiple sources**.
  * You want **advanced DAX** and complex modeling.
  * Data can be refreshed daily / hourly.

* Use **DirectQuery / Live** when:

  * You need **near real-time** numbers, or data changes constantly.
  * Your datasets are too **large** to import comfortably.
  * IT already has a **well-designed relational or SSAS model** you can reuse.

---

## ☑️ 4. Using relational databases on-premises

For **on-prem relational databases** (e.g., SQL Server in your data center), the chapter shows how you: 

1. Connect using **Power BI Desktop** → Get Data → SQL Server.
2. Choose **Import** or **DirectQuery**.
3. Build your model and reports.
4. Publish to the Power BI service.
5. Configure a **gateway** so the service can reach your on-prem database for:

   * Scheduled refresh (Import).
   * Live querying (DirectQuery).

Important points for learners:

* With **Import**, Power BI stores data in the cloud; refresh relies on the gateway.
* With **DirectQuery**, every visual sends queries through the gateway to your on-prem database, so **performance depends on that database**.
* IT often needs to tune indexes, aggregations, and capacity because DirectQuery can generate many queries quickly.

---

## ☑️ 5. Using relational databases in the cloud

For **cloud relational databases** (Azure SQL Database, Azure SQL Data Warehouse), the story is simpler: Power BI can connect to them **directly over the internet** without an on-prem gateway. 

Typical pattern:

* You choose **Import** or **DirectQuery** just like for on-prem SQL Server.
* But the connectivity is usually easier to manage since both Power BI and the database live in the **Azure cloud**.
* Credentials and security are handled via the connector (e.g., SQL Authentication, Azure AD).

💡 *Practical learner perspective*: If you’re experimenting, spinning up an **Azure SQL Database** and connecting Power BI to it is one of the easiest ways to simulate “real company” data in the cloud.

---

## ☑️ 6. Live connections to Analysis Services

The chapter then covers **live connections to SQL Server Analysis Services (SSAS)** and similar semantic models. 

With a **Connect Live** connection to SSAS:

* Power BI **does not store data**; it uses the cube / tabular model directly.
* All measures, hierarchies, KPIs, and calculations are defined in **Analysis Services** (IT-managed).
* In Power BI Desktop, your job becomes designing **reports and visuals**, not the underlying model.
* Row-level security is enforced by SSAS itself.

From a learner’s point of view, this is the typical **enterprise BI scenario**:

* IT builds robust, governed models in SSAS.
* Analysts use Power BI as the **visual front-end** via live connections.

---

## ☑️ 7. Integrating Power BI with Office (especially Excel)

The chapter shows how tightly Power BI and **Office/Excel** work together. 

### 🔰 7.1 Publish Excel data models to Power BI

If you have an Excel workbook with a **Power Pivot data model**:

* You can **publish** that workbook to Power BI.
* Power BI treats the embedded data model as a **dataset**.
* You can:

  * Build Power BI reports on top of it.
  * Or just view Excel reports via **Excel Online** inside Power BI.

This is ideal for people who already invested heavily in Excel models.

### 🔰 7.2 Consume Power BI content from Excel

You can also go in the opposite direction:

* Use Excel to connect to a **Power BI dataset** and create **PivotTables** and **PivotCharts** from it.
* It feels like a normal Excel pivot against a cube, but the cube is your **Power BI dataset**.

So:

* Power BI is great for dashboards and modern visuals.
* Excel is great for detailed analysis, ad-hoc pivoting, and grid-style reports.

You can choose whichever front-end you’re more comfortable with.

### 🔰 7.3 Power BI Tiles in Office

Using an **Office add-in** (Power BI Tiles from Office Store), you can embed **live Power BI tiles** in documents like PowerPoint. 

* When the Power BI dashboard updates, the tile in your PowerPoint can reflect the **latest numbers**.
* This reduces the “export to PNG every time” pain when preparing presentations.

---

## ☑️ 8. Managing security to access data

Security in Power BI has multiple layers, and this chapter focuses on **data access**: 

* **Workspace & sharing security**

  * Who can view or edit dashboards, reports, and datasets through workspaces or app sharing.
* **Data source credentials**

  * How the dataset connects to the database (username/password, OAuth, etc.).
* **Gateway security**

  * Which users are allowed to use a specific gateway for refresh or DirectQuery.
* **Row-Level Security (RLS)**

  * Filters applied per user so different users see different slices of the same dataset.

💡 *Learner mindset*: Remember that **sharing a report or dashboard may expose all underlying data** unless RLS or other limits are in place. Always check with your data owner before sharing widely.

---

## ☑️ 9. Row-Level Security (RLS)

Row-Level Security is one of the most “real-world” features in this chapter. 

Concept:

* You define **roles** (e.g., “Region Manager”) in the data model.
* For each role, you define one or more **DAX filters** on tables:

  * Example: `[Region] = "West"` or `[SalespersonEmail] = USERPRINCIPALNAME()`
* When users view reports, Power BI applies the proper filters, so each person sees **only the rows they’re allowed to see**.

Key points:

* RLS is defined on the **dataset** (for imported models or certain DirectQuery models).
* For **live connections to SSAS**, RLS is defined in **Analysis Services**.
* Power BI provides a **“View as role”** feature so you can test what different roles see.

For learners, RLS is critical to understand because it’s often the **difference between a toy solution and something deployable in a real company**.

---

## ☑️ 10. Extending and customizing Power BI

The last big block of the chapter shows how Power BI can be **extended and embedded** beyond the standard UI. 

### 🔰 10.1 Custom visuals

You can use:

* Marketplace visuals from the **Office / Power BI visuals gallery** (e.g., Sankey, Bullet charts, Gantt, KPI indicators).
* Or build your own **custom visualizations** using:

  * TypeScript / JavaScript.
  * D3.js or other visualization libraries.
  * The Power BI visuals SDK.

For most learners, you will **consume** existing custom visuals rather than build them, but it’s useful to know that organizations can create **company-specific visuals** that integrate seamlessly into Power BI.

### 🔰 10.2 Power BI REST API

The **REST API** allows developers to: 

* Programmatically **create and manage** dashboards, reports, and datasets.
* **Refresh** datasets.
* **Push data** directly into Power BI from external systems (IoT devices, web services, back-end jobs).

You might not write this code yourself, but you may use dashboards that rely on it (e.g., real-time operations boards).

### 🔰 10.3 Pushing real-time data to dashboards

Using the REST API or streaming features, you can create **streaming / push datasets**: 

* As new data arrives (e.g., sensor readings, website events), it’s pushed to Power BI.
* Tiles on the dashboard update almost instantly.
* Ideal for **monitoring**, **operations**, and **IoT** scenarios.

### 🔰 10.4 Embedding Power BI in applications

Finally, **Power BI Embedded** lets you bring Power BI visuals into your own apps or portals. 

* ISVs and internal dev teams can embed reports inside custom web or mobile apps.
* Users may not even know they’re looking at Power BI – it feels native to the app.
* Authentication and authorization are handled via embed tokens and Azure AD.

---

## ☑️ 11. What you, as a learner, should practice from Chapter 8?

To really internalize Chapter 8, you could:

1. **Experiment with Import vs DirectQuery**

   * Take any SQL database (or a sample like AdventureWorks).
   * Connect once with **Import** and once with **DirectQuery**.
   * Observe differences in available modeling features and performance.

2. **Simulate on-prem + gateway**

   * Use a local SQL Server / SQL Express.
   * Connect from Power BI Desktop, publish, and try setting up a **Personal Gateway**.
   * Schedule a refresh and watch it succeed/fail → read the logs.

3. **Play with Excel + Power BI**

   * Build a small Excel Power Pivot data model and **Publish to Power BI**.
   * Then, in Excel, **Analyze in Excel** a dataset from Power BI.

4. **Create a simple RLS example**

   * Model a table with a [Region] column.
   * Create roles for different regions using DAX filters.
   * Use “View as role” to see what each role sees.

5. **Try a custom visual and a streaming dataset**

   * Install a custom visual from the marketplace and use it in a report.
   * Create a small **streaming dataset**, push sample rows via UI or script, and pin a live tile.

---

# ✅ Question & Answer on Chapter 8 (Using Microsoft Power BI in your company):

---

### 🔴 1. What are the main types of data sources Power BI typically connects to in a company?

**✴️ Answer:**
Power BI commonly connects to:

* **Cloud databases** (e.g., Azure SQL Database, Azure SQL Data Warehouse)
* **On-premises relational databases** (SQL Server, Oracle, DB2, MySQL, PostgreSQL, etc.)
* **Semantic models** (SQL Server Analysis Services, SAP HANA, SAP BW)
* **Online services** (like those covered in earlier chapters, e.g., Google Analytics) 

Gateways are used when those sources are **on-premises** and the Power BI service is in the cloud.

---

### 🔴 2. What is the difference between the Personal Gateway and the Enterprise Gateway?

**✴️ Answer:**

* **Personal Gateway**

  * Installed on a **user’s own machine**
  * Intended for **personal or small-scale** refresh scenarios
  * Works only when that machine is **powered on and connected**

* **Enterprise Gateway**

  * Installed on a **server**, managed by IT
  * Can serve **many users and datasets**
  * Designed for **always-on** production use, with better governance and performance 

---

### 🔴 3. In Power BI, what is the difference between Import mode and DirectQuery / Live connection?

**✴️ Answer:**

* **Import mode**

  * Copies data **into** the Power BI dataset.
  * Requires **refresh** (manual or scheduled) to stay up to date.
  * Allows rich **data modeling** and combining multiple sources.

* **DirectQuery / Live connection**

  * Keeps data in the **source system**; queries it at run time.
  * No data copy inside the dataset (or minimal cache).
  * Great for **near real-time** scenarios, but usually limited to **one source** per model and depends heavily on source performance.

---

### 🔴 4. Why is a gateway needed for on-premises databases but not for Azure SQL Database?

**✴️ Answer:**
For **on-premises** databases, your data is inside the company network and **not directly reachable** from the Power BI cloud. A **gateway** acts as a secure bridge: Power BI sends refresh/DirectQuery requests to the gateway, and the gateway connects to the on-prem database.

For **Azure SQL Database** (and other Azure PaaS databases), both Power BI and the database live in the **cloud**, so Power BI can connect directly over the internet using standard connectors **without** an on-premises gateway.

---

### 🔴 5. What is a live connection to SQL Server Analysis Services (SSAS), and when is it useful?

**✴️ Answer:**
A **live connection** to SSAS means:

* Power BI **does not import** data.
* All queries and calculations are executed directly on the **SSAS model** (tabular or multidimensional).
* Measures, hierarchies, KPIs, and security are defined and managed in **Analysis Services** by IT. 

It’s useful when your organization already has **well-designed corporate models** and you want to use Power BI primarily for **visualization and reporting** rather than modeling.

---

### 🔴 6. How can Excel and Power BI work together in both directions?

**✴️ Answer:**

1. **Excel → Power BI**

   * You can publish an Excel workbook that contains a **Power Pivot data model** to Power BI.
   * Power BI treats it as a **dataset**, and you can build dashboards/reports on top of it.

2. **Power BI → Excel**

   * From Excel, you can connect to a **Power BI dataset** and build **PivotTables and PivotCharts** against it, just like connecting to a cube. 

This lets users choose the front end they prefer (Excel or Power BI) while sharing the **same underlying model**.

---

### 🔴 7. What is Row-Level Security (RLS) in Power BI?

**✴️ Answer:**
**Row-Level Security (RLS)** lets you restrict which **rows** of a table each user can see:

* You define **roles** (e.g., “US Sales Manager”).
* For each role, you specify a **DAX filter** on tables (for example, `[Country] = "United States"` or `[SalespersonEmail] = USERPRINCIPALNAME()`).
* When a user views reports, Power BI applies the filters for their role, so they only see the data they’re allowed to see. 

This is critical for safely sharing a single dataset with many users who should see **different slices** of the data.

---

### 🔴 8. What parts of security do you need to think about when sharing reports in a company?

**✴️ Answer:**
You need to consider multiple layers:

* **Workspace / app permissions** – who can view or edit dashboards, reports, and datasets.
* **Data source credentials** – how the dataset logs in to the database or service.
* **Gateway permissions** – which users are allowed to use a particular gateway for refresh/DirectQuery.
* **Row-Level Security (RLS)** – ensuring each user only sees rows they’re entitled to. 

Ignoring any of these can lead to people seeing **too much** or being unable to see what they need.

---

### 🔴 9. What can developers do with the Power BI REST API?

**✴️ Answer:**
With the **Power BI REST API**, developers can:

* Programmatically **create and manage** dashboards, reports, and datasets.
* **Refresh** datasets on demand.
* **Push data** into streaming or push datasets from external apps or services (IoT devices, web back-ends, job schedulers). 

This is what enables scenarios like **real-time dashboards** and automated deployment of BI artifacts.

---

### 🔴 10. What does it mean to embed Power BI in an application, and why would a company do that?

**✴️ Answer:**
Embedding Power BI means integrating **Power BI reports and dashboards directly inside another application** (web portal, SaaS product, internal line-of-business app), using **Power BI Embedded** and the REST APIs. 

Companies do this to:

* Give users rich, interactive analytics **without leaving the app** they already use.
* Control branding and user experience.
* Deliver analytics to external customers as part of a product, rather than sending them to the Power BI portal.

---
