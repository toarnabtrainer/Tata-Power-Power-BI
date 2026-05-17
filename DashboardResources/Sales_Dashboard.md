# Project based on creation of "Sales Dashboard"

✅ **Lookup/Dimension Tables and Data/Fact Tables:** <br>
* **Lookup Tables or Dimension Tables** will have Primary Keys, will answer Who, What, Where, When and How <br>
* **Lookup/Dimension Tables:** Customer (Who), Product (What), Territories (Where), Calendar (When and How) <br>
* **Data Tables or Fact Table** will have Foreign Keys, and will contain transactional data <br>
* **Data/Fact Tables:** Sales, Budget

✅ **Operations on Budget Workbook:** <br>
* Delete the promoted header <br>
* Delete first 3 null rows <br>
* Promote the first row as a header <br>
* Delete last total column <br>
* Filter out rows from the first column containing “Total” <br>
* Unpivot month columns labelled from Jan to Dec <br>
* Rename new created columns to Month and BudgetAmount <br>
* Change the data type of Month column to Date <br>
* Select close and apply <br>

✅ **Data Relationships for the Budget Project:** <br>
* Sales(CustomerKey) <- Customer(CustomerKey) <br>
* Sales(OrderDate) <- Calendar(Date) <br>
* Sales(ProductKey) <- Product(ProductKey) <br>
* Sales(SalesTerritoryKey) <- Territories(SalesTerritoryKey) <br>
* Budget(ProductKey) <- Product(ProductKey) <br>
* Budget(Month) <- Calendar(Date) <br>

✅ **Suggested DAX Formulaes for Creating Measures:** <br>
* MyBudget = SUM(Budget[BudgetAmount]) <br>
* MySales = SUM(Sales[SalesAmount]) <br>
* MyVariance = [MySales] - [MyBudget] <br>
* MyVariance% = DIVIDE([MyVariance], [MyBudget], 0) <br>
* MyComments = IF([MyVariance] < -100000, "Take Care", IF([MyVariance] < 0, "Not OK", "OK")) <br>

✅ **Suggested Tables Summaries (After filtering on Year 2016):** <br>
* **Table1:** Calendar[Year], Calendar[Month], Sum of Sales[SalesAmount] <br>
* **Table2:** Calendar[Year], Calendar[Month], Sum of Budget[BudgetAmount] <br>
* **Table3:** Territories[Country], Sum of Sales[SalesAmount] <br>
* **Table4:** Calendar[Year], Calendar[Month], Sum of Sales[SalesAmount], Sum of Budget[BudgetAmount] <br>
* **Table5:** Calendar[Year], Calendar[Month], Budget[MySales], Budget[MyBudget], Budget[MyVariance], Budget[MyVariance%], Budget[MyComment] <br>

<hr>

# ✅ Dataset Field Descriptions
## ☑️ Budget Worksheet

| **Field Name**   | **Description**                                                                                                                                                             |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Category**     | The broad classification of products. In this case, all products fall under the `Accessories` category.                                                                     |
| **Subcategory**  | A more specific classification within the category. For example, `Bike Racks` and `Bike Stands` are subcategories of `Accessories`.                                         |
| **ProductName**  | The full name or description of the individual product. Examples: `Hitch Rack - 4-Bike`, `All-Purpose Bike Stand`.                                                          |
| **ProductKey**   | A unique identifier (numeric code) for each product. Example: `483` for Hitch Rack - 4-Bike, `486` for All-Purpose Bike Stand.                                              |
| **Month**        | The month and year for which the budget was allocated or tracked. Format: `dd-mm-yyyy` (though the `day` part appears redundant here and can be cleaned to just `mm-yyyy`). |
| **BudgetAmount** | The planned or expected revenue/sales (budget) amount for the given product in that month. It's a numeric field (likely in currency). Example: `1131`, `2635`, etc.         |


<hr>

## ☑️ Sales Worksheet

