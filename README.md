# 🚀 Modern Data Pipeline with dbt, Snowflake & Airflow

<div align="center">

![Project Banner](Project_thumbnail.png)

![dbt](https://img.shields.io/badge/dbt-FF694B?style=for-the-badge&logo=dbt&logoColor=white)
![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8?style=for-the-badge&logo=snowflake&logoColor=white)
![Apache Airflow](https://img.shields.io/badge/Apache%20Airflow-017CEE?style=for-the-badge&logo=Apache%20Airflow&logoColor=white)
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)

*A production-ready data transformation pipeline showcasing modern data engineering best practices*

[Features](#-features) • [Architecture](#-architecture) • [Setup](#-setup) • [Usage](#-usage) • [Project Structure](#-project-structure)

</div>

---

## 📋 Overview

This project demonstrates a complete **end-to-end data pipeline** built with industry-standard tools and best practices. It processes TPC-H sample data from Snowflake, applies transformations using dbt, and orchestrates the entire workflow with Apache Airflow using Astronomer Cosmos.

### What You'll Find Here

- ✅ **Enterprise-grade RBAC** implementation in Snowflake
- ✅ **Modular dbt project** with staging, intermediate, and fact layers
- ✅ **Custom macros** for reusable transformation logic
- ✅ **Comprehensive testing** suite (generic + singular tests)
- ✅ **Airflow orchestration** with Astronomer Cosmos integration
- ✅ **Containerized development** environment with Docker
- ✅ **Git workflow** with feature branches and pull requests

---

## ✨ Features

### 🔐 Snowflake RBAC Configuration
- **Dedicated warehouse** (`dbt_wh`) for compute isolation
- **Role-based access control** with `dbt_role` 
- **Separate database and schema** (`dbt_db.dbt_schema`)
- **User management** with proper permissions

### 🏗️ dbt Project Architecture

#### **Staging Layer** (`models/staging/`)
- Clean, standardized data from raw sources
- Source definitions with data quality tests
- Consistent naming conventions

#### **Data Marts** (`models/marts/`)
- **Intermediate models** (`int_*`): Business logic and joins
- **Fact tables** (`fct_*`): Final analytical models
- Optimized for downstream analytics

#### **Custom Macros** (`macros/`)
- `discounted_amount()`: Calculate discounted prices
- Reusable transformation logic
- DRY (Don't Repeat Yourself) principles

#### **Data Quality Tests** (`tests/`)
- **Generic tests**: Uniqueness, relationships, accepted values
- **Singular tests**: Custom business logic validation
  - `fct_orders_date_valis.sql`: Date range validation
  - `fct_orders_discount.sql`: Discount amount checks

### 🔄 Airflow Orchestration
- **Astronomer Cosmos** integration for dbt DAGs
- **Automatic dependency detection** from dbt lineage
- **Containerized execution** with isolated dbt virtual environment
- **Daily scheduling** with configurable retry logic

---

## 🏛️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Apache Airflow + Cosmos                   │
│                   (Orchestration Layer)                      │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                      dbt Transformation Layer                │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────┐   ┌──────────────┐   ┌────────────────┐   │
│  │   Staging   │──▶│ Intermediate │──▶│  Fact Tables   │   │
│  │   Models    │   │    Models    │   │   (Marts)      │   │
│  └─────────────┘   └──────────────┘   └────────────────┘   │
│                                                              │
│  Custom Macros │ Data Tests │ Sources                       │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│              Snowflake Data Warehouse (AWS)                  │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  dbt_db.dbt_schema (Target)                          │   │
│  │  snowflake_sample_data.tpch_sf1 (Source)             │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Data Warehouse** | Snowflake | Cloud data storage and compute |
| **Transformation** | dbt | SQL-based data transformations |
| **Orchestration** | Apache Airflow | Workflow scheduling and monitoring |
| **Airflow Distribution** | Astronomer | Local development environment |
| **dbt-Airflow Integration** | Cosmos | Automatic DAG generation from dbt |
| **Containerization** | Docker | Isolated, reproducible environments |
| **Version Control** | Git + GitHub | Source code management |
| **Language** | Python + SQL | Data processing and transformations |

---

## 🚀 Setup

### Prerequisites

- Docker Desktop installed and running
- Astronomer CLI (`astro`)
- Git
- Snowflake account with appropriate permissions

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Ignacio-Antequera/data-pipeline-project.git
   cd data-pipeline-project
   ```

2. **Configure Snowflake connection**
   
   In Airflow UI (after starting), create a connection with ID `snowflake_conn`:
   - **Connection Type**: Snowflake
   - **Account**: Your Snowflake account identifier
   - **User**: Your username
   - **Password**: Your password
   - **Database**: `dbt_db`
   - **Schema**: `dbt_schema`
   - **Warehouse**: `dbt_wh`
   - **Role**: `dbt_role` or `ACCOUNTADMIN`

3. **Start Airflow**
   ```bash
   astro dev start
   ```

4. **Access Airflow UI**
   - Open browser to: `http://localhost:8080`
   - Username: `admin`
   - Password: `admin`

---

## 📖 Usage

### Running dbt Locally

```bash
# Activate virtual environment
source .venv/bin/activate

# Install dependencies
dbt deps

# Run models
dbt run

# Run tests
dbt test

# Generate documentation
dbt docs generate
dbt docs serve
```

### Running via Airflow

1. Navigate to Airflow UI (`http://localhost:8080`)
2. Find the `dbt_dag` in the DAGs list
3. Toggle the DAG to "On"
4. Trigger manually or wait for scheduled run
5. Monitor execution in the Graph/Grid view

### Git Workflow

```bash
# Create feature branch
git checkout -b feature/your-feature-name

# Make changes and commit
git add .
git commit -m "Description of changes"

# Push to GitHub
git push -u origin feature/your-feature-name

# Create pull request
gh pr create --title "Your PR title" --body "Description"

# Merge when ready
gh pr merge <PR_NUMBER> --merge --delete-branch
```

---

## 📁 Project Structure

```
data-pipeline-project/
├── dags/
│   ├── dbt/                         # dbt project root
│   │   ├── dbt_project.yml         # dbt configuration
│   │   ├── models/                 # Data models
│   │   │   ├── staging/            # Staging layer
│   │   │   │   ├── stg_tpch_orders.sql
│   │   │   │   ├── stg_tpch_line_items.sql
│   │   │   │   └── tpch_sources.yml
│   │   │   └── marts/              # Data marts
│   │   │       ├── int_order_items.sql
│   │   │       ├── int_order_items_summary.sql
│   │   │       ├── fct_orders.sql
│   │   │       └── generic_tests.yml
│   │   ├── macros/                 # Custom macros
│   │   │   └── pricing.sql
│   │   ├── tests/                  # Singular tests
│   │   │   ├── fct_orders_date_valis.sql
│   │   │   └── fct_orders_discount.sql
│   │   └── packages.yml            # dbt dependencies
│   ├── dbt_dag.py                  # Cosmos DAG definition
│   └── exampledag.py               # Example Airflow DAG
├── Dockerfile                       # Airflow + dbt environment
├── requirements.txt                 # Python dependencies
├── airflow_settings.yaml           # Airflow configuration
├── packages.txt                     # System packages
└── README.md                       # This file
```

---

## 🧪 Testing

The project includes comprehensive data quality tests:

### Generic Tests (via schema.yml)
- **Uniqueness**: Primary key constraints
- **Not null**: Required field validation
- **Relationships**: Referential integrity
- **Accepted values**: Enum/categorical validation

### Singular Tests (custom SQL)
- **Date validation**: Ensures order dates are within valid ranges
- **Discount validation**: Verifies discount calculations

**Run all tests:**
```bash
dbt test
```

---

## 📊 Models

| Model | Type | Description |
|-------|------|-------------|
| `stg_tpch_orders` | Staging | Cleaned orders from TPC-H |
| `stg_tpch_line_items` | Staging | Cleaned line items from TPC-H |
| `int_order_items` | Intermediate | Orders joined with line items |
| `int_order_items_summary` | Intermediate | Aggregated order metrics |
| `fct_orders` | Fact | Final orders fact table |

---

## 🤝 Contributing

1. Create a feature branch
2. Make your changes
3. Commit with descriptive messages
4. Push and create a Pull Request
5. Wait for review and merge

---

## 📚 Resources

- [dbt Documentation](https://docs.getdbt.com/)
- [Snowflake Documentation](https://docs.snowflake.com/)
- [Astronomer Documentation](https://docs.astronomer.io/)
- [Astronomer Cosmos](https://astronomer.github.io/astronomer-cosmos/)
- [Apache Airflow](https://airflow.apache.org/)

---

## 📝 License

This project is for educational and demonstration purposes.

---

<div align="center">

**Built with ❤️ by [Ignacio Antequera](https://github.com/Ignacio-Antequera)**

⭐ Star this repo if you find it helpful!

</div>
