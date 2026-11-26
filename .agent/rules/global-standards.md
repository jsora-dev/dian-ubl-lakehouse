---
trigger: always_on
---

# Estándares Globales Obligatorios (`dian-ubl-lakehouse`)

Estas reglas son de cumplimiento **estricto, permanente e innegociable** para todos los agentes del proyecto (`00-orchestrator`, `01-data-engineer`, `02-dbt-bi-architect`, `03-qa-sec-auditor` y `04-docs-devops`). Ninguna instrucción táctica puede omitirlas.

---

## 1. Protección de `main` y Respeto Estricto a GitHub Flow

- **Prohibido trabajar sobre `main`:** Ningún agente debe crear commits, editar archivos estando posicionado en `main` ni sugerir comandos de push directo (`git push origin main` o `--force`).
- **Ramas cortas por propósito:** Todo desarrollo se ejecuta sobre ramas derivadas de `main` con prefijos semánticos (`feat/`, `fix/`, `docs/`, `chore/`, `refactor/`, `test/`, `ci/`).
- **Integración exclusiva vía Pull Request:** Todo cambio pasa por revisión del Auditor QA/Sec (`03-qa-sec-auditor`), validación humana en VS Code y apertura de Pull Request completando `.github/pull_request_template.md`.

---

## 2. Datos 100 % Sintéticos y Anonimizados (Cero PII / Cero Facturas Reales)

- **Cumplimiento absoluto de `SECURITY.md`:** Está estrictamente prohibido generar, solicitar, procesar, almacenar o versionar facturas electrónicas reales de la DIAN, NITs privados reales asociados a personas o empresas identificables, certificados digitales reales (`.p12`, `.pfx`, `.pem`, `.key`) o cualquier dato personal identificable (**PII**).
- **Fuentes permitidas:**
  1. Documentos **XML UBL 2.1 100 % sintéticos y deterministas** generados internamente por el proyecto.
  2. Series públicas oficiales de **TRM** consultadas desde el portal de Datos Abiertos Colombia (`datos.gov.co`).
- **Higiene de artefactos de datos:** Los binarios y capas locales (`data/raw/`, `data/processed/`, `*.duckdb`, `*.pbix`) jamás se versionan en Git; solo se permiten muestras sintéticas ligeras y controladas en `data/sample/`.

---

## 3. Cero Secretos Hardcodeados

- **Prohibición total en código y configuración:** Jamás incluir tokens (`SOCRATA_APP_TOKEN`), llaves API, contraseñas, rutas locales privadas de usuario ni cadenas de conexión sensibles en scripts Python, modelos `dbt`, tests, `Dockerfile`, `docker-compose.yml` o workflows de GitHub Actions.
- **Gestión por entorno:** Toda parametrización externa debe leerse mediante variables de entorno y documentarse únicamente con valores ficticios/descriptivos en `.env.example`. El archivo `.env` local nunca se sube al repositorio.

---

## 4. Planificación Obligatoria Antes de Actuar (*Plan-First*)

- **Nunca escribir código a ciegas:** Antes de crear, modificar o eliminar cualquier archivo de código, SQL, pruebas o infraestructura, el agente activo **debe presentar un plan conciso** indicando:
  1. Objetivo del cambio y archivos exactos a crear/modificar.
  2. Decisiones clave, supuestos y posibles casos borde.
  3. Estrategia de validación.
- **Espera de confirmación:** La ejecución solo inicia una vez que el usuario aprueba explícitamente el plan propuesto.

---

## 5. Calidad de Ingeniería, Código Limpio y Conventional Commits

### a) Mensajes de Commit (Conventional Commits)
- Todo commit propuesto debe respetar el formato: `<tipo>(<alcance opcional>): <descripción breve en imperativo>` (ej. `feat(ingestion): add socrata trm client with retry backoff`, `test(dbt): verify credit note sign reconciliation`).
- Tipos válidos: `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `ci`.

### b) Estándares en Python (`src/`, `tests/`)
- **Modularidad y responsabilidad única:** Separar claramente generación sintética (`generator/`), extracción/carga (`ingestion/`) y coordinación secuencial (`orchestration/`).
- **Tipado estricto y contratos:** Uso obligatorio de *type hints* modernos en funciones/métodos y modelos **Pydantic** para validar estructuras semiestructuradas XML y respuestas REST.
- **Resiliencia e idempotencia:** Manejo explícito de errores de red/parseo, logging estructurado (sin imprimir tokens ni payloads sensibles) y cargas idempotentes compatibles con el modelo de **escritor único secuencial de DuckDB** documentado en `docs/adr/0001-initial-architecture-and-stack.md`.

### c) Estándares en SQL y dbt (`dbt_project/`)
- **SQL declarativo y legible:** Estructurar modelos mediante CTEs pequeñas y autoexplicativas (*import CTEs → logical CTEs → final select*), evitando subconsultas anidadas y `SELECT *` en capas finales.
- **Gobernanza del DAG:** Referenciar tablas crudas exclusivamente con `{{ source() }}` y modelos intermedios con `{{ ref() }}`, respetando las capas Medallion (*Bronze → Silver → Gold*).
- **Exactitud financiera:** Utilizar siempre tipos numéricos exactos (`DECIMAL` / `NUMERIC`) para importes monetarios, impuestos y tasas TRM; queda prohibido el uso de `FLOAT` / `DOUBLE` en cálculos contables o fiscales.
