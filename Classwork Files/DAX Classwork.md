# ✅ DAX Classwork

## 0️⃣ Data preparation: RN_East1

**Goal:** bring the Excel sheet into a star-schema-friendly shape and create a fact table.

1. In **Power BI Desktop → Home → Get Data → Excel**
   Load **PracticeData.xlsx**, select **RN_East1** worksheet.
2. In **Power Query**:

   * Select all **month columns** (e.g., JAN, FEB, …, DEC).
   * Right-click → **Unpivot Columns**.
   * Rename:

     * *Attribute* → **Month**
     * *Value* → **EastSales**
3. Close & Apply.

Now `RN_East1` acts as your **fact table** with columns like:

* `Product`
* `Month`
* `EastSales` (numeric)

All the DAX measures below work on this model.

---

## 1️⃣ Basic aggregation measures (SUM, AVERAGE, COUNT, MAX, MIN)

> All of these are **measures**, not calculated columns.
> They are responsive to **filter context** (slicers, visuals, etc.).

### 1. TotalSales

```DAX
TotalSales = SUM ( RN_East1[EastSales] )
```

* Adds up all `EastSales` values in the current filter context.
* If you put this measure in a matrix by Month, it gives **total sales per month**.
* If you slice by Product, it returns **total sales for the selected product(s).**

**Expert tip:** Always prefer measures (like `TotalSales`) over creating extra “total” columns. Measures are dynamic and don’t inflate the data model.

---

### 2. AverageSales

```DAX
AverageSales = AVERAGE ( RN_East1[EastSales] )
```

* Returns the **average** of `EastSales` for whatever is currently filtered (month, product, region, etc.).

---

### 3. CountSale

```DAX
CountSale = COUNT ( RN_East1[EastSales] )
```

* Counts **how many rows** have a **non-blank** value in `EastSales`.
* Good for “number of transactions / records” in the current context.

**Expert tip:**
Use `COUNTROWS ( RN_East1 )` when you want to count rows regardless of which column has blanks.
Use `COUNTA` when working with text columns.

---

### 4–5. MaxSales & MinSales

```DAX
MaxSales = MAX ( RN_East1[EastSales] )
MinSales = MIN ( RN_East1[EastSales] )
```

* `MaxSales`: highest sale amount in the current context.
* `MinSales`: lowest sale amount in the current context.

These are useful for KPIs, outlier analysis, or comparing with average.

---

## 2️⃣ Statistical measures (Standard deviation and variance)

### 6. SDSales

```DAX
SDSales = STDEV.S ( RN_East1[EastSales] )
```

* **Sample standard deviation** of `EastSales`.
* Measures **spread/variability** around the mean.
  Higher SD = more variation between sales values.

---

### 7. SDSales1 (via square root of variance)

```DAX
SDSales1 = SQRT ( VAR.S ( RN_East1[EastSales] ) )
```

* First calculates the **sample variance**, then takes **square root**.
* Mathematically, this should equal `SDSales` (allowing for rounding).

---

### 8. VarSales

```DAX
VarSales = VAR.S ( RN_East1[EastSales] )
```

* **Sample variance** of `EastSales`.
* Variance = average squared distance from the mean.
  Units are “sales²”, which is why standard deviation is often easier to interpret.

---

### 9. VarSales1 (via SD squared)

```DAX
VarSales1 = STDEV.S ( RN_East1[EastSales] ) ^ 2
```

* Should match (approximately) `VarSales`.
* This pair shows learners the relationship:

> **Standard deviation² = variance**

**Expert teaching angle:**
Ask learners to compare `VarSales` & `VarSales1` and `SDSales` & `SDSales1` to reinforce the math link between variance and SD.

---

## 3️⃣ Iterator functions: SUMX / AVERAGEX / MINX / MAXX

Now we introduce **row-by-row expressions**.

Imagine an 18% GST on sales. We want to aggregate **GST amount**.

### Key concept: X-functions

* `SUM (column)` → simple aggregation directly on a column.
* `SUMX (table, expression)` → iterates row by row:

  1. For each row in `table`, evaluate `expression`.
  2. Sum the results.

Same pattern for `AVERAGEX`, `MINX`, `MAXX`.

### 10. GSTSalesSumX

```DAX
GSTSalesSumX =
    SUMX (
        RN_East1,
        RN_East1[EastSales] * 0.18
    )
```

* For each row of `RN_East1`, compute `EastSales * 0.18`, then sum.
* Gives **total GST amount** (18%) for current filter context.

---

