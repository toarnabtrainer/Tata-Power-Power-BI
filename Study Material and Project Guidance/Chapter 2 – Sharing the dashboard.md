# ✅ Chapter 2 – Sharing the dashboard

---

## ☑️ 1. Why sharing matters?

The story still follows **David**, the budgeting manager at Contoso. He has built a sales dashboard, but that’s only the *starting point* of the real work: collaborating with country/region managers, collecting their feedback and numbers, and aligning on a final budget. To do that, he needs to share his dashboards and reports with colleagues in a controlled, secure way. 

In this chapter you learn:

* How to share a dashboard with **individual users**
* How to share with **people outside your organization**
* How to collaborate using **group workspaces** and **OneDrive for Business**
* How to view dashboards and reports on **mobile devices**

---

## ☑️ 2. Inviting a user to see a dashboard (inside your organization)

### 🔰 2.1 The Share button and the Share Dashboard dialog

To share a dashboard, David opens it in Power BI and clicks the **Share** button in the upper-right corner of the dashboard. 

This opens the **Share Dashboard** dialog, which has two main tabs:

1. **Invite** – where you:

   * Enter one or more **email addresses** of people you want to invite
   * Optionally type a **message** that will be included in the email
   * Choose whether recipients are allowed to **re-share** the dashboard
   * Choose whether Power BI should **send an email notification**

   If you enable email notifications, Power BI automatically includes a **link** to the dashboard in the email. 

2. **Shared With / Access** – where you:

   * See the **list of people** who already have access
   * Obtain the **dashboard URL** to share manually (for example, from your own email account)
   * Manage or remove existing access if you’re the owner 

> Learner tip
> Think of the **Invite** tab as “who should see this?” and the **Access/Shared With** tab as “who *already* sees this and what link can I give them?”

### 🔰 2.2 Sharing via link vs email

There are two common ways to share:

* **Email invitation from Power BI**

  * Recipients get an email with a link.
  * Permissions are granted automatically when you click *Share*.

* **Copy link and share yourself**

  * You clear the “Send email notification” checkbox.
  * You then go to the **Access/Shared With** tab, copy the dashboard URL, and paste it into your own email, chat, or documentation. 

In both cases, access is controlled by the **email addresses** you specified, not by who happens to know the link.

---

## ☑️ 3. Inviting users outside your organization

So far, David shared with Wendy, who works in the *same* company (same email domain, like `@contoso-bi.com`). But what happens when you want to share with someone in another company, such as a vendor or external partner? 

### 🔰 3.1 How Power BI defines “organization”

Power BI defines an “organization” mostly by **email domains**:

* Everyone has a work email, for example `name@contoso-bi.com`.
* All users with the **same domain** are considered part of the *same organization*.
* Power BI does **not** allow generic personal email domains like `gmail.com`, `hotmail.com`, etc. for organizational sharing. 
* If your company uses **Office 365** and **Azure Active Directory**, it may have multiple domains all belonging to the same tenant; in that case, those different domains can still be treated as a single organization in Power BI. 

> Learner tip
> If you’re unsure whether your company has Office 365 / Azure AD, you usually just ask your **IT admin**. 

### 🔰 3.2 Sharing with external users

Even though Power BI is designed mainly for **internal** sharing, you *can* share dashboards with people in other organizations:

* You invite them **by email**, just like internal users.
* When they receive the email, they must **sign in to Power BI using that same email address**.
* If they’ve never used Power BI before, they are prompted to **create a free account** the first time they sign in. 

When you type an external email (a domain different from your tenant’s), Power BI shows a warning message explaining that this is outside your organization, as illustrated in Figure 2-7 in the book. 

### 🔰 3.3 Internal vs external users (summary)

The chapter clearly separates:

* **Internal users**

  * Can be invited by email, or access via URL if they already have permission.
  * If they click a dashboard link without permissions, they can **request access**. 

* **External users**

  * Must be invited **by email**.
  * Must sign in using the same email address they were invited with.
  * May need to create a Power BI account if they don’t have one. 

> Learner tip
> Think: **Inside = flexible access + URL; Outside = always email invite + extra governance.**

### 🔰 3.4 Publishing a report to the web (public sharing)

The chapter also introduces **Publish to web**, which is a very different kind of sharing:

* It works for **reports**, not dashboards.
* You go to the report → **File → Publish to web** → **Create embed code**. 
* Power BI creates:

  * A **public URL**, and
  * **HTML embed code** you can paste into any website. 

⚠ **Important warning**:
Publish to web makes the report **completely public** – *anyone* with the link (or who visits the page where it’s embedded) can view the data. You **cannot** control who sees it. The book strongly recommends using this option **only for data intended for public consumption**, such as content on your public company website. 

---

## ☑️ 4. Creating a group workspace in Power BI

Sharing with individuals is useful, but many teams need a **shared environment** where everyone can collaborate on the same dashboards, reports, and datasets. That’s where **group workspaces** come in.

A **group workspace** (today called “workspace” in the Power BI service) is:

* A place in Power BI where:

  * Multiple people are **members**.
  * All members can see the **same dashboards, reports, and datasets**.
* Backed by an **Office 365 group**, which handles:

  * Membership (who’s in the group)
  * Shared resources such as a group mailbox and a group OneDrive 

Within a group workspace:

* Anything you publish or create is **visible to all group members** by default.
* You can collaborate on the same content without having to share dashboards individually.

> Learner tip
> Use a **group workspace** when a dashboard is “for the team” rather than “for one person”.

---

## ☑️ 5. Turning on sharing with OneDrive for Business

The chapter then explains how Power BI and **OneDrive for Business** can work together to improve collaboration around Excel workbooks and datasets.

### 🔰 5.1 How groups and OneDrive for Business work together

When you use an Office 365 group with Power BI:

* The group gets a **OneDrive for Business** site.
* You can store **Excel workbooks** there that act as the source for Power BI datasets.
* Everyone in the group:

  * Can **edit** the shared workbook in OneDrive for Business.
  * Automatically sees the **updated data** in Power BI after each refresh. 

This means:

* Users can enter or update **budget numbers directly in Excel** on OneDrive for Business.
* Power BI refreshes the dataset and then updates the **reports and dashboards** using that data. 

So you get a nice workflow: **edit numbers in Excel → refresh → new insight in Power BI**.

### 🔰 5.2 Personal OneDrive vs OneDrive for Business

The book also clarifies what happens if you do **not** have OneDrive for Business:

* You can still use the **personal OneDrive** (free) to store Excel files.
* You can share the Excel file with others (even outside your organization) and let them edit it.
* You can import that Excel file into Power BI as a **dataset**, and build reports on top of it.

However:

* You can import these into only your **personal workspace**, not a group workspace.
* Dashboards built on that dataset can be shared **only within your organization**, not with external users.
* You **don’t** get the same automatic “everyone in the group sees everything” behavior that group workspaces with OneDrive for Business offer. 

> Learner tip
>
> * **OneDrive for Business + group** → best option for team-wide collaboration.
> * **Personal OneDrive + personal workspace** → fine for individual work or simple sharing.

---

## ☑️ 6. Viewing reports and dashboards on mobile devices

The final big topic in Chapter 2 is **mobile access**.

### 🔰 6.1 Power BI mobile apps

Power BI provides native apps for:

* **Windows** (Windows Store)
* **iOS** (Apple App Store)
* **Android** (Google Play) 

These apps:

* Let you view **reports and dashboards** on your phone or tablet.
* Often give a better experience than using the browser on a small screen.
* Support features like **annotations** (drawing on visuals) and are updated regularly with new capabilities. 

### 🔰 6.2 Offline viewing scenario

The book gives a realistic scenario:

* David wants to show a dashboard during a meeting, but the meeting room has **poor Internet connectivity**.
* He uses the **Power BI mobile app** (for example, on a Windows 10 tablet) and relies on **offline availability**:

  * Dashboards and reports can be **cached** for offline viewing.
  * He can still see the data as of the **last refresh**, even without an active connection. 

### 🔰 6.3 Different layouts on different devices

The chapter then shows how the same dashboard appears:

* On a **Windows 10 app** – similar layout to the browser view. 
* On an **iPad** – visuals arranged for a tablet-sized screen; you can zoom in to individual tiles. 
* On an **Android smartphone** – visuals stacked vertically, with zoom options for each visualization. 

The idea: Power BI adapts the layout so the dashboard is usable on **any screen size**.

### 🔰 6.4 Excel app on mobile devices

If users are entering data directly into Excel workbooks:

* They can use the **Excel mobile app** on phones or tablets.
* The Excel app is optimized for mobile, so editing cells and formulas is more convenient than using Excel Online in a browser.
* The chapter shows examples of the **Sales** and **Budget** worksheets in the Excel Android app, where users can edit figures on the go. 

This supports the Excel + Power BI + OneDrive for Business workflow end-to-end, even on mobile devices.

---

## ☑️ 7. Chapter 2 conclusions – key takeaways for learners

The chapter ends by summarizing what you can now do with Power BI: 

* Invite people in your **organization** to see a dashboard, with access to connected reports.
* Share dashboards with **external users**, following email-based invitations.
* **Publish** reports to the web for public data, understanding that this is fully open, not secure.
* Use **group workspaces** to share datasets, reports, and dashboards inside your organization.
* Combine **Power BI groups** with **OneDrive for Business** so colleagues can edit shared Excel workbooks and have Power BI update automatically. 
* View dashboards and reports on **mobile devices** using native apps, including offline usage scenarios. 

---

## ☑️ 8. How to use this as a learner

If you’re learning from this chapter, here’s how to **practice**:

1. **Take an existing dashboard** you have (or build a very simple one).
2. Try **sharing it** with:

   * A colleague in your organization (via email + via link).
3. If allowed in your environment, experiment with:

   * **Publish to web** – but with dummy or public data only.
4. Create or join a **workspace** and:

   * Upload an Excel file from **OneDrive for Business**.
   * Ask a teammate to edit the Excel file and watch how Power BI updates.
5. Install the **Power BI mobile app** on your phone or tablet and:

   * Pin a few important dashboards.
   * Turn on offline viewing and try accessing them without internet.

---

# ✅ Question & Answer on Chapter 2 (Sharing the dashboard):

---

### 🔴 Q1. How do you share a dashboard with someone inside your organization?

**✴️ Answer:**
Open the dashboard → click the **Share** button (top-right) → in the **Invite** tab, type the person’s **work email address**, optionally add a message, and click **Share**.
You can choose whether to send an email notification and whether that person is allowed to **re-share** the dashboard with others.

---

### 🔴 Q2. What is the difference between sharing via “email invite” and sharing via “link”?

**✴️ Answer:**

* With **email invite**, Power BI sends an email to the people you specify and **grants them access** at the same time.
* With **link sharing**, you first give them access (through the Invite tab), then you copy the **dashboard URL** from the **Access / Shared with** tab and paste it into your own email or chat.

In both cases, only people who have been granted access can open the dashboard, even if they know the link.

---

### 🔴 Q3. What does “inside your organization” mean in Power BI?

**✴️ Answer:**
In Power BI, “inside your organization” usually means everyone whose work email belongs to the **same company domain** (for example, `@contoso.com`).
If your company uses **Office 365/Azure AD**, it can include several related domains, all treated as part of the same organization. These users are considered **internal** and can be shared with more freely.

---

### 🔴 Q4. Can you share a dashboard with someone outside your organization? What do they need to do?

**✴️ Answer:**
Yes, you can share a dashboard with **external users** (people in another company), if your admin allows it. To do this:

1. You invite them by **email address** from the Share dialog.
2. They receive an email and must sign in to Power BI using **that same email address**.
3. If they don’t have a Power BI account, they’ll be asked to **create a (usually free) account** before they can see the dashboard.

They can’t just “guess” the link; they must be invited and sign in properly.

---

### 🔴 Q5. What is “Publish to web” and when should you *not* use it?

**✴️ Answer:**
**Publish to web** is a feature for **reports** (not dashboards) that creates a **public link and embed code**. Anyone who has that link or visits the website where it’s embedded can see the report **without signing in**.

You should **not** use Publish to web for:

* Confidential or internal company data
* Anything that must be controlled by permissions

Use it only for **public data** that you are happy to share with the entire internet.

---

### 🔴 Q6. What is a group workspace and why would a team use it?

**✴️ Answer:**
A **group workspace** (just called a “workspace” in the current UI) is a shared area in Power BI where multiple people are **members**. Everyone in the workspace can see the same:

* Dashboards
* Reports
* Datasets

Teams use it because:

* They don’t need to share each dashboard one by one.
* Any content created or published in the workspace is automatically visible to all members.
* It is backed by an **Office 365 group**, which can also provide a shared mailbox and a shared OneDrive for Business.

---

### 🔴 Q7. How does OneDrive for Business help with Power BI collaboration?

**✴️ Answer:**
With **OneDrive for Business**:

* You can store shared **Excel workbooks** there (for example, a budget file).
* Everyone in the group can **edit the workbook** using Excel (desktop, web, or mobile).
* Power BI can connect to that workbook as a **dataset** and automatically pick up changes when it refreshes.

This means your team can edit numbers in **one central Excel file**, and Power BI dashboards and reports will stay in sync after refresh.

---

### 🔴 Q8. What is the difference between using OneDrive for Business and personal OneDrive with Power BI?

**✴️ Answer:**

* **OneDrive for Business**

  * Tied to your **organization** and often to an **Office 365 group**.
  * Works very well with **group workspaces**: everyone in the group can access and edit the shared workbook, and Power BI can refresh from it for the whole team.

* **Personal OneDrive** (free Microsoft account)

  * Linked to you as an **individual**, not your organization.
  * You can still store Excel files, import them into your **personal workspace** in Power BI, and share dashboards inside your organization.
  * But it doesn’t integrate as smoothly with group workspaces and organizational governance.

---

### 🔴 Q9. How can you view dashboards and reports on a phone or tablet?

**✴️ Answer:**
You can use the **Power BI mobile app**, available for:

* **Windows**
* **iOS (iPhone/iPad)**
* **Android**

Once you sign in:

* You can view all dashboards and reports you have access to.
* Layouts adapt to the screen size (tiles may be stacked vertically on phones).
* Some content can be viewed **offline**, using cached data from the last time it was synced.

This lets you take your Power BI content into meetings or on the go.

---

### 🔴 Q10. How can someone update data in Excel on the go and still keep Power BI dashboards current?

**✴️ Answer:**
A typical workflow is:

1. Store the Excel workbook on **OneDrive for Business**.
2. Connect that workbook to Power BI as a **dataset**.
3. Use the **Excel mobile app** (or Excel on desktop/web) to edit numbers in the workbook from anywhere.
4. Power BI refreshes the dataset (manually or on schedule), and the updated numbers appear in the **reports and dashboards**.

---
