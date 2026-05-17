# ✅ Chapter 3 – Understanding data refresh

---

## ☑️ 1. The problem: great reports, stale data

We’re still following **David**, the budgeting manager at Contoso.

* In earlier chapters, he:

  * Pulled sales data from **SQL Server** into **Excel**.
  * Uploaded the Excel file to **OneDrive**.
  * Built dashboards and reports in **Power BI** on top of that Excel file.
* Time passes. The Excel file contains data only up to **October 2015**, but now it’s **mid-December**.

  * For products with strong **seasonality**, October numbers are no longer a good basis for forecasting.
  * Country/region managers keep asking for updated figures.

So David ends up doing this **every morning**:

1. Connect to SQL Server.
2. Export the latest sales data into Excel.
3. Update the Excel model.
4. Save/upload the file again, so Power BI can pick it up.

This daily manual process works, but it’s annoying, error-prone, and clearly something that should be **automated**.

> Core idea:
> The **insights** in your dashboards are only as good as the **freshness of your data**. A beautiful dashboard with stale data is dangerous.

---

## ☑️ 2. The current data flow (and why it’s not ideal)

The book explains the current pipeline roughly like this:

1. **SQL Server (original data source)**
2. Exported to **Excel** on David’s machine.
3. Excel file saved to **OneDrive**.
4. **Power BI Service** reads the Excel file and loads it into an internal **Analysis Services (SSAS) model** in the cloud.
5. **Dashboards and reports** are built on that model.

This works, but:

* Power BI only sees **what is in the Excel file**.
* It has **no idea** how to go back to SQL Server by itself.
* If SQL Server gets new rows, nothing happens until:

  * Excel is manually refreshed, and
  * The updated file is saved where Power BI can read it.

This is not “data refresh” in the full sense; it’s **file refresh** driven by a human.

---

## ☑️ 3. What “data refresh” really means in Power BI

Chapter 3 differentiates between two ideas:

1. **Refreshing an Excel file**

   * You refresh data inside Excel (for example, with a connection to SQL Server).
   * Then you upload/save that file to OneDrive.
   * Power BI re-reads the file and updates the dataset.
   * This still relies on **you** to refresh the file.

2. **Refreshing a Power BI dataset from the original data source**

   * Power BI itself reaches back to the **source system** (like SQL Server).
   * It re-runs queries defined in the dataset.
   * It updates the SSAS model behind your reports and dashboards.
   * This is what the book calls **data refresh**, and it can be scheduled and automated.

David wants the second approach:

> “I want Power BI to go back to SQL Server automatically, get the latest sales data, and update my reports without me exporting Excel every morning.”

To make this possible, two big things must happen:

1. Power BI must **know how to query** the original data source (SQL Server).
2. Power BI must **be able to connect** securely from the cloud to that data source (which is usually on-premises).

---

## ☑️ 4. Two requirements for a proper refresh setup

The book states two key requirements for automatic refresh:

### 🔰 Requirement 1 – The dataset can’t just be a plain Excel table

If you import a simple Excel table into Power BI:

* The Power BI dataset only “knows” about the columns and rows in the table.
* It does **not** know:

  * That this data originally came from SQL Server.
  * What query was used.
  * Which server/database/table to connect to.

For automatic data refresh, Power BI needs a **formal query definition** in a language it understands (Power Query “M”, or similar). That query has to be stored in the dataset, not just hidden inside an Excel file.

### 🔰 Requirement 2 – The Power BI service must be able to reach the data source

The SSAS engine in Power BI’s cloud environment needs a way to access the original data source:

* Often, this source (e.g., SQL Server) is:

  * On a **company network**,
  * Or even just on someone’s **laptop**, behind a firewall.

Power BI can’t just “magically” open a connection into your network. You need some type of **gateway software** that:

* Lives in your network,
* Knows how to talk to the data source, and
* Securely responds to refresh requests from Power BI.

These two requirements lead directly to:

* Using **Power BI Desktop** to build the model (with proper connection info).
* Using a **Gateway** (Personal Gateway in this chapter) to bridge between the cloud and on-premises data.

---

## ☑️ 5. Introducing Power BI Desktop (and why it matters for refresh)

Power BI Desktop is a **Windows application** you install on your PC. In this chapter it plays a crucial role:

* It can connect directly to data sources like **SQL Server**.
* It can shape and transform data using the **Query Editor**.
* It can build a **data model** (tables, relationships, measures).
* It can create **reports** on top of that model.
* It then **publishes** everything as a dataset + report into the Power BI Service.

For data refresh:

* Power BI Desktop **remembers** the connection details and query definitions.
* When you publish from Desktop:

  * The dataset in the Power BI Service includes the **queries and connection settings**.
  * That means the service knows:

    * Where the data came from,
    * How to re-query it.

So, David’s new plan:

1. Use **Power BI Desktop** to build a model directly from **SQL Server**.
2. Publish that model to Power BI.
3. Configure refresh in the Power BI Service, so that the cloud model re-queries SQL Server when needed.

Now we have the foundation for **true automatic data refresh**.

---

