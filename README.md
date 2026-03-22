# 🛒 E-Commerce Real-Time Data Pipeline

> End-to-end production-grade data pipeline on the Brazilian E-Commerce dataset using the modern Azure data stack — built as an AI Data Engineer portfolio project.

![Python](https://img.shields.io/badge/Python-3.11-blue) ![Spark](https://img.shields.io/badge/PySpark-3.x-orange) ![Databricks](https://img.shields.io/badge/Azure_Databricks-Latest-red) ![dbt](https://img.shields.io/badge/dbt-Core-green) ![Airflow](https://img.shields.io/badge/Airflow-2.x-blue) ![Delta Lake](https://img.shields.io/badge/Delta_Lake-3.x-black)

---

## 🏗️ Architecture
```
CSV / Azure Event Hubs
         │
         ▼
  [ Bronze ]  →  Raw ingestion via PySpark → Delta Lake on ADLS Gen2
         │
         ▼
  [ Silver ]  →  Cleaned & validated (Great Expectations)
         │
         ▼
  [ Gold   ]  →  dbt Star Schema → Databricks SQL Warehouse → Power BI
         │
         ▼
  [ Orchestration ]  →  Apache Airflow DAGs on Azure Container Instance
```

---

## 🧰 Tech Stack

| Layer | Tool |
|---|---|
| Language | Python 3.11 |
| Processing | Apache Spark (PySpark) |
| Storage | Azure Data Lake Storage Gen2 + Delta Lake |
| Platform | Azure Databricks |
| Orchestration | Apache Airflow |
| Transformation | dbt (data build tool) |
| Streaming | Azure Event Hubs |
| Secrets | Azure Key Vault |
| Data Quality | Great Expectations |
| CI/CD | GitHub Actions |
| Visualisation | Power BI |

---

## ☁️ Azure Services Used

| Azure Service | Purpose |
|---|---|
| **Azure Databricks** | Spark compute, Delta Lake, notebooks, SQL Warehouse |
| **Azure Data Lake Storage Gen2** | Bronze / Silver / Gold layer storage |
| **Azure Event Hubs** | Streaming ingestion (Kafka-compatible API) |
| **Azure Key Vault** | Secure secrets and connection strings |
| **Azure Container Instance** | Host Apache Airflow for orchestration |
| **Azure Active Directory** | Service principal authentication |
| **Azure Monitor** | Pipeline logging and alerting |

---

## 📦 Dataset

**Brazilian E-Commerce by Olist** — [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)  
100k+ orders · 9 tables · orders, payments, customers, products, sellers, reviews

---

## 🗂️ Project Structure
```
ecommerce-data-pipeline/
├── ingestion/
│   ├── extract_orders.py            # Bronze ingestion → ADLS Gen2
│   ├── extract_customers.py
│   └── eventhub_ingest.py           # Azure Event Hubs streaming
├── transformation/
│   ├── spark_jobs/
│   │   ├── clean_orders.py          # Silver cleaning (PySpark)
│   │   └── join_enrichment.py       # Customer + product joins
│   └── dbt_models/
│       ├── staging/                 # stg_orders, stg_customers
│       └── marts/
│           ├── fact_orders.sql
│           ├── dim_customers.sql    # SCD Type 2
│           └── dim_products.sql
├── orchestration/dags/
│   ├── daily_pipeline_dag.py
│   └── quality_check_dag.py
├── quality/expectations/
│   ├── orders_suite.json
│   └── customers_suite.json
├── infrastructure/
│   ├── adls_setup.py
│   └── keyvault_secrets.py
├── .github/workflows/dbt_ci.yml     # Auto dbt tests on every push
├── docker-compose.yml               # Local Airflow dev setup
├── requirements.txt
└── README.md
```

---

## 🚀 Quickstart
```bash
# 1. Clone
git clone https://github.com/YOUR_USERNAME/ecommerce-data-pipeline.git
cd ecommerce-data-pipeline

# 2. Install dependencies
pip install -r requirements.txt

# 3. Set Azure credentials
export AZURE_TENANT_ID="your-tenant-id"
export AZURE_CLIENT_ID="your-client-id"
export AZURE_CLIENT_SECRET="your-client-secret"
export AZURE_STORAGE_ACCOUNT="your-adls-account"

# 4. Run Bronze ingestion
python ingestion/extract_orders.py

# 5. Run Silver transformation
spark-submit transformation/spark_jobs/clean_orders.py

# 6. Run Gold dbt models
cd transformation/dbt_models && dbt run && dbt test

# 7. Start Airflow locally
docker-compose up
# Open → http://localhost:8080
```

---

## 📐 Data Model (Star Schema)
```
dim_customers (SCD Type 2)
        │
fact_orders ──── dim_products
        │
   dim_sellers
```

**fact_orders** — grain: one row per order item  
**dim_customers** — tracks historical address changes via SCD Type 2  
**dim_products** — product catalogue with translated categories  
**dim_sellers** — seller details with geolocation

---

## ✅ Data Quality

Great Expectations validates at every layer boundary:
- No null `order_id` in Bronze
- `payment_value` > 0 in Silver
- No duplicates in Gold
- Referential integrity: all customer IDs exist in `dim_customers`

---

## 📊 Gold Layer Outputs

- Revenue trends (daily / weekly / monthly)
- Average delivery time by seller state
- Customer Lifetime Value (CLV)
- Late delivery rate by region
- Real-time order volume via Event Hubs stream

---

## 🔄 CI/CD

GitHub Actions runs on every push:
1. `dbt compile` — validates all SQL models
2. `dbt test` — runs all data quality tests
3. `pytest` — unit tests on transformation logic

---

## 🔮 Roadmap

- [ ] CDC with Azure SQL + Debezium
- [ ] Infrastructure as Code with Azure Bicep / Terraform
- [ ] Azure Monitor alerting and SLA tracking
- [ ] MLflow + Databricks Feature Store layer
- [ ] Publish Power BI dashboard via Azure App Service

---

## 🧠 What I Learned

- Medallion architecture (Bronze / Silver / Gold) on ADLS Gen2
- Production PySpark jobs with schema enforcement and error handling
- Airflow DAGs with dependencies, retries, and Azure integration
- SCD Type 2 in dbt for slowly changing dimensions
- Data quality gates with Great Expectations between layers
- Azure Databricks with Unity Catalog and Delta Live Tables
- Secure secret management with Azure Key Vault
- Streaming ingestion with Azure Event Hubs (Kafka-compatible)
- CI/CD automation with GitHub Actions for dbt testing

---

## 👨‍💻 About

Built as part of my **AI Data Engineer** transition targeting product-based companies.

**Skills:** PySpark · Delta Lake · Azure Databricks · ADLS Gen2 · Airflow · dbt · Data Modelling · Python · Azure Key Vault · GitHub Actions · Power BI

📧 rachigarg1997@email.com · 💼 [LinkedIn](https://linkedin.com/in/yourprofile) · 🏆 Databricks Certified Data Engineer Associate *(in progress)*

---

📄 MIT License
