# MS-PowerBI Classwork Resources

<hr>

- ✅ **GitHub Link:** <https://github.com/toarnabtrainer/Tata-Power-Power-BI> or <https://tinyurl.com/3fr8a75r>
- ✅ **Online Session MS-Teams Link for all sessions:** <https://tinyurl.com/2s3yxjnr>
* ✅ **G-Meet Link:** <https://meet.google.com/ugx-iskt-vbc>
- ✅ **Notepad.pw Link:** <https://notepad.pw/p5Achs3EAtvkDLsbUxOg>
  
<hr>

## MS-Power BI Resources:

**Power BI Download and Tutorial Links:**<br>
* **☑️ PowerBI Download Link:** https://www.microsoft.com/en-us/download/details.aspx?id=58494 <br>
* **☑️ Tutorial and Official Documentation on MS-Power BI Link:** https://docs.microsoft.com/en-us/power-bi/desktop-create-and-manage-relationships <br>
* **☑️ Learn Power BI:** https://powerbi.microsoft.com/en-us/learning/ <br>
* **☑️ Power BI for Absolute Beginners:** https://www.tutorialspoint.com/power_bi/index.htm <br>
* **☑️ Microsoft E-Book on MS-Power BI Download Link:** https://download.microsoft.com/download/0/8/1/0816F8D1-D1A5-4F60-9AF5-BC91E18D6D64/Microsoft_Press_ebook_Introducing_Power_BI_PDF_mobile.pdf

**Important Classwork Resources:**<br>
* **☑️ HTML Color Codes Web-Link-1:** <https://colorhunt.co/> <br>
* **☑️ HTML Color Codes Web-Link-2:** <https://htmlcolorcodes.com/> <br>
* **☑️ Dashboard Background Image Lnk:** <https://www.freepik.com/free-photos-vectors/dashboard-background> <br>
* **☑️ PDF Link on the Cloud:** <https://racemedia1.s3.ap-south-1.amazonaws.com/wp-content/uploads/20240103171943/Countries-Capitals.pdf> <br>
* **☑️ Web Portal Link-1:** <https://worldpopulationreview.com/countries>
* **☑️ Web Portal Link-2:** <https://www.contextures.com/xlsampledata01.html>
* **☑️ Web Portal Link-3:** <http://arnabchakraborty.info/prog_data/iris.html>

**MS-Power BI Data Compresion Capability:**<br>
* **☑️ PracticeData1.xlsx ->** Size 705 KB <br>
* **☑️ ClassWork.pbix (After all data import) ->** Size 64 KB <br>
* **☑️ ClassWork.pbix (After Unpivot) ->** Size 56 KB <br>
* **☑️ ClassWork.pbix (After Country_Sales Query creation) ->** Size 61 KB <br>
* **☑️ ClassWork.pbix (After Total_Sales_PQEW) ->** Size 65 KB <br>
* **☑️ ClassWork.pbix (After Total_Sales_PBIDW) ->** Size 66 KB <br>
* **☑️ ClassWork.pbix (After Total_Sales measure creation) ->** Size 66 KB <br>
* **☑️ ClassWork.pbix (After Total_Sales_Measure column creation) ->** Size 67 KB

**Table Formats:**<br>
<b>
<pre>
* ☸️ Wide Format (Unstacked Format) ------------------------> Long Format (Stacked Format)
                                        Unpivot Operation

* ☸️ Long Format (Stacked Format) ------------------------> Wide Format (Unstacked Format)
                                        Pivot Operation
</pre>
</b>

**Assignment on Merge operations:**<br>
* **🔴 Query-1:** List those product details which has no discount. <br>
<br>**Output:** ![image](https://github.com/toarnabtrainer/Data_Sources/assets/111301975/eb4c8b10-b987-4991-904a-d34bc240cca6) <br>
* **🔴 Query-2:** List only those discounted product details along with discount amount and discounted price. <br>
<br>**Output:** ![image](https://github.com/toarnabtrainer/Data_Sources/assets/111301975/050f2529-37db-471f-ac10-ff18360a74a5) <br>
* **🔴 Query-3:** Against all products, display discount amount and discounted price. Fill with 0 where discount % is null. <br>
<br>**Output:** ![image](https://github.com/toarnabtrainer/Data_Sources/assets/111301975/a5eb2259-e78e-4a1d-94c1-18d2da8c7ffe) <br>
* **🔴 Query-4:** List those category details which has no product. <br>
<br>**Output:** ![image](https://github.com/toarnabtrainer/Data_Sources/assets/111301975/6f144d15-1dd3-41e8-828e-574c07f2af75)

<hr>

**DAX Functions:** <br>
Import RN_East1 worksheet from PracticeData.xlsx and unpivot all month columns. Rename new columns as Month and EastSales. <br><br>

* ✔️ TotalSales = SUM(RN_East1[EastSales])  // SUM(Coolumn Name) <br>
* ✔️ AverageSales = AVERAGE(RN_East1[EastSales])  // AVERAGE(Column Name) <br>
* ✔️ CountSales = COUNT(RN_East1[EastSales])   // COUNT(Column Name) <br>
* ✔️ MaxSales = MAX(RN_East1[EastSales])   // MAX(Column Name) <br>
* ✔️ MinSales = MIN(RN_East1[EastSales])   // MIN(Column Name) <br>
* ✔️ SDSales = STDEV.S(RN_East1[EastSales])   // STDEV.S(Column Name) <br>
* ✔️ VarSales = VAR.S(RN_East1[EastSales])   // VAR.S(Column Name) <br>
* ✔️ SDSales1 = SQRT(VAR.S(RN_East1[EastSales]))   // SQRT(VAR.S(Column Name)) <br>
* ✔️ VarSales1 = STDEV.S(RN_East1[EastSales])^2   // STDEV.S(Column Name) ^ 2 <br>
* ✔️ GSTSalesSumX = SUMX(RN_East1, RN_East1[EastSales] * 0.18)   // SUMX(Table Name, Expression) <br>
* ✔️ GSTSalesAvgX = AVERAGEX(RN_East1, RN_East1[EastSales] * 0.18)   // AVERAGEX(Table Name, Expression) <br>
* ✔️ GSTSalesMinX = MINX(RN_East1, RN_East1[EastSales] * 0.18)   // MINX(Table Name, Expression) <br>
* ✔️ GSTSalesMaxX = MAXX(RN_East1, RN_East1[EastSales] * 0.18)   // MAXX(Table Name, Expression) <br>
* ✔️ CountOfRows = COUNTROWS(RN_East1)   // COUNTROWS(Table Name) <br>
* ✔️ CountXOfRows = COUNTAX(RN_East1, RN_East1[EastSales] * 0.18)   // COUNTAX(Table Name, Expression) <br>
* ✔️ CalculateMinGST = CALCULATE([TotalSales] * 0.18, RN_East1[Month] = "AUG", RN_East1[Product] = "P013")   // CALCULATE(Expression, Filter1, Filter2, ...) <br>
* ✔️ FilterMinGST = MINX(FILTER(RN_East1, RN_East1[Month] = "AUG"), [TotalSales] * 0.18)   // FILTER(Table Name, Filter) <br>
* ✔️ TotalSalesAll = CALCULATE(sum(RN_East1[EastSales]), ALL(RN_East1))   // ALL(Table Name[<Column>]) <br>

<br>

☸️ Create a Table with (Month, EastSales, TotalSales, GSTSalesSumX, GSTSalesMinX, GSTSalesMaxX)<br>
☸️ Create a Multi-row Card with all these 18 measures. <br>

<hr>

**☸️ Lookup/Dimension Tables and Data/Fact Tables:** <br>
Lookup Tables or Dimension Tables will have Primary Keys, will answer Who, What, Where, When and How <br>
Data Tables or Fact Table will have Foreign Keys, and will contain transactional data <br> <br>
**☸️ Lookup/Dimension Tables:** Customer (Who), Product (What), Territories (Where), Calendar (When and How) <br>
**☸️ Data/Fact Tables:** Sales, Budget

**Operations on Budget Workbook:** <br>
* ✔️ Delete the promoted header <br>
* ✔️ Delete first 3 null rows <br>
* ✔️ Promote the first row as a header <br>
* ✔️ Delete last total column <br>
* ✔️ Filter out rows from the first column containing “Total” <br>
* ✔️ Unpivot month columns labelled from Jan to Dec <br>
* ✔️ Rename new created columns to Month and BudgetAmount <br>
* ✔️ Change the data type of Month column to Date <br>
* ✔️ Select close and apply <br>

**Data Relationships for the Budget Project:** <br>
* ✔️ Sales(CustomerKey) <- Customer(CustomerKey) <br>
* ✔️ Sales(OrderDate) <- Calendar(Date) <br>
* ✔️ Sales(ProductKey) <- Product(ProductKey) <br>
* ✔️ Sales(SalesTerritoryKey) <- Territories(SalesTerritoryKey) <br>
* ✔️ Budget(ProductKey) <- Product(ProductKey) <br>
* ✔️ Budget(Month) <- Calendar(Date) <br>

**Suggested DAX Formulaes for Creating Measures:** <br>
* ✔️ MyBudget = SUM(Budget[BudgetAmount]) <br>
* ✔️ MySales = SUM(Sales[SalesAmount]) <br>
* ✔️ MyVariance = [MySales] - [MyBudget] <br>
* ✔️ MyVariance% = DIVIDE([MyVariance], [MyBudget], 0) <br>
* ✔️ MyComments = IF([MyVariance] < -100000, "Take Care", IF([MyVariance] < 0, "Not OK", "OK"))

**Suggested Tables Summaries (After filtering on Year 2016):** <br>
*	**✔️ Table1:** Calendar[Year], Calendar[Month], Sum of Sales[SalesAmount]  <br>
*	**✔️ Table2:** Calendar[Year], Calendar[Month], Sum of Budget[BudgetAmount] <br>
*	**✔️ Table3:** Territories[Country], Sum of Sales[SalesAmount] <br>
*	**✔️ Table4:** Calendar[Year], Calendar[Month], Sum of Sales[SalesAmount], Sum of Budget[BudgetAmount] <br>
*   **✔️ Table5:** Calendar[Year], Calendar[Month], Budget[MySales], Budget[MyBudget], Budget[MyVariance], Budget[MyVariance%], Budget[MyComment]

<hr>

# Different Join Operations:

![image](https://github.com/toarnabtrainer/Data_Sources/assets/111301975/5715ba40-1546-40d7-984f-3002d411cc57)

<hr>

![image](https://github.com/toarnabtrainer/Data_Sources/assets/111301975/e2a3d88f-e1ab-4459-b1ec-28fce08a21b1)

<hr>

Here’s a well-rounded list of **trusted portals** where we can collect datasets for our ML projects — covering free, academic, and domain-specific sources:


### **♻️ 1. General & Open Data Portals**

| Portal                              | What it Offers                                                                | Link                                                                                   |
| ----------------------------------- | ----------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| **Kaggle**                          | Huge collection of user-uploaded datasets, competitions, notebooks, and code. | [https://www.kaggle.com/datasets](https://www.kaggle.com/datasets)                     |
| **UCI Machine Learning Repository** | Classic datasets used in ML research and education.                           | [https://archive.ics.uci.edu](https://archive.ics.uci.edu)                             |
| **Google Dataset Search**           | Search engine dedicated to finding public datasets across the internet.       | [https://datasetsearch.research.google.com](https://datasetsearch.research.google.com) |
| **Data.gov**                        | US Government’s open data repository; varied categories.                      | [https://www.data.gov](https://www.data.gov)                                           |
| **Open Data Portal (EU)**           | European Union’s public datasets.                                             | [https://data.europa.eu](https://data.europa.eu)                                       |


### **♻️ 2. Domain-Specific Sources**

| Domain                  | Portal                                                        | Link                                                                                                                             |
| ----------------------- | ------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **Finance & Economics** | Yahoo Finance (via APIs), World Bank Open Data                | [https://finance.yahoo.com](https://finance.yahoo.com), [https://data.worldbank.org](https://data.worldbank.org)                 |
| **Healthcare**          | PhysioNet (medical), WHO data                                 | [https://physionet.org](https://physionet.org), [https://www.who.int/data](https://www.who.int/data)                             |
| **Social Media**        | Twitter/X API, Reddit API (requires developer account)        | [https://developer.twitter.com](https://developer.twitter.com), [https://www.reddit.com/dev/api](https://www.reddit.com/dev/api) |
| **Geospatial**          | NASA EarthData, OpenStreetMap                                 | [https://earthdata.nasa.gov](https://earthdata.nasa.gov), [https://www.openstreetmap.org](https://www.openstreetmap.org)         |
| **E-commerce**          | Amazon Product Dataset (via AWS), Online Retail Dataset (UCI) | [Amazon AWS Datasets](https://registry.opendata.aws/)                                                                            |


### **♻️ 3. Community & Collaborative Platforms**

* **Data.world** – [https://data.world](https://data.world) (community-curated datasets)
* **Awesome Public Datasets** (GitHub) – [https://github.com/awesomedata/awesome-public-datasets](https://github.com/awesomedata/awesome-public-datasets)
* **Papers with Code Datasets** – [https://paperswithcode.com/datasets](https://paperswithcode.com/datasets)

### **♻️ 4. Python-Accessible Built-in Datasets**

These can be accessed **directly in Python** without manual download:

* **Scikit-learn** datasets → `from sklearn import datasets`
* **Seaborn** datasets → `import seaborn as sns; sns.get_dataset_names()`
* **Statsmodels** datasets → `import statsmodels.api as sm; sm.datasets`

### **♻️ 5. Premium & Subscription-based Sources**

| Portal       | What it Offers                                                                                | Link                                                 | Notes                                                                                                        |
| ------------ | --------------------------------------------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **Statista** | Aggregated statistics, market research data, industry reports from 22,500+ sources worldwide. | [https://www.statista.com](https://www.statista.com) | Many datasets are behind a paywall; students may need institutional access through their college/university. |


For Data Analytics projects, **Statista** is useful if we are working on:

* Market trends forecasting
* Business analytics and KPIs
* Consumer behavior modeling
* Industry-specific data visualizations

<hr>

## Sample Template for User Stories:

![image](https://github.com/toarnabtrainer/USA_BA/assets/111301975/847c29a0-177c-470a-8da5-59e698f02fff)

<hr>

## UML Classwork File Hands on with www.draw.io

![ATM System drawio](https://github.com/toarnabtrainer/USA_BA/assets/111301975/516f94ec-0953-4cb9-90a8-280a6a14e2af)

<hr>

## Towards Growth and Towards Success

![image](https://github.com/user-attachments/assets/078e5be0-8bfb-457b-8864-4c9fc4fcedf9)

<hr>

## 9 Time Management Tools for Leaders

![image](https://github.com/user-attachments/assets/cc9d7325-c4da-42ce-a3fb-f43993766925)

<hr>
