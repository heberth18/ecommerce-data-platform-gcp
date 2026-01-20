# 🚀 E-commerce Data Engineering Pipeline

## 📊 Contexto de Negocio

**Empresa:** Startup e-commerce B2C en Chile  
**Problema:** Necesidad de métricas confiables, reproducibles y auditables de ventas y clientes  
**Solución:** Pipeline de datos end-to-end orquestado, contenerizado y escalable

---

## 🎯 Objetivo

Generar KPIs mensuales de ventas y clientes listos para consumo analítico (BI y toma de decisiones).

---

## 🏗️ Arquitectura

```
FakeAPI → Airflow → GCS (Bronze) → BigQuery (Bronze) → dbt → BigQuery (Gold) → BI
```

### Capas de Datos

1. **Bronze (Raw):** Datos crudos desde la API, almacenados en GCS y BigQuery sin transformaciones
2. **Silver (Staging):** Limpieza básica, tipado correcto, deduplicación
3. **Gold (Analytics):** Modelos dimensionales (fact/dim) y KPIs agregados

---

## 🛠️ Stack Tecnológico

| Componente | Tecnología | Justificación |
|------------|------------|---------------|
| **Orquestación** | Apache Airflow | Estándar industria para workflows complejos, monitoreo visual, retry logic |
| **Almacenamiento** | Google Cloud Storage | Data Lake escalable, bajo costo, integración nativa con BigQuery |
| **Data Warehouse** | Google BigQuery | Separación compute/storage, SQL estándar, optimizado para analytics |
| **Transformaciones** | dbt | Versionamiento de lógica de negocio, tests, documentación como código |
| **Contenedores** | Docker Compose | Reproducibilidad, portabilidad, aislamiento de dependencias |
| **Fuente de Datos** | FakeAPI | Simula sistema transaccional de e-commerce |

---

## 📁 Estructura del Proyecto

```
project_ecommerce/
├── airflow/                    # Orquestación
│   ├── dags/                   # DAGs de Airflow
│   ├── plugins/                # Operadores custom
│   ├── scripts/                # Scripts de extracción/carga
│   └── config/                 # Configuraciones
├── dbt/                        # Transformaciones
│   ├── models/
│   │   ├── staging/            # Limpieza y validación
│   │   ├── marts/
│   │   │   ├── core/           # Facts y Dimensions
│   │   │   └── kpis/           # KPIs agregados
│   ├── tests/                  # Tests de calidad
│   └── macros/                 # Funciones reutilizables
├── docker/                     # Configuraciones Docker
├── docs/                       # Documentación técnica
├── .gitignore
├── docker-compose.yml
├── requirements.txt
└── README.md
```

---

## 🔄 Pipeline de Datos

### DAG Principal: `ecommerce_pipeline`

**Frecuencia:** Diaria (puede ajustarse a mensual para KPIs)

**Tareas:**

1. `extract_from_fakeapi`: Extrae datos de FakeAPI → GCS (formato JSON)
2. `load_to_bigquery_bronze`: Carga desde GCS → BigQuery (tablas bronze)
3. `dbt_run_staging`: Limpieza y validación (modelos staging)
4. `dbt_run_marts`: Genera facts, dimensions y KPIs
5. `dbt_test`: Valida calidad de datos

---

## 🗂️ Modelos de Datos

### Bronze (Raw)
- `bronze_customers`
- `bronze_products`
- `bronze_orders`
- `bronze_order_items`
- `bronze_payments`

### Staging
- `stg_customers`
- `stg_products`
- `stg_orders`
- `stg_order_items`
- `stg_payments`

### Marts
- **Dimensions:**
  - `dim_customers`
  - `dim_products`
  - `dim_date`
- **Facts:**
  - `fact_orders`
- **KPIs:**
  - `kpi_monthly_sales`
  - `kpi_customer_metrics`

---

## 🚀 Cómo Ejecutar

### Prerrequisitos

1. Docker y Docker Compose instalados
2. Cuenta de Google Cloud Platform
3. Proyecto GCP creado
4. Service Account con permisos:
   - BigQuery Admin
   - Storage Admin

### Setup Inicial

```bash
# 1. Clonar repositorio
git clone <repo-url>
cd project_ecommerce

# 2. Configurar variables de entorno
cp .env.example .env
# Editar .env con tus credenciales GCP

# 3. Colocar service account key
# Guardar archivo JSON en: ./secrets/gcp-service-account.json

# 4. Levantar servicios
docker-compose up -d

# 5. Acceder a Airflow
# http://localhost:8080
# User: airflow
# Password: airflow
```

---

## 📊 KPIs Implementados (Fase 1)

1. **Ventas Mensuales Totales:** Ingresos agregados por mes
2. **Cantidad de Órdenes:** Total de órdenes procesadas
3. **Ticket Promedio:** Valor promedio por orden
4. **Clientes Activos:** Clientes únicos con compras en el período
5. **Productos Más Vendidos:** Top 10 productos por cantidad

---

## 🧪 Validaciones y Tests

- **Unicidad:** Primary keys únicos en facts y dimensions
- **Completitud:** Campos obligatorios no nulos
- **Relaciones:** Foreign keys válidas
- **Rango:** Montos positivos, fechas válidas

---

## 📝 Convenciones de Commits

- `feat:` Nueva funcionalidad
- `fix:` Corrección de bugs
- `test:` Agregar tests
- `docs:` Documentación
- `refactor:` Mejoras de código sin cambiar funcionalidad
- `perf:` Optimizaciones de rendimiento

