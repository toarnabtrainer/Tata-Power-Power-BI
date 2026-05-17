# ✅ Chapter 5 – Getting data from services and content packs

---

## ☑️ 1. What Chapter 5 is really about?

Up to Chapter 4, you mainly worked with:

* Files (like Excel)
* Databases (like SQL Server)

Chapter 5 adds a new dimension:

> **Getting data directly from online services** (e.g., Google Analytics) and using **content packs** to jump-start dashboards and reports.

It shows:

* How **service content packs** work (prebuilt dashboards/reports for SaaS services like Google Analytics). 
* Why those content packs are useful, but also limited. 
* How to build your own **custom dataset** from the same service using **Power BI Desktop connectors**.
* How **organizational content packs** let people in your company share standard dashboards and reports.

David’s goal in this chapter: combine **website data from Google Analytics** with his **sales and budget data** to improve the budget process.

---

## ☑️ 2. The scenario: why David cares about services

David realizes that understanding **website visitors** could help explain and predict **sales**:

* If visits increase in a country/region, sales might also grow there.
* He wants to look at at least **two years** of visitor data (e.g., 2014 and 2015) to see trends by country/region. 

He knows his company site is tracked by **Google Analytics** and wonders:

> “Can Power BI pull data from Google Analytics?”

Reading the docs, he finds that Google Analytics is available both as:

* A **service content pack** in the Power BI service, and
* A **connector** in Power BI Desktop. 

The chapter walks through both approaches.

---

## ☑️ 3. First approach: using a service content pack

### 🔰 3.1 Getting data from a service via the Power BI service

In the Power BI service, David goes to **Get Data** (the button in the lower-left corner). This shows the **Get Data page** with several options, including a section for **Content Pack Library → Services**.

He clicks **Get** under Services and sees a list of supported services such as Google Analytics.

When you load data from a service:

* Power BI automatically creates:

  * A **dataset**
  * One or more **reports**
  * One or more **dashboards**
* All of these are **preconfigured templates** for that service. 
* Power BI uses **your credentials** to access the service, so the numbers reflect **your own data**, even though the structure of the report/dashboard is shared. 

### 🔰 3.2 What is a content pack?

The book defines a **content pack** as a bundle of:

* **Dataset(s)** – one or more datasets that supply data to the reports and dashboards in the pack. 
* **Report(s)** – each report uses a dataset in the same pack. 
* **Dashboard(s)** – each dashboard shows visuals from the reports in the same pack. 

These three parts are linked: dashboards depend on reports, reports depend on datasets.

Content packs are **only available in the Power BI service**, not in Desktop. 

### 🔰 3.3 Connecting to Google Analytics via content pack

David selects the **Google Analytics** tile and sees an explanation of what the pack offers. 

He clicks **Connect**, and Power BI asks him to choose an **authentication method**—for Google Analytics, that’s **OAuth2**.

The steps are:

1. Click **Connect** on the content pack tile. 
2. Choose **OAuth2** as auth method and click **Sign in**. 
3. Sign in to Google (if not already signed in). 
4. Allow **offline access** so Power BI can retrieve data on your behalf.

After that, Power BI imports a dataset, report, and dashboard with predefined visuals for Google Analytics.

---

## ☑️ 4. Limitations of service content packs

When David explores the dataset created by the Google Analytics content pack, he notices something important:

* The number of **tables, attributes, and measures** exposed in the Power BI dataset is only a **small subset** of what’s available in Google Analytics itself.

Why so limited?

* The pack’s authors chose the **minimum set of fields** needed to support their template dashboards and reports. 
* This keeps the resulting dataset **small and fast**, improving performance. 

But for David, this is a problem:

1. The content pack only includes about **180 days of data**, which is not enough for his **two-year** trend analysis. 
2. He wants **one combined dataset** that includes:

   * Google Analytics metrics,
   * Past sales, and
   * Forecast/budget data.
     The content pack’s dataset can’t be customized to add those extra connections. 
3. You **cannot modify** the data model of a dataset that comes from a content pack. You *can* change reports and dashboards, but not the underlying dataset. 

So the conclusion:

> Service content packs are **great for a quick overview**, but **not enough** when you need deep history, custom relationships, or combined models.

---

## ☑️ 5. Second approach: creating a custom dataset from a service (Power BI Desktop)

To overcome those limits, David decides to **create his own dataset** using the **Google Analytics connector in Power BI Desktop**.

### 🔰 5.1 Starting from the existing model

He starts from the model built in Chapter 4, which already has:

* A table containing **sales and budget** in different columns (the “Sales + Budget” table). 

Now he wants to add **Google Analytics** data into this same model.

### 🔰 5.2 Using the Google Analytics connector in Desktop

In Power BI Desktop:

1. He clicks **Home → Get Data** and chooses **Google Analytics**.
2. Desktop prompts for **Google credentials**, similar to the online experience.
3. Once connected, Desktop exposes a **navigator-like view** of the Google Analytics “cube” (semantic model).

In **Query Editor**, this source behaves like a cube, so the ribbon shows a special tab **Cube Tools | Manage**. 

There’s also an **Add Items** button:

* **Attributes** (dimensions, like Country, Region, Device Category, etc.)
* **Measures** (numeric values, like Sessions, New Users, Page Views)

You pick the attributes and measures you need; Power BI Desktop then builds a query that returns those specific combinations.

> Important note from the book:
> If you want to change the **granularity** (e.g., remove an attribute so data is aggregated at a different level), you should use **Collapse Columns** instead of just deleting a column in Query Editor. Removing a column alone doesn’t change the underlying query’s cardinality; the cube still processes data at the original granularity.

### 🔰 5.3 Filtering and shaping the Google Analytics data

David only cares about **China, Germany, and the United States**—the countries/regions involved in the budget process. 

In Query Editor, he:

* Applies a filter on the **Country/Region** column to keep just those three. 

After confirming his selection:

* Power BI Desktop sends the query to Google Analytics.
* The result is loaded into a **new table** named something like `Website`, representing the website data he requested.

For this scenario, the book notes that David **does not** create relationships between `Website` and the other table, but in real life it’s common to create relationships (for example, via Date or Country).

### 🔰 5.4 Why a custom dataset is more powerful

By creating his own dataset with the service connector, David now has:

* Full control over:

  * Historical depth (he can request more than 180 days),
  * Which fields to bring in,
  * How to relate them to sales and budget tables.
* The ability to add:

  * New measures,
  * Calculations,
  * Additional tables,
  * Custom relationships.

This gives him the **flexibility** that the read-only content pack could not provide. 

---

## ☑️ 6. Organizational content packs (inside your company)

The chapter then shifts to **organizational content packs**, which are different from service content packs:

* A **service content pack** is provided by Microsoft to connect to an external service like Google Analytics. 
* An **organizational content pack** is created by **someone in your company** and shared with others in the same organization. 

### 🔰 6.1 What’s inside an organizational content pack?

Just like service packs, they can contain:

* Datasets
* Reports
* Dashboards 

They typically represent a **standard set of analytics** for a specific domain: e.g., “Sales KPIs,” “HR Headcount,” “Finance overview.”

Key points:

* Any user with permission can **publish** an organizational content pack. 
* Consumers can:

  * Use the pack in **read-only mode**, or
  * Create a **personal copy** that they can modify. 

The **connections to data sources** (server, database, credentials) can be changed **only by the publisher** of the content pack, not by consumers. 

### 🔰 6.2 How updates behave

If the publisher updates the organizational content pack:

* Users who consume it in **read-only mode** automatically see the updates (new visuals, new measures, etc.). 
* Users who created a **personal copy** do **not** get automatic changes; their copy is independent.

This is similar to how templates work in many tools: the “master” is updated, but your personal copy stays as you edited it.

### 🔰 6.3 Re-importing and naming best practice

If you import the same content pack again later:

* Power BI brings in **another copy** of all objects with the **same names** as before. 
* To avoid confusion, it’s a good idea to **rename dashboards, reports, and datasets** in your personal copy (e.g., add “– My Version” to the name). 

---

## ☑️ 7. Chapter 5 conclusions – big-picture takeaways

The chapter wraps up by summarizing the most important ideas:

* A **content pack** bundles **datasets, reports, and dashboards** that users can import quickly into their workspace.
* You can **customize dashboards and reports** imported from a content pack, but **not the underlying dataset**.
* **Content packs exist only in the Power BI service**, not in Desktop.
* A **service content pack** (like Google Analytics) is published by Microsoft to connect to external services; you supply your credentials.
* If the service content pack doesn’t give you the dataset you need, you can often **create a custom dataset** in Power BI Desktop using a corresponding connector (if available).
* **Organizational content packs** are created by users inside your company:

  * Consumers can view them read-only or create a personal copy.
  * Only the publisher can modify the data connections.
  * Updates from the publisher flow automatically to read-only consumers.
* Content packs are a handy way to **standardize** analytics across an organization or to **quickly explore** external service data.

---

## ☑️ 8. How to practice as a learner

To internalize Chapter 5, you can:

1. In the **Power BI service**, go to **Get Data → Services**, pick a sample service (or a demo one if you don’t have real accounts), and see what content pack it offers.
2. Explore the **dataset, reports, and dashboard** that come with the content pack:

   * Identify what fields/measures are included.
   * Think about what’s *missing* for your own scenarios.
3. In **Power BI Desktop**, use a **service connector** (like Google Analytics, if you have it) to:

   * Select your own attributes and measures.
   * Bring this data into a model that also includes **sales/budget data** from another source.
4. If you’re in an organization with Power BI:

   * Try consuming an **organizational content pack**.
   * Then make a **personal copy** and tweak the reports (e.g., add your own visuals, filters, or measures).

Do that sequence once or twice and Chapter 5 will feel very concrete: you’ll know when to lean on **prebuilt packs**, and when to build your **own custom dataset** for serious analysis.

---

# ✅ Question & Answer on Chapter 5 (Getting data from services and content packs):

---

### 🔴 Q1. What is a content pack in Power BI?

**✴️ Answer:**
A **content pack** is a bundle that typically includes:

* One or more **datasets**
* One or more **reports**
* One or more **dashboards**

It’s a ready-made package you can import into your workspace so you don’t have to build everything from scratch. Content packs can come from online services (like Google Analytics) or from inside your organization.

---

### 🔴 Q2. What is the difference between a service content pack and an organizational content pack?

**✴️ Answer:**

* A **service content pack**:

  * Is provided by Microsoft for a specific **online service** (e.g., Google Analytics, Salesforce).
  * Connects to that service using **your credentials**.
  * Gives you prebuilt dashboards and reports for that service.

* An **organizational content pack**:

  * Is created and published by **someone in your company**.
  * Contains standard datasets/reports/dashboards for internal use (e.g., “Sales KPIs” for all sales managers).
  * Is only visible to users in the same organization/tenant.

---

### 🔴 Q3. What happens when you connect to a service like Google Analytics using the content pack in the Power BI service?

**✴️ Answer:**
When you connect to a service via its **content pack**:

1. Power BI asks you to sign in (often using OAuth).
2. It imports a **prebuilt dataset**, plus one or more **reports** and **dashboards** into your workspace.
3. The visuals are templates, but the **data is yours**, based on your Google Analytics (or other service) account.

You can then use or customize those dashboards and reports as a starting point.

---

### 🔴 Q4. Why might a service content pack not be enough for advanced analysis?

**✴️ Answer:**
Service content packs are designed as **quick-start templates**, so they usually:

* Expose only a **limited subset** of fields and measures from the service.
* Sometimes include only **recent history** (e.g., ~180 days instead of multiple years).
* Use a fixed data model that you **cannot modify** (you can edit reports and dashboards, but not the dataset structure).

If you need long-term history, more fields, or want to **combine service data with your own sales/budget data**, a content pack alone may not be sufficient.

---

### 🔴 Q5. How can Power BI Desktop help when the service content pack is too limited?

**✴️ Answer:**
When a service content pack is too limited, you can:

1. Open **Power BI Desktop**.
2. Use the service’s **connector** (e.g., Google Analytics connector).
3. Choose **exactly which attributes and measures** you want from the service.
4. Combine that data with other sources (SQL Server, Excel, etc.) in a single model.
5. Publish this **custom dataset and report** to the Power BI service.

This gives you full control over the **schema, relationships, and calculations**.

---

### 🔴 Q6. What are the main components you typically see when you import a content pack into your workspace?

**✴️ Answer:**
When you import a content pack, you usually see:

* A **dataset** – the underlying data model.
* One or more **reports** – with multiple pages of visuals.
* One or more **dashboards** – with key visuals pinned as tiles.

All three are interconnected: dashboards use visuals from reports, and reports are powered by the dataset.

---

### 🔴 Q7. As a consumer of an organizational content pack, what can you customize and what can you *not* change?

**✴️ Answer:**

As a **consumer**:

* You **can**:

  * Customize dashboards (rearrange, add new tiles).
  * Customize reports (add visuals, change layouts).
  * Create your own **personal copy** of the content pack objects and modify them freely.

* You **cannot**:

  * Change the **underlying dataset’s data source connections** (server, database, credentials).
  * Change the original content pack for everyone else; only the **publisher** can do that.

---

### 🔴 Q8. What happens if the publisher updates an organizational content pack after you’ve imported it?

**✴️ Answer:**

* If you are using the content pack in **read-only mode** (no personal copy), you will typically **see the updates automatically** in your dashboards and reports.
* If you created a **personal copy** (renamed or modified the imported items), your copy is **independent** and does **not** receive updates from the publisher. You keep your customized version as-is.

---

### 🔴 Q9. Why is it recommended to rename dashboards/reports after importing a content pack for personal use?

**✴️ Answer:**
If you import the same content pack more than once, Power BI will create **multiple objects with the same names** (dashboard, report, dataset). This can get confusing. Renaming your personal copies (e.g., adding “– My Version” or “– Custom”) helps you:

* Distinguish between the **original pack** and your **personal customizations**.
* Avoid mixing up which report/dashboard you’re editing or sharing.

---

### 🔴 Q10. In David’s scenario, why did he eventually prefer a custom dataset from Google Analytics over the service content pack?

**✴️ Answer:**
David preferred a **custom dataset** because:

* He needed **more than 180 days** of website data to analyze trends over two years.
* He wanted to **combine** Google Analytics data with **sales and budget data** in the same model.
* The service content pack’s dataset was **fixed** and limited; he couldn’t change its structure or add new relationships.

By using the **Google Analytics connector in Power BI Desktop**, he could choose his own fields, get the required history, combine it with other tables, and build a unified model tailored to his budgeting scenario.

---
