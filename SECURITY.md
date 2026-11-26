# Política de Seguridad y Datos / Security & Data Policy

## Español

### 1. Política Estricta de Datos
Este repositorio es un proyecto de ingeniería de datos con fines técnicos y demostrativos:
- **Datos sintéticos:** Todas las facturas electrónicas bajo estándar **DIAN UBL 2.1 (XML)** utilizadas o generadas en este proyecto son **100 % sintéticas y anonimizadas**.
- **Datos públicos:** Las tasas de cambio (TRM) provienen exclusivamente de la API abierta oficial de **Datos Abiertos Colombia (`datos.gov.co`)**.
- **Prohibición absoluta:** Queda estrictamente prohibido almacenar, procesar o subir al repositorio información tributaria real, facturas reales, certificados digitales criptográficos reales o información personal identificable (**PII**).

### 2. Controles de Seguridad Activos
El repositorio cuenta con las siguientes protecciones habilitadas en GitHub:
- **Secret Scanning & Push Protection:** Bloqueo preventivo de tokens, llaves y credenciales antes de ingresar al historial Git.
- **Dependabot:** Revisión automatizada semanal de vulnerabilidades y actualizaciones en dependencias de `pip`, `github-actions` y `docker`.

### 3. Reporte de Vulnerabilidades
Si identificas una vulnerabilidad de seguridad o una exposición accidental de credenciales:
1. **No abras un Issue público.**
2. Repórtalo de forma confidencial mediante **[GitHub Private Vulnerability Reporting](https://github.com/jsora-dev/dian-ubl-lakehouse/security/advisories/new)** indicando pasos de reproducción e impacto estimado.

---

## English

### 1. Strict Data Policy
This repository is a data platform engineering project built strictly for technical and educational purposes:
- **Synthetic data only:** Every **DIAN UBL 2.1 XML** electronic invoice generated or processed here is **100% synthetic and anonymized**.
- **Open public data:** Exchange rate series (TRM) are sourced exclusively from Colombia's official open data portal (**`datos.gov.co`**).
- **Zero real tax data or PII:** Committing real tax records, authentic digital signatures/certificates, or Personally Identifiable Information (**PII**) is strictly prohibited.

### 2. Active Repository Protections
This repository enforces automated security hygiene via:
- **Secret Scanning & Push Protection:** Prevents credentials and API tokens from being pushed to Git history.
- **Dependabot:** Weekly automated dependency and security checks across `pip`, `github-actions`, and `docker`.

### 3. Reporting a Vulnerability
If you discover a security issue or accidental secret exposure:
1. **Do not open a public GitHub Issue.**
2. Submit a confidential report through **[GitHub Private Vulnerability Reporting](https://github.com/jsora-dev/dian-ubl-lakehouse/security/advisories/new)** with reproduction steps and potential impact.