---

## 📧 Contacto

Heberth Caripa - [LinkedIn](www.linkedin.com/in/heberth-caripa) - [GitHub](https://github.com/heberth18)

---

## 📄 Licencia

MIT

---
---

# 🚀 E-commerce Data Engineering Pipeline

**[English Version]**

## 📊 Business Context

**Company:** B2C e-commerce Startup in Chile  
**Problem:** Need for reliable, reproducible, and auditable sales and customer metrics  
**Solution:** End-to-end orchestrated, containerized, and scalable data pipeline

---

## 🎯 Objective

Generate monthly sales and customer KPIs ready for analytical consumption (BI and decision-making).

---

## 🏗️ Architecture

```
FakeAPI → Airflow → GCS (Bronze) → BigQuery (Bronze) → dbt → BigQuery (Gold) → BI
```

### Data Layers

1. **Bronze (Raw):** Raw data from the API, stored in GCS and BigQuery without transformations
2. **Silver (Staging):** Basic cleaning, correct typing, deduplication
3. **Gold (Analytics):** Dimensional models (fact/dim) and aggregated KPIs

---

## 🛠️ Technology Stack

| Component | Technology | Justification |
|-----------|------------|---------------|
| **Orchestration** | Apache Airflow | Industry standard for complex workflows, visual monitoring, retry logic |
| **Storage** | Google Cloud Storage | Scalable Data Lake, low cost, native BigQuery integration |
| **Data Warehouse** | Google BigQuery | Compute/storage separation, standard SQL, optimized for analytics |
| **Transformations** | dbt | Business logic versioning, tests, documentation as code |
| **Containers** | Docker Compose | Reproducibility, portability, dependency isolation |
| **Data Source** | FakeAPI | Simulates e-commerce transactional system |

---

## 📁 Project Structure

```
project_ecommerce/
├── airflow/                    # Orchestration
│   ├── dags/                   # Airflow DAGs
│   ├── plugins/                # Custom operators
│   ├── scripts/                # Extraction/loading scripts
│   └── config/                 # Configurations
├── dbt/                        # Transformations
│   ├── models/
│   │   ├── staging/            # Cleaning and validation
│   │   ├── marts/
│   │   │   ├── core/           # Facts and Dimensions
│   │   │   └── kpis/           # Aggregated KPIs
│   ├── tests/                  # Quality tests
│   └── macros/                 # Reusable functions
├── docker/                     # Docker configurations
├── docs/                       # Technical documentation
├── .gitignore
├── docker-compose.yml
├── requirements.txt
└── README.md
```

---

## 🔄 Data Pipeline

### Main DAG: `ecommerce_pipeline`

**Frequency:** Daily (can be adjusted to monthly for KPIs)

**Tasks:**

1. `extract_from_fakeapi`: Extract data from FakeAPI → GCS (JSON format)
2. `load_to_bigquery_bronze`: Load from GCS → BigQuery (bronze tables)
3. `dbt_run_staging`: Cleaning and validation (staging models)
4. `dbt_run_marts`: Generate facts, dimensions and KPIs
5. `dbt_test`: Validate data quality

---

## 🗂️ Data Models

### Bronze (Raw)
- `bronze_customers`
- `bronze_products`
- `bronze_orders`
- `bronze_order_items`
- `bronze_payments`

### Staging
- `stg_customers`
- `stg_products`
- `stg_orders`
- `stg_order_items`
- `stg_payments`

### Marts
- **Dimensions:**
  - `dim_customers`
  - `dim_products`
  - `dim_date`
- **Facts:**
  - `fact_orders`
- **KPIs:**
  - `kpi_monthly_sales`
  - `kpi_customer_metrics`

---

## 🚀 How to Run

### Prerequisites

1. Docker and Docker Compose installed
2. Google Cloud Platform account
3. GCP project created
4. Service Account with permissions:
   - BigQuery Admin
   - Storage Admin

### Initial Setup

```bash
# 1. Clone repository
git clone <repo-url>
cd project_ecommerce

# 2. Configure environment variables
cp env.example .env
# Edit .env with your GCP credentials

# 3. Place service account key
# Save JSON file to: ./secrets/gcp-service-account.json

# 4. Start services
docker-compose up -d

# 5. Access Airflow
# http://localhost:8080
# User: airflow
# Password: airflow
```

---

## 📊 Implemented KPIs (Phase 1)

1. **Total Monthly Sales:** Revenue aggregated by month
2. **Order Quantity:** Total processed orders
3. **Average Ticket:** Average value per order
4. **Active Customers:** Unique customers with purchases in the period
5. **Best-Selling Products:** Top 10 products by quantity

---

## 🧪 Validations and Tests

- **Uniqueness:** Unique primary keys in facts and dimensions
- **Completeness:** Non-null mandatory fields
- **Relationships:** Valid foreign keys
- **Range:** Positive amounts, valid dates

---

## 📝 Commit Conventions

- `feat:` New functionality
- `fix:` Bug fixes
- `test:` Add tests
- `docs:` Documentation
- `refactor:` Code improvements without changing functionality
- `perf:` Performance optimizations

---

## 📧 Contact

Heberth Caripa - [LinkedIn](www.linkedin.com/in/heberth-caripa) - [GitHub](https://github.com/heberth18)

---

## 📄 License

MIT
