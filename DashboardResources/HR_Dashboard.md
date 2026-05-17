# Project based on creation of "Employee Attrition Dashboard"

## ✅ Data Gathering:
* ☑️ In PBIDW goto "Get Data" and load the csv file named "HR_Analytics_Dataset.csv".
* ☑️ It contains 1480 rows and 36 columns.
* ☑️ Dataset Attribute Descriptions:

| **Column Name**            | **Description**                                                                                    |
| -------------------------- | -------------------------------------------------------------------------------------------------- |
| `EmpID`                    | **Employee ID** – Unique identifier for each employee. E.g., `RM297`, `RM302`                      |
| `Age`                      | Employee’s age in years. E.g., `18`, `19`                                                          |
| `Attrition`                | Indicates whether the employee has left (`Yes`) or is still with the company (`No`).               |
| `BusinessTravel`           | Frequency of travel required: `Travel_Rarely`, `Travel_Frequently`, `Non-Travel`                   |
| `DailyRate`                | Daily earning or billing rate of the employee. E.g., `230`, `1306`                                 |
| `Department`               | Department the employee works in: `Sales`, `Research & Development`, etc.                          |
| `DistanceFromHome`         | Distance from employee's home to workplace (in kms or miles). E.g., `3`, `10`, `22`                |
| `Education`                | Education level as numeric category:<br>1=Below College, 2=College, 3=Bachelor, 4=Master, 5=Doctor |
| `EducationField`           | Specialization in education. E.g., `Life Sciences`, `Medical`, `Marketing`                         |
| `EmployeeCount`            | Always `1` – used as a placeholder (can be dropped).                                               |
| `EmployeeNumber`           | System-generated unique number for employee. E.g., `405`, `614`                                    |
| `EnvironmentSatisfaction`  | Satisfaction with work environment: 1 (Low) to 4 (Very High)                                       |
| `Gender`                   | Employee gender: `Male` or `Female`                                                                |
| `HourlyRate`               | Hourly rate of pay. E.g., `54`, `69`, `80`                                                         |
| `JobInvolvement`           | Level of involvement in job: 1 (Low) to 4 (Very High)                                              |
| `JobLevel`                 | Job seniority level: 1 (Entry) to 5 (Executive)                                                    |
| `JobRole`                  | Designation or role: `Sales Representative`, `Laboratory Technician`, etc.                         |
| `JobSatisfaction`          | Satisfaction with job: 1 (Low) to 4 (Very High)                                                    |
| `MaritalStatus`            | Marital status: `Single`, `Married`, or `Divorced`                                                 |
| `MonthlyIncome`            | Monthly base salary (net/gross). E.g., `1420`, `1569`                                              |
| `MonthlyRate`              | Monthly rate billed to client or internal standard rate. (Earnings with incentives, bonus, or overtime included) |
| `NumCompaniesWorked`       | Total number of previous companies worked at.                                                      |
| `Over18`                   | Indicates employee is over 18: Always `Y` (can be dropped).                                        |
| `OverTime`                 | Does the employee work overtime: `Yes` or `No`                                                     |
| `PercentSalaryHike`        | Recent percentage salary hike (increment).                                                         |
| `PerformanceRating`        | Rating in most recent performance review: 1 (Low) to 4 (Excellent)                                 |
| `RelationshipSatisfaction` | Satisfaction with colleagues: 1 (Low) to 4 (Very High)                                             |
| `StandardHours`            | Always `80` (default full-time hours in many orgs) – can be dropped                                |
| `StockOptionLevel`         | Stock options given: 0 (None) to 3 (High)                                                          |
| `TotalWorkingYears`        | Total years of work experience                                                                     |
| `TrainingTimesLastYear`    | Number of training programs attended last year                                                     |
| `WorkLifeBalance`          | Rating of balance between work and personal life: 1 (Bad) to 4 (Excellent)                         |
| `YearsAtCompany`           | Total years worked in this company                                                                 |
| `YearsInCurrentRole`       | Years spent in current job role                                                                    |
| `YearsSinceLastPromotion`  | Years since the employee was last promoted                                                         |
| `YearsWithCurrManager`     | Years working with the current manager                                                             |

<hr>

