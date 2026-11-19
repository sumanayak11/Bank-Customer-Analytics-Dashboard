# Bank-Customer-Analytics-Dashboard
# Bank Customer Analytics Dashboard

## 1. Project Title
Bank Customer Analytics Dashboard

## 2. Short Description / Purpose
An interactive 2-page Power BI report built to analyze bank customer data (4,014 customers, 159.62M total balance) and help the bankroll out new schemes to existing customers and attract new ones.

## 3. Tech Stack
- Power BI Desktop – Dashboard and report creation
- Power Query – Data preparation
- DAX – Calculated columns and measures (SUM, AVERAGE, COUNT, DISTINCTCOUNT, CALCULATE, SWITCH, IF)
- Data Modelling – Financial year → quarter → month hierarchy
- Row-Level Security – Role restricting data to the England region
- File Format – .pbix (Power BI file), .png / .pdf (dashboard preview)

## 4. Data Source
The dataset and business case were provided as part of a Power BI course and used for hands-on practice.

The dataset includes:
- Customer ID
- Customer Name
- Gender
- Age
- Region
- Job Classification
- Date Joined
- Balance

## 5. Business Requirement
The bank owner asked for an analysis of the bank data to produce key insights that help roll out new schemes to existing customers and attract new customers.

Questions answered:
1. Categorize customers by age (Young < 20, Middle < 40, Old < 60, Very Old 60+)
2. Categorize customers by balance (Silver < 25K, Gold < 50K, Diamond < 100K, Titanium < 150K, Platinum 150K+)
3. Average balance for each balance category
4. Number of customers in each age category
5. Number of customers in the Diamond and Middle category
6. Top five customers in the bank
7. Distribution of customers by gender and job classification
8. Region with the highest number of customers
9. Month with the highest number of customers joined
10. Average balance of customers in the Old and Gold category

## 6. KPI Requirements
- Total Customers – 4,014
- Total Balance – 159.62M
- Diamond & Middle Customer Count – 597
- Old & Gold Customer Average Balance – 36.64K

## 7. Charts Requirements

### Page 1: Dashboard
1. Total Customers by Age Category – Bar Chart (Q4)
2. Average Balance by Balance Category – Donut Chart (Q3)
3. Top 5 Customers – Column Chart (Q6)
4. Job Classification by Gender – Table (Q7)
5. Customers Joined by Month – Line Chart (Q9)
6. Customers by Region – Treemap (Q8)

Slicers: Quarter, Age Category, Region, Balance Category

### Page 2: Exploratory Data Analysis
7. Customer Count by Balance (bins) – Histogram
8. Customer Count by Age (bins) – Histogram

## 8. DAX Used

### Calculated Columns
```dax
AgeCategory = SWITCH(TRUE(),'Bank Data'[Age]<20,"Young",'Bank Data'[Age]<40,"Middle",'Bank Data'[Age]<60,"Old","veryold")

BalCategory = SWITCH(TRUE(),'Bank Data'[Balance]<25000,"Silver",'Bank Data'[Balance]<50000,"Gold",'Bank Data'[Balance]<100000,"Diamond",'Bank Data'[Balance]<150000,"Titanium","Platinum")

monthnumber = MONTH('Bank Data'[Date Joined])

fy = IF('Bank Data'[monthnumber]<=3,YEAR('Bank Data'[Date Joined])-1,YEAR('Bank Data'[Date Joined]))

QTR = VAR cm='Bank Data'[monthnumber]
RETURN
SWITCH(TRUE(),cm>=4 && cm<=6,"Q1",
cm>=7 && cm<=9,"Q2",
cm>=10 && cm<=12, "Q3","Q4")
```

A `financialy` column combines the quarter and financial year into a label (e.g. Q2-2020). The columns `monthnumber`, `fy` and `QTR` are grouped into a "financial year" hierarchy for drill-down.

### Measures
```dax
totalcustomers = COUNT('Bank Data'[Customer ID])

Totalbalance = SUM('Bank Data'[Balance])

averagebalance = AVERAGE('Bank Data'[Balance])

customersineachcategory = COUNT('Bank Data'[AgeCategory])

DMCATEGORY = CALCULATE(DISTINCTCOUNT('Bank Data'[Customer ID]),'Bank Data'[BalCategory]="Diamond",'Bank Data'[AgeCategory]="Middle")

oldgoldCATEGORY = CALCULATE([averagebalance],'Bank Data'[BalCategory]="Gold",'Bank Data'[AgeCategory]="Old")
```

## 9. Row-Level Security
A role restricts the data to the England region, so users assigned to it see only England customers. It was tested using View as in Power BI Desktop.

## 10. Key Insights

### Customer Segmentation
- Middle age group is the largest segment with 2,262 customers (56.4%), followed by Old (1,602), Very Old (96) and Young (54)
- Average balance by tier: Platinum 174,688, Titanium 117,754, Diamond 68,978, Gold 36,705, Silver 12,416
- Only 4 customers fall in the Platinum tier, so its average comes from a very small group
- 597 customers are in the Diamond and Middle category
- Old and Gold customers have an average balance of 36,639

### Customer Mix
- White Collar is the largest job group (1,955), ahead of Blue Collar (1,049) and Other (1,010)
- 2,165 male and 1,849 female customers; Blue Collar is male-heavy (794 vs 255), while White Collar is nearly even (945 male vs 1,010 female)

### Regional Analysis
- England has the most customers (2,159, 53.8%), followed by Scotland (1,124), Wales (520) and Northern Ireland (211)

### Acquisition Trends
- October had the most new customers (545)
- Monthly joins jumped from 47 in March to 222 in April, and stayed above 400 from May onwards, except June (278)

### High-Value Customers
- The top 5 customers hold balances from 150K to 183K: Dorothy Jackson, Carl Fraser, Connor North, Sebastian Arnold and Anna Randall

## 11. Acknowledgment
This project was built as hands-on practice from a Power BI course case study. The dataset and business requirement were provided by the course.

## 12. Dashboard
