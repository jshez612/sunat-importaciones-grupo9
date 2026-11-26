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
- **Nota expresa de aprobación docente:** El docente Angel Agüero Correa validó en clase trabajar con microdatos de SUNAT bajo el requisito de superar **2,000,000 de registros**
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
