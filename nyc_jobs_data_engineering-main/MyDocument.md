## Learnings & Assumptions

### Data Assumptions
- Salary ranges in the source data are not always complete or consistent, so records with clearly invalid salary values were filtered out.
- Salary frequency varies across postings (hourly, daily, annual), which required normalization to a common annual scale for meaningful comparison.
- Skill requirements are not structured fields and had to be inferred from free-text columns such as "Preferred Skills" and "Job Description".


### Challenges
- Salary data required careful validation to avoid misleading averages caused by missing or inverted ranges.
- Skill extraction involved working with semi-structured text, which limits precision without applying heavier NLP techniques.
- Writing output files locally using Spark inside Docker on Windows exposed filesystem permission limitations, which are documented rather than worked around.


### Design Decisions
- Transformation logic was grouped into clearly defined steps (cleaning, normalization, feature engineering, KPI computation) to keep the notebook readable and easy to follow.
- Paths and environment-specific values were kept configurable rather than hard-coded to allow the same logic to run across different environments.
- Testing was intentionally kept lightweight using small Spark DataFrames to validate critical logic without introducing a full Spark test harness.


### Improvements for Production
- Analytical thresholds (salary bands, lookback windows, skill lists) would be fully externalized into configuration files.
- Data quality checks and Spark tests would be integrated into a CI pipeline.
- Curated outputs would be written in Parquet format to cloud storage (e.g., S3, ADLS, or HDFS) for better performance and downstream consumption.