## ☑️ 6. Building and publishing a refreshable model (the “Sales PBD” example)

The chapter walks through an example where David:

1. **Installs Power BI Desktop** from the Power BI website.
2. **Connects to SQL Server**:

   * Chooses the server and database.
   * Picks the relevant tables/views.
3. Uses **Query Editor** to:

   * Clean and shape data (filter rows, rename columns, etc.).
   * Load them into the model.
4. **Designs a report** in Desktop:

   * Adds visuals similar to those he built earlier from Excel.
5. Saves the file, for example as `Sales PBD.pbix`.
6. Clicks **Publish** (on the Home tab in Desktop).

   * Signs in to Power BI Service if needed.
   * Desktop uploads both:

     * The **model (dataset)**, and
     * The **report**.
   * In the online workspace, he now sees a dataset and a report called **Sales PBD**.

Important detail:

* The dataset in the service is now its **own copy**.
* If you change the model in the browser (where allowed) or re-publish from Desktop, you must be mindful of which version is “source of truth”.
* But the key win is: the online dataset now **knows how to talk to SQL Server**.

---

## ☑️ 7. Configuring data refresh in the Power BI Service

Once the dataset is published to Power BI:

1. David goes to the Power BI **workspace**.
2. Finds the **Sales PBD** dataset.
3. Opens its **Settings**.
4. Goes to the **Schedule Refresh** section.

Here he can:

* Configure **credentials** for each data source (e.g., SQL Server).
* Decide how often to refresh:

  * **Refresh now** for a one-off refresh.
  * **Scheduled refresh** (e.g., once or several times per day, depending on license).

At this point, if the data source is **cloud-based** (like Azure SQL Database), Power BI can often connect directly from the service using the credentials you provide.

However, if the data source is **on-premises** (like an internal SQL Server), the Power BI service can’t reach it directly. That’s where the **Personal Gateway** comes in.

---

## ☑️ 8. Personal Gateway – the bridge between Power BI and on-premises data

The **Personal Gateway** is a small piece of software you install on a machine inside your network (often your own PC) that:

* Runs as a **Windows service** (so it can work even when you’re not actively using the PC, as long as it’s on and connected).
* Connects **outbound** to the Power BI Service (so you usually don’t need to open inbound firewall ports).
* Receives data refresh requests from the service.
* Executes the queries defined in your dataset against the on-premises data source.
* Sends the results back up to Power BI.

In short, it’s the “messenger” between **cloud Power BI** and your **internal data sources**.

The setup process is roughly:

1. Download the **Personal Gateway** installer from the Power BI website.
2. Install it on a PC that:

   * Has network access to the data sources (e.g., SQL Server).
   * Is usually turned on when refresh is scheduled.
3. During setup:

   * Sign in with your **Power BI account**, so the gateway is linked to your tenant.
   * Define which user credentials it should use to talk to the data source.

After that:

* When Power BI runs a scheduled refresh for your dataset:

  * It sends a job to the gateway.
  * The gateway runs the queries locally against SQL Server.
  * The data is pushed back to the dataset in the cloud.

> Practical consequence:
> Once this is set up, David no longer needs to export Excel every morning. Power BI automatically refreshes from SQL Server through the gateway.

---

## ☑️ 9. Refresh frequency and licensing

Chapter 3 also touches on **how often** you can refresh:

* With a **free** Power BI license:

  * You can typically set refresh up to **once per day**.
* With a **Pro / professional** license:

  * You can schedule refresh **multiple times per day** (the exact limit may change over time, but conceptually, Pro allows more frequent refresh).

So, your ability to keep dashboards very close to real time depends partly on your **license** and **architecture**.

---

## ☑️ 10. What data refresh does *not* do

It’s also important to understand some **limits** of data refresh:

* Refresh **does**:

  * Re-run your queries.
  * Pull in **new / changed rows** and update the dataset.
* Refresh **does not**:

  * Automatically change the **structure** of your model.
  * Add new columns if you add them directly in the source without updating the model.
  * Fix broken queries—if you rename or delete a table/column in SQL Server, refresh can fail.

If you change the **schema** (fields, relationships, logic), you typically need to:

* Update the model in **Power BI Desktop**.
* Re-publish it to the service.

Refresh is about keeping the **data values** up to date, not redesigning your model.

---

## ☑️ 11. Chapter 3 – what you, as a learner, should walk away with

By the end of Chapter 3, you should have a clear mental model of how data flows and refresh works in Power BI. You should understand:

1. **Why** manual Excel-based updates don’t scale for serious reporting.
2. The difference between:

   * Refreshing an **Excel file** that Power BI reads.
   * Refreshing a **Power BI dataset** from the original data source.
3. That for true automatic refresh, your dataset must:

   * Have proper **connection and query information** (best created with Power BI Desktop).
   * Have a **secure path** to the data source (often via a gateway for on-premises data).
4. The basic use of **Power BI Desktop**:

   * Connect to SQL Server or other sources.
   * Shape data.
   * Build a model + report.
   * Publish to Power BI.
5. How to configure **scheduled refresh** and why it may require:

   * A **Personal Gateway**, and
   * The right **license** for multiple refreshes per day.

---

# ✅ Question & Answer on Chapter 3 (Understanding data refresh):

---

### 🔴 Q1. Why is “data refresh” important in Power BI?

**✴️ Answer:**
Because your reports and dashboards are only as useful as the **freshness of the data** behind them. If the underlying data (like sales or budget numbers) changes in the source system but your Power BI dataset isn’t refreshed, you’ll be making decisions based on **outdated information**, even if the visuals look perfect.

---

### 🔴 Q2. What is the difference between refreshing an Excel file and refreshing a Power BI dataset?

**✴️ Answer:**

* **Refreshing an Excel file** means updating the data inside Excel (for example, by re-running a query in Excel or copy-pasting latest numbers) and then saving/uploading the file again. Power BI just reads the new file.
* **Refreshing a Power BI dataset** means Power BI itself connects back to the **original data source** (like SQL Server) and re-runs the queries defined in the dataset, automatically updating the model without you manually touching Excel.

---

### 🔴 Q3. Why can’t a simple imported Excel table support true automatic data refresh from the original source?

**✴️ Answer:**
A plain imported Excel table only contains **static values**. The Power BI dataset doesn’t know:

* Which server or database the data originally came from
* What query was used to pull that data

Since the connection details and query logic are missing, Power BI has no instructions for how to go back to the source system on its own. That’s why it can’t perform a true automatic refresh directly from the original source in this case.

---

### 🔴 Q4. How does Power BI Desktop help enable automatic data refresh?

**✴️ Answer:**
Power BI Desktop:

1. Connects directly to data sources (like SQL Server, Excel, etc.).
2. Stores **connection details and queries** (using Power Query and the data model).
3. Packages that model and queries into a **PBIX** file.
4. When you publish from Desktop to the Power BI service, the **dataset in the service includes those queries and connection settings**.

Because of this, the Power BI service knows **how and where** to re-query the data, making scheduled refresh possible.

---

### 🔴 Q5. What is the basic process to create a refreshable dataset using Power BI Desktop?

**✴️ Answer:**
A typical process is:

1. Open **Power BI Desktop**.
2. **Get Data** from the source (e.g., SQL Server), and load or transform it using Query Editor.
3. Build your **data model** (tables, relationships, measures) and design a report.
4. Click **Publish** to send the dataset and report to the Power BI service.
5. In the Power BI service, configure **refresh settings** (credentials, schedule, gateway if needed).

After that, the online dataset can be refreshed automatically according to the schedule.

---

### 🔴 Q6. Why is a gateway needed when your data source is on-premises?

**✴️ Answer:**
When your data source is on-premises (inside your company network or even on your laptop), the cloud-based Power BI service **cannot directly reach it** because of firewalls and network boundaries. A **gateway**:

* Runs inside your network (e.g., on your PC or a server).
* Maintains a secure **outbound** connection to Power BI.
* Receives refresh requests from the service, queries the local data source, and sends the updated data back.

Without a gateway, the service has no secure path to on-premises data for automatic refresh.

---

### 🔴 Q7. What is the Personal Gateway and how does it work in simple terms?

**✴️ Answer:**
The **Personal Gateway** is a lightweight gateway you install on a single machine (often your own PC). In simple terms:

* It connects to Power BI using your account.
* It knows how to connect to your local data sources (like SQL Server).
* When a scheduled refresh runs, Power BI sends a request to your gateway.
* The gateway fetches the latest data from the local source and returns it to the Power BI dataset in the cloud.

It’s ideal for individual use or small setups where one person owns the refresh process.

---

### 🔴 Q8. How does licensing affect how often a dataset can be refreshed?

**✴️ Answer:**
Refresh **frequency** depends on your Power BI license:

* With a **Free** license, you usually get **limited refreshes per day** (often once).
* With a **Pro** or higher license, you can schedule **multiple refreshes per day** (several times, depending on current limits).

So if you need near-real-time or very frequent updates, you typically need a **Pro (or better)** license along with the correct architecture.

---

### 🔴 Q9. What kind of changes can break a scheduled refresh, even if it was working before?

**✴️ Answer:**
Refresh can fail if something in the setup changes, for example:

* The **data source schema** changes (tables/columns renamed or deleted).
* The **server/database** name changes or moves.
* The **credentials** used by the gateway or the dataset expire or are revoked.
* The PC or server running the **gateway** is turned off or disconnected from the network.

In these cases, Power BI might show refresh errors until you fix the underlying issue.

---

### 🔴 Q10. After learning Chapter 3, what should you be able to explain in your own words?

**✴️ Answer:**
By the end of Chapter 3, you should be able to explain:

* Why manual Excel exports are **not scalable** for keeping dashboards up to date.
* The difference between **file refresh** (refreshing Excel) and **dataset refresh** (Power BI re-querying the source).
* How **Power BI Desktop** and **PBIX publishing** enable the service to know how to refresh data.
* Why an **on-premises gateway** (like Personal Gateway) is needed for internal data sources.
* How to configure **scheduled refresh** in the service and why licensing and gateway health matter.

---
