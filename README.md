# Caso 2 — Pipeline de Datos en la Nube DataCo

## Integrantes
- Melanny Arianna Cordoba Bermudez Cc 1000899222
- Jerónimo Londoño Madrid Cc 1000412774
- Luis Miguel Carrillo García Cc 1017239239
- Nombre Apellido

## Curso
Computación en la Nube — Semestre 2026-1  
Tecnológico de Antioquia — Institución Universitaria

Profesor: Julian David Florez Sanchez

---

# 1. Introducción

DataCo es una empresa colombiana dedicada a la distribución de productos de consumo masivo, con operaciones distribuidas en 12 departamentos del país.

Actualmente la empresa presenta problemas de integración y consolidación de información debido a que sus sistemas de ventas, inventario, logística y CRM funcionan de manera aislada.

El objetivo de este proyecto es diseñar e implementar una arquitectura cloud moderna en Microsoft Azure que permita:

- Centralizar la información empresarial
- Automatizar procesos ETL
- Mejorar la calidad de datos
- Reducir tiempos de generación de reportes
- Garantizar trazabilidad y escalabilidad
- Permitir visualización analítica en Power BI

---

# 2. Objetivos

## Objetivo General

Diseñar e implementar un pipeline de datos en la nube utilizando servicios Azure para consolidar información empresarial de DataCo.

## Objetivos Específicos

- Integrar datos provenientes de SAP, Oracle, GPS y Salesforce.
- Automatizar procesos de ingesta y transformación.
- Implementar validaciones y limpieza de datos.
- Construir un almacén analítico centralizado.
- Visualizar información mediante dashboards en Power BI.
- Garantizar tolerancia a fallos y trazabilidad.

---

# 3. Arquitectura General

La solución implementada sigue una arquitectura moderna de analítica basada en servicios PaaS de Microsoft Azure.

## Stack Tecnológico

| Servicio | Función |
|---|---|
| Azure Data Factory | Orquestación e ingesta ETL |
| Azure Data Lake Storage Gen2 | Almacenamiento RAW y CURATED |
| Azure Databricks | Procesamiento y transformación Spark |
| Azure SQL Database | Data Warehouse analítico |
| Power BI Desktop | Dashboards y visualización |

---

# 4. Modelo C4

---

# 4.1 Diagrama C1 — Contexto

El diagrama C1 representa el sistema como una caja negra e identifica:

- Actores del negocio
- Sistemas externos
- Relaciones principales
- Flujo de información

## Actores

| Actor | Función |
|---|---|
| Analista BI | Consulta datos consolidados |
| Gerente Comercial | Toma decisiones basadas en reportes |
| Auditor | Revisa trazabilidad y calidad |

## Sistemas Externos

| Sistema | Tecnología | Datos |
|---|---|---|
| ERP de ventas | SAP On-premise | Facturas, pedidos, devoluciones |
| Inventario | Oracle Database | Stock y movimientos |
| GPS Flota | CSV manual | Rutas y entregas |
| CRM Comercial | Salesforce | Clientes y acuerdos |

## Diagrama

![C1](./assets/C1_Pipeline%20DataCo_Contexto.jpg)

## Flujo General

1. Los sistemas fuente generan datos.
2. El pipeline cloud centraliza la información.
3. Los datos consolidados son publicados en Power BI.
4. Los usuarios consumen dashboards y reportes.

---

# 4.2 Diagrama C2 — Contenedores

El diagrama C2 describe los servicios Azure utilizados y el flujo de datos entre ellos.

## Componentes Principales

### Azure Data Factory
Responsable de:
- Orquestación ETL
- Programación de pipelines
- Reintentos automáticos
- Tolerancia a fallos parciales

Frecuencia:
- Ejecución cada 4 horas

---

### Data Lake RAW Zone
Almacena:
- CSV
- JSON
- Datos originales sin transformar

Características:
- Particionado por fecha
- Persistencia histórica
- Trazabilidad

---

### Azure Databricks
Responsable de:
- Limpieza de duplicados
- Normalización de códigos
- Validación de calidad
- Enriquecimiento logístico

Tecnología:
- Apache Spark

---

### Data Lake CURATED Zone
Almacena:
- Datos transformados
- Datos enriquecidos
- Archivos Parquet

Ventajas:
- Mejor rendimiento en Spark
- Compresión optimizada
- Consultas analíticas eficientes

---

### Azure SQL Database
Responsable de:
- Modelo relacional analítico
- Tablas de hechos y dimensiones
- Seguridad por roles

---

### Power BI Desktop
Responsable de:
- Dashboards ejecutivos
- KPIs comerciales
- Reportes automáticos

---

## Diagrama

![C2](./assets/C2_Pipeline%20DataCo_Contenedores.jpg)

---

# 4.3 Diagrama C3 — Componentes

El diagrama C3 detalla los componentes internos de Azure Databricks.

## Notebooks Implementados

| Notebook | Responsabilidad |
|---|---|
| ingest_sap.py | Lectura de datos RAW |
| clean_inventory.py | Limpieza y validación |
| enrich_deliveries.py | Enriquecimiento logístico |
| load_warehouse.py | Carga al Data Warehouse |

---

## Flujo Interno

### 1. ingest_sap.py
Funciones:
- Leer archivos CSV/JSON
- Validar estructura
- Generar DataFrames Spark
- Registrar logs de ingesta

---

### 2. clean_inventory.py
Funciones:
- Eliminar duplicados
- Estandarizar fechas
- Normalizar códigos
- Validar calidad de datos (>98%)

---

### 3. enrich_deliveries.py
Funciones:
- Cruce ERP + GPS + CRM
- Relacionar ventas con entregas
- Generar trazabilidad logística
- Crear datasets curados

---

### 4. load_warehouse.py
Funciones:
- Cargar dimensiones
- Cargar tabla de hechos
- Ejecutar cargas incrementales
- Publicar datos en Azure SQL

---

## Spark Cluster

Características:
- Procesamiento distribuido
- Ejecución batch cada 4 horas
- Tolerancia a fallos parciales

---

## Diagrama

![C3](./assets/C3_Pipeline%20DataCo_ContextoDatabricks.png)

---

# 5. Pipeline ETL

## Flujo Completo

### Paso 1 — Ingesta RAW
Azure Data Factory detecta archivos:
- CSV
- JSON
- Exportaciones SAP
- Datos GPS

Destino:
- Data Lake RAW Zone

---

### Paso 2 — Limpieza y Calidad
Databricks ejecuta:
- Eliminación de duplicados
- Validación de formatos
- Corrección de fechas
- Normalización de códigos

---

### Paso 3 — Enriquecimiento
Cruce de información:
- Ventas
- Inventario
- Rutas GPS
- Información CRM

Resultado:
- Dataset enriquecido y trazable

---

### Paso 4 — Curated Layer
Datos almacenados en:
- Formato Parquet
- Zona CURATED

Beneficios:
- Mejor rendimiento
- Menor almacenamiento
- Optimización analítica

---

### Paso 5 — Data Warehouse
Carga hacia Azure SQL:
- Tabla de hechos ventas
- Dimensiones:
  - Cliente
  - Producto
  - Fecha
  - Región

---

### Paso 6 — Visualización
Power BI consume información desde Azure SQL para:
- Dashboards ejecutivos
- KPIs comerciales
- Indicadores logísticos

---

# 6. Problemas Resueltos

| Problema Inicial | Solución Implementada |
|---|---|
| Reportes manuales | Automatización ETL |
| Datos inconsistentes | Normalización y validación |
| Retrasos de información | Actualización cada 4 horas |
| Sin trazabilidad | Integración ERP + GPS |
| Escalabilidad limitada | Arquitectura cloud distribuida |

---

# 7. ADRs — Decisiones Arquitectónicas

## ADR-01 — Azure Data Factory vs Logic Apps

### Decisión
Se seleccionó Azure Data Factory para la orquestación ETL.

### Justificación
- Mejor integración con Data Lake
- Diseñado para pipelines de datos
- Reintentos automáticos
- Menor complejidad operativa

---

## ADR-02 — Azure Databricks vs Synapse

### Decisión
Se seleccionó Azure Databricks.

### Justificación
- Community Edition gratuita
- Compatible con Spark
- Menor costo
- Adecuado para transformación ETL

---

## ADR-03 — Data Lake Gen2 vs Blob Storage

### Decisión
Se seleccionó Data Lake Storage Gen2.

### Justificación
- Jerarquía de carpetas
- Optimización analítica
- Mejor integración con Spark

---

## ADR-04 — Azure SQL vs Cosmos DB

### Decisión
Se seleccionó Azure SQL Database.

### Justificación
- Modelo relacional
- Integración nativa con Power BI
- Consultas analíticas eficientes

---

## ADR-05 — Power BI vs Azure Analysis Services

### Decisión
Se seleccionó Power BI Desktop.

### Justificación
- Ya licenciado por la empresa
- Sin costos adicionales
- Fácil adopción por usuarios

---

# 8. Evidencias de Implementación

## Azure Data Factory
- Capturas de pipelines ejecutados
- Monitor de ejecución

## Data Lake
- Archivos RAW cargados
- Archivos Parquet CURATED

## Databricks
- Notebooks ejecutados
- Transformaciones aplicadas

## Azure SQL
- Tablas cargadas
- Consultas SQL exitosas

## Power BI
- Dashboard conectado
- Visualizaciones funcionales

---

# 9. Estructura del Repositorio

```bash
/
├── README.md
├── assets/
│   ├── C1_Pipeline_DataCo_Contexto.jpg
│   ├── C2_Pipeline_DataCo_Contenedores.jpg
│   └── C3_Pipeline_DataCo_ContextoDatabricks.png
├── src/
│   ├── databricks/
│   ├── Datafactory/
│   ├── sql/
│   ├── pipelines/
│   └── datasets/