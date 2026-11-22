# Datos Crudos SUNAT — Importación Definitiva (Formato A)

**Universidad de Lima — Facultad de Economía**  
**Asignatura:** Ciencia de Datos para Economistas (2026-2) | **Grupo 9**

---

## 1. Documentación de la fuente

- **Fuente oficial:** SUNAT — Portal de Operatividad Aduanera (Bases de Regímenes Definitivos y Catálogo de Códigos).
- **URLs oficiales de origen:**
  - Bases semanales (`ma`): `http://www.aduanet.gob.pe/aduanas/informae/`
  - Catálogos de códigos: `http://www.aduanet.gob.pe/aduanas/sgdbf/`
- **Período cubierto:** 10 semanas consecutivas (del 07/09/2026 al 15/11/2026).
- **Cobertura:** Nacional (todas las intendencias de aduana del Perú).
- **Unidad de análisis:** Serie declarada dentro de cada Declaración Aduanera de Mercancías (DUI/DAM) bajo el régimen de Importación Definitiva (Ejemplar A).
- **Volumen total:** **3,155,169 registros exactos × 58 variables** (185.32 MB comprimidos en `.zip`).
- **Fecha de acceso y respaldo crudo:** 2026-11-22.
- **Licencia y restricciones de uso:** Datos públicos administrativos difundidos por SUNAT al amparo del Art. 1 de la **Ley N° 27806** (Ley de Transparencia y Acceso a la Información Pública). Respaldados con fines estrictamente académicos y sin ninguna modificación previa.

## 2. Propósito del repositorio (reproducibilidad)

SUNAT publica las bases semanales de regímenes definitivos y las retira automáticamente de su servidor después de dos meses. Este repositorio público aloja los archivos `.zip` y `.xls` **crudos e inalterados** tal como fueron descargados desde `aduanet.gob.pe`, garantizando la carga remota y reproducibilidad íntegra en Google Colab.

## 3. Estructura del repositorio (`datos_crudos/`)

| Tipo | Archivos incluidos | Descripción |
| :--- | :--- | :--- |
| **Bases semanales (10)** | `ma07130926.zip` hasta `ma09151126.zip` | Archivos `.dbf` comprimidos de Importación Definitiva (Formato A). |
| **Catálogos oficiales (8)** | `Paises.zip`, `Aduanas.zip`, `MedTransporte.zip`, `Puertos.zip`, `UnidMedida.zip`, `EstMercancia.zip`, `Documento.zip`, `Tablibe.zip` | Tablas oficiales de SUNAT para decodificar códigos aduaneros en Python. |
| **Documentación y control (2)** | `estructura_bases.xls`, `totales.xls` | Diccionario oficial de campos y totales de control publicados por SUNAT. |

## 4. Carga remota reproducible en Google Colab

- **Dependencias mínimas:** `requests`, `dbfread`, `pandas`, `pyarrow`
- **URL base de acceso directo (sin credenciales):**

```python
BASE_URL = "https://raw.githubusercontent.com/jshez612/sunat-importaciones-grupo9/main/datos_crudos"
# Ejemplo: f"{BASE_URL}/ma09151126.zip" o f"{BASE_URL}/Paises.zip"
```
