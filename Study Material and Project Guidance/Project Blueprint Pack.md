# ✅ Project blueprint pack:

---

## ☑️ Project 1: Retail Banking – Customer 360 & Churn Risk Dashboard

### 🔰 1. Business Context & Problem Statement

Retail banks struggle with:

* Low product penetration per customer (1–2 products per customer vs 4–5 potential).
* Customer churn, especially among high-value segments.
* Fragmented view of customer across accounts, cards, loans, digital usage.

**Goal**: Build a **Customer 360 dashboard** that:

* Consolidates all key customer metrics.
* Highlights **churn risk** and **cross-sell opportunities**.
* Gives Relationship Managers (RMs) a prioritized list of customers to focus on.

---

### 🔰 2. Key Objectives & Questions

* What is each customer’s **total relationship value** with the bank?
* Which customers are **inactive or at risk of churn**?
* Which customers are **under-penetrated** (e.g., only a savings account, no credit card, no loan)?
* Which RMs / branches manage the highest-value customers?

**Success Criteria**

* RMs can easily see **Top 50 at-risk customers**.
* Management can see **trend of churned customers vs total base**.
* Improvement in cross-sell / retention campaigns (you can simulate in training).

---

### 🔰 3. Suggested Data Sources & Tables

You can simulate these as CSV/Excel or SQL tables:

* **Customers**

  * CustomerID, Name, Age, Gender, Segment (Mass / Affluent / HNI), RM_ID, BranchID, OnboardDate, RiskScore, KYCStatus, City, Region

* **Accounts** (CASA, Savings, Current, FD/RD)

  * AccountID, CustomerID, AccountType, OpenDate, Balance, Status (Active/Closed)

* **Loans**

  * LoanID, CustomerID, LoanType, SanctionAmount, OutstandingAmount, EMIAmount, StartDate, Status, DelinquencyFlag

* **Cards**

  * CardID, CustomerID, CardType, CreditLimit, CurrentOutstanding, IsActive, LastSwipeDate

* **Transactions (Aggregated)**

  * CustomerID, Month, TotalCredits, TotalDebits, NumTransactions, DigitalTxnCount, BranchTxnCount, LastActivityDate

* **RM / Branch tables**

  * RM_ID, RMName, BranchID
  * BranchID, BranchName, City, Region

* **Churn Label (optional, for training)**

  * CustomerID, IsChurned (Yes/No), ChurnDate

---

### 🔰 4. Data Model Design (Star Schema)

* Fact tables:

  * `FactCustomerMonth` (aggregated monthly metrics per customer)
  * `FactAccounts`, `FactLoans`, `FactCards` (optional, for more granularity)
* Dimensions:

  * `DimCustomer`, `DimBranch`, `DimRM`, `DimDate`, `DimProduct` (AccountType/LoanType/CardType as products)

Relationships:

* `DimCustomer[CustomerID]` → `FactCustomerMonth[CustomerID]`
* `DimCustomer[CustomerID]` → Accounts/Loans/Cards facts
* `DimBranch[BranchID]` → `DimCustomer[BranchID]`
* `DimRM[RM_ID]` → `DimCustomer[RM_ID]`
* Standard one-to-many from `DimDate[DateKey]` to Fact tables.

---

### 🔰 5. Important DAX Measures (Examples)

* **Total Relationship Value**

  ```DAX
  Total Relationship Value =
      SUM(Accounts[Balance])
      + SUM(Loans[OutstandingAmount] * -1)
      + SUM(Cards[CurrentOutstanding] * -1)
  ```

  (You can refine signs/logic as you like.)

* **Products per Customer**

  ```DAX
  Products per Customer =
  VAR AccountsCnt = DISTINCTCOUNT(Accounts[AccountID])
  VAR LoansCnt    = DISTINCTCOUNT(Loans[LoanID])
  VAR CardsCnt    = DISTINCTCOUNT(Cards[CardID])
  RETURN AccountsCnt + LoansCnt + CardsCnt
  ```

* **Is Inactive (Flag)**

  ```DAX
  Inactive Flag =
  IF (
      MAX(FactCustomerMonth[MonthsSinceLastActivity]) > 3,
      1, 0
  )
  ```

