# Trabajo Final Integrador — Ciencia de Datos para Economistas (2026-2)
## Importaciones Definitivas del Perú (SUNAT) — Grupo 9

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jshez612/sunat-importaciones-grupo9/blob/main/01_Notebook/Trabajo_Final_Grupo_9.ipynb)

**Universidad de Lima — Facultad de Economía — Carrera de Economía**  
**Asignatura:** Ciencia de Datos para Economistas | **Período Académico:** `2026-2`  
**Docente:** Angel Agüero Correa  
**Equipo (Grupo 9):** Leandro Chavez Cueva, Lesly Condori Huanca, Jose Mariano Castro, Fabrizio Delgado Chavez

---

## 1. Descripción del proyecto, aprobación docente y trazabilidad de la fuente

Este proyecto analiza el universo completo de **Importación Definitiva (Formato A, `ma`)** de la **SUNAT** durante 10 semanas consecutivas (**07/09/2026 al 15/11/2026**, cobertura nacional) para explicar y predecir el **sobrecosto logístico internacional ad valorem** `((FLE_DOLAR + SEG_DOLAR) / FOB_DOLPOL)`, evaluar la carga tributaria aduanera efectiva y descubrir tipologías estructurales de importación mediante **las seis familias de clustering (13 algoritmos reales; ≥ 2 por familia)**.

- **Aprobación docente:** El docente Angel Agüero Correa otorgó el **visto bueno** para trabajar con microdatos de SUNAT (*Importación Definitiva Formato A `ma`*, superando el requisito de **2,000,000 de registros**) y su respaldo crudo e inalterado en el espejo público de GitHub.
- **Fuente oficial original:** SUNAT — Portal de Operatividad Aduanera:
  - Bases semanales (`ma`): `http://www.aduanet.gob.pe/aduanas/informae/`
  - Catálogo de códigos oficiales: `http://www.aduanet.gob.pe/aduanas/sgdbf/`
- **Espejo público crudo e inalterado (sin credenciales):** `https://github.com/jshez612/sunat-importaciones-grupo9/tree/main/datos_crudos`  
  *(Respaldo crudo indispensable porque SUNAT elimina automáticamente los bloques semanales tras 2 meses).*
- **URL base directa de ingesta remota en Colab:** `https://raw.githubusercontent.com/jshez612/sunat-importaciones-grupo9/main/datos_crudos/`
- **Volumen verificado en Google Colab:** **3,155,169 registros × 58 variables crudas** (185.31 MB en `.zip`, 2,554.66 MB en `.DBF`) → **3,155,169 registros × 69 variables finales** (`> 2,000,000` filas exigidas).
- **Licencia y uso:** Datos públicos administrativos difundidos por SUNAT al amparo del Art. 1 de la **Ley N° 27806** (Ley de Transparencia y Acceso a la Información Pública). Uso estrictamente académico. Fecha de respaldo crudo: `2026-11-22`.
- **Ejecución verificada en la nube:** `ejecutado_en_colab = true` | Fecha UTC: `2026-11-26 00:12:53 UTC` | Python `3.13.18` | `SEED = 2026`.

---

## 2. Principales resultados verificados en Google Colab (`SEED = 2026`)

| Dimensión | Resultado Principal Verificado |
| :--- | :--- |
| **Magnitud macro (10 semanas)** | **3,155,169** series (`206,668` DAMs, `21,924` importadores, `178` países); FOB = **USD 14.62 B**, CIF = **USD 15.84 B**, Tributos = **USD 2.94 B** (93.8% IGV+IPM, 3.8% ADV). |
| **Sobrecosto logístico** | Agregado = **8.36%** del FOB (USD 1,222.1 M); Mediana por serie = **5.59%**; P90 = **22.67%**; 12.02% enfrenta sobrecosto ≥ 20%. Winsorización P99.5 estimada solo en S01–S08 (`140.07%`, sin fuga temporal). |
| **Contraste EDA (H1–H4)** | **H1 (Alchian–Allen):** Confirmada (Spearman ρ = `-0.4659`, p < 10⁻³⁰⁰). **H2 (Brecha modal):** Confirmada (Kruskal–Wallis H = `2,075.6`). **H3 (Geografía):** Asia 41.7% (China 30.3%), Callao 77.8% y hallazgo de Aduana `370` (**Chancay**, USD 493.9 M, decodificada e integrada al mapa). **H4 (`TRAT_PREFE`):** No confirmada en Formato A crudo (0.17% de series; explicado por arancel NMF 0% y codificación TPI en Ejemplar B). |
| **Regresión Supervisada (S09–S10)** | Base completa (`2,528,876` train / `626,293` test futuro, 0 DAMs compartidas, exclusión automática de constante `ES_TELEDESPACHO`): Dummy R² = `-0.0001` → **Ridge** R² = `0.6269` (RMSE `0.5611`) → **HistGB** R² = `0.7407` → **LightGBM** **R² = `0.7544`** (RMSE `0.4553`, MdAE `1.52 pp`; GroupKFold-5 por DAM: R² = `0.7045 ± 0.0225`; Ex-Ante Estricto sin `PAGO_INMEDIATO`/`DIAS_DESPACHO`: R² = `0.7547`). |
| **Clasificación Simétrica (≥ 20%)** | Dummy Accuracy `88.0%` (`F1 = 0.00`, `PR-AUC = 0.120`) → **Logistic Reg. (umbral S08)** `PR-AUC = 0.7027`, `F1 = 0.6309`, `Brier = 0.0593` → **HistGB** `PR-AUC = 0.7463`, `F1 = 0.6657` → **LightGBM (umbral S08 = 0.328)**: **ROC-AUC = `0.9379`**, **PR-AUC = `0.7493`**, **F1 = `0.6733`**, **Brier = `0.0544`** (Top 5% SUNAT: Precisión `89.0%`). |
| **Clustering (6 Familias / 13 Métodos)** | PCA sobre 3.15M filas estrictamente como apoyo (`CP1–CP2 = 58.24%`, `CP1–CP4 = 89.04%`). 13 algoritmos evaluados en benchmark común (`N = 3,000`, incluye **Grid-Based CLIQUE 3D**, **BIRCH** y **Fuzzy C-Means** en Familia 6) con matriz ARI 13×13. Campeón **K-Means (`k = 4`, óptimo global en `k=2..8`)** en muestra estratificada `N = 12,000` (Silueta `0.2663`, **ARI semillas = `0.9982`**, **ARI split-half = `0.9641`**, robustez colinealidad 7v vs. 5v ARI = `0.6097`). |

---

## 3. Estructura del paquete final (`Trabajo_Final_Grupo_9.zip`)

```text
Trabajo_Final_Grupo_9/
├── 01_Notebook/
│   └── Trabajo_Final_Grupo_9.ipynb        # Notebook ejecutado 100% en Google Colab (19/19 celdas sin error, sincronizado en GitHub)
├── 02_Informe/
│   ├── Informe_Grupo_9.docx               # Informe académico editable (10 secciones + 10 figuras secuenciales + Anexo A 106 variables)
│   └── Informe_Grupo_9.pdf                # Versión PDF fiel exportada directamente desde Microsoft Word
├── 03_HTML/
│   └── Dashboard_Grupo_9.html             # Dashboard interactivo generado en Python/Colab (Mapas Mundial y Perú + H1/H2/H4 + GroupCV-5 + 13 algoritmos)
├── 04_Datos/
│   ├── Base_Inicial_Grupo_9.parquet       # Base cruda íntegra generada en Colab (3,155,169 × 58 | 148.19 MB ZSTD)
│   └── Base_Final_Grupo_9.parquet         # Base limpia + ingeniería generada en Colab (3,155,169 × 69 | 189.23 MB ZSTD)
└── 05_Documentacion/
    ├── README_Grupo_9.md                  # Esta guía de ejecución, trazabilidad y estructura
    ├── requirements.txt                   # Dependencias reproducibles verificadas en Google Colab
    ├── diccionario_variables.csv          # Diccionario exhaustivo de 106 variables (58 crudas + 48 derivadas/decodificadas)
    └── resumen_metricas_colab.json        # Métricas y tablas exactas exportadas desde Google Colab (ejecutado_en_colab = true)
```

---

## 4. Instrucciones breves de ejecución reproducible en Google Colab

1. Abrir directamente el notebook sincronizado en Google Colab mediante el enlace:  
   `https://colab.research.google.com/github/jshez612/sunat-importaciones-grupo9/blob/main/01_Notebook/Trabajo_Final_Grupo_9.ipynb`
2. Seleccionar **Entorno de ejecución → Ejecutar todas** (`Ctrl + F9`). No se requieren credenciales, tokens ni archivos locales: el notebook instala las dependencias ligeras (`dbfread`, `xlrd`, `pycountry`, `kmedoids`), descarga los 10 `.zip` semanales y los 8 catálogos desde GitHub, fija `SEED = 2026` y exporta todos los entregables en `/content/entregables_colab/`.
3. Tiempo aproximado de ejecución completa en CPU estándar de Google Colab: **15–25 minutos**.
4. Para visualizar el producto interactivo de sustentación, abrir `03_HTML/Dashboard_Grupo_9.html` en cualquier navegador moderno (Chrome, Edge, Firefox) con conexión a internet para cargar Plotly.js desde CDN (`plotly-2.35.2.min.js`).

---

## 5. Dependencias principales

Consulte `requirements.txt` para el listado completo (`numpy>=2.0`, `pandas>=2.2.3`, `pyarrow>=17.0`, `scikit-learn>=1.6`, `lightgbm>=4.5`, `scipy>=1.14`, `plotly>=5.24`, `matplotlib>=3.9`, `seaborn>=0.13`, `requests`, `dbfread`, `xlrd`, `pycountry`, `kmedoids`).
