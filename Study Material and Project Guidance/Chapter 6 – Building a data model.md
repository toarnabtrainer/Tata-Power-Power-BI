# ✅ Chapter 6 – Building a data model

---

## ☑️ 1. Big picture: what Chapter 6 is about

By this point in the story, David (the Contoso guy) has already built some basic reports. Now he:

* Stops relying on a single, pre-aggregated “sales view”.
* Loads **raw detailed tables** (Sales, Products, Stores, Date). 
* Builds a **proper data model** (star schema).
* Starts using **DAX measures, calculated columns, and calculated tables**.
* Integrates **budget data** from Excel and solves tricky modeling problems like many-to-many relationships and budget allocation. 

The core message: in Power BI, **your data model + DAX = the real engine**. Visuals are just the front end.

---

## ☑️ 2. Loading individual tables and building the first model

Earlier, David used a single SQL Server view (Sales2015) that Karin had created for him. Now he learns that for serious analysis, it’s better to load **granular tables** directly:

* **Products** – all product attributes (category, subcategory, brand, etc.).
* **Sales** – one row per sale (fact table).
* **Stores** – store attributes (location, region, etc.).
* **Date** – a calendar table. The book stresses this is “of paramount importance” for a good BI model. 

He connects to SQL Server, and in the Navigator he selects all four tables at once. Before loading, he:

* Clicks **Edit Queries** to open Power Query.
* Renames tables like `ContosoBi.Sales` to just `Sales` for cleaner, user-friendly naming. 

When he closes Query Editor, Power BI:

* Loads the tables.
* Automatically detects some relationships (e.g., Sales ↔ Product, Sales ↔ Store), but **not all** of them (the Date–Sales link is missing). 

David then:

* Switches to **Model (Relationship) View**.
* Manually creates the missing relationship by dragging `DateKey` in Sales onto `DateKey` in Date.

The result is a simple **star schema**:

* **Sales** at the center (fact).
* **Product**, **Store**, and **Date** around it (dimensions). 

### 🔰 What you should learn from this

As a learner, you should be comfortable with:

* Choosing **raw tables instead of pre-aggregated views** for more flexible analysis.
* Using **Power Query** to clean up table names before loading.
* Checking and fixing **relationships** in Model View.
* Understanding that a **key column** (like `DateKey`) must be unique on the “one” side for a valid relationship.

---

## ☑️ 3. Implementing measures (and why default summarization is dangerous)

Out of the box, Power BI will happily sum any numeric column. That’s okay for simple Excel-like models, but with warehouse data it often produces **nonsense**.

David starts improving the model by:

* Hiding **technical columns** (keys, surrogate IDs).
* Hiding columns that would be misleading if summed directly—for example, `Net Price` in the Sales table (summing prices without considering quantities is wrong). 

He then creates an **explicit measure** for sales:

```text
Sales Amount = SUMX ( Sales, Sales[Quantity] * Sales[Unit Price] )
```

This is a DAX measure defined on the Sales table. Instead of summing a price, it:

* For each row, multiplies `Quantity * Unit Price`.
* Sums those row-level results using `SUMX`. 

Now, when he builds visuals:

* He uses **[Sales Amount]** instead of relying on “Sum of Unit Price”.
* He can slice by any dimension: category, brand, store, country, date, etc. 

### 🔰 What you should learn from this

* **Never trust default aggregation** for warehouse-style data.
* Use **measures** for business logic (Sales Amount, Margin, etc.).
* A **measure** is a DAX expression evaluated at query time, respecting filters from the report.

If you’re learning, this is where you should start writing your first simple measures (SUM, SUMX, COUNTROWS, etc.) instead of dragging raw numeric fields into visuals.

---

## ☑️ 4. Creating calculated columns to improve time analysis

Once Sales Amount is in place, David builds richer visuals: a chart of brands inside the Computers category and a line chart over time. But he hits a visualization problem:

* The **Date** table has a `Month Name` column (Jan, Feb, …).
* If he uses Month as the axis and keeps Year in the legend, he gets **multiple lines** (one for each year). Good for comparing years.
* If he **removes** Year from the legend, Power BI aggregates all three years into a single value per month (all Januaries together, all Februaries together), which is not what he wants for a three-year trend.

To fix this, he needs a **“Month + Year”** column that:

* Shows something like `Jan 13`, `Feb 13`, ..., `Jan 14`, etc.
* Sorts correctly in chronological order.

He creates two **calculated columns** in the Date table:

```text
Month Year       = FORMAT ( 'Date'[Date], "mmm YY" )
Month Year Number = 'Date'[Year] * 100 + MONTH ( 'Date'[Date] )
```

Then he uses **Sort by Column** so that `Month Year` is sorted by `Month Year Number`. 

Now:

