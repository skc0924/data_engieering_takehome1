Implementation Summary
1️⃣ Data Ingestion (Raw / Bronze Layer)
The raw dataset (nyc-jobs.csv) was first ingested into Spark.

Deterministic data types

Avoidance of schema drift

Improved performance compared to inferSchema

The raw dataset was stored as the Bronze layer without transformation for traceability and reproducibility.

2️⃣ Column Standardization
From a Data Engineering best practice perspective:

All column names were standardized:

Converted to lowercase

Spaces replaced with underscores

Special characters removed

Multiple underscores normalized

This ensures:

Consistent naming convention (snake_case)

SQL compatibility

Avoidance of backticks in queries

Improved readability and maintainability

3️⃣ Data Cleaning & Processing (Silver Layer)
The dataset was processed with the following steps:

Trimmed whitespace from all string columns.

Cast salary fields (salary_range_from, salary_range_to) to numeric types.

Converted date fields (e.g., posting_date) into proper date format.

Removed duplicate records.

Removed null values from critical fields (such as salary columns) to ensure accurate KPI calculations.

Derived additional columns required for analytical computation.

The processed dataset was stored as the Silver layer in Parquet format.

4️⃣ Feature Engineering Applied
The following features were engineered:

Average Salary → Derived from salary range columns.

Salary Range Difference → Measures salary band spread.

Masters Degree Indicator → Derived from qualification text.

These features improved analytical capabilities and avoided repetitive computation during KPI processing.

5️⃣ KPI Processing (Gold Layer)
Each KPI was implemented as a separate modular function:

Top 10 job postings per category

Salary distribution per category

Correlation between higher degree and salary

Highest salary per agency

Average salary per agency (last 2 years)

Highest paid skills

For each KPI:

A separate output dataset was generated.

The results were stored in the Gold layer.

All outputs were written in Parquet format for performance and storage efficiency.

6️⃣ Visualization
Matplotlib was used to visualize:

Salary distribution

Top job categories

This helped in understanding trends and validating analytical outputs.

Visualization supports exploratory analysis and technical discussion.

Storage Format Decision
Parquet format was selected because:

Columnar storage

High compression

Reduced storage footprint

Faster analytical queries

Industry-standard format for data lakes

Assumptions
Salary values represent annual compensation.

Qualification text parsing using keyword search (e.g., "master") is sufficient for degree identification.

Preferred skills are comma-separated.

Duplicate records represent redundant entries and can be removed.

Salary rows with null values were excluded for KPI accuracy.

Posting date follows a consistent format.

Challenges Faced
Handling multi-line text fields during ingestion.

Parsing semi-structured skills data.

Identifying qualification indicators from free-text fields.

Ensuring proper data type casting before KPI calculations.

Managing null salary values without affecting business insights.

Designing modular KPI functions within time constraints.

Learnings
Importance of schema enforcement over automatic inference.

Value of layered architecture (Bronze → Silver → Gold).

Significance of feature engineering in analytical tasks.

Importance of consistent column naming standards.

Proper storage format selection impacts performance significantly.

Modular design improves maintainability and readability.

Test Cases Note
Due to time constraints, full unit test coverage was not implemented. However:

All KPI logic was designed in modular functions.

Functions are independently testable.

The structure allows easy addition of unit tests in future iterations.