## Overview
This project is my solution to the coding challenge. The objective was not only to compute KPIs,but to design a pipeline that reflects how I would approach a real production data engineering problem.

Using PySpark, the pipeline ingests raw NYC job postings data, performs cleaning and normalization, applies feature engineering, and generates analytical KPIs that can be used for downstream reporting or analytics.

The focus of the implementation is correctness, readability, and realistic design choices rather than over-engineering.

## Tech Stack
* PySpark (core data processing)
* Docker (local Spark cluster setup)
* Jupyter Notebook (analysis and orchestration)
* Python  3.10.11

## Project Structure

The solution is implemented within the provided assessment notebook ('assesment_notebook.ipynb') to align with the repository and Docker-based execution model supplied as part of the challenge.

The notebook is organized into clear sections:
- Data ingestion
- Data exploration and profiling
- Cleaning and transformations
- Feature engineering
- KPI computation
- Validation and output logic

The code is written in a modular style so that individual sections or functions could be easily extracted into standalone Python modules in a production setting.

## KPIs Implemented
The following KPIs were derived from the dataset:

- Top 10 job postings by job category
- Salary distribution per job category
- Relationship between higher degree requirements and salary
- Highest paying job posting per agency
- Average salary per agency for the last two years
- Highest paid skills observed in the market

Each KPI is computed using Spark transformations and aggregated into readable outputs for inspection.


## Data Processing & Feature Engineering
Key processing steps include:

- Schema normalization and explicit type casting
- Salary normalization across different salary frequencies
- Handling missing or inconsistent salary ranges
- Extraction and explosion of skills from free-text fields
- Derivation of analytical features such as:
  - Annualized salary
  - Salary bands
  - Degree requirement flag
  - Posting year

Columns not required for analytics were removed after profiling to keep the curated dataset lean and focused.


## Execution Instructions
1. Start the local Spark cluster using Docker (as provided in INSTALL.md)
2. Open the Jupyter Notebook interface
3. Run the notebook cells sequentially from top to bottom

# Note: 
On some local Docker + Windows setups, writing files to mounted directories may fail due to filesystem permission limitations The write logic is intentionally included and validated logically, even if it does not execute successfully in all local environments.


## Testing Approach
Basic test cases are included to validate transformation logic using small in-memory Spark DataFrames.

For a production system, these checks would typically be extended and executed as part of a CI pipeline using Spark test fixtures.


## Deployment Proposal
In a production environment, this pipeline could be deployed as:

- A containerized Spark job
- Scheduled via Airflow, Databricks Jobs, or a Kubernetes CronJob
- Config-driven for input paths, thresholds, and output locations
- Writing curated data to cloud storage (S3 / ADLS / HDFS)

Monitoring would include:
- Row count validation
- Salary anomaly checks
- Job failure alerts


## Trigger Strategy
The pipeline could be triggered:
- On a schedule (daily / weekly batch)
- On arrival of new raw data
- Via an orchestration tool such as Airflow

For production, the notebook logic would be refactored into a Python entrypoint while keeping the same transformation functions.


## Notes
Design decisions, assumptions, challenges, and learnings are documented in 'MyDocument.md'.