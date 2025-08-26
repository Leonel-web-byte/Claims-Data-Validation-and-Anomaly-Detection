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
-	Cleaned data

In a real-world dataset, it is always beneficial to return to the EHR system to find missing value before taking the decision to impute or drop them.