| **Field Name**           | **Description**                                                                                                         |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------- |
| **ProductKey**           | A unique identifier for each product sold. Can be used to join with a product table to get product names or categories. |
| **OrderDate**            | The date on which the sales order was placed. Format: `dd-mm-yyyy hh:mm`.                                               |
| **ShipDate**             | The date on which the product was shipped to the customer. Usually ≥ OrderDate.                                         |
| **CustomerKey**          | A unique customer ID that can be joined with a customer table for names, geography, etc.                                |
| **PromotionKey**         | Refers to the marketing/promotion campaign linked with this sale. Useful for campaign performance tracking.             |
| **SalesTerritoryKey**    | Identifies the sales region/territory. Can be joined with a territory table for region names.                           |
| **SalesOrderNumber**     | Unique order number that groups the full transaction.                                                                   |
| **SalesOrderLineNumber** | Line item number within a SalesOrder (if multiple products ordered in one transaction).                                 |
| **OrderQuantity**        | Number of units sold for the line item.                                                                                 |
| **UnitPrice**            | Price per unit of the product.                                                                                          |
| **TotalProductCost**     | Total cost incurred for the product(s) sold (could be cost of production or procurement).                               |
| **SalesAmount**          | Total revenue generated from the sale. Ideally: `UnitPrice × OrderQuantity`.                                            |
| **TaxAmt**               | Amount of tax levied on the sale.                                                                                       |


<hr>

## ☑️ Product Worksheet

| **Field Name**         | **Description**                                                                                                             |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **ProductKey**         | Unique identifier for each product. This is the **primary key** and can be used to **join with sales or inventory tables**. |
| **ProductName**        | Name of the product (e.g., BB Ball Bearing, Chainring).                                                                     |
| **SubCategory**        | Subdivision within a Category (e.g., "Ball Bearings" under "Components"). Currently not filled in the image.                |
| **Category**           | Main grouping of products (e.g., Components, Accessories). Also not filled in yet.                                          |
| **StandardCost**       | The internal cost to produce or acquire the product. Useful for **profit/margin** calculations.                             |
| **Color**              | Color of the product (e.g., Black, Silver, NA). Can be used for filtering, categorization, or visual grouping.              |
| **ListPrice**          | Retail price before any discount. Often used as a reference price.                                                          |
| **DaysToManufacture**  | Number of days required to manufacture the product. Helpful for operational KPIs or lead time analysis.                     |
| **ProductLine**        | Product line classification (e.g., Road, Touring, Mountain). Empty for now. Can help in segmentation.                       |
| **ModelName**          | Associated model name (e.g., "HL", "LL", etc.). Also empty in this snapshot.                                                |
| **Photo**              | Image URL of the product. This can be shown in Power BI visuals like **Table** or **Card with Image URL** support.          |
| **ProductDescription** | Description of the product. Empty in the image. Could be useful for hover tooltips or product cards.                        |
| **StartDate**          | Launch date or availability start date of the product. Can be used for time-based filtering or shelf-life analysis.         |


<hr>

## ☑️ Customer Worksheet

| **Field Name**         | **Description**                                                                        |
| ---------------------- | -------------------------------------------------------------------------------------- |
| **CustomerKey**         | A **unique identifier** (Primary Key) for each customer.                               |
| **FirstName**            | Customer’s **first name**.                                                             |
| **LastName**             | Customer’s **last name**.                                                              |
| **FullName**             | Concatenation of `LastName`, `FirstName` (e.g., *Doe, John*).                          |
| **BirthDate**            | Customer’s **date of birth** (format: DD-MM-YYYY). Useful for age calculation.         |
| **MaritalStatus**        | Marital status: <br>`S` = Single, `M` = Married.                                       |
| **Gender**               | Gender of the customer: <br>`M` = Male, `F` = Female.                                  |
| **YearlyIncome**         | Customer's **annual income** (in currency units, probably USD or INR).                 |
| **TotalChildren**        | **Total number of children** the customer has.                                         |
| **NumberChildrenAtHome** | Number of **dependent children at home**.                                              |
| **Education**            | Highest level of education. In this dataset, it's mostly `Bachelors`.                  |
| **Occupation**           | Type of employment or professional role (e.g., `Professional`, `Management`).          |
| **HouseOwnerFlag**       | Binary indicator of homeownership:<br>`1` = Owns house, `0` = Does not own.            |
| **NumberCarsOwned**      | Number of **cars owned** by the customer.                                              |
| **AddressLine1**         | Street-level **address** (useful for geographic segmentation).                         |
| **DateFirstPurchase**    | Date of **first recorded purchase** (helps analyze customer tenure).                   |
| **CommuteDistance**      | Typical **commute distance from home to workplace**:<br>`0-1 Miles`, `1-2 Miles`, etc. |



<hr>

## ☑️ Calendar Worksheet

| **Column**       | **Meaning / Use**                                                                         |
| ---------------- | ----------------------------------------------------------------------------------------- |
| **Date**           | Full date in DD-MM-YYYY format. Used as a primary date field in visualizations.           |
| **DateKey**        | Integer representation (yyyymmdd) used for fast joins with fact tables.                   |
| **Year**           | Calendar year of the date (e.g., 2016).                                                   |
| **Quarter**        | Calendar quarter (`Q1` to `Q4`) for the date.                                             |
| **MonthNum**       | Numeric month (e.g., 4 for April, 7 for July).                                            |
| **Month**          | Short month name (e.g., `Apr`, `Jul`).                                                    |
| **FiscalYear**     | Fiscal year (e.g., `FY2016`, `FY2017`) — often starting in April or July.                 |
| **FiscalQuarter**  | Fiscal quarter (`FQ1` to `FQ4`). Useful in financial reporting.                           |
| **FiscalMonthNum** | Numeric order of month within the **fiscal year**. Example: `Apr` is 10 in FY2016.        |
| **FiscalMonth**    | Month name in fiscal calendar.                                                            |
| **MonthYear**      | First day of the month for that date (e.g., `01-04-2016`). Used for grouping.             |
| **MonthYearLong**  | Duplicate of `MonthYear`, sometimes used for label formatting.                            |
| **MonthYearNum**   | Concatenated year and month (e.g., `201604`), for sorting month-wise data.                |
| **WeekdayNum**     | Number of the day in the week (`1` = Sunday, `2` = Monday, ..., `7` = Saturday).          |
| **Weekday**        | Day of the week name (e.g., `Mon`, `Wed`).                                                |
| **WeekdayWeekend** | Classification of the day: `Weekday` or `Weekend`. Useful for business activity analysis. |


<hr>

## ☑️ Territories Worksheet

| **Column**          | **Meaning / Use**                                                                                 |
| ------------------- | ------------------------------------------------------------------------------------------------- |
| **SalesTerritoryKey** | Unique ID for each sales territory (used as primary key for lookup).                              |
| **Region**            | Name of the regional area (e.g., Northwest, France, etc.).                                        |
| **Country**           | Country where the region belongs.                                                                 |
| **Group**             | Geographic grouping — like North America, Europe, Pacific. Used for regional grouping in reports. |
| **RegionImage**       | URL to an image representing the region — can be used in Power BI dashboards for visual context.  |


<hr>

<hr>

<img width="1308" height="526" alt="image" src="https://github.com/user-attachments/assets/7e6a8d12-d21a-4381-b101-75b0a7718d5c" />



<img width="1308" height="526" alt="image" src="https://github.com/user-attachments/assets/7cbe1b2c-4dd7-4bbe-9494-fa5b00ea9b1c" />



<img width="1308" height="526" alt="image" src="https://github.com/user-attachments/assets/cb5553c6-d06b-44f7-8061-deeead2ac672" />



<img width="1308" height="526" alt="image" src="https://github.com/user-attachments/assets/df85c34b-75a2-4d8e-b900-524070c7b154" />



<img width="1308" height="526" alt="image" src="https://github.com/user-attachments/assets/a36b3a05-19d1-45d9-aa86-bd1d23ea96bb" />
