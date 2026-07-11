# HR Analytics ETL Pipeline

An end-to-end ETL pipeline that integrates HR analytics data from multiple heterogeneous sources into a unified analytical database for downstream reporting and analysis.


## Project Overview

This project demonstrates the implementation of an end-to-end ETL (Extract, Transform, Load) pipeline for HR Analytics.

The pipeline integrates data from multiple heterogeneous sources into a centralized SQLite analytical database, creating a clean and consistent dataset for downstream reporting, business analysis, and predictive modeling.

This project was completed as part of the Swiss Coding Academy Data Analytics Program.
# Business Problem

A data science training company collects learner information from multiple independent systems, including Google Sheets, Excel files, CSV files, MySQL databases, and web sources.

Because these datasets are maintained separately, analysts face challenges such as:

- Inconsistent data formats
- Missing values
- Duplicate information
- Fragmented reporting

The objective of this project is to automate the ETL process, improve data quality, and prepare an integrated dataset that supports HR analytics and employment analysis.
---
<img width="562" height="461" alt="image" src="https://github.com/user-attachments/assets/e7ce7c8c-337c-4b44-a9fc-7d58a4818249" />

## Data Sources
### 1. Enrollies' data
As enrollies are submitting their request to join the course via Google Forms, we have the Google Sheet that stores data about enrolled students, containing the following columns:

enrollee_id: unique ID of an enrollee
full_name: full name of an enrollee
city: the name of an enrollie's city
gender: gender of an enrollee
The pipeline extracts data from multiple heterogeneous sources:
https://docs.google.com/spreadsheets/d/1VCkHwBjJGRJ21asd9pxW4_0z2PWuKhbLR3gUHm-p4GI/edit?usp=sharing

- Google Sheets
- Excel
- CSV
- MySQL Database
- HTML Web Table
