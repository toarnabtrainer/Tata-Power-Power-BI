# ✅ Chapter 7 – Improving Power BI reports

---

## ☑️ 1. What this chapter is really about?

Chapter 7 is all about **turning “okay” reports into genuinely good ones**. Instead of teaching you every button in the Visualizations pane, it focuses on:

* Picking the **right chart for the question** you’re answering
* Using **standard visuals** well (you can go very far without fancy ones) 
* Knowing **when custom visuals actually help**, and when they’re just decoration 
* Using **DAX (measures & calculated columns)** to shape the data so visuals become clearer 
* Designing **high-density pages** that show a lot of information without overwhelming your audience 

The chapter uses real example reports (Contoso Sales, stock portfolios, Google Analytics for DAXFormatter site) from the companion files so you can open them and experiment yourself later. 

---

## ☑️ 2. Choosing the right visualizations

Power BI ships with a fairly rich set of **standard visuals**—around 27 at the time this book was written. 
Before you think about custom visuals, you should know what the standard ones can do and **when to use each category**:

Some key standard visuals and their typical usage from the chapter:

* **Bar / Column charts (stacked, clustered, 100%)**

  * Compare categories (brands, products, regions)
  * Show part-to-whole comparisons (e.g., 100% stacked) where each bar represents 100% and segments show contribution. 

* **Line / Area / Stacked Area charts**

  * Line chart → best for **trends over time** (sales over months, website sessions by day). 
  * Area chart → similar to line, but fills the space under the line to emphasize total volume.
  * Stacked area → shows cumulative contribution of multiple series over time (e.g., how different stocks contribute to total portfolio value).

* **Combo charts (Line + Bar/Column)**

  * Use when metrics are on **different scales** (e.g., revenue vs margin%). 

The authors emphasize some simple but very important design rules:

* Start from your **business question**, not from “I want to use this fancy chart”
* Prefer **simple, well-known visual types** if they answer the question clearly 
* Use a **limited color palette** so users focus on the data, not the rainbow 
* Avoid unnecessary 3D, gradients, and decorations that don’t add meaning (this is implied throughout and then reinforced in the conclusions). 

They demonstrate this with a Contoso sales dashboard where:

* Only **two colors** are used (black for Sales Amount, yellow for comparison metrics)
* A small set of chart types (line, bar, column) is reused consistently across the report. 

---

## ☑️ 3. Choosing between standard visuals in practice

The chapter then goes deeper into specific choices, using the Contoso example:

* **Line chart** – Sales Amount vs Target over Date

  * Perfect for comparing actual vs target across time.
  * Data Colors property is used to explicitly set colors for each measure so they’re consistent and meaningful.

* **Area chart** – Sales Amount vs Sales Cost

  * Here the gap between sales and cost (margin) becomes visually clearer because the area is filled.
  * They remind you that the **Y-axis should start at zero** for area charts, otherwise the filled shape is misleading. 

The big takeaway:

> Don’t randomly pick visuals. Think: *“What relationship do I want to show—comparison, trend, composition, distribution?”* and then pick the simplest visual that does the job.

---

## ☑️ 4. Using custom visualizations

After you master standard visuals, Power BI lets you expand your toolbox with **custom visuals** from the gallery.

The chapter explains that:

* Custom visuals are **downloaded/installed** from the Power BI visuals gallery or imported from a file.
* They extend Power BI with visuals that aren’t available out of the box (e.g., candlestick charts, bullet charts, special maps, chiclet slicers, etc.).
* They are often built by **third-party developers** (consultants, companies, open-source contributors), so quality and maintenance vary.

Examples the chapter mentions / uses:

* **Candlestick by SQLBI** for stock prices (open, high, low, close). 
* **Synoptic Panel / Synoptic Designs** to color shapes on custom maps or diagrams (e.g., world map shaded by “Users per Million”). 
* Other examples in earlier sections like dashboard cards with states or enhanced selectors (in the original book they use things like card visuals with states, bullet charts, etc.).

The authors are very clear that custom visuals are **not mandatory**: they’re the **“icing on the cake”**, not the cake itself. 

---

## ☑️ 5. First steps with custom visuals

Practically, as a learner, your workflow with custom visuals is:

1. **Import the visual** into your report (from the marketplace or a `.pbiviz` file).
2. **Add it to the canvas** like any other visual.
3. **Assign fields** (measures, categories, legends) to the visual’s well-defined “buckets”.
4. **Configure formatting** (colors, labels, tooltips, axes) to match your report’s design.

In the chapter examples, they show how a standard report can be upgraded:

* A regular line/column chart for portfolio values becomes a **stacked area chart** + **candlestick chart**:

  * Stacked area shows **total portfolio value and each stock’s contribution** over time. 
  * Candlestick shows **open, close, high, low** for each stock per day, which a simple line chart cannot. 

The lesson:
Use custom visuals where standard visuals **cannot** represent the required info clearly (multiple price measures, advanced maps, special chart types).

---

## ☑️ 6. Improving reports by using custom visualizations

The chapter then walks through full report redesigns where custom visuals help to:

* Show more **dimensions** of the same data (e.g., OHLC stock prices, or users vs population by country).
* Make the **main message** clearer without adding more text or clutter.
* Preserve the **same page layout** but make individual visuals more meaningful.

Important nuance: when they compare the “before” and “after” versions of the high-density web analytics report, they say the improvement from custom visuals is **incremental**, not magical. The core clarity comes from layout, grouping, and good measures; custom visuals just polish things further. 

---

## ☑️ 7. When are custom visuals *actually* required?

There’s a dedicated section on **“Identifying conditions when custom visualizations are required”**. 

You should consider custom visuals when:

* **Standard visuals can’t express the structure of your data**

  * Example: Stock OHLC data → candlestick chart. 
  * Network graphs, Sankey flows, Gantt charts, etc., often need custom visuals.

* You need **special interactions or layouts**

  * Map with custom shapes (Synoptic Panel), advanced filters like timeline slicers or chiclet slicers.

* There is a **clear analytical benefit**, not just aesthetics

  * If the new visual **helps users answer questions faster** or see relationships that were hidden before, it’s worth it.

But you should *not* overuse them:

* Too many visual types confuse users.
* Some custom visuals may not be certified, might be slower, or may have limited support over time.

The chapter’s guideline is: use custom visuals **when they provide a concrete advantage over what you can do with standard visuals.** 

---

## ☑️ 8. Using DAX in data models to support visuals

This is a key turning point: visuals alone can’t fix everything. Sometimes you must **reshape the data** with DAX so the visuals can do their job.

The chapter shows:

### 🔰 a) Calculated measures for ratios

Example: **Users per Million** – you want to compare website users relative to each country’s population.

They create a measure: 

```DAX
Users per Million =
DIVIDE (
    SUM ( 'Website'[Users] ),
    SUM ( 'Countries'[Population] )
) * 1000000
```

This is then used in a map (Synoptic Panel) to show **users per million inhabitants**, which is much more meaningful than raw users alone. 

### 🔰 b) Calculated columns for grouping / bucketing

Example: **Browser resolution** from Google Analytics:

* Raw data has >5,000 unique “width x height” strings → impossible to plot nicely. 
* They create two calculated columns:

  * `Width Size` – extracts the numeric width from strings like `"1920x1080"`.
  * `Width Category` – groups widths into categories like 1024, 1280, 1440, 1920, 2560. 

This lets them build a much cleaner chart (e.g., a waterfall chart of **Sessions % by width bucket**) instead of thousands of tiny bars. 

### 🔰 c) Measures for percentages within a visual

Example: **Sessions %** measure:

```
Sessions % =
DIVIDE (
    SUM ( Website[Sessions] ),
    CALCULATE (
        SUM ( Website[Sessions] ),
        ALL ( Website[Width Category] )
    )
)
```

This expresses each width category as a **share of total sessions**, perfect for charts comparing contributions.

Overall DAX lesson for learners:

- Use **measures** for calculations that depend on filters (totals, ratios, growth percentages).  
- Use **calculated columns** for static classifications or grouping (buckets, labels, categories).  
- Don’t expect visuals to compute everything; often you need to create a DAX expression first and then bind it to the visual. :contentReference[oaicite:33]{index=33}  

---

## ☑️ 9. Creating high-density reports

The last part of the chapter looks at a **very dense web analytics report**:

- Built on Google Analytics data for the DAXFormatter website, using a `.pbix` file with 28 visuals + slicer + decorative elements. :contentReference[oaicite:34]{index=34}  
- Uses only **around 7 visual types**, but arranged cleverly into 3 zones:
  - **Left** – user metrics (users, new users, returning users…)  
  - **Center** – session metrics (sessions over time, page views…)  
  - **Right** – technical metrics (device type, browser, OS, page load time, resolution). :contentReference[oaicite:35]{index=35}  

Core design principles:

- **Group related visuals into zones** so the eye can scan logically.  
- Use a **limited set of visual types**, just variations of bar/column/line/etc. :contentReference[oaicite:36]{index=36}  
- Remove or tone down **anything that doesn’t help interpretation**:
  - Excess borders, gridlines, decorative images  
  - Too many different colors or legends  
- Rely on **DAX + thoughtful visuals** (e.g., Users per Million, Width Categories, Sessions % waterfall chart) to make complex data readable.   

The chapter concludes by stressing that in high-density reports, your users are already overloaded with information. Your job is to:

> Reduce distractions so their attention stays on the **data and its meaning**, not the visual noise. :contentReference[oaicite:38]{index=38}  

---

## ☑️ 10. Key takeaways for you as a learner

If you’re turning this into your own study material, the chapter’s core messages are:

1. **Master standard visuals first** – you can do 80–90% of your work with them.  
2. **Keep visuals simple and consistent** – few types, few colors, clear labels.  
3. **Use custom visuals only where they clearly improve understanding** (e.g., candlestick, advanced maps, special ratio displays).  
4. **Let DAX do the heavy lifting** – create measures and columns that make the visuals meaningful (ratios, groupings, classifications).  
5. **Design the page, not just the visuals** – group related info, reduce clutter, and treat high-density pages as a layout problem as much as a chart problem.

---

# ✅ Question & Answer on Chapter 7 (Improving Power BI reports):

---

### 🔴 1. What is the main goal of Chapter 7 in “Introducing Power BI”?

**✴️ Answer:**
The main goal is to show how to turn a “working” report into a **good, communicative report** by:

* Choosing appropriate visuals for the question you’re answering,
* Using standard visuals effectively,
* Knowing when (and when not) to use custom visuals, and
* Supporting visuals with the right DAX measures and calculated columns.

---

### 🔴 2. Why should you start with standard visuals before exploring custom visuals?

**✴️ Answer:**
Standard visuals:

* Are **well-tested, supported, and familiar** to most users,
* Cover the majority of common analysis needs (comparison, trends, composition, distribution),
* Make reports easier to understand and maintain.

Custom visuals should only be added when there is a **clear analytical benefit** that standard visuals cannot provide.

---

### 🔴 3. How do you decide which type of visual to use for a given question?

**✴️ Answer:**
You start from the **business question**, not from the chart type you like. For example:

* “How do values change over time?” → **Line or area chart**.
* “How do categories compare?” → **Bar/column chart**.
* “What part does each category contribute to the whole?” → **Stacked bar/column or 100% stacked**.

The simplest visual that clearly answers the question is usually the best choice.

---

### 🔴 4. Why is a line chart often preferred for time-series data?

**✴️ Answer:**
A **line chart**:

* Shows **trends and direction** over time very clearly,
* Makes it easy to compare multiple series (e.g., actual vs target, or multiple regions),
* Emphasizes the continuity of time (unlike a disconnected set of bars).

That’s why it’s often the first choice for **time-based metrics**.

---

### 🔴 5. When does it make sense to use a custom visual like a candlestick chart?

**✴️ Answer:**
A candlestick chart is useful when you have **OHLC (Open, High, Low, Close)** data, such as stock prices.
Standard line or bar charts can’t easily show all four values at once per time period. A candlestick visual:

* Encodes open and close as the “body”,
* High and low as “wicks”,
* Lets users quickly see volatility and daily price range.

So, you use a custom visual when it represents the **natural structure** of your data better than any standard visual.

---

### 🔴 6. What are some risks of overusing custom visuals?

**✴️ Answer:**
Overusing custom visuals can:

* Confuse users (too many unfamiliar chart types),
* Make reports feel inconsistent,
* Introduce performance or compatibility issues (some custom visuals are heavier or less maintained),
* Increase maintenance effort if the visual changes or is deprecated.

That’s why the chapter recommends using custom visuals **sparingly and purposefully**.

---

### 🔴 7. How can DAX measures improve the quality of your visuals?

**✴️ Answer:**
DAX measures:

* Let you create **meaningful metrics** like ratios, growth rates, “per capita” values, and percentages,
* Adapt automatically to filters and slicers,
* Make charts answer more insightful questions (e.g., “Users per million population” instead of just “Users”).

Good visuals almost always sit on top of **well-designed measures**, not raw columns.

---

### 🔴 8. Why might you create calculated columns like “Width Category” for browser resolutions?

**✴️ Answer:**
Raw browser resolution data (like “1920x1080”, “1366x768”, etc.) can produce **thousands of unique values**, which is:

* Too detailed to visualize cleanly,
* Hard for users to interpret.

By creating calculated columns to:

* Extract the width, then
* Bucket it into categories (e.g., 1024, 1280, 1440, 1920, 2560),

you can build **clean, readable charts** showing sessions by resolution range instead of an unreadable forest of tiny bars.

---

### 🔴 9. What is a high-density report page, and what design principles help keep it usable?

**✴️ Answer:**
A **high-density report page** is one that has many visuals (e.g., 15–30) on a single page. To keep it usable, you should:

* Group related visuals into **logical zones** (e.g., user metrics on the left, session metrics in the middle, technical metrics on the right),
* Use a **limited set of visual types and colors** for consistency,
* Reduce visual noise: unnecessary borders, gridlines, legends, and decorations,
* Make sure each visual has a **clear purpose** and readable labels.

The goal is to present a lot of information **without overwhelming the user**.

---

### 🔴 10. When should you change the data model (with DAX) instead of just tweaking visuals?

**✴️ Answer:**
You should modify the data model when:

* The raw data is too granular or messy to visualize directly (e.g., thousands of resolutions, many small categories),
* You need **derived metrics** (ratios, percentages, per-capita values, groupings) that visuals alone cannot compute reliably,
* You want visuals to respond correctly to filter context (e.g., measure as % of total).

In short: if the visual can’t answer the question cleanly with the current fields, it’s often a sign that you need a **new measure or calculated column/table** in the model, not just a different chart.

---