* **Churn Rate**

  ```DAX
  Churn Rate =
  DIVIDE (
      CALCULATE ( DISTINCTCOUNT(Churn[CustomerID]), Churn[IsChurned] = "Yes" ),
      DISTINCTCOUNT(DimCustomer[CustomerID])
  )
  ```

* **Churn Risk Score (Simple Proxy)**

  * Combine inactivity, low balances, low product count:

  ```DAX
  Churn Risk Score =
  VAR Inactive = [Inactive Flag]
  VAR LowBal   = IF ( [Total Relationship Value] < 50000, 1, 0 )
  VAR LowProd  = IF ( [Products per Customer] < 2, 1, 0 )
  RETURN Inactive * 0.5 + LowBal * 0.3 + LowProd * 0.2
  ```

---

### 🔰 6. Report Pages & Visual Design

**Page 1 – Executive Overview**

* KPI cards: Total Customers, Active Customers, Churned Customers (Last 12 months), Avg Products per Customer.
* Line chart: Churn Rate % over time.
* Bar chart: Customers by Segment & Region.
* Map: Customer distribution by Region/City.

**Page 2 – RM / Branch Performance**

* Table/Matrix: RM → #Customers, Total Relationship Value, Avg Churn Risk Score.
* Scatter plot: Branch size (customers) vs Total Relationship Value vs Avg Risk Score.
* Drill-through into RM → Customer 360 view.

**Page 3 – Customer 360 (Drill-through)**

* Customer details, KYC status, segment, RM, branch.
* Tiles: Total Relationship Value, #Products, Churn Risk Score.
* Visuals: Product portfolio breakdown, last 6 months transactions, digital vs branch usage, recent delinquency info.

**Page 4 – Churn Analysis**

* Top N at-risk customers.
* Breakdown of churned customers by segment, age group, branch.
* Funnel: At-risk → Contacted → Retained (manual or simulated).

---

### 🔰 7. Security (Row-Level Security)

* **RMs** see only **their customers**:

  ```DAX
  [RM_ID] = USERPRINCIPALNAME()  // or map via a DimUser table
  ```
* **Branch Managers** see all customers for their branch.
* **Region Heads** see all branches in their region.

You can implement this via RLS roles mapped to `DimUser`.

---

### 🔰 8. Refresh & Architecture

* Data from Core Banking / CRM exported to SQL or data warehouse (simulated).
* Power BI dataset in **Import mode** with scheduled refresh (e.g., daily).
* Optionally DirectQuery if connected to a performant data warehouse.

---

### 🔰 9. Learning Outcomes

* Build full **Customer 360 model**.
* Use **aggregation & flags** to identify at-risk customers.
* Implement **RLS for RMs and Branch Managers**.
* Design **drill-through** based detailed views.

---

## ☑️ Project 2: Loan Portfolio Quality & NPA Monitoring

### 🔰 1. Business Context & Problem

Banks must monitor:

* Loan portfolio quality.
* **NPA (Non-Performing Assets)** and delinquency by segment, product, geography.
* Early-warning signals.

**Goal**: Power BI solution for Risk / Collections team to:

* View portfolio by **Stage (0, 1, 2, 3)**, delinquency buckets.
* Track NPA ratio, provisioning, and write-off trends.

---

### 🔰 2. Data & Tables

* **Loans**

  * LoanID, CustomerID, LoanType, BranchID, SanctionAmount, DisbursedAmount, OutstandingAmount, InterestRate, Tenure, StartDate, MaturityDate, CollateralType
* **Repayments (EMI level)**

  * LoanID, DueDate, PaymentDate, EMIAmount, AmountPaid, OverdueDays
* **LoanStatus Snapshot (Monthly)**

  * LoanID, AsOfDate, Stage (0/1/2/3), IsNPA, DaysPastDueBucket (0, 1-30, 31-60, 61-90, >90)
* **Provisions**

  * AsOfDate, LoanType, Stage, ProvisionAmount
* Standard dimensions: Customer, Branch, Date, Product, Region.

---

### 🔰 3. Data Model

Fact tables:

* `FactLoanStatus` (at given AsOfDate for each loan).
* `FactRepayments` (transaction level).
  Dimensions:
* `DimLoan`, `DimCustomer`, `DimBranch`, `DimDate`, `DimProduct`.

Relationships:

* `DimLoan[LoanID]` → `FactLoanStatus[LoanID]`, `FactRepayments[LoanID]`.

---

### 🔰 4. Key Metrics (DAX)

* **Outstanding Portfolio**

  ```DAX
  Total Outstanding =
      SUM(FactLoanStatus[OutstandingAmount])
  ```

* **NPA Outstanding**

  ```DAX
  NPA Outstanding =
      CALCULATE(
          [Total Outstanding],
          FactLoanStatus[IsNPA] = 1
      )
  ```

* **NPA Ratio**

  ```DAX
  NPA Ratio =
  DIVIDE ( [NPA Outstanding], [Total Outstanding] )
  ```

* **Stage-wise Outstanding**

  ```DAX
  Stage Outstanding =
      SUM(FactLoanStatus[OutstandingAmount])
  // Sliced by FactLoanStatus[Stage]
  ```

* **Provision Coverage Ratio**

  ```DAX
  Provision Coverage Ratio =
  DIVIDE (
      SUM(Provisions[ProvisionAmount]),
      [NPA Outstanding]
  )
  ```

* **Collection Efficiency (%)**

  ```DAX
  Collection Efficiency =
  DIVIDE (
      SUM(FactRepayments[AmountPaid]),
      SUM(FactRepayments[EMIAmount])
  )
  ```

---

### 🔰 5. Report Pages

**Page 1 – Portfolio Overview**

* KPIs: Total Outstanding, NPA Outstanding, NPA Ratio, Provision Coverage.
* Column chart: Outstanding by LoanType.
* Stacked column: Stage (0–3) mix by month.
* Map: NPA ratio by Region.

**Page 2 – Delinquency Analysis**

* Matrix: Bucket (0, 1–30, 31–60, 61–90, >90) vs LoanType/Region.
* Trend line: Bucket distribution over time.
* Table: Top 50 delinquent accounts (outstanding and DPD).

**Page 3 – Collections Performance**

* Line chart: Collection Efficiency % by month.
* Bar chart: Collections vs Overdues by Branch.
* RM-wise collections table.

**Page 4 – Loan-Level Drill-through**

* For a selected LoanID:

  * Customer profile, branch, product.
  * EMI history, overdue pattern, payments timeline.

---

### 🔰 6. RLS & Governance

* Collection officers see only **their assigned accounts**.
* Risk team sees **all** loans (no RLS).
* Branch/Region heads see loans for **their geography**.

---

### 🔰 7. Learning Outcomes

* Model **time-dependent status data** (snapshots).
* Calculate **NPA ratio, provisioning, delinquency**.
* Use drill-through for loan-level analysis.
* Implement **bucket-based visuals** (Days Past Due buckets).

---

## ☑️ Project 3: Branch Performance & Profitability Dashboard

### 🔰 1. Business Context

Bank branches:

* Drive deposits, loans, fee income.
* Incur costs (staff, rent, operations).
* Must be monitored on **profitability**, not just volume.

**Goal**: Build a **Branch Scorecard**:

* Compare branches on revenue, cost, profit, productivity.
* Identify high-performing & underperforming branches.

---

### 🔰 2. Data & Tables

* **BranchMaster**

  * BranchID, BranchName, Region, City, Tier (Metro/Urban/Rural), OpeningDate.
* **BranchFinancials (Monthly)**

  * BranchID, Month, InterestIncome, FeeIncome, TradingIncome, OperatingCosts, StaffCosts, OtherCosts.
* **CustomerMetrics (per branch)**

  * BranchID, Month, #ActiveCustomers, TotalDeposits, TotalLoans, NewCustomers, ClosedCustomers.
* **Staff**

  * StaffID, BranchID, Role, JoinDate, Salary.

---

### 🔰 3. Data Model

* `FactBranchMonth` (financial & customer metrics by month).
* Dimensions: `DimBranch`, `DimDate`, `DimRegion`, `DimCity`, `DimStaff` (optional).

---

### 🔰 4. Key Measures

