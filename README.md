# Trabajo Final Integrador — Ciencia de Datos para Economistas (2026-2)
## Importaciones Definitivas del Perú (SUNAT) — Grupo 9

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jshez612/sunat-importaciones-grupo9/blob/main/01_Notebook/Trabajo_Final_Grupo_9.ipynb)

**Universidad de Lima — Facultad de Economía — Carrera de Economía**  
**Asignatura:** Ciencia de Datos para Economistas | **Período Académico:** `2026-2`  
**Docente:** Angel Agüero Correa  
**Equipo (Grupo 9):** Leandro Chavez Cueva, Lesly Condori Huanca, Jose Mariano Castro, Fabrizio Delgado Chavez

---

## 1. Descripción del proyecto y trazabilidad de la fuente

Este proyecto analiza el universo completo de **Importación Definitiva (Formato A, `ma`)** de la **SUNAT** durante 10 semanas consecutivas (**07/09/2026 al 15/11/2026**, cobertura nacional) para explicar y predecir el **sobrecosto logístico internacional ad valorem** `((FLE_DOLAR + SEG_DOLAR) / FOB_DOLPOL)`, evaluar la carga tributaria aduanera efectiva y descubrir tipologías estructurales de importación mediante **las seis familias de clustering (12 algoritmos reales)**.

- **Fuente oficial original:** SUNAT — Portal de Operatividad Aduanera:
  - Bases semanales (`ma`): `http://www.aduanet.gob.pe/aduanas/informae/`
  - Catálogo de códigos oficiales: `http://www.aduanet.gob.pe/aduanas/sgdbf/`
- **Espejo público crudo e inalterado (sin credenciales):** `https://github.com/jshez612/sunat-importaciones-grupo9/tree/main/datos_crudos`  
  *(Se aloja el respaldo crudo exacto porque SUNAT elimina automáticamente los bloques semanales tras 2 meses).*
- **URL base directa de ingesta remota en Colab:** `https://raw.githubusercontent.com/jshez612/sunat-importaciones-grupo9/main/datos_crudos/`
- **Volumen verificado:** **3,155,169 registros × 58 variables crudas** (185.31 MB en `.zip`, 2,554.66 MB en `.DBF`) → **3,155,169 registros × 69 variables finales** (`> 2,000,000` filas exigidas por el docente).
- **Licencia y uso:** Datos públicos administrativos difundidos por SUNAT al amparo del Art. 1 de la **Ley N° 27806** (Ley de Transparencia y Acceso a la Información Pública). Uso estrictamente académico. Fecha de respaldo crudo: `2026-11-22`.
- **Nota expresa de aprobación docente:** El docente Angel Agüero Correa validó en clase trabajar con microdatos de SUNAT bajo el requisito de superar **2,000,000 de registros**; se envió el correo formal solicitando aprobación escrita de *Importación Definitiva (`ma`)* y del espejo en GitHub, quedando pendiente dicha confirmación escrita formal.

---

## 2. Principales resultados verificados (ejecutados 100% en Google Colab, `SEED = 2026`)

| Dimensión | Resultado Principal Verificado |
| :--- | :--- |
| **Magnitud macro (10 semanas)** | **3,155,169** series (`206,668` DAMs, `21,924` importadores, `178` países); FOB = **USD 14.62 B**, CIF = **USD 15.84 B**, Tributos = **USD 2.94 B** (93.8% IGV+IPM, 3.8% ADV). |
| **Sobrecosto logístico** | Agregado = **8.36%** del FOB (USD 1,222.1 M); Mediana por serie = **5.59%**; P90 = **22.67%**; 12.02% enfrenta sobrecosto ≥ 20%. |
| **Contraste EDA (H1–H4)** | **H1 (Alchian–Allen):** Confirmada ($\rho = -0.4659$, $p < 10^{-300}$). **H2 (Brecha modal):** Confirmada ($H = 2,075.6$). **H3 (Geografía):** Asia 41.7% (China 30.3%), Callao 77.8% y hallazgo de Aduana `370` (Chancay, USD 493.9 M). **H4 (`TRAT_PREFE`):** No confirmada en Formato A crudo (0.17% de series; explicado por arancel NMF 0% y codificación TPI en Ejemplar B). |
| **Regresión Supervisada (S09–S10)** | Base completa (`2,528,876` train / `626,293` test futuro): Dummy $R^2 = -0.0001$ → **Ridge** $R^2 = 0.6268$ (RMSE `0.5617`) → **HistGB** $R^2 = 0.7426$ → **LightGBM** **$R^2 = 0.7584$** (RMSE `0.4519`, MdAE `1.51 pp`, CV-5 $R^2 = 0.8085 \pm 0.0017$). |
| **Clasificación Desbalanceada (≥ 20%)** | Dummy Accuracy `88.0%` pero `F1 = 0.00` / `PR-AUC = 0.120` → **Logistic Regression** `PR-AUC = 0.6953` → **LightGBM (umbral S08 = 0.308)**: **ROC-AUC = `0.9382`**, **PR-AUC = `0.7539`**, **F1 = `0.6771`**, **Brier = `0.0538`**. |
| **Clustering (6 Familias / 12 Métodos)** | PCA sobre 3.15M filas solo como apoyo (`CP1–CP2 = 58.2%`, `CP1–CP4 = 89.0%`). 12 algoritmos evaluados en benchmark común (`N = 3,000`) con matriz ARI 12×12. Campeón **K-Means (`k = 4`)** en muestra estratificada `N = 12,000` (Silueta `0.2663`, **ARI semillas = `0.9974`**, **ARI split-half = `0.9646`**) con 4 perfiles económicos en unidades originales. |

---

## 3. Estructura del paquete final (`Trabajo_Final_Grupo_9.zip`) y del repositorio

```text
Trabajo_Final_Grupo_9/
├── 01_Notebook/
│   └── Trabajo_Final_Grupo_9.ipynb        # Notebook completo ejecutado en Colab (19/19 celdas sin error)
├── 02_Informe/
│   ├── Informe_Grupo_9.docx               # Informe académico editable (10 secciones + Anexo 80 variables)
│   └── Informe_Grupo_9.pdf                # Versión PDF fiel para lectura y entrega
├── 03_HTML/
│   └── Dashboard_Grupo_9.html             # Dashboard interactivo generado por Python (Mapa mundial + Perú)
├── 04_Datos/
│   ├── Base_Inicial_Grupo_9.parquet       # Base cruda íntegra (3,155,169 × 58 | 148.19 MB ZSTD)
│   └── Base_Final_Grupo_9.parquet         # Base limpia + ingeniería (3,155,169 × 69 | 189.23 MB ZSTD)
└── 05_Documentacion/
    ├── README_Grupo_9.md                  # Esta guía de ejecución, trazabilidad y estructura
    ├── requirements.txt                   # Dependencias reproducibles verificadas en Colab
    ├── diccionario_variables.csv          # Diccionario completo de 80 variables (58 originales + 22 derivadas)
    └── resumen_metricas_colab.json        # Métricas y tablas exactas exportadas desde Google Colab
```

> **Nota sobre archivos `.parquet` en GitHub:** Debido al límite estricto de **100 MB por archivo** de GitHub, `Base_Inicial_Grupo_9.parquet` (148.19 MB) y `Base_Final_Grupo_9.parquet` (189.23 MB) se entregan dentro de `04_Datos/` en **`Trabajo_Final_Grupo_9.zip`** y se regeneran de forma 100% determinística desde `datos_crudos/` al ejecutar `Trabajo_Final_Grupo_9.ipynb` en Google Colab. En el repositorio GitHub también se conservan intactos `datos_crudos/` y las 10 figuras a 300 DPI en `06_Figuras/`.

---

## 4. Instrucciones breves de ejecución reproducible en Google Colab

1. Abrir el notebook directamente en Google Colab mediante el enlace:  
   `https://colab.research.google.com/github/jshez612/sunat-importaciones-grupo9/blob/main/01_Notebook/Trabajo_Final_Grupo_9.ipynb`
2. Seleccionar **Entorno de ejecución → Ejecutar todas** (`Ctrl + F9`). No se requieren credenciales, tokens ni archivos locales: el notebook instala las dependencias ligeras (`dbfread`, `xlrd`, `pycountry`, `kmedoids`), descarga los 10 `.zip` semanales y los 8 catálogos desde GitHub, fija `SEED = 2026` y genera todos los entregables en `/content/entregables_colab/`.
3. Tiempo aproximado de ejecución completa en CPU estándar de Colab: **10–14 minutos**.
4. Para visualizar el producto interactivo de sustentación, abrir localmente `03_HTML/Dashboard_Grupo_9.html` en cualquier navegador moderno (Chrome, Edge, Firefox) con conexión a internet para cargar Plotly.js desde CDN.

---

## 5. Dependencias principales (verificadas en Google Colab)

Consulte `requirements.txt` para el listado completo (`python==3.13.18`, `numpy>=2.0`, `pandas>=2.2`, `pyarrow>=17.0`, `scikit-learn>=1.6`, `lightgbm>=4.5`, `scipy>=1.14`, `plotly>=5.24`, `matplotlib>=3.9`, `seaborn>=0.13`, `requests`, `dbfread`, `xlrd`, `pycountry`, `kmedoids`).
