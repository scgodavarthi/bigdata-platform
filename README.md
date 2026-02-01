# 🚀 DataFlow Platform - Complete Project Documentation

## 📋 Project Overview

| Attribute              | Details                              |
| ---------------------- | ------------------------------------ |
| **Project Name**       | DataFlow Platform                    |
| **Domain**             | E-Commerce Data Engineering          |
| **Type**               | End-to-End Data Engineering Platform |
| **Duration**           | 12 months (26 sprints × 2 weeks)     |
| **Total Story Points** | 653                                  |
| **Total PBIs**         | 222                                  |
| **Cost**               | $0 (100% Free & Open Source)         |

### 🎯 Project Goal

Build a production-grade, end-to-end data engineering platform that demonstrates skills required for Big Tech Data Engineering roles. The platform processes e-commerce data through batch and streaming pipelines, implements a modern data lakehouse architecture, and provides analytics dashboards.

---

## 🏗️ Architecture Overview

```
┌────────────────────────────────────────────────────────────────────────────────────┐
│                           DATAFLOW PLATFORM ARCHITECTURE                            │
├────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                    │
│   DATA SOURCES                                                                     │
│   ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │
│   │ PostgreSQL  │  │  MongoDB    │  │  REST API   │  │  CSV/JSON   │              │
│   │  (Orders,   │  │ (Products,  │  │ (External   │  │  (Historical│              │
│   │  Customers) │  │  Reviews)   │  │   Data)     │  │   Data)     │              │
│   └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘              │
│          │                │                │                │                      │
│          ▼                ▼                ▼                ▼                      │
│   ┌──────────────────────────────────────────────────────────────────┐            │
│   │                      INGESTION LAYER                              │            │
│   │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐         │            │
│   │  │ Debezium │  │  Kafka   │  │  Python  │  │  Spark   │         │            │
│   │  │  (CDC)   │  │ Connect  │  │  Scripts │  │  Batch   │         │            │
│   │  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘         │            │
│   └───────┼─────────────┼─────────────┼─────────────┼────────────────┘            │
│           │             │             │             │                              │
│           ▼             ▼             ▼             ▼                              │
│   ┌──────────────────────────────────────────────────────────────────┐            │
│   │                    MESSAGE QUEUE (KAFKA)                          │            │
│   │  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────┐    │            │
│   │  │ orders     │ │ customers  │ │ products   │ │ clickstream│    │            │
│   │  │ topic      │ │ topic      │ │ topic      │ │ topic      │    │            │
│   │  └────────────┘ └────────────┘ └────────────┘ └────────────┘    │            │
│   └───────────────────────────┬──────────────────────────────────────┘            │
│                               │                                                    │
│           ┌───────────────────┼───────────────────┐                               │
│           ▼                   ▼                   ▼                               │
│   ┌─────────────┐     ┌─────────────┐     ┌─────────────┐                         │
│   │   SPARK     │     │   SPARK     │     │   AIRFLOW   │                         │
│   │  STREAMING  │     │   BATCH     │     │   (DAGs)    │                         │
│   └──────┬──────┘     └──────┬──────┘     └──────┬──────┘                         │
│          │                   │                   │                                 │
│          └───────────────────┼───────────────────┘                                 │
│                              ▼                                                     │
│   ┌──────────────────────────────────────────────────────────────────┐            │
│   │                 DATA LAKEHOUSE (MinIO + Delta Lake)               │            │
│   │                                                                   │            │
│   │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐           │            │
│   │  │    BRONZE    │  │    SILVER    │  │     GOLD     │           │            │
│   │  │   (Raw Data) │─▶│  (Cleaned)   │─▶│  (Business)  │           │            │
│   │  └──────────────┘  └──────────────┘  └──────────────┘           │            │
│   └──────────────────────────────────────────────────────────────────┘            │
│                              │                                                     │
│          ┌───────────────────┼───────────────────┐                                │
│          ▼                   ▼                   ▼                                │
│   ┌─────────────┐     ┌─────────────┐     ┌─────────────┐                         │
│   │     dbt     │     │    FEAST    │     │   GREAT     │                         │
│   │   (Trans-   │     │  (Feature   │     │ EXPECTATIONS│                         │
│   │  formations)│     │   Store)    │     │  (Quality)  │                         │
│   └──────┬──────┘     └──────┬──────┘     └──────┬──────┘                         │
│          │                   │                   │                                 │
│          └───────────────────┼───────────────────┘                                 │
│                              ▼                                                     │
│   ┌──────────────────────────────────────────────────────────────────┐            │
│   │                      SERVING LAYER                                │            │
│   │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐         │            │
│   │  │  TRINO   │  │ SUPERSET │  │  REDIS   │  │  FastAPI │         │            │
│   │  │ (Query)  │  │  (BI)    │  │ (Cache)  │  │  (API)   │         │            │
│   │  └──────────┘  └──────────┘  └──────────┘  └──────────┘         │            │
│   └──────────────────────────────────────────────────────────────────┘            │
│                              │                                                     │
│                              ▼                                                     │
│   ┌──────────────────────────────────────────────────────────────────┐            │
│   │                   OBSERVABILITY LAYER                             │            │
│   │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐         │            │
│   │  │PROMETHEUS│  │ GRAFANA  │  │ DATAHUB  │  │OPENLINEAG│         │            │
│   │  │(Metrics) │  │(Dashboard│  │(Catalog) │  │(Lineage) │         │            │
│   │  └──────────┘  └──────────┘  └──────────┘  └──────────┘         │            │
│   └──────────────────────────────────────────────────────────────────┘            │
│                                                                                    │
│   ┌──────────────────────────────────────────────────────────────────┐            │
│   │                   INFRASTRUCTURE LAYER                            │            │
│   │      Docker  │  Kubernetes (minikube)  │  Terraform  │  Git      │            │
│   └──────────────────────────────────────────────────────────────────┘            │
└────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Technology Stack

### **All Technologies (100% Free & Open Source)**

| Category           | Technology            | Purpose                                | Docker Port |
| ------------------ | --------------------- | -------------------------------------- | ----------- |
| **Databases**      | PostgreSQL            | Transactional data (orders, customers) | 5432        |
| **Databases**      | MongoDB               | Document store (products, reviews)     | 27017       |
| **Databases**      | Redis                 | Cache, online feature store            | 6379        |
| **Streaming**      | Apache Kafka          | Message queue, event streaming         | 9092        |
| **Streaming**      | Zookeeper             | Kafka coordination                     | 2181        |
| **Streaming**      | Schema Registry       | Schema management                      | 8081        |
| **CDC**            | Debezium              | Change Data Capture from databases     | 8083        |
| **Processing**     | Apache Spark          | Batch & stream processing              | 8080, 7077  |
| **Storage**        | MinIO                 | S3-compatible object storage           | 9000, 9001  |
| **Table Format**   | Delta Lake            | ACID transactions on data lake         | -           |
| **Orchestration**  | Apache Airflow        | Workflow orchestration                 | 8080        |
| **Transformation** | dbt Core              | SQL transformations                    | -           |
| **Data Quality**   | Great Expectations    | Data validation                        | -           |
| **Data Quality**   | Soda Core             | SQL-based quality checks               | -           |
| **Feature Store**  | Feast                 | ML feature management                  | -           |
| **Query Engine**   | Trino                 | SQL analytics on data lake             | 8080        |
| **Visualization**  | Apache Superset       | BI dashboards                          | 8088        |
| **Monitoring**     | Prometheus            | Metrics collection                     | 9090        |
| **Monitoring**     | Grafana               | Metrics visualization                  | 3000        |
| **Data Catalog**   | DataHub               | Metadata management                    | 9002        |
| **Lineage**        | OpenLineage           | Data lineage tracking                  | -           |
| **Containers**     | Docker                | Containerization                       | -           |
| **Orchestration**  | Kubernetes (minikube) | Container orchestration                | -           |
| **IaC**            | Terraform             | Infrastructure as Code                 | -           |
| **CI/CD**          | GitHub Actions        | Automation                             | -           |

---

## 📁 Project Structure

```
dataflow-platform/
├── .github/
│   ├── workflows/           # GitHub Actions CI/CD
│   │   ├── ci.yml
│   │   └── cd.yml
│   ├── ISSUE_TEMPLATE/
│   └── pull_request_template.md
│
├── docker/
│   ├── spark/
│   │   └── Dockerfile
│   ├── airflow/
│   │   └── Dockerfile
│   └── kafka/
│       └── Dockerfile
│
├── src/
│   ├── __init__.py
│   ├── ingestion/
│   │   ├── __init__.py
│   │   ├── kafka_producer.py
│   │   ├── kafka_consumer.py
│   │   └── debezium_config.py
│   ├── processing/
│   │   ├── __init__.py
│   │   ├── batch/
│   │   │   ├── bronze_to_silver.py
│   │   │   ├── silver_to_gold.py
│   │   │   └── cleaning_jobs.py
│   │   └── streaming/
│   │       ├── clickstream_processor.py
│   │       └── realtime_aggregations.py
│   ├── transformation/
│   │   └── __init__.py
│   ├── quality/
│   │   ├── expectations/
│   │   └── soda_checks/
│   └── utils/
│       ├── __init__.py
│       ├── helpers.py
│       └── spark_utils.py
│
├── dags/
│   ├── bronze_ingestion_dag.py
│   ├── silver_transformation_dag.py
│   ├── gold_aggregation_dag.py
│   └── data_quality_dag.py
│
├── dbt/
│   ├── dbt_project.yml
│   ├── profiles.yml
│   ├── models/
│   │   ├── staging/
│   │   │   ├── stg_orders.sql
│   │   │   ├── stg_customers.sql
│   │   │   └── stg_products.sql
│   │   ├── intermediate/
│   │   ├── marts/
│   │   │   ├── dim_customers.sql
│   │   │   ├── dim_products.sql
│   │   │   ├── dim_date.sql
│   │   │   ├── fct_orders.sql
│   │   │   ├── daily_sales.sql
│   │   │   └── customer_lifetime_value.sql
│   │   └── schema.yml
│   ├── tests/
│   └── macros/
│
├── feature_store/
│   ├── feature_store.yaml
│   ├── features/
│   │   ├── customer_features.py
│   │   ├── product_features.py
│   │   └── session_features.py
│   └── materialization/
│
├── data_generators/
│   ├── __init__.py
│   ├── customer_generator.py
│   ├── order_generator.py
│   ├── product_generator.py
│   └── clickstream_generator.py
│
├── tests/
│   ├── __init__.py
│   ├── unit/
│   ├── integration/
│   └── conftest.py
│
├── docs/
│   ├── architecture/
│   ├── setup/
│   └── runbooks/
│
├── scripts/
│   ├── setup/
│   │   ├── init_databases.sql
│   │   └── create_topics.sh
│   └── utils/
│
├── config/
│   ├── spark/
│   ├── kafka/
│   └── trino/
│
├── .env.example
├── .gitignore
├── .pre-commit-config.yaml
├── docker-compose.yml
├── Makefile
├── requirements.txt
├── requirements-dev.txt
├── pyproject.toml
└── README.md
```

---

## 📊 Data Model

### **E-Commerce Domain Model**

#### **Source Tables (PostgreSQL)**

```sql
-- Customers
CREATE TABLE customers (
    customer_id UUID PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    phone VARCHAR(20),
    address_line1 VARCHAR(255),
    address_line2 VARCHAR(255),
    city VARCHAR(100),
    state VARCHAR(100),
    postal_code VARCHAR(20),
    country VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Products
CREATE TABLE products (
    product_id UUID PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    description TEXT,
    category VARCHAR(100),
    subcategory VARCHAR(100),
    price DECIMAL(10,2),
    cost DECIMAL(10,2),
    sku VARCHAR(50) UNIQUE,
    stock_quantity INTEGER,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Orders
CREATE TABLE orders (
    order_id UUID PRIMARY KEY,
    customer_id UUID REFERENCES customers(customer_id),
    order_date TIMESTAMP NOT NULL,
    status VARCHAR(50),
    total_amount DECIMAL(12,2),
    shipping_address TEXT,
    payment_method VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Order Items
CREATE TABLE order_items (
    order_item_id UUID PRIMARY KEY,
    order_id UUID REFERENCES orders(order_id),
    product_id UUID REFERENCES products(product_id),
    quantity INTEGER NOT NULL,
    unit_price DECIMAL(10,2),
    discount DECIMAL(10,2) DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Payments
CREATE TABLE payments (
    payment_id UUID PRIMARY KEY,
    order_id UUID REFERENCES orders(order_id),
    amount DECIMAL(12,2),
    payment_method VARCHAR(50),
    payment_status VARCHAR(50),
    transaction_id VARCHAR(255),
    payment_date TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### **Medallion Architecture Layers**

**Bronze Layer (Raw)**

- `bronze.raw_orders` - CDC events from orders table
- `bronze.raw_customers` - CDC events from customers table
- `bronze.raw_products` - CDC events from products table
- `bronze.raw_clickstream` - Real-time clickstream events
- `bronze.raw_payments` - Payment events

**Silver Layer (Cleaned)**

- `silver.customers` - Deduplicated, cleaned customers
- `silver.products` - Standardized products
- `silver.orders` - Validated orders
- `silver.order_items` - Clean order line items
- `silver.sessions` - Sessionized clickstream

**Gold Layer (Business)**

- `gold.dim_customers` - Customer dimension (SCD Type 2)
- `gold.dim_products` - Product dimension
- `gold.dim_date` - Date dimension
- `gold.fct_orders` - Order facts
- `gold.fct_order_items` - Order item facts
- `gold.daily_sales` - Daily sales aggregates
- `gold.customer_lifetime_value` - CLV calculations
- `gold.product_performance` - Product metrics

---

## 🗓️ Project Timeline

### **Sprint Schedule (26 Sprints)**

| Sprint | Weeks | Epic Focus       | Key Deliverables              |
| ------ | ----- | ---------------- | ----------------------------- |
| 1      | 1-2   | Foundation       | Dev environment, Docker setup |
| 2      | 3-4   | Foundation       | CI/CD pipeline                |
| 3      | 5-6   | Foundation       | Documentation                 |
| 4      | 7-8   | Data Sources     | PostgreSQL, MongoDB           |
| 5      | 9-10  | Data Sources     | Kafka cluster                 |
| 6      | 11-12 | Data Sources     | Debezium CDC                  |
| 7      | 13-14 | Data Lake        | MinIO, Delta Lake             |
| 8      | 15-16 | Data Lake        | Bronze pipelines              |
| 9      | 17-18 | Data Lake        | Schema management             |
| 10     | 19-20 | Batch Processing | Spark cluster                 |
| 11     | 21-22 | Batch Processing | Silver layer                  |
| 12     | 23-24 | Batch Processing | SCD Type 2                    |
| 13     | 25-26 | Orchestration    | Airflow setup                 |
| 14     | 27-28 | Orchestration    | DAGs & alerting               |
| 15     | 29-30 | Transformation   | dbt setup, staging            |
| 16     | 31-32 | Transformation   | Fact models                   |
| 17     | 33-34 | Transformation   | Business marts                |
| 18     | 35-36 | Data Quality     | Great Expectations            |
| 19     | 37-38 | Data Quality     | Quality automation            |
| 20     | 39-40 | Streaming        | Spark Streaming               |
| 21     | 41-42 | Streaming        | Aggregations                  |
| 22     | 43-44 | Feature Store    | Feast setup                   |
| 23     | 45-46 | Feature Store    | Online serving                |
| 24     | 47-48 | Analytics        | Trino, Superset               |
| 25     | 49-50 | Analytics        | Dashboards                    |
| 26     | 51-52 | Observability    | Monitoring, catalog           |

---

## 📋 Epic & Feature Breakdown

### **EPIC-1: Project Foundation & Infrastructure** (Sprints 1-3, 34 pts)

- FEAT-1.1: Development Environment Setup
- FEAT-1.2: Docker Infrastructure
- FEAT-1.3: CI/CD Pipeline Setup
- FEAT-1.4: Project Documentation Structure

### **EPIC-2: Data Sources & Ingestion** (Sprints 4-6, 42 pts)

- FEAT-2.1: PostgreSQL Database Setup
- FEAT-2.2: MongoDB Setup
- FEAT-2.3: Apache Kafka Cluster
- FEAT-2.4: Debezium CDC Implementation

### **EPIC-3: Data Lake & Bronze Layer** (Sprints 7-9, 38 pts)

- FEAT-3.1: MinIO Object Storage
- FEAT-3.2: Delta Lake Configuration
- FEAT-3.3: Bronze Layer Pipelines
- FEAT-3.4: Schema Evolution & Management

### **EPIC-4: Batch Processing & Silver Layer** (Sprints 10-12, 44 pts)

- FEAT-4.1: Spark Cluster Setup
- FEAT-4.2: Data Cleaning Jobs
- FEAT-4.3: Silver Layer Implementation
- FEAT-4.4: SCD Type 2 & Historical Tracking

### **EPIC-5: Orchestration & Scheduling** (Sprints 13-14, 32 pts)

- FEAT-5.1: Apache Airflow Installation
- FEAT-5.2: Pipeline DAGs Development
- FEAT-5.3: Monitoring & Alerting

### **EPIC-6: Transformation & Gold Layer** (Sprints 15-17, 46 pts)

- FEAT-6.1: dbt Project Setup
- FEAT-6.2: Dimension Models
- FEAT-6.3: Fact Models
- FEAT-6.4: Business Marts

### **EPIC-7: Data Quality & Testing** (Sprints 18-19, 30 pts)

- FEAT-7.1: Great Expectations Setup
- FEAT-7.2: Soda Core Integration
- FEAT-7.3: Quality Automation

### **EPIC-8: Stream Processing** (Sprints 20-21, 34 pts)

- FEAT-8.1: Spark Streaming Setup
- FEAT-8.2: Real-time Pipelines
- FEAT-8.3: Streaming Aggregations

### **EPIC-9: Feature Store** (Sprints 22-23, 32 pts)

- FEAT-9.1: Feast Installation
- FEAT-9.2: Feature Engineering
- FEAT-9.3: Online Feature Serving

### **EPIC-10: Analytics & Visualization** (Sprints 24-25, 30 pts)

- FEAT-10.1: Trino Query Engine
- FEAT-10.2: Apache Superset Dashboards
- FEAT-10.3: Self-Service Analytics

### **EPIC-11: Observability & Governance** (Sprint 26, 26 pts)

- FEAT-11.1: Prometheus & Grafana Monitoring
- FEAT-11.2: DataHub Data Catalog
- FEAT-11.3: OpenLineage Data Lineage

### **EPIC-12: Documentation & Portfolio** (Ongoing, 20 pts)

- FEAT-12.1: Technical Documentation
- FEAT-12.2: Portfolio & Career Materials

---

## 🐳 Docker Services

### **docker-compose.yml Services**

```yaml
services:
  # Databases
  postgres: # Port 5432 - Transactional DB
  mongodb: # Port 27017 - Document DB
  redis: # Port 6379 - Cache/Online Store

  # Streaming
  zookeeper: # Port 2181 - Kafka coordination
  kafka: # Port 9092 - Message broker
  schema-registry: # Port 8081 - Schema management
  kafka-connect: # Port 8083 - Connectors (Debezium)
  kafdrop: # Port 9000 - Kafka UI

  # Processing
  spark-master: # Port 8080, 7077 - Spark master
  spark-worker: # Spark worker nodes

  # Storage
  minio: # Port 9000, 9001 - Object storage

  # Orchestration
  airflow-webserver: # Port 8080 - Airflow UI
  airflow-scheduler: # Airflow scheduler
  airflow-worker: # Airflow workers

  # Analytics
  trino: # Port 8080 - Query engine
  superset: # Port 8088 - BI dashboards

  # Monitoring
  prometheus: # Port 9090 - Metrics
  grafana: # Port 3000 - Dashboards

  # Governance
  datahub-gms: # DataHub backend
  datahub-frontend: # Port 9002 - DataHub UI
```

---

## 🔑 Environment Variables

```env
# Project
PROJECT_NAME=dataflow-platform
ENVIRONMENT=development

# PostgreSQL
POSTGRES_HOST=postgres
POSTGRES_PORT=5432
POSTGRES_DB=ecommerce
POSTGRES_USER=dataflow
POSTGRES_PASSWORD=your_password

# MongoDB
MONGODB_HOST=mongodb
MONGODB_PORT=27017
MONGODB_DB=products
MONGODB_USER=dataflow
MONGODB_PASSWORD=your_password

# Kafka
KAFKA_BOOTSTRAP_SERVERS=kafka:9092
KAFKA_SCHEMA_REGISTRY=http://schema-registry:8081

# MinIO
MINIO_ENDPOINT=minio:9000
MINIO_ACCESS_KEY=minioadmin
MINIO_SECRET_KEY=your_secret_key
MINIO_BUCKET_BRONZE=bronze
MINIO_BUCKET_SILVER=silver
MINIO_BUCKET_GOLD=gold

# Spark
SPARK_MASTER=spark://spark-master:7077

# Airflow
AIRFLOW_UID=50000
AIRFLOW__CORE__EXECUTOR=LocalExecutor

# Redis
REDIS_HOST=redis
REDIS_PORT=6379

# Trino
TRINO_HOST=trino
TRINO_PORT=8080
```

---

## 📊 Key Metrics & KPIs

### **Data Pipeline Metrics**

- Records processed per minute
- Pipeline latency (source to gold)
- Data freshness (time since last update)
- Error rate per pipeline
- Kafka consumer lag

### **Data Quality Metrics**

- Test pass rate (%)
- Data completeness (%)
- Schema violations count
- Anomaly detection alerts

### **Business Metrics (Dashboards)**

- Daily/Weekly/Monthly Revenue
- Order Count & Average Order Value
- Customer Acquisition & Retention
- Product Performance
- Conversion Funnel Metrics

---

## 🚀 Common Commands

```bash
# Start all services
make up
docker compose up -d

# Stop all services
make down
docker compose down

# View logs
make logs
docker compose logs -f [service_name]

# Run tests
make test
pytest tests/ -v

# Run linting
make lint
flake8 src/ tests/

# Format code
make format
black src/ tests/
isort src/ tests/

# Run dbt
cd dbt && dbt run
cd dbt && dbt test

# Airflow CLI
docker compose exec airflow-webserver airflow dags list
docker compose exec airflow-webserver airflow tasks test [dag_id] [task_id] [date]

# Kafka
docker compose exec kafka kafka-topics --list --bootstrap-server localhost:9092
docker compose exec kafka kafka-console-consumer --topic [topic_name] --bootstrap-server localhost:9092

# Spark submit
docker compose exec spark-master spark-submit --master spark://spark-master:7077 /app/jobs/my_job.py
```

---

## 🔧 Troubleshooting

### **Common Issues**

| Issue                    | Solution                                                |
| ------------------------ | ------------------------------------------------------- |
| Docker out of memory     | Increase Docker RAM to 8GB+ in settings                 |
| Kafka not starting       | Check Zookeeper is healthy first                        |
| Spark job fails          | Check executor memory settings                          |
| Airflow DAG not showing  | Run `airflow dags list` and check for import errors     |
| MinIO connection refused | Verify MinIO container is running and ports are exposed |
| dbt connection error     | Check profiles.yml configuration                        |

### **Useful Debug Commands**

```bash
# Check container health
docker compose ps

# View container logs
docker compose logs [service_name] --tail 100

# Enter container shell
docker compose exec [service_name] bash

# Check network connectivity
docker compose exec [service_name] ping [other_service]

# Restart specific service
docker compose restart [service_name]
```

---

## 📚 Learning Resources

### **Courses (Free)**

- DataTalks.Club Data Engineering Bootcamp
- dbt Learn (dbt Fundamentals)
- Confluent Developer (Kafka)
- Astronomer Academy (Airflow)

### **Documentation**

- Apache Spark: https://spark.apache.org/docs/latest/
- Apache Kafka: https://kafka.apache.org/documentation/
- Apache Airflow: https://airflow.apache.org/docs/
- dbt: https://docs.getdbt.com/
- Delta Lake: https://docs.delta.io/
- Great Expectations: https://docs.greatexpectations.io/
- Feast: https://docs.feast.dev/

---

## 🎯 Resume Bullets (After Completion)

```
• Architected end-to-end data platform processing 1M+ daily events using Kafka,
  Spark, and Delta Lake, implementing medallion architecture with 99.9% data accuracy

• Built real-time CDC pipeline with Debezium capturing database changes with
  sub-minute latency, enabling near real-time analytics

• Designed and implemented ML feature store using Feast, serving 100+ features
  with <10ms p99 latency for online inference

• Created comprehensive data quality framework using Great Expectations,
  implementing 200+ automated checks reducing data incidents by 70%

• Developed self-service analytics platform with Apache Superset, enabling
  business users to create custom dashboards and reports

• Implemented data observability solution with Prometheus, Grafana, and DataHub,
  providing end-to-end lineage tracking and monitoring
```

---

## 🆘 Getting Help

When asking AI for help with this project, provide:

1. **Which PBI/Feature you're working on**: e.g., "I'm working on PBI-47: Deploy Debezium connector"
2. **Current Sprint**: e.g., "Sprint 6"
3. **Technology involved**: e.g., "Debezium, Kafka Connect, PostgreSQL"
4. **Error message or issue**: Copy exact error
5. **What you've tried**: List attempted solutions

### **Example Help Request**

```
I'm working on PBI-47 (Deploy and test Debezium connector) in Sprint 6.

I'm trying to set up Debezium PostgreSQL connector but getting this error:
[paste error message]

My docker-compose has Kafka Connect running. I've verified:
- PostgreSQL is running
- Kafka is running
- wal_level is set to 'logical'

What could be wrong?
```

---

## 📄 License

MIT License - See LICENSE file for details.

---

**Last Updated**: February 2026
**Project Duration**: 12 months
**Total Effort**: 222 PBIs, 653 Story Points