* The line chart axis uses `Month Year`.
* The chart shows a **single continuous line** across months over multiple years—exactly the behavior he wanted. 

### 🔰 What you should learn from this

* **Calculated columns** reshape data for better visuals.
* They are evaluated at data refresh time and stored in the model.
* You frequently build columns that exist purely to make charts read correctly (e.g., Month-Year strings, custom sort keys, concatenated labels).

---

## ☑️ 5. Improving the report with more measures (bubble chart example)

To show the power of combining measures, the chapter uses a **bubble chart**:

* X-axis: number of products in each category.
* Y-axis: gross margin.
* Bubble size: total amount sold. 

Two simple measures power this chart:

```text
NumOfProducts = COUNTROWS ( 'Product' )

Gross Margin = SUMX (
    Sales,
    Sales[Quantity] * ( Sales[Unit Price] - Sales[Unit Cost] )
)
```

* `NumOfProducts` gives the size of the product portfolio per category.
* `Gross Margin` calculates profit by subtracting cost from unit price and multiplying by quantity. 

This single visual now shows:

* How many products exist in each category.
* How profitable those categories are.
* How much they sell in total (bubble size).

### 🔰 What you should learn from this

* Once you know basic DAX, you can combine measures to build **high-information visuals**.
* Learn to think: “What measures do I need to tell this story?” and then design the chart.

---

## ☑️ 6. Integrating budget information and solving relationship issues

Now comes the **budget** from the Excel workbook. David loads the Budget table into the model. Immediately:

* He sees Budget is **isolated**—no relationships to the existing tables. 
* Unlike the Date–Sales case, this is not a failure of auto-detection; there is genuinely no valid key column to link Budget directly to Store or other tables. 

The book walks through an important modeling concept:

* To create a relationship, the column used on the “one” side must be a **key**: unique values only.
* `CountryRegion` is not unique in Budget or Store, so it cannot be used directly as a “one-side” key. 

### 🔰 Creating a bridge table (calculated table)

They solve this by building a **bridge (lookup) table** called `CountryRegions` as a **calculated table** in DAX:

```text
CountryRegions =
    SUMMARIZE (
        UNION (
            DISTINCT ( Budget[CountryRegion] ),
            DISTINCT ( Store[CountryRegion] )
        ),
        [CountryRegion]
    )
```

This table:

* Contains all distinct country/region values present in either Budget or Store.
* Has **CountryRegion** as a proper key (one row per region).

Now David can:

* Create a relationship between Store and CountryRegions.
* Create another relationship between Budget and CountryRegions.

This effectively sets up a **many-to-many** relationship between Budget and Store, with CountryRegions as the bridge. 

He then defines:

```text
Budget Amount = SUM ( Budget[Budget 2016] )
```

and builds a report with CountryRegion, Sales Amount, and Budget Amount—now both are sliced correctly by region. 

### 🔰 Fixing year comparability

However:

* Budget 2016 is being compared to **all years** of sales combined, which is misleading.
* He defines a measure `Sales 2015 = CALCULATE( [Sales Amount], 'Date'[Year] = 2015 )` and uses that instead. Now the comparison is meaningful (2015 actuals vs 2016 budget). 

### 🔰 What you should learn from this

* Sometimes you need **bridge tables** (via calculated tables or Power Query) to link datasets.
* Understand **many-to-many relationships** and how to resolve them.
* Use `CALCULATE` to **override or add filters** (e.g., fix a measure to a specific year).

---

## ☑️ 7. Relationship directions and ambiguity

As more tables and relationships are added, the model can become **ambiguous** if too many relationships are set to **bidirectional** (filters flow both ways). In the chapter’s example:

* There are multiple valid paths from Product to Budget (via Brands, or via Sales → Store → CountryRegions → Budget).
* If every link is bidirectional, filter propagation becomes ambiguous and results can be wrong or unpredictable. 

To fix this, David:

* Opens **Edit Relationship** for key relationships.
* Sets **Cross filter direction = Single** for several links:

  * Sales–Store
  * Sales–Product
  * Budget–CountryRegions
  * Budget–Brands 

The final model:

* Still allows needed filters (dimensions → facts).
* Removes ambiguous filter paths.

### 🔰 What you should learn from this

* Do **not** leave everything bidirectional “just in case”.
* Prefer **single-direction** relationships from dimensions to facts, and carefully add bidirectional ones only when needed (e.g., for certain bridge tables).

---

## ☑️ 8. Reallocating the budget using allocation factors

Up to now, the budget numbers are correct **only** at the level where the budget is defined:

* By **brand** and **country/region**. 

If he builds a matrix like:

* Rows: Country/Region and Color
* Values: Budget Amount

he notices each color row within a country often shows the **same** budget number as the total for that country. This isn’t “wrong”, it’s just answering a different (confusing) question: “sum of budgets for brands that have at least one product of this color.” For common colors like Black and Silver, that becomes the grand total. 

But what he really wants is: **budget assigned to Black products in the US**, etc. The source workbook does **not** contain color-level budgets, but we can **allocate** the higher-level budget down to colors based on sales patterns.

### 🔰 Allocation idea (conceptually)

For a specific brand in a country:

1. Take total budget for that brand–country (e.g., 239,500).
2. Compute an **allocation factor** for each color =
   (Sales 2015 for that color) / (Total Sales 2015 for that brand–country).
3. Multiply the brand–country budget by that factor to get color-level budget. 

The chapter defines:

```text
AllocationFactor =
DIVIDE (
    [Sales 2015],
    CALCULATE (
        [Sales 2015],
        ALLEXCEPT( 'Product', 'Product'[Brand] )
    )
)
```

then refines it to also keep CountryRegion context and clear all Date filters:

```'text
AllocationFactor =
DIVIDE (
    [Sales 2015],
    CALCULATE (
        [Sales 2015],
        ALLEXCEPT( 'Product', 'Product'[Brand] ),
        ALLEXCEPT( Store, Store[CountryRegion] ),
        ALL ( Date )
    )
)

Budget Amount = SUM ( Budget[Budget 2016] ) * [AllocationFactor]
```

Now:

- At brand–country level, Budget Amount still matches the original budget.
- At finer levels (e.g., color), Budget Amount is **distributed** according to last year’s sales pattern.
- A chart comparing Budget Amount vs Sales 2015 and Sales 2014 shows that the budget follows Sales 2015’s distribution. :contentReference[oaicite:28]{index=28}  

### 🔰 What you should learn from this

- Power BI + DAX can **create data that doesn’t exist explicitly** in the source (like color-level budgets).
- Allocation using historical data is a common real-world scenario (for budgets, targets, quotas).
- Functions like `DIVIDE`, `CALCULATE`, `ALLEXCEPT`, and `ALL` are key for advanced measures.

---

## ☑️ 9. Conclusions and learner takeaways

The chapter closes by emphasizing three main ideas: :contentReference[oaicite:29]{index=29}  

1. **Building your own model**  
   - Loading raw facts and dimensions from the warehouse gives you far richer analysis than working with pre-aggregated views.
   - But you are responsible for designing relationships, keys, and structure.

2. **DAX as your main analytical tool**  
   - You saw DAX used for **measures**, **calculated columns**, and **calculated tables**.
   - DAX is a full language—this chapter just gives a taste; deeper learning requires a dedicated DAX course or book.

3. **Building columns for specific charts is normal**  
   - If a chart needs a Month-Year label or some special sorting, just build the column.
   - Modeling is driven by the kind of **story** you want your visuals to tell.

---

### 🔰 If you’re studying this chapter as a learner, by the end you should be able to:

- Load **multiple related tables** and check / fix their relationships.
- Explain the difference between:
  - **Measures** vs **calculated columns** vs **calculated tables**.
- Write and use basic measures like:
  - `[Sales Amount]`, `[Gross Margin]`, `[NumOfProducts]`, `[Sales 2015]`, etc.
- Create a simple **bridge table** to solve a many-to-many relationship.
- Understand and adjust **relationship directions** (single vs both).
- Implement a simple **allocation** logic to redistribute budgets or targets from a higher level (brand, country) to a lower level (color, category).

---

# ✅ Question & Answer on Chapter 6 (Building a data model):

---

### 🔴 1. Why does David switch from using a single “Sales2015” view to loading individual tables like Sales, Product, Store, and Date?

**✴️ Answer:**
Because working with **granular tables** gives him much more flexibility. With separate tables:

* He can slice and dice by many dimensions (product, store, date, etc.).
* He relies on **relationships** instead of pre-aggregated views.
* It follows a proper **star schema** (fact table + dimension tables), which is easier to maintain and extend.

A single “all-in-one” view is simpler at first, but it limits deeper analysis and can become messy as requirements grow.

---

### 🔴 2. What is the role of the Date table, and why is it so important?

**✴️ Answer:**
The **Date** table is a dedicated table with one row per calendar date and useful columns like year, month, quarter, month name, etc. It’s important because:

* It enables **time intelligence** (year-over-year, YTD, prior period comparisons).
* It provides a consistent way to slice all measures over time.
* Relationships from the Date table to fact tables (like Sales) let filters on dates propagate correctly.

In a serious Power BI model, a proper Date table is considered essential, not optional.

---

### 🔴 3. What is a DAX measure, and why is it better than just using a numeric column directly in visuals?

**✴️ Answer:**
A **DAX measure** is a calculation evaluated **on the fly** based on the current filter context (e.g., what’s selected in the report). For example, `Sales Amount = SUMX(Sales, Sales[Quantity] * Sales[Unit Price])`.

It’s better than using a numeric column like `Unit Price` directly because:

* It captures **correct business logic** (e.g., quantity × price, not just summing prices).
* It automatically responds to filters by product, date, region, etc.
* It avoids misleading totals that can come from naively summing raw numeric fields.

---

### 🔴 4. How is a **calculated column** different from a **measure**?

**✴️ Answer:**

* A **calculated column**:

  * Is computed **row by row** when the data is loaded or refreshed.
  * Its values are stored in the model like normal columns.
  * Commonly used for grouping, labeling, or sorting (e.g., Month-Year text, flags).

* A **measure**:

  * Is computed **at query time**, based on current filters.
  * Does not store values per row; it recalculates as you interact with the report.
  * Used for aggregations (sums, averages, ratios, etc.).

Think: **columns shape the data structure**, **measures answer questions**.

---

### 🔴 5. Why does David create a “Month-Year” column, and how does he make sure it sorts correctly?

**✴️ Answer:**
He creates a **Month-Year** column (e.g., “Jan 2014”, “Feb 2014”, …) so his line chart can show a **single continuous timeline** across months and years, instead of grouping all Januaries together.

To sort it correctly:

1. He creates a **numeric key** (e.g., `MonthYearNumber = Year * 100 + Month`).
2. In Power BI, he selects the Month-Year column and uses **Sort by Column** → MonthYearNumber.

This ensures the chart’s X-axis follows actual chronological order instead of alphabetical order.

---

### 🔴 6. What is the purpose of the bridge (lookup) table `CountryRegions`, and how does it help with relationships?

**✴️ Answer:**
The `CountryRegions` table is a **bridge/lookup table** containing one row per Country/Region. It exists because:

* `CountryRegion` is repeated in both **Budget** and **Store** tables (not unique).
* You can’t create a proper one-to-many relationship if the “one” side doesn’t have unique values.

By building `CountryRegions` (distinct list of regions) and relating both Budget and Store to it:

* CountryRegions becomes the **“one” side** of two one-to-many relationships.
* Budget and Store are connected **through** CountryRegions.
* This effectively resolves a many-to-many relationship between Budget and Store.

---

### 🔴 7. Why can bidirectional relationships cause problems, and what is usually recommended?

**✴️ Answer:**
Bidirectional relationships let filters flow in **both directions** between tables. While powerful, they can:

* Create **ambiguous filter paths** (multiple ways a filter can reach a table).
* Produce **unexpected or incorrect results** in measures.
* Make the model harder to reason about.

The usual recommendation is:

* Use **single-direction** relationships (from dimension to fact) by default.
* Enable bidirectional filtering **only when necessary** and when you fully understand the impact (e.g., for certain bridge tables or specific report scenarios).

---

### 🔴 8. How does David compare 2016 budget with 2015 sales in a meaningful way?

**✴️ Answer:**
Instead of comparing the 2016 budget to **all years of sales**, he defines a measure like:

```DAX
Sales 2015 =
CALCULATE(
    [Sales Amount],
    'Date'[Year] = 2015
)
```

This locks the measure to **only 2015** using `CALCULATE` and a filter. Then:

* He compares **Budget 2016** with **Sales 2015** at the same grain (e.g., per CountryRegion or Brand).
* The comparison becomes meaningful: “next year’s budget vs last year’s actuals.”

---

### 🔴 9. What is the idea behind using an “allocation factor” to distribute budget to a finer level (e.g., by color)?

**✴️ Answer:**
The source budget is only defined at the **Brand–Country** level; there’s no explicit budget by color. To approximate a color-level budget:

1. Compute an **allocation factor** for each color within a Brand–Country, usually:
   `AllocationFactor = (Sales 2015 for that color) / (Total Sales 2015 for that Brand–Country).`
2. Multiply the **Brand–Country budget** by this factor to get the portion of budget assigned to that color.

This way, the budget is **redistributed** across colors based on historical sales patterns, which is a realistic approach when detailed budgets don’t exist.

---

### 🔴 10. After completing Chapter 6, what modeling and DAX skills should a learner aim to have?

**✴️ Answer:**
By the end of Chapter 6, you should be able to:

* Build a simple **star schema** with fact and dimension tables.
* Create and manage **relationships** (including understanding keys and direction).
* Write basic and intermediate **measures** (e.g., Sales Amount, Gross Margin, Sales for a specific year).
* Create **calculated columns** for labeling and sorting (like Month-Year).
* Create **calculated tables** to act as bridge/lookup tables in many-to-many situations.
* Use DAX functions like `SUMX`, `CALCULATE`, `ALLEXCEPT`, and `DIVIDE` to implement logic such as **fixed-year measures** and **budget allocation**.

These skills turn you from just a “report builder” into someone who can **design and power the data model** behind the reports.

---
