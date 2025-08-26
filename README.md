# Claims Data Validation and Anomaly Detection

## Objective: Identify anomalies in healthcare claims data and ensure data integrity.

## Question KPIs:
-	What percentage of claims are underpaid vs. overpaid relative to the allowed amount?
-	What percentage of claims have a payment variance greater than $100 compared to those with variance at or below $100?
-	What are the root causes?
## Steps:
# Data Cleaning:
-	Standardized   all dates to “mm/dd/yyyy” format to ensure accuracy. For this project, I used date of services (DOS) as claim submission date.
-	Applied conditional formatting to highlight missing values in key categories.
-	Dropped claims missing patients ID since they cannot be linked.
-	Imputed 8 missing Billed Amounts with the median $2631.5 to maintain data consistency.”
-	Dropped Claims missing Status and Insurance type information to preserve integrity. 
-	Cleaned data: <a href= "https://github.com/Leonel-web-byte/Claims-Data-Validation-and-Anomaly-Detection/blob/main/synthetic_claims_P.xlsx"> View Dataset</a>

In a real-world dataset, it is always beneficial to return to the EHR system to find missing value before taking the decision to impute or drop them.

## Created new fields:
To compute and display the percentage of Claims underpaid and those overpaid, I created two new columns, one called “Flagged Error”, which contains two categories, Underpayment and Overpayment. The second column was called “Variance”, which is the difference between the paidAmount column and the AllowedAmount column. So, when Variance is positive, it means that PaidAmount is greater than AllowedAmount and it is overpaid. However, if PaidAmount is less than AllowedAmount, it is underpaid.

I also created “Week” and “Flag_high_variance” column. “Week” column allowed me to extract week from DOS for each claim and “Flag_high_variance” allowed me by using the excel function ABS(), to identify which claim has payment under or over the threshold $100.

## Data validation and SQL
I used the database PostgreSQL to identify from my table I created base on the dataset, Claims answering KPIs Questions we asked earlier.

## SQL syntax:

## /*Total Claims*/
- SELECT COUNT("ClaimID")
- FROM public."CB";

## /*Anomaly Detection*/
- SELECT "ClaimID","PaidAmount","AllowedAmount","PaidAmount" -"AllowedAmount" as Variance, 
- CASE 
- WHEN ("PaidAmount" -"AllowedAmount")>0 THEN 'Overpayment'
- WHEN ("PaidAmount" -"AllowedAmount")<0 THEN 'Underpayment'
- ELSE 'No Issue'
- END as Flagged_Error
- FROM public."CB";

## /*Root Causes A*/ 
- SELECT "ClaimID","InsuranceType",Round(AVG("PaidAmount" -"AllowedAmount"),2) as Root, COUNT("ClaimID")
- FROM public."CB"
- GROUP BY "InsuranceType" ,"ClaimID";

## /*Root Causes B*/
- SELECT "ClaimID","InsuranceType",EXTRACT(week from "DOS") as Weeknum
- FROM public."CB"
- ORDER BY Weeknum, "ClaimID", "InsuranceType";