## ✅ Data Cleaning:
* ☑️ In PQEW, under View tab enable "Column quality". Check null values in all the columns and delete the column "YearsWithCurrManager" as it is containing 4% null values and also not required column. (Optional)
* ☑️ Do the Group by on "EmpID" with row count to check for duplicate rows. And found 10 duplicate rows on "EmpID". Delete this step.
* ☑️ In the main query table select all the columns and go for "Remove Duplicates". Now after remove duplicates 1480 rows will be reduced to 1473 rows.
* ☑️ Check for misspelled entries. Under column "BusinessTravel" there we shall replace "TravelRarely" with "Travel_Rarely".
* ☑️ Select all columns and goto "Transform" and "Detect Data Type" to assign right datatypes to all the columns. Now click "Close & Apply" and return to PBIDW. 
* ☑️ In PBIDW insert following columns:
  * 🔰 Insert column **AttritionFlag** against formula **AttritionFlag = IF(HR_Analytics_Dataset[Attrition] = "Yes", 1, 0)**
  * 🔰 Insert column **SalarySlab** against formula **SalarySlab = if(HR_Analytics_Dataset[MonthlyIncome] <= 5000, "Upto 5K", if(HR_Analytics_Dataset[MonthlyIncome] <= 10000, "5K+ to 10K", if(HR_Analytics_Dataset[MonthlyIncome] <= 15000, "10K+ to 15K", "15K+")))**
  * 🔰 Insert column **AgeGroup** against formula **AgeGroup = if(HR_Analytics_Dataset[Age] <= 25, "18-25", if(HR_Analytics_Dataset[Age] <= 35, "26-35", if(HR_Analytics_Dataset[Age] <= 45, "36-45", if(HR_Analytics_Dataset[Age] <= 55, "46-55","55+"))))**
  * 🔰 Create a measure variable **AttritionRate** against this DAX formula **AttritionRate = DIVIDE(SUM(HR_Analytics_Dataset[AttritionFlag]), COUNT(HR_Analytics_Dataset[EmpID]))**. Make it of % type and with one decimal digit.
  * 🔰 Otherwise we can create the same measure variable **AttritionRate** againstthe DAX formula **AttritionRate = DIVIDE(CALCULATE(COUNT(HR_Analytics_Dataset[EmpID]), HR_Analytics_Dataset[Attrition] = "Yes"),
       COUNT(HR_Analytics_Dataset[EmpID]))**. Make it of % type and with one decimal digit.

<hr>

## ✅ Dashboard Preparation Steps:
* ☑️ Load canvas background image "HR_Dashboard_Background.jpg" with **Image fit:** Stretch
* ☑️ For color code refer: <https://colorhunt.co/>
* ☑️ Color code used here: **FFB4B4** <img width="97" height="48" alt="image" src="https://github.com/user-attachments/assets/58950f9f-c642-4d12-9acd-38dbdc4b532a" /> and **E66C37** <img width="106" height="50" alt="image" src="https://github.com/user-attachments/assets/7cc65795-efa1-4204-9dee-cf6653ffbafa" />

<img width="1288" height="728" alt="image" src="https://github.com/user-attachments/assets/c3e3d3b3-1508-4007-b7b0-c7b9c23262dc" />

* ☑️ Insert **Text Box** with **Text:** HR ANALYTICS DASHBOARD, **Font:** Segoe (Bold), **Font size:** 20, **Bold:** Enabled, **Background:** No, **Color:** White, **Height:** 56, **Width:** 406
* 🔰 1. Insert **Card** with **Fields:** Count (Distinct) of EmpID, **Callout value (Color):** FFB4B4, **Category label:** No, **Height:** 97, **Width:** 175, **Title:** Count of Employees, **Text Color:** FFB4B4, **Visual Border Color:** FFB4B4, **Rounded corners:** 13 px, **Width:** 3 px
* 🔰 2. Insert **Card** with **Fields:** Sum of AttritionFlag, **Title:** Attrition
* 🔰 3. Insert **Card** with **Fields:** AttritionRate, **Title:** Attrition Rate
* 🔰 4. Insert **Card** with **Fields:** Average of Age, **Title:** Avg. Age, **Value decimal places:** 0
* 🔰 5. Insert **Card** with **Fields:** Average of MonthlyIncome, **Title:** Avg. Salary, **Value decimal places:** 1
* 🔰 6. Insert **Card** with **Fields:** Average of YearsAtCompany, **Title:** Years at Company, **Value decimal places:** 1

<img width="1288" height="728" alt="image" src="https://github.com/user-attachments/assets/8e678ece-4931-4444-89f2-521cc463593d" />

* ☑️ Insert **Donut chart** with **Legend:** EducationField, **Value:** Sum of AttritionFlag, **Detail labels Values:** <Category, percentage of total>, Bold, FFB4B4, 0, **Height:** 263, **Width:** 354, **Title:** Attrition by Education, **Text Color:** FFB4B4, Center aligned, **Visual Border Color:** FFB4B4, **Rounded corners:** 13 px, **Width:** 3 px
* ☑️ Insert **Stacked column chart** with **X-axis:** AgeGroup, **Y-axis:** Sum of AttritionFlag, **X-axis & Y-axis:**, Bold, FFB4B4, **Height:** 263, **Width:** 533, **Title:** Attrition by Age, **Text Color:** FFB4B4, Center aligned, **Visual Border Color:** FFB4B4, **Rounded corners:** 13 px, **Width:** 3 px

<img width="1288" height="728" alt="image" src="https://github.com/user-attachments/assets/a747bc94-3018-4d2f-aa26-b73b332a6fd7" />

* ☑️ Insert **Clustered bar chart** with **Y-axis:** SalarySlab, **X-axis:** Sum of AttritionFlag, **X-axis & Y-axis:**, Bold, FFB4B4, **Height:** 263, **Width:** 533, **Title:** Attrition by Salary Slab, **Text Color:** FFB4B4, Center aligned, **Visual Border Color:** FFB4B4, **Rounded corners:** 13 px, **Width:** 3 px
* ☑️ Insert **Area chart** with **X-axis:** YearAtCompany, **Y-axis:** Sum of AttritionFlag, **X-axis & Y-axis:**, Bold, FFB4B4, **Lines color:** E66C37, **Height:** 284, **Width:** 532, **Title:** Attrition by Years at Company, **Text Color:** FFB4B4, Center aligned, **Visual Border Color:** FFB4B4, **Rounded corners:** 13 px, **Width:** 3 px, **Filters on this visual:** is less than 12

<img width="1288" height="728" alt="image" src="https://github.com/user-attachments/assets/696e9a79-bb46-4d16-bfcd-5202a0b0ccf7" />

* ☑️ Insert **Clustered bar chart** with **Y-axis:** JobRole, **X-axis:** Sum of AttritionFlag, **X-axis & Y-axis:**, Bold, FFB4B4, Adjust Y-axis width to 37%, **Height:** 284, **Width:** 362, **Title:** Attrition by Job Role, **Text Color:** FFB4B4, Center aligned, **Visual Border Color:** FFB4B4, **Rounded corners:** 13 px, **Width:** 3 px, **Filters on this visual:** JobRole, Top 5, By value Sum of AttritionFlag
* ☑️ Insert **Matrix** with **Rows:** JobRole, **Columns:** JobSatisfaction, **Values:** Sum of AttritionFlag, **Values Text color and Alternate Text Color:** Bold, FFB4B4 and E66C37, **Height:** 259, **Width:** 362, **Visual Border Color:** FFB4B4, **Rounded corners:** 13 px, **Width:** 3 px

<img width="1288" height="728" alt="image" src="https://github.com/user-attachments/assets/b4daab05-e190-4fcd-904e-3581a8b3dfab" />

* ☑️ Insert **Treemap** with **Category:** Gender, **Values:** Sum of AttritionFlag, **Colors:**, FFB4B4 and E66C37, **Height:** 142, **Width:** 185, **Title:** Attrition by Gender,  **Visual Border Color:** FFB4B4, **Rounded corners:** 13 px, **Width:** 3 px
* ☑️ Insert **Slicer** with **Field:** Department, **Colors:**, FFB4B4 and 6B2328, **Height:** 56, **Width:** 692, **Visual Border Color:** FFB4B4, **Rounded corners:** 13 px, **Width:** 3 px

<img width="1288" height="728" alt="image" src="https://github.com/user-attachments/assets/55c38a00-5547-4939-9f19-f8ba618d9f71" />