* **Total Income**

  ```DAX
  Total Income =
      SUM(FactBranchMonth[InterestIncome])
      + SUM(FactBranchMonth[FeeIncome])
      + SUM(FactBranchMonth[TradingIncome])
  ```

* **Total Costs**

  ```DAX
  Total Costs =
      SUM(FactBranchMonth[OperatingCosts])
      + SUM(FactBranchMonth[StaffCosts])
      + SUM(FactBranchMonth[OtherCosts])
  ```

* **Branch Profit**

  ```DAX
  Branch Profit = [Total Income] - [Total Costs]
  ```

* **Cost-to-Income Ratio**

  ```DAX
  Cost to Income =
  DIVIDE ( [Total Costs], [Total Income] )
  ```

* **Customer Productivity**

  ```DAX
  Profit per Customer =
  DIVIDE ( [Branch Profit], SUM(FactBranchMonth[#ActiveCustomers]) )
  ```

* **Staff Productivity**

  ```DAX
  Profit per Staff =
  DIVIDE ( [Branch Profit], DISTINCTCOUNT(Staff[StaffID]) )
  ```

---

### 🔰 5. Report Pages

**Page 1 – Branch Scorecard**

* KPIs: Total Income, Total Costs, Total Profit, Avg Cost-to-Income.
* Bar chart: Branch Profit by Branch (Top N).
* Scatter: Branch Profit vs Cost-to-Income, bubble size = Active Customers, color by Tier.
* Map: Profit by City/Region.

**Page 2 – Trend & Benchmarking**

* Line chart: Total Profit over time (bank vs selected region).
* Small multiples: Branch Profit trends for selected top 5 branches.
* Benchmark table: Branch vs Region average on key metrics.

**Page 3 – Branch Detail (Drill-through)**

* Financial breakdown (stacked bar by income and cost type).
* Customer metrics: growth in customers, deposits, loans.
* Staff metrics: staff count, tenure, attrition (optional).

---

### 🔰 6. RLS

* Branch Manager sees only **his/her branch**.
* Regional Manager sees all branches in **their region**.
* Top management sees **all**.

---

### 🔰 7. Learning Outcomes

* Design **scorecards & benchmarking views**.
* Use **scatter and small multiples** for performance comparisons.
* Practice **drill-through** with branch-level details.

---

## ☑️ Project 4: ATM & Digital Channel Performance Analytics

### 🔰 1. Business Context

Banks focus on:

* Reducing branch load.
* Improving **ATM uptime** and **digital adoption** (Internet/Mobile banking).
* Monitoring failed transactions & customer experience.

**Goal**: Power BI solution to:

* Track ATM usage & downtime.
* Compare branch vs digital transaction share.
* Identify locations with poor service quality.

---

### 🔰 2. Data & Tables

* **ATMMaster**

  * ATMID, LocationType (Onsite/Offsite), BranchID, City, Region, InstallationDate, Owner (Bank/Third party).
* **ATMTransactions**

  * ATMID, TxnID, TxnDateTime, TxnType (Withdrawal, Balance Enq, MiniStmt), Amount, Status (Success/Failed), CardType.
* **ATMUptimeLogs**

  * ATMID, Date, UptimeMinutes, DowntimeMinutes, ReasonCode (Network, CashOut, Hardware).
* **DigitalChannelsUsage (Monthly)**

  * CustomerID, Month, Channel (Mobile/Internet/UPI/Wallet), TxnCount, TxnAmount.
* Standard dimensions: Date, Branch, Region, Customer (optional).

---

### 🔰 3. Data Model

* `FactATMTransactions`, `FactATMUptime`, `FactDigitalUsage`.
* Dimensions: `DimATM`, `DimBranch`, `DimDate`, `DimCustomer`.

---

### 🔰 4. KPIs & DAX

* **ATM Transactions Count**

  ```DAX
  ATM Txn Count = COUNTROWS(FactATMTransactions)
  ```

* **Failure Rate**

  ```DAX
  ATM Failure Rate =
  DIVIDE (
      CALCULATE ( [ATM Txn Count], FactATMTransactions[Status] = "Failed" ),
      [ATM Txn Count]
  )
  ```

* **ATM Uptime %**

  ```DAX
  ATM Uptime % =
  DIVIDE (
      SUM(ATMUptimeLogs[UptimeMinutes]),
      SUM(ATMUptimeLogs[UptimeMinutes]) + SUM(ATMUptimeLogs[DowntimeMinutes])
  )
  ```

* **Digital Adoption Ratio**

  ```DAX
  Digital Adoption =
  DIVIDE (
      SUM(FactDigitalUsage[TxnCount]),
      SUM(FactDigitalUsage[TxnCount]) + SUM(FactBranchTransactions[TxnCount]) // branch txn fact (optional)
  )
  ```

---

### 🔰 5. Report Pages

**Page 1 – ATM Network Overview**

* KPIs: Total ATMs, Total ATM Txns, Failure Rate, Avg Uptime %.
* Map: ATM locations colored by Uptime%.
* Bar chart: Failure Rate by ReasonCode.
* Table: Top 20 ATMs with highest failures/downtime.

**Page 2 – Channel Mix & Digital Adoption**

* Stacked column: Transaction volume by Channel (ATM, Branch, Mobile, Internet).
* Line chart: Digital Adoption % over time.
* Segment breakdown: Adoption by Age group / Region.

**Page 3 – ATM Detail Drill-through**

* Selected ATM: historical uptime, transactions, failure reasons.
* Chart: Hour-of-day transaction pattern (peak usage hours).

---

### 🔰 6. Learning Outcomes

* Model operational data (uptime, failures).
* Use **ratio KPIs** (failure rate, uptime %).
* Combine **geospatial visuals** (ATMs on map) with performance measures.

---

## ☑️ Project 5: Credit Card Spend & Fraud Monitoring Dashboard

### 🔰 1. Business Context

Banks issuing credit cards must:

* Monitor spend patterns.
* Detect potential **fraudulent transactions** or unusual usage.
* See merchant/category distribution of spends.

**Goal**: Power BI solution for Cards Analytics / Fraud Team:

* High-level view of **spend, activation, delinquency**.
* Ability to drill into **suspicious transactions** patterns (e.g., cross-border, high-value at odd times).

---

### 🔰 2. Data & Tables

* **CardMaster**

  * CardID, CustomerID, CardType (Gold/Platinum/Corporate), Limit, IssueDate, Status, BillingCycleDay.
* **CardTransactions**

  * TxnID, CardID, TxnDateTime, Amount, MerchantCategory, MerchantCountry, Channel (POS/E-Com/ATM), IsForeign, AuthResult, FraudFlag (Y/N).
* **Bills & Payments**

  * CardID, StatementDate, DueDate, StatementAmount, MinDueAmount, PaymentDate, PaymentAmount.
* `DimCustomer`, `DimDate`, `DimMerchantCategory`, `DimRegion`.

---

### 🔰 3. KPIs (DAX)

* **Total Spend**

  ```DAX
  Total Spend =
      SUM(CardTransactions[Amount])
  ```

* **Active Cards**

  ```DAX
  Active Cards =
      CALCULATE(
          DISTINCTCOUNT(CardMaster[CardID]),
          CardMaster[Status] = "Active"
      )
  ```

* **Average Spend per Active Card**

  ```DAX
  Avg Spend per Card =
  DIVIDE ( [Total Spend], [Active Cards] )
  ```

* **Fraud Rate**

  ```DAX
  Fraud Txn Count =
      CALCULATE(
          COUNTROWS(CardTransactions),
          CardTransactions[FraudFlag] = "Y"
      )

  Fraud Rate =
  DIVIDE ( [Fraud Txn Count], COUNTROWS(CardTransactions) )
  ```

* **Delinquency Flag (Card-Level)**

  ```DAX
  Delinquent Card Flag =
  IF (
      MAX(Bills[PaymentDate]) > MAX(Bills[DueDate]) 
      && MAX(Bills[PaymentAmount]) < MAX(Bills[StatementAmount]),
      1, 0
  )
  ```

---

### 🔰 4. Report Pages

**Page 1 – Portfolio & Spend Overview**

* KPIs: Active Cards, Total Spend (Current Month), Delinquent Cards Count, Fraud Rate.
* Line chart: Monthly Spend trend by CardType.
* Tree map: Spend by MerchantCategory.
* Map: Spend by MerchantCountry (for foreign spends).

**Page 2 – Fraud & Risk Monitoring**

* Chart: Fraud Rate by MerchantCategory / Channel.
* Heat map: Hour of Day vs Day of Week for Fraud transactions.
* Table: Top suspicious transactions (high amount, cross-border, odd time).

**Page 3 – Customer/Card Drill-through**

* For a specific card: last 30 transactions, category distribution, payment history, delinquency status.

---

### 🔰 5. Learning Outcomes

* Time series and **category mix** analysis.
* Building **fraud-oriented views** (filters, heatmaps).
* Combining transaction and billing/payment tables.

---

## ☑️ Project 6: Regulatory Liquidity & Risk Reporting (Simplified LCR View)

### 🔰 1. Business Context

Banks must comply with regulations like:

* **LCR (Liquidity Coverage Ratio)**.
* Maintain sufficient **High-Quality Liquid Assets (HQLA)** vs net cash outflows.

**Goal**: Build a **simplified regulatory dashboard**:

* Show HQLA, expected outflows/inflows.
* Compute LCR and track it over time.
* Breakdown by major categories.

*(You can keep this at a simplified training level, not a full Basel implementation.)*

---

### 🔰 2. Data & Tables

* **LiquidityAssets**

  * AssetID, AssetType (HQLA Level 1/2A/2B), Currency, MarketValue, Haircut%, AdjustedValue, AsOfDate.
* **CashOutflows**

  * AsOfDate, ProductType (RetailDeposits, WholesaleFunding, Derivatives), ContractualOutflow, RunoffFactor%.
* **CashInflows**

  * AsOfDate, ProductType, ContractualInflow, InflowFactor%.
* `DimDate`, `DimCurrency`, `DimProductType`.

---

### 🔰 3. DAX Measures (Simplified LCR)

* **Total HQLA**

  ```DAX
  Total HQLA =
      SUM(LiquidityAssets[AdjustedValue])
  ```

* **Total Expected Outflows**

  ```DAX
  Total Outflows =
      SUMX(
          CashOutflows,
          CashOutflows[ContractualOutflow] * CashOutflows[RunoffFactor%]
      )
  ```

* **Total Expected Inflows (Capped per regs, simulate cap at 75% of outflows)**

  ```DAX
  Raw Inflows =
      SUMX(
          CashInflows,
          CashInflows[ContractualInflow] * CashInflows[InflowFactor%]
      )

  Capped Inflows =
      MIN( [Raw Inflows], [Total Outflows] * 0.75 )
  ```

* **Net Cash Outflows**

  ```DAX
  Net Outflows = [Total Outflows] - [Capped Inflows]
  ```

* **LCR (%)**

  ```DAX
  LCR % =
  DIVIDE ( [Total HQLA], [Net Outflows] ) * 100
  ```

---

### 🔰 4. Report Pages

**Page 1 – LCR Overview**

* KPIs: Total HQLA, Net Outflows, LCR %.
* Line chart: LCR % over time (AsOfDate).
* Stacked bar: HQLA by Level (1/2A/2B).
* Bar chart: Outflows & Inflows by ProductType.

**Page 2 – Asset & Liability Breakdown**

* Matrix: HQLA by Currency & AssetType.
* Matrix: Outflows by ProductType & Tenor bucket (0–30, 31–60, 61–90 days).
* Sensitivity scenario (use slicer): adjust RunoffFactors (via parameter table) and see LCR impact.

---

### 🔰 5. Learning Outcomes

* Model concepts of **assets vs cash flows**.
* Use **measure-based logic** (caps, min/max, ratios).
* Build **regulatory-style dashboards** with strong numeric focus and less “flash.”

---

## ♻️ How You Can Use These 6 Projects

Each project can be:

* A **separate module** in your training:

  * Beginner: Customer 360 & Branch Performance
  * Intermediate: Loan NPA, ATM/Digital, Credit Cards
  * Advanced: Liquidity & Regulatory Reporting

* Or a **multi-chapter capstone** where you:

  * Reuse some common dimensions (Date, Customer, Branch).
  * Show how different departments (Retail, Risk, Operations, Treasury) all use Power BI.

---
