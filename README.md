# 🏠 Airbnb End-to-End Data Engineering Project

An end-to-end data engineering pipeline transforming raw Airbnb data into analytics-ready datasets using **Snowflake**, **dbt**, and **AWS**.

---

## 🏗️ Architecture

The pipeline follows a **Medallion Architecture**:

```
Source Data (CSV / S3) ➔ Snowflake Staging ➔ Bronze (Raw) ➔ Silver (Cleaned) ➔ Gold (Analytics)
```

- **Bronze Layer**: Raw data ingestion (`bronze_bookings`, `bronze_hosts`, `bronze_listings`)
- **Silver Layer**: Cleaned, standardized, and validated data (`silver_bookings`, `silver_hosts`, `silver_listings`)
- **Gold Layer**: Business-ready fact and dimensional models (`obt` One Big Table, `fact` table)
- **Snapshots**: Slowly Changing Dimensions (SCD Type 2) tracking historical changes (`dim_bookings`, `dim_hosts`, `dim_listings`)

---

## 📁 Project Structure

```
├── DDL/                           # Snowflake table definitions & staging scripts
├── SourceData/                    # Raw CSV files (bookings, hosts, listings)
├── aws_dbt_snowflake_project/     # Main dbt project
│   ├── dbt_project.yml            # dbt project configuration
│   ├── ExampleProfiles.yml        # Sample connection profile
│   ├── models/                    # Bronze, Silver, and Gold SQL models
│   ├── macros/                    # Reusable SQL & Jinja macros
│   └── snapshots/                 # SCD Type 2 snapshot definitions
├── pyproject.toml                 # Project dependencies
└── README.md
```

---

## 🚀 Quickstart

### 1. Prerequisites
- Python 3.12+
- A Snowflake account

### 2. Setup Environment
```bash
# Create and activate virtual environment
python -m venv .venv
.venv\Scripts\activate   # Windows (.venv/bin/activate on Mac/Linux)

# Install dbt and dependencies
pip install -e .
```

### 3. Configure Snowflake Connection
Create or edit your `~/.dbt/profiles.yml` (see `aws_dbt_snowflake_project/ExampleProfiles.yml` for reference):
```yaml
aws_dbt_snowflake_project:
  outputs:
    dev:
      type: snowflake
      account: <your_account_identifier>
      user: <your_username>
      password: <your_password>
      role: ACCOUNTADMIN
      database: AIRBNB
      warehouse: COMPUTE_WH
      schema: dbt_schema
      threads: 4
  target: dev
```

### 4. Run the Pipeline
Navigate to the dbt project folder and execute:

```bash
cd aws_dbt_snowflake_project

# 1. Test your Snowflake connection
dbt debug

# 2. Run snapshots (SCD Type 2)
dbt snapshot

# 3. Build all models (Bronze, Silver, Gold)
dbt run

# 4. Run data tests
dbt test

# 5. Generate and view documentation
dbt docs generate
dbt docs serve
```
---
## 🛠️ Tech Stack
- **Data Warehouse**: Snowflake
- **Transformations**: dbt (Data Build Tool)
- **Cloud Storage**: AWS S3
- **Language**: SQL & Python (Jinja SQL)