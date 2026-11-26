# ADR 0001: Arquitectura Inicial y Stack Local (DuckDB + dbt + Python + Docker)

- **Estado:** Aceptado
- **Fecha:** 2026-11-25

## Contexto

El proyecto `dian-ubl-lakehouse` requiere procesar facturas electrónicas sintéticas en formato semiestructurado **DIAN UBL 2.1 (XML)** combinadas con series diarias de **TRM (`datos.gov.co`)** bajo una arquitectura **Medallion** (*Bronze, Silver, Gold*) que alimente un modelo analítico en **Power BI**.

Necesitamos demostrar prácticas de ingeniería de datos de nivel productivo (ingesta idempotente, modelado dimensional, pruebas de calidad de datos, linaje y gobernanza), garantizando al mismo tiempo que cualquier evaluador técnico o desarrollador pueda clonar y ejecutar la plataforma sin cuentas pagas en la nube ni costos recurrentes.

## Decisión

Adoptamos una arquitectura **Single-Node Lakehouse** contenerizada basada en:

1. **Python:** Generación de XML UBL 2.1 sintéticos, extracción de datos semiestructurados (XML/REST) y carga a la capa *Bronze*.
2. **DuckDB:** Motor OLAP columnar en proceso (*in-process*) almacenado localmente como motor de cómputo analítico.
3. **dbt (`dbt-duckdb`):** Transformaciones SQL declarativas hacia capas *Silver* y *Gold*, documentación de catálogo, linaje y pruebas de calidad (`dbt test`).
4. **Docker:** Empaquetado reproducible del entorno de ejecución de principio a fin.

## Alternativas Descartadas

- **Clústeres distribuidos o Data Warehouses Cloud (Spark/Databricks, BigQuery, Snowflake) en Fase Inicial:** Descartados para este volumen de datos por introducir costos de infraestructura innecesarios (*over-engineering*), latencia de aprovisionamiento y fricción de credenciales IAM/facturación para quienes auditan el repositorio localmente.

## Consecuencias

### Positivas
- **Costo cero ($0):** Sin gasto en cómputo cloud ni almacenamiento externo.
- **Reproducibilidad inmediata:** Ejecución determinista local mediante Python/Docker sin depender de infraestructura externa (salvo el endpoint público de `datos.gov.co`).
- **Escalabilidad posterior desacoplada:** Al separar la ingesta en Python y escribir la lógica analítica en SQL estándar gobernado por **dbt**, migrar en el futuro hacia **MotherDuck**, **BigQuery**, **Snowflake** o **Databricks** solo exige ajustar el adaptador/perfil de conexión sin reescribir el modelo dimensional.

### Compromisos (Trade-offs)
- **Concurrencia de escritura:** DuckDB opera con escritor único local, por lo que las cargas se ejecutan de manera secuencial orquestada (totalmente adecuado para lotes analíticos de este alcance).
- **Escalamiento vertical:** El volumen máximo procesable queda acotado a la memoria/disco del contenedor o máquina anfitriona hasta que se migre el adaptador.