### 11. GSTSalesAvgX

```DAX
GSTSalesAvgX =
    AVERAGEX (
        RN_East1,
        RN_East1[EastSales] * 0.18
    )
```

* Average GST amount per row in the current context.

---

### 12–13. GSTSalesMinX & GSTSalesMaxX

```DAX
GSTSalesMinX =
    MINX (
        RN_East1,
        RN_East1[EastSales] * 0.18
    )

GSTSalesMaxX =
    MAXX (
        RN_East1,
        RN_East1[EastSales] * 0.18
    )
```

* `GSTSalesMinX`: smallest GST amount on any single row.
* `GSTSalesMaxX`: largest GST amount on any single row.

**Expert tip:**
A common alternative is to define:

```DAX
GST Amount = [TotalSales] * 0.18
```

This uses a **measure on top of a measure**, which usually performs better on large models.
Your SUMX/AVERAGEX example is perfect to teach **iterators** though.

---

## 4️⃣ CALCULATE and FILTER – changing filter context

This is the heart of DAX.

### 14. CalculateMinGST (CALCULATE with filters)

Let’s fix the syntax and use the measure `[TotalSales]`:

```DAX
CalculateMinGST =
CALCULATE (
    [TotalSales] * 0.18,
    RN_East1[Month]   = "AUG",
    RN_East1[Product] = "P013"
)
```

* `CALCULATE`:

  1. **Changes** the filter context to:

     * Month = "AUG"
     * Product = "P013"
  2. Evaluates the expression `[TotalSales] * 0.18` under that context.

So even if your visual is showing **all months** and **all products**, this measure will always calculate **GST for AUG & P013 only**.

**Expert tip:**
Make learners remember this sentence:

> **“CALCULATE = evaluate this expression, but pretend the filters are…”**

That mental model is more useful than any formal definition.

---

### 15 & 17. FilterMinGST (FILTER + MINX)

Let’s create a cleaner, final version:

```DAX
FilterMinGST =
MINX (
    FILTER (
        RN_East1,
        RN_East1[Month] = "AUG"
    ),
    [TotalSales] * 0.18
)
```

What is happening?

1. `FILTER ( RN_East1, RN_East1[Month] = "AUG" )`
   → returns a **table** containing only AUG rows.

2. `MINX ( <that filtered table>, [TotalSales] * 0.18 )`
   → for each of those rows, evaluate `[TotalSales] * 0.18` and return the **minimum** value.

---

### CALCULATE vs FILTER (important concept)

* **CALCULATE**:

  * Modifies the filter context and then re-evaluates a measure.
  * Filters are **mutable**: they overwrite existing context.

* **FILTER**:

  * Returns a **table** based on a condition.
  * It doesn’t “modify” context by itself; it’s a building block used by other functions like `SUMX`, `MAXX`, `CALCULATE` etc.
  * Filters are **not mutable** — they just **define a subset** of rows.

> Think of `CALCULATE` as **“change the world, then calculate”**
> and `FILTER` as **“build a smaller table, then pass it to someone else.”**

---

## 5️⃣ Ignoring filters with ALL

Now we show how a measure can **ignore filters/slicers** and give a grand total.

### 16 & 18. TotalSalesAll

(They are the same idea; here’s a final, correct version:)

```DAX
TotalSalesAll =
CALCULATE (
    SUM ( RN_East1[EastSales] ),
    ALL ( RN_East1 )
)
```

* `ALL ( RN_East1 )` removes **all filters** on the `RN_East1` table:

  * Month, Product, etc.
* So `TotalSalesAll` always returns **overall total** – a grand total – even if the report is filtered to:

  * Only AUG
  * Only Product P013
  * Only 2024, etc.

**Class exercise idea:**

1. Put `TotalSales` and `TotalSalesAll` in a table by Product.
2. Use a slicer on Month.
3. Ask learners:

   * “Why does `TotalSales` change with the slicer, but `TotalSalesAll` does not?”
   * This drives home **filter context** vs **ignoring filter context**.

You can also show `ALL ( RN_East1[Month] )` to remove only Month filters but keep Product filters.

---

## 6️⃣ Visuals for this classwork

### A. Table visual

Create a **Table** with:

* `Month`
* `EastSales` (as a column)
* Measures:

  * `[TotalSales]`
  * `[GSTSalesSumX]`
  * `[GSTSalesMinX]`
  * `[GSTSalesMaxX]`

Use this to visually compare:

* Month-wise totals vs individual row values.
* Aggregate GST vs min/max per row.

