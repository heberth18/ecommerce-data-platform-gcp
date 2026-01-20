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

## 📈 Roadmap

### ✅ Fase 1 - Junior (Actual)
- Pipeline funcional end-to-end
- Orquestación con Airflow
- Modelos dbt básicos
- Validaciones básicas

### 🔜 Fase 2 - Semi-Senior
- Incremental models
- Particionado y clustering
- Manejo de backfills
- Optimización de costos

### 🔜 Fase 3 - Senior
- Observabilidad completa
- CI/CD con GitHub Actions
- Infrastructure as Code
- Data contracts

---

## 🤝 Decisiones Técnicas

### ¿Por qué Airflow y no Prefect/Dagster?
- **Airflow:** Madurez, comunidad grande, más usado en Chile
- **Trade-off:** Configuración más compleja vs más oportunidades laborales

### ¿Por qué BigQuery y no Snowflake?
- **BigQuery:** Serverless, pago por consulta, integración GCP
- **Trade-off:** Vendor lock-in vs menor overhead operacional

### ¿Por qué Docker Compose y no Kubernetes?
- **Docker Compose:** Suficiente para scope de portafolio, más simple
- **Trade-off:** No escalable a producción real vs facilidad de setup

### ¿Por qué GCS como staging y no directo a BigQuery?
- **GCS:** Auditabilidad, posibilidad de re-procesamiento, costo
- **Trade-off:** Latencia adicional vs mayor resiliencia

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

Heberth - [LinkedIn](#) - [GitHub](#)

---

## 📄 Licencia

MIT
