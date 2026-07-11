#  HR Analytics: Job Change of Data Scientists
## Introduction
![image](https://github.com/user-attachments/assets/fba35d28-afd0-4314-9270-7d7ad3105853)


## Business Problem

A data science training company collects learner information from multiple independent systems including Google Sheets, Excel, CSV, MySQL databases, and web sources.

Because these datasets are stored separately, analysts face challenges such as inconsistent formats, missing values, and fragmented reporting.

The objective of this project is to automate the ETL process and prepare a clean analytical dataset for downstream HR analytics.

## Project Overview

This project demonstrates an end-to-end ETL (Extract, Transform, Load) pipeline that integrates HR data from multiple heterogeneous sources into a centralized SQLite database for downstream analytics and reporting.
The project was developed as part of the Swiss Coding Academy Data Analytics Program.
The dataset includes:
- Demographic information
- Education and major discipline
- Work experience
- Training enrollment and job change status

![image](https://github.com/user-attachments/assets/51249091-11a9-4911-9b12-db23ede8eecf)


Google Sheets
      │
Excel ───────┐
CSV ─────────┤
MySQL ───────┤
HTML ────────┘
      │
      ▼
 ETL Pipeline
      │
      ▼
 Data Cleaning
      │
      ▼
SQLite Database
      │
      ▼
Analytics


## Data Description
### 1. Enrollies' data
As enrollies are submitting their request to join the course via Google Forms, we have the Google Sheet that stores data about enrolled students, containing the following columns:

`enrollee_id`: unique ID of an enrollee

`full_name`: full name of an enrollee

`city`: the name of an enrollie's city

`gender`: gender of an enrollee

Source : <https://docs.google.com/spreadsheets/d/1VCkHwBjJGRJ21asd9pxW4_0z2PWuKhbLR3gUHm-p4GI/edit?usp=sharing>

### 2.  Enrollies' education

After enrollment everyone should fill the form about their education level. This form is being digitalized manually. Educational department stores it in the Excel format here: <https://assets.swisscoding.edu.vn/company_course/enrollies_education.xlsx>

This table contains the following columns:

`enrollee_id`: A unique identifier for each enrollee. This integer value uniquely distinguishes each participant in the dataset.

`enrolled_university`: Indicates the enrollee's university enrollment status. Possible values include no_enrollment, Part time course, and Full time course.

`education_level`: Represents the highest level of education attained by the enrollee. Examples include Graduate, Masters, etc.

`major_discipline`: Specifies the primary field of study for the enrollee. Examples include STEM, Business Degree, etc.
### 3. Enrollies' working experience
Another survey that is being collected manually by educational department is about working experience.

Educational department stores it in the CSV format here: <https://assets.swisscoding.edu.vn/company_course/work_experience.csv>

This table contains the following columns:

`enrollee_id`: A unique identifier for each enrollee. This integer value uniquely distinguishes each participant in the dataset.

`relevent_experience`: Indicates whether the enrollee has relevant work experience related to the field they are currently studying or working in. Possible values include Has relevent experience and No relevent experience.

`experience`: Represents the number of years of work experience the enrollee has. This can be a specific number or a range (e.g., >20, <1).

`company_size`: Specifies the size of the company where the enrollee has worked, based on the number of employees. Examples include 50−99, 100−500, etc.

`company_type`: Indicates the type of company where the enrollee has worked. Examples include Pvt Ltd, Funded Startup, etc.

`last_new_job`: Represents the number of years since the enrollee's last job change. Examples include never, >4, 1, etc.

### 4. Training hours

From LMS system's database you can retrieve a number of training hours for each student that they have completed.

Database credentials:

```Database type: MySQL```

```Host: 112.213.86.31```

```Port: 3360```

```Login: etl_practice```

```Password: 550814```

```Database name: company_course```

```Table name: training_hours```

### 5. City development index
Another source that can be usefull is the table of City development index.

The City Development Index (CDI) is a measure designed to capture the level of development in cities. It may be significant for the resulting prediction of student's employment motivation.

It is stored here: https://sca-programming-school.github.io/city_development_index/index.html

### 6. Employment
From LMS database you can also retrieve the fact of employment. If student is marked as employed, it means that this student started to work in our company after finishing the course.

Database credentials:

`Database type: MySQL`

`Host: 112.213.86.31`

`Port: 3360`

`Login: etl_practice`

`Password: 550814`

`Database name: company_course`

`Table name: employment`
# ELT Process
## Import Libraries
```
import pandas as pd
!pip install pymysql
from sqlalchemy import create_engine
import pymysql
```
### 1. Extract
1.1 Enrollies' data
```
google_sheet_id = '1VCkHwBjJGRJ21asd9pxW4_0z2PWuKhbLR3gUHm-p4GI'
url = 'https://docs.google.com/spreadsheets/d/'+ google_sheet_id+ '/export?format=xlsx'
enrollies_data = pd.read_excel(url, sheet_name = 'enrollies')
enrollies_data.head()
```
![image](https://github.com/user-attachments/assets/a309f2e6-96b8-40e7-abe5-7d16a4ae19f2)

1.2 Enrollies' education
```
enrollies_education = pd.read_excel('/content/enrollies_education (1).xlsx')
enrollies_education
```
![image](https://github.com/user-attachments/assets/fbdb5667-3cdd-4788-9e66-52158081cd18)

1.3 Enrollies' working experience
```
work_experience = pd.read_csv('/content/work_experience (1).csv')
work_experience
```
![image](https://github.com/user-attachments/assets/19304fac-4c8b-4b7a-b799-70488d688420)

1.4 Training hours
```
engine = create_engine('mysql+pymysql://etl_practice:550814@112.213.86.31:3360/company_course')
training_hours = pd.read_sql_table('training_hours', engine)
training_hours.head()
```
![image](https://github.com/user-attachments/assets/c4db82d7-f96f-443c-8fe1-4adad87a99d3)

1.5 City development index
```
table = pd.read_html('https://sca-programming-school.github.io/city_development_index/index.html')
cities = table[0]
cities.head()
```
![image](https://github.com/user-attachments/assets/2a727c35-d7b2-4646-92ae-81d3fc0cdd8b)

1.6 Employment
```
employment = pd.read_sql_table('employment',engine)
employment.head()
```
![image](https://github.com/user-attachments/assets/d5f3a0bf-b86c-4444-bfcc-d4a292263e89)

### 2. Transform Data
2.1 Enrollies' Data
``` enrollies_data.info() ```

![image](https://github.com/user-attachments/assets/50e8e069-95c9-4f36-9a5d-4115a3e373b5)

>`gender`: has missing values (~4,500 rows) => missing values need to be handled, currently of type object, it should be converted to category

>`full_name`: currently of type object, it should be converted to string 

>`city`: currently of type object, it should be converted to category

### Fix Data Type
```
enrollies_data['full_name'] = enrollies_data['full_name'].astype('string')
enrollies_data['city'] = enrollies_data['city'].astype('category')
enrollies_data['gender'] = enrollies_data['gender'].astype('category')
```
> Object-type columns were converted to either `string` or `category` for consistency and efficiency:
>- Columns containing free text (e.g., names) were cast to `string` for better readability and string operations.
>- Columns with a limited set of repeated values were cast to `category` to reduce memory usage and prepare for potential encoding in future analysis.

### Fill Missing Value
``` gender_mode = enrollies_data['gender'].mode()[0] ```
```
enrollies_data['gender'] = enrollies_data['gender'].fillna(gender_mode)
```
>=> Missing values in categorical columns were filled with the mode (most frequent value) because:
>- It preserves the existing distribution of the data
>- It's a simple and effective strategy when no additional context is available

``` enrollies_data.info() ```

![image](https://github.com/user-attachments/assets/9fff605f-8835-422b-9490-df535b0dbe7b)

2.2 enrollies_education
```
enrollies_education.info()
```
>`enrolled_university`: has missing values => missing values need to be handled, currently of type object, it should be converted to string

>`education_level`: has missing values => missing values need to be handled, currently of type object, it should be converted to category

>`major_discipline`: has missing values => missing values need to be handled, currently of type object, it should be converted to string

![image](https://github.com/user-attachments/assets/bcd868ef-ea62-4efb-9a22-9875506df111)

### Fix Data Type
```
enrollies_education['enrolled_university'] = enrollies_education['enrolled_university'].astype('string')
enrollies_education['education_level'] = enrollies_education['education_level'].astype('category')
enrollies_education['major_discipline'] = enrollies_education['major_discipline'].astype('string')
```
>Object-type columns were converted to either `string` or `category` for consistency and efficiency:
>- Columns containing free text (e.g., names) were cast to `string` for better readability and string operations.
>- Columns with a limited set of repeated values were cast to `category` to reduce memory usage and prepare for potential encoding in future analysis.

### Fill Missing Value
```
enrollies_education['enrolled_university'] = enrolled_university.fillna('no_enrollment')
enrollies_education['major_discipline'] = major_discipline.fillna('No Major')
education_level_mode = enrollies_education['education_level'].mode()[0]
enrollies_education['education_level'] = enrollies_education['education_level'].fillna(education_level_mode)
```
>Two specific columns were treated as exceptions during missing value imputation:
>- `enrolled_university`: missing values were filled with `'no_enrollment'` to indicate that the person is not currently enrolled in any university. This label provides a meaningful and interpretable category instead of using the mode.
>- `major_discipline`: missing values were filled with `'No Major'` to explicitly represent that the individual does not have a specific major field of study. Using a descriptive placeholder avoids ambiguity and improves clarity.
>- `education_level`: missing values were filled with the mode to preserve the existing distribution and avoid introducing bias.
```
enrollies_education.info()
```
![image](https://github.com/user-attachments/assets/7aa0ee40-9d12-49ad-8e42-80cbc9990d99)

2.3 Work_experience
```
work_experience.info()
```
![image](https://github.com/user-attachments/assets/fc6ed19f-e077-4f59-9981-52dd77cad022)

>`relevent_experience`: no missing values, currently of type object, it should be converted to category

>`experience`: has missing values => missing values need to be handled, currently of type object, it should be converted to category

>`company_size`: has missing values => missing values need to be handled, currently of type object, it should be converted to category

>`company_type`: has missing values => missing values need to be handled, currently of type object, it should be converted to category

>`last_new_job`: has missing values => missing values need to be handled, currently of type object, it should be converted to category

### Fix data type
```
work_experience['relevent_experience'] = work_experience['relevent_experience'].astype('category')
work_experience['experience'] = work_experience['experience'].astype('category')
work_experience['company_size'] = work_experience['company_size'].astype('category')
work_experience['company_type'] = work_experience['company_type'].astype('category')
work_experience['last_new_job'] = work_experience['last_new_job'].astype('category')
```
> All relevant columns in the work_experience table were converted to `category` type to reduce memory usage and improve processing efficiency, as they contain repeated discrete values.
```
experience = work_experience['experience'].mode()[0]
work_experience['experience'] = work_experience['experience'].fillna(experience)
company_size = work_experience['company_size'].mode()[0]
work_experience['company_size'] = work_experience['company_size'].fillna(company_size)
company_type = work_experience['company_type'].mode()[0]
work_experience['company_type'] = work_experience['company_type'].fillna(company_type)
last_new_job = work_experience['last_new_job'].mode()[0]
work_experience['last_new_job'] = work_experience['last_new_job'].fillna(last_new_job)
```
>Missing values in categorical columns were filled with the mode (most frequent value) because:
>- It preserves the existing distribution of the data
>- It's a simple and effective strategy when no additional context is available.

```
work_experience.info()
```
![image](https://github.com/user-attachments/assets/d22e33eb-9fed-4e8f-9dd3-ef2b4748fcd5)

2.4 Training_hours
```
training_hours.info()
```
![image](https://github.com/user-attachments/assets/4b58f23a-fc2a-45f3-b896-3ae58472b3cb)

>`training_hours`: no missing values, already in numeric format, no data cleaning required.

2.5 Cities
```
cities.info()
```
![image](https://github.com/user-attachments/assets/36957c5f-084c-4f07-ace6-ffd7269b48be)

>`City`: no missing values, currently of type object, it should be converted to string

>`City Development Index`: no missing values, already in numeric format, no data cleaning required

### Fix Data Type
```
cities['City'] = cities['City'].astype('string')
cities.info()
```
>The `City` column was converted to `string` type to preserve text format and ensure compatibility with string operations.

![image](https://github.com/user-attachments/assets/4f4d8b50-1152-4d30-af72-82954a35411e)

2.6 Employment
```
employment.info()
```
![image](https://github.com/user-attachments/assets/528f7918-b856-4301-9f71-fc5848707123)

>`enrollee_id`: no missing values, already in integer format, no data cleaning required.

>`employed`: no missing values, already in numeric format (float), no data cleaning required.

### 3.Load Data
3.1 Manually Inserting DataFrames into SQLite
```
db_path = 'data_warehouse.db'
engine = create_engine(f'sqlite:///{db_path}')
enrollies_data.to_sql('dim_enrollies_data', engine, if_exists = 'replace', index = False)
enrollies_education.to_sql('fact_enrollies_education', engine, if_exists = 'replace', index = False)
work_experience.to_sql('dim_work_experience', engine, if_exists = 'replace', index = False)
cities.to_sql('dim_cities', engine, if_exists = 'replace', index = False)
employment.to_sql('dim_enrollies', engine, if_exists = 'replace', index = False)
```
3.2 Refactoring DataFrame to SQL Insertion with SQLAlchemy
```
from sqlalchemy import create_engine
```
```
def save_to_sql(tables, db_path='data_warehouse.db'):
    engine = create_engine(f'sqlite:///{db_path}')
    for df, table_name in tables:
        df.to_sql(table_name, engine, if_exists='replace', index=False)

tables_to_save = [
    (enrollies_data, 'dim_enrollies_data'),
    (enrollies_education, 'dim_enrollies_education'),
    (work_experience, 'dim_work_experience'),
    (cities, 'dim_cities'),
    (employment, 'fact_enrollies')
]

save_to_sql(tables_to_save)
```
# How to Schedule the Script using Google Colab and Google Drive

To automate the ETL process, you can run the script regularly using **Google Colab + Google Drive**:

### Step 1: Upload your script to Google Drive
1. Save your ETL script as `etl_script.ipynb` or `etl_script.py`
2. Upload it to a folder in your Google Drive

### Step 2: Open it with Google Colab
1. Right-click the file in Google Drive → **Open with** → **Colaboratory**

### Step 3: Use built-in scheduling tools

#### Use [Google Apps Script](https://script.google.com/)
1. Create a new Apps Script project
2. Paste this code:
```javascript
function runETL() {
  var url = 'PASTE_YOUR_COLAB_URL_HERE';
  UrlFetchApp.fetch(url);
}
Set up a trigger to run runETL() daily via:
```
Triggers → Add Trigger → Time-based → e.g., every day at 9am

## Repository Purpose

This repository demonstrates practical ETL implementation for Data Analytics.

The focus is on preparing clean, integrated data from multiple operational sources to support downstream reporting and analytical applications.