### B. Multi-row Card visual

Create a **Multi-row card** and drop in (at least):

1. `TotalSales`
2. `AverageSales`
3. `CountSale`
4. `MaxSales`
5. `MinSales`
6. `SDSales`
7. `SDSales1`
8. `VarSales`
9. `VarSales1`
10. `GSTSalesSumX`
11. `GSTSalesAvgX`
12. `GSTSalesMinX`
13. `GSTSalesMaxX`
14. `CalculateMinGST`
15. `FilterMinGST`
16. `TotalSalesAll`

Learners will see **how all measures respond to slicers**:

* Filter on Month → most measures change.
* `TotalSalesAll` stays fixed → shows the power of `ALL`.
* `CalculateMinGST` might remain fixed for AUG & P013 regardless of slicer.

---

## 7️⃣ What this classwork really teaches (big picture)

1. **Core aggregations** (SUM, AVG, COUNT, MIN, MAX)
   → basic building blocks of almost every report.

2. **Statistical insight** (STDEV.S, VAR.S)
   → more advanced interpretation of sales performance, volatility, risk.

3. **Iterators (X-functions)**
   → when you need row-by-row expressions, not just column aggregation.

4. **Filter context vs Row context**
   → CALCULATE & FILTER are your first serious step into “thinking in DAX”.

5. **ALL and global totals**
   → essential for ratios, percentage of total, and KPI benchmarks.

---

```DAX
Important and Useful DAX Functions
Import RN_East1 worksheet from PracticeData.xlsx and unpivot all month columns. Rename new columns as Month and EastSales.
1.	TotalSales = SUM(RN_East1[EastSales])            // SUM (Column Name)
2.	AverageSales = AVERAGE(RN_East1[EastSales])      // AVERAGE (Column Name)
3.	CountSale = COUNT(RN_East1[EastSales])           // COUNT (Column Name)
4.	MaxSales = MAX(RN_East1[EastSales])              // MAX (Column Name)
5.	MinSales = MIN(RN_East1[EastSales])              // MIN (Column Name)
6.	SDSales = STDEV.S(RN_East1[EastSales])           // STDEV.S (Column Name)
7.	SDSales1 = SQRT(VAR.S(RN_East1[EastSales]))      // VAR.S (Column Name)
8.	VarSales = VAR.S(RN_East1[EastSales])            // VAR.S (Column Name)
9.	VarSales1 = STDEV.S(RN_East1[EastSales]) ^ 2     // STDEV.S (Column Name)
10.	GSTSalesSumX = SUMX(RN_East1, RN_East1[EastSales] * 0.18)          // SUMX (Table, Expression)
11.	GSTSalesAvgX = AVERAGEX(RN_East1, RN_East1[EastSales] * 0.18) // AVERAGEX (Table Name, Expression)
12.	GSTSalesMinX = MINX(RN_East1, RN_East1[EastSales] * 0.18      // MINX(<table>, < expression>)
13.	GSTSalesMaxX = MAXX(RN_East1, RN_East1[EastSales] * 0.18)    // MAXX(<table>, <expression>)
14.	CalculateMinGST = CALCULATE(RN_East1[TotalSales] * 0.18, RN_East1[Month] = "AUG", RN_East1[Product] = "P013")                      // CALCULATE(<expression>, <filter1>, <filter2>…)
15.	FilterMinGST = MINX(FILTER(RN_East1, RN_East1[Month] = "AUG"), RN_East1[TotalSales] * 0.18)	       // FILTER(<expression>,<filter1>)
// The filter function works the same way as CALCULATE; however, its major difference is that the FILTER functions are not mutable; It can only subset the data. FILTER is an expression that can be used in union with an existing function such as SUMX, MAXX etc.

// Now do a filtering on Product and check the TotalSales has changed
// Now create this measure and check it is showing grand total as usual ignoring filter
16.	TotalSalesAll = CALCULATE(SUM(RN_East1[EastSales]), ALL(RN_East1))         // ALL(<table>[<column>])
17.	FilterMinGST = MINX(FILTER(RN_East1, RN_East1[Month] = "AUG"), [TotalSales] * 0.18)   // FILTER(Table Name, Filter)<br>
18.	TotalSalesAll = CALCULATE(sum(RN_East1[EastSales]), ALL(RN_East1))   // ALL(Table Name[<Column>])

Create a Table with (Month, EastSales, TotalSales, GSTSalesSumX, GSTSalesMinX, GSTSalesMaxX)
Create a Multi-row Card with all these 18 measures.
```

---