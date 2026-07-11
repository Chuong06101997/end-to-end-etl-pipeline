# HR Analytics ETL Pipeline

An end-to-end ETL pipeline that integrates HR analytics data from multiple heterogeneous sources into a unified analytical database for downstream reporting and analysis.

## Project Overview

This project demonstrates the implementation of an end-to-end ETL (Extract, Transform, Load) pipeline using Python.

The objective is to consolidate employee and training data from multiple data sources into a centralized database suitable for analytics and reporting.

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
