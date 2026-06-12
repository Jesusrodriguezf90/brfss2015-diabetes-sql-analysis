# BRFSS 2015 — Análisis SQL de Riesgo de Diabetes

**Autor:** Jesús Rodríguez  
**Dataset:** BRFSS 2015 — Behavioral Risk Factor Surveillance System (CDC)  
**Stack:** DuckDB · pandas · Jupyter · GitHub Actions

---

## Tabla de Contenidos

- [Descripción](#descripción)
- [Relación con el proyecto TFM](#relación-con-el-proyecto-tfm)
- [Funcionalidades](#funcionalidades)
- [Estructura del Proyecto](#estructura-del-proyecto)
- [Notebooks](#notebooks)
- [Hallazgos Clave](#hallazgos-clave)
- [Stack Tecnológico](#stack-tecnológico)
- [Instalación y Puesta en Marcha](#instalación-y-puesta-en-marcha)
- [Fuente de Datos](#fuente-de-datos)
- [Hoja de Ruta](#hoja-de-ruta)
- [Limitaciones](#limitaciones)
- [Licencia](#licencia)

---

## Descripción

Análisis exploratorio SQL sobre el dataset **BRFSS 2015** del CDC para identificar patrones epidemiológicos y cuantificar el impacto de los factores de riesgo modificables asociados a la diabetes.

El análisis utiliza **DuckDB** como motor SQL embebido — sin servidor, sin configuración, directamente sobre el DataFrame de pandas en Jupyter — y progresa desde consultas básicas hasta CTEs y window functions sobre un dataset preprocesado de 257.709 registros clínicos.

---

## Relación con el proyecto TFM

Este repositorio es un análisis complementario al **TFM de Detección de Diabetes**, desarrollado en el Máster en Data Science de KSchool (2025–2026).

| Recurso | Enlace |
|---|---|
| 📦 Dataset en HF Hub | [brfss2015-diabetes-detection](https://huggingface.co/datasets/Jesusrodriguezf90/brfss2015-diabetes-detection) |
| 🤖 Modelo LightGBM | [lgbm-diabetes-early-detection](https://huggingface.co/Jesusrodriguezf90/lgbm-diabetes-early-detection) |
| 🚀 Demo interactiva | [diabetes-risk-demo — HF Spaces](https://huggingface.co/spaces/Jesusrodriguezf90/diabetes-risk-demo) |
| 💻 Repositorio TFM | [TFM — GitHub](https://github.com/Jesusrodriguezf90/TFM) |

El dataset utilizado es la versión preprocesada publicada en HF Hub: 257.709 registros, 22 variables de entrada y variable objetivo `DIABETE3` (desbalance 8:1). Consulta el [README del dataset](https://huggingface.co/datasets/Jesusrodriguezf90/brfss2015-diabetes-detection) para el detalle completo del preprocesamiento y la codificación de variables CDC.

---

## Funcionalidades

> Las funcionalidades marcadas con 🔲 están especificadas y pendientes de implementación.

- ✅ Carga del dataset directamente desde HF Hub — sin datos locales
- ✅ Registro del DataFrame de pandas como tabla DuckDB — sin servidor, sin configuración
- ✅ Perfil epidemiológico de la población por edad, IMC y sexo con etiquetas CDC decodificadas
- ✅ Análisis de prevalencia de diabetes por categorías clínicas
- ✅ Cuantificación del impacto de factores de riesgo modificables — dieta, ejercicio y actividad física
- ✅ Comparativa de comorbilidades entre población diabética y no diabética
- ✅ Análisis avanzado con window functions — percentiles y rankings por clase
- ✅ Hallazgos clave con cifras reales extraídas de las queries
- 🔲 CI/CD con GitHub Actions — ejecución automática de los notebooks en cada push

---

## Estructura del Proyecto

```
brfss2015-diabetes-sql-analysis/
│
├── .github/
│   └── workflows/
│       └── ci.yml                      # CI — ejecución de notebooks con nbconvert
│
├── notebooks/
│   ├── 01_eda_sql.ipynb                # Perfil epidemiológico — GROUP BY, CASE WHEN
│   ├── 02_risk_factors_sql.ipynb       # Factores de riesgo modificables — CTEs, subqueries
│   └── 03_advanced_sql.ipynb           # Window functions + hallazgos clave
│
├── data/
│   └── .gitkeep                        # Dataset cargado desde HF Hub — no incluido en Git
│
├── .gitignore
├── LICENSE
├── pyproject.toml
├── requirements.txt
└── README.md
```

---

## Notebooks

### `01_eda_sql.ipynb` — Perfil epidemiológico

| Sección | Contenido | Técnicas SQL |
|---|---|---|
| **0 — Setup** | Carga del dataset desde HF Hub, registro en DuckDB, validación del esquema | `SELECT COUNT`, inspección de tipos |
| **1 — Perfil de la población** | Distribución por edad, IMC y sexo cruzada con clase diabética | `GROUP BY`, `CASE WHEN`, `HAVING`, `ROUND` |

### `02_risk_factors_sql.ipynb` — Factores de riesgo modificables

| Sección | Contenido | Técnicas SQL |
|---|---|---|
| **2 — Factores de riesgo** | Delta de medias entre clases, prevalencia por actividad física × IMC, comparativa de comorbilidades | Subqueries, CTEs, `JOIN` sobre agregados, `AVG` |

### `03_advanced_sql.ipynb` — Análisis avanzado y hallazgos

| Sección | Contenido | Técnicas SQL |
|---|---|---|
| **3 — Análisis avanzado** | Percentiles de dieta por clase, ranking de grupos de edad por prevalencia | `PERCENT_RANK()`, `ROW_NUMBER()`, `PARTITION BY` |
| **4 — Hallazgos clave** | Conclusiones cuantificadas con cifras reales extraídas de las queries | — |

---

## Hallazgos Clave

> Resultados extraídos directamente de las queries SQL sobre el dataset completo (257.709 registros).

- La **prevalencia global de diabetes** en la muestra es del **11.0%** — desbalance 8:1 respecto a la clase negativa.
- El **grupo de edad con mayor prevalencia** es **75-79 años (19.0%)**, con diferencia significativa por sexo: 22.9% en hombres frente a 16.4% en mujeres.
- La **obesidad multiplica por 4.4 la prevalencia** respecto al normopeso: **20.8% vs 4.7%**.
- El **73.2% de los diabéticos** presenta hipertensión confirmada, frente al **32.2% de los no diabéticos** — el mayor delta de todas las comorbilidades analizadas.
- La **mediana de consumo de fruta (P50)** es ligeramente inferior en diabéticos: **1.02 vs 1.11 porciones/día**.

---

## Stack Tecnológico

| Componente | Herramienta | Propósito |
|---|---|---|
| Motor SQL | [DuckDB](https://duckdb.org/) | Consultas analíticas embebidas sobre DataFrames de pandas |
| Manipulación de datos | [pandas](https://pandas.pydata.org/) | Carga del dataset desde HF Hub |
| Dataset | [HF Hub](https://huggingface.co/datasets/Jesusrodriguezf90/brfss2015-diabetes-detection) | Fuente de datos — cargado directamente con `pd.read_csv` |
| Visualización | [matplotlib](https://matplotlib.org/) / [seaborn](https://seaborn.pydata.org/) | Gráficos complementarios a los resultados SQL |
| Entorno | [Jupyter](https://jupyter.org/) | Desarrollo interactivo del análisis |
| CI/CD | [GitHub Actions](https://github.com/features/actions) | Ejecución automática del notebook en cada push |

---

## Instalación y Puesta en Marcha

### Requisitos previos

- Python 3.11+

### Ejecución local

```bash
# 1. Clonar el repositorio
git clone https://github.com/Jesusrodriguezf90/brfss2015-diabetes-sql-analysis.git
cd brfss2015-diabetes-sql-analysis

# 2. Crear y activar entorno virtual
python -m venv venv
source venv/bin/activate        # En Windows: venv\Scripts\activate

# 3. Instalar dependencias
pip install -r requirements.txt

# 4. Abrir el primer notebook
jupyter notebook notebooks/01_eda_sql.ipynb
```

---

## Fuente de Datos

**BRFSS 2015 — Behavioral Risk Factor Surveillance System**  
Centers for Disease Control and Prevention (CDC)  
🔗 [CDC BRFSS 2015](https://www.cdc.gov/brfss/annual_data/annual_2015.html)

| Característica | Valor |
|---|---|
| Observaciones | 257.709 |
| Variables de entrada | 22 |
| Variable objetivo | `DIABETE3` — riesgo de diabetes (1.0 = riesgo · 3.0 = sin riesgo) |
| Desbalance | 8:1 — 11% positivos |
| Formato | CSV preprocesado — publicado en [HF Hub](https://huggingface.co/datasets/Jesusrodriguezf90/brfss2015-diabetes-detection) |

El dataset se utiliza exclusivamente con fines de investigación y desarrollo. Los datos del BRFSS 2015 son producidos por el CDC (Gobierno Federal de EE.UU.) y son de dominio público bajo 17 U.S.C. § 105. Su uso requiere atribución al CDC y un disclaimer de no-endorsement conforme a los [términos de uso del CDC](https://www.cdc.gov/other/agencymaterials.html).

> El uso de este dataset no implica endorsement por parte del CDC, HHS ni del Gobierno de los Estados Unidos.

---

## Hoja de Ruta

| Estado | Componente |
|---|---|
| ✅ | Estructura del proyecto y documentación |
| ✅ | `01_eda_sql` — carga del dataset, perfil epidemiológico |
| ✅ | `02_risk_factors_sql` — factores de riesgo modificables con CTEs |
| ✅ | `03_advanced_sql` — window functions y hallazgos clave |
| 🔲 | CI/CD con GitHub Actions — ejecución automática de notebooks |
| 🔲 | Visualizaciones complementarias con matplotlib/seaborn |

---

## Limitaciones

- Los datos del BRFSS son **autorreportados mediante encuesta telefónica** — sujetos a sesgo de respuesta y de memoria
- Población exclusivamente **estadounidense adulta (≥18 años)** — puede no generalizar a otras poblaciones sin recalibración
- **Año de recogida: 2015** — los patrones de salud y los factores de riesgo pueden haber evolucionado desde entonces
- El dataset **no incluye datos biométricos objetivos** (glucemia, HbA1c) — la variable objetivo `DIABETE3` es autorreportada
- El análisis SQL opera sobre el **dataset preprocesado**, no sobre el fichero raw del CDC — las transformaciones aplicadas (filtrado, recodificación, imputación) están documentadas en el [README del dataset](https://huggingface.co/datasets/Jesusrodriguezf90/brfss2015-diabetes-detection)
- El **desbalance 8:1** entre clases implica que las prevalencias calculadas reflejan la distribución real de la encuesta, no una muestra equilibrada

---

## Licencia

Este proyecto está distribuido bajo la Licencia MIT. Consulta el archivo [LICENSE](LICENSE) para más detalles.

---

<p align="center">
  Construido sobre datos reales del CDC — análisis SQL para detección temprana de diabetes
</p>
