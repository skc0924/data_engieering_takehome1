1️⃣ Project Overview

This project implements an end-to-end data engineering pipeline using PySpark on the NYC Jobs dataset. The objective was to perform structured data processing, derive analytical insights (KPIs), and store the results in a scalable and optimized format.

The solution follows a layered architecture approach and applies data engineering best practices such as schema standardization, feature engineering, modular design, and structured output storage.


---

2️⃣ Data Processing Approach

🔹 Raw Data Ingestion (Bronze Layer)

The dataset was first read in its raw format.

No transformation was applied at this stage.

This layer preserves original data integrity.

It enables reproducibility if business logic changes in the future.



---

🔹 Column Standardization

After ingestion, all column names were standardized:
Converted to lowercase.
Removed special characters.
Replaced spaces with underscores.
Normalized inconsistent formatting.


Assumption:

Column naming should follow snake_case convention to:
Improve readability.
Avoid SQL compatibility issues.
Maintain consistency across processing layers.
Follow industry-standard data engineering practices.



---

🔹 Data Cleaning

The dataset was cleaned before performing KPI calculations:
Trimmed whitespace from string fields.
Removed duplicate records.
Cast salary fields to numeric format.
Converted date fields to proper date format.
Removed null values from critical columns (such as salary and job category).


Assumptions:

Salary values are required for KPI accuracy.
Records missing critical fields were excluded to prevent skewed analytics.
Duplicate entries represent redundant job postings.



---

3️⃣ Feature Engineering

To enhance analytical capability, the following features were engineered:

1️⃣ Average Salary

Calculated from salary range fields to create a consistent salary metric.

2️⃣ Salary Normalization

Salary values were normalized based on salary frequency (hourly/daily/annual) to ensure fair comparison.

3️⃣ Education Level Extraction

Education requirements were extracted from qualification text.

Assumption: Education level can be inferred from keyword-based parsing.

4️⃣ Skill Indicators

From the “Preferred Skills” column, specific skills were extracted:

Python

SQL

Cloud


Binary indicators were created for these skills.

Assumption: Skill presence can be identified through keyword matching in free-text fields.


---

4️⃣ KPI Implementation – Conceptual Explanation


---

KPI 1 – Top 10 Job Postings per Category

Aggregated total number of positions per job category.

Ranked categories in descending order.

Selected top 10.


Assumption: Number of positions represents real demand rather than simple row count.


---

KPI 2 – Salary Distribution per Job Category

Calculated minimum, average, and maximum salary per category.

Used normalized salary values for fair comparison.


Assumption: Normalized annual salary is appropriate for cross-category comparison.


---

KPI 3 – Correlation Between Higher Degree and Salary

Education levels were numerically encoded.

Pearson correlation was computed between education level and salary.


Assumption: Education hierarchy is ordinal and can be numerically represented.

Observation: There is a positive but not necessarily strong correlation between education level and salary.


---

KPI 4 – Highest Salary per Agency

Identified the highest paying job posting within each agency.

Used ranking logic to ensure only top result per agency is selected.


Assumption: Highest salary reflects agency compensation capability.


---

KPI 5 – Average Salary per Agency (Last 2 Years)

Filtered postings from the last 2 years based on posting date.

Calculated average salary per agency.


Assumption: “Last 2 years” was calculated relative to the maximum posting date in dataset.


---

KPI 6 – Highest Paid Skills

Analyzed postings containing Python, SQL, and Cloud.

Calculated average salary for each skill group.

Compared and identified the highest paying skill.


Assumption: Only selected skills (Python, SQL, Cloud) were considered due to scope constraints. Skill detection was based on keyword matching.

Observation: Cloud-related skills showed comparatively higher salary averages.


---

5️⃣ Target File Strategy

The solution writes outputs into a structured target folder:

Silver Layer

Cleaned and feature-engineered dataset.

Stored in Parquet format.

Partitioned by posting year for scalability.


Gold Layer

Each KPI result is stored as an individual Parquet dataset.

Reason for using Parquet:

Columnar storage.

High compression.

Reduced storage footprint.

Faster analytical queries.

Industry-standard format for data lakes.



---

6️⃣ Visualization

Matplotlib was used to visualize:

Salary distribution.

Top job categories.


Visualization helps:

Validate analytical results.

Support technical discussions.

Improve interpretability of KPIs.



---

7️⃣ Assumptions Made

1. Salary represents annual compensation after normalization.


2. Education level can be inferred using keyword matching.


3. Skills can be extracted via text parsing.


4. Duplicate records are non-essential and removable.


5. Missing salary records impact KPI accuracy and were excluded.


6. Only selected skills (Python, SQL, Cloud) were analyzed for skill-based KPI.


7. Posting date format is consistent.




---

8️⃣ Challenges Faced

Parsing semi-structured text fields (skills and qualifications).

Handling salary frequency normalization.

Designing modular KPI functions.

Managing null values without affecting insights.

Time constraints for writing full unit test coverage.



---

9️⃣ Deployment Plan (AWS-Based)

If deployed in production, the following architecture would be implemented:

Step 1: Package Application

Convert PySpark code into a structured project.

Create a ZIP artifact of the application.


Step 2: Upload to AWS S3

Upload artifact to S3 bucket.

Store raw data in S3 as data lake storage.


Step 3: Create AWS Glue Job

Configure Glue job to:

Read application code from S3.

Access raw dataset from S3.

Execute processing pipeline.

Generate Silver and Gold outputs.

Store results back into S3.



Step 4: CI/CD via GitHub

GitHub repository stores source code.

GitHub Actions can:

Package code.

Deploy artifact to S3.

Trigger Glue job update.



Step 5: Scheduling & Triggering

Glue job scheduled via:

Time-based schedule (daily/weekly).

OR event-based trigger when new file arrives in S3.



This ensures:

Scalability

Automation

Reproducibility

Enterprise-ready architecture



---

🔟 Key Learnings

Importance of structured layered architecture.

Schema and column standardization improves maintainability.

Feature engineering significantly enhances analytics.

Parquet is optimal for large-scale analytics storage.

Modular KPI design simplifies scaling and testing.

Text parsing in real-world datasets requires careful assumptions.



---

Final Summary

This solution demonstrates:

Strong data engineering fundamentals.

Clean data processing practices.

Modular and scalable design.

Analytical capability.

Production-oriented deployment thinking.

Clear documentation and assumption transparency.