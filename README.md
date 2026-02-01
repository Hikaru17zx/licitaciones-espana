# 🇪🇸 Datos Abiertos de Contratación Pública - España

Dataset completo de contratación pública española: nacional (PLACSP) + datos autonómicos (Catalunya, Valencia).

## 📊 Resumen de Datos

| Fuente | Registros | Período | Tamaño |
|--------|-----------|---------|--------|
| Nacional (PLACSP) | 8.7M | 2012-2026 | 780 MB |
| Catalunya | 20.6M | 2014-2025 | ~180 MB |
| Valencia | 8.5M | 2000-2026 | 156 MB |
| **TOTAL** | **37.8M** | **2000-2026** | **~1.1 GB** |

---

## 🏛️ Nacional - PLACSP

Licitaciones de la [Plataforma de Contratación del Sector Público](https://contrataciondelsectorpublico.gob.es/).

| Conjunto | Registros | Período |
|----------|-----------|---------|
| Licitaciones | 3.6M | 2012-actualidad |
| Agregación CCAA | 1.7M | 2016-actualidad |
| Contratos menores | 3.3M | 2018-actualidad |
| Encargos medios propios | 14.7K | 2021-actualidad |
| Consultas preliminares | 3.7K | 2022-actualidad |

### Archivos

```
nacional/
├── licitaciones_espana.parquet              # Última versión (641 MB)
└── licitaciones_completo_2012_2026.parquet  # Historial completo (780 MB)
```

### Campos principales (48 columnas)

| Categoría | Campos |
|-----------|--------|
| Identificación | id, expediente, objeto, url |
| Órgano | organo_contratante, nif_organo, dir3_organo, ciudad_organo |
| Tipo | tipo_contrato, subtipo_code, procedimiento, estado |
| Importes | importe_sin_iva, importe_con_iva, importe_adjudicacion |
| Adjudicación | adjudicatario, nif_adjudicatario, num_ofertas, es_pyme |
| Clasificación | cpv_principal, cpvs, ubicacion, nuts |
| Fechas | fecha_publicacion, fecha_limite, fecha_adjudicacion |

---

## 🏴 Catalunya

Datos del portal [Transparència Catalunya](https://analisi.transparenciacatalunya.cat) (Socrata API) y del portal [Contractació Pública de Catalunya](https://contractaciopublica.cat) (REST API).

| Categoría | Registros | Período | Fuente |
|-----------|-----------|---------|--------|
| **Contratación pública** | **4.3M** | 2014-2025 | |
| ↳ Contratos regulares | 1.3M | 2014-2025 | Transparència Catalunya |
| ↳ Contratos menores 🆕 | 866K (3M brutos) | 2014-2025 | contractaciopublica.cat |
| Subvenciones RAISC | 9.6M | 2014-2025 | Transparència Catalunya |
| Presupuestos | 3.1M | 2014-2025 | Transparència Catalunya |
| Convenios | 62K | 2014-2025 | Transparència Catalunya |
| RRHH | 3.4M | 2014-2025 | Transparència Catalunya |
| Patrimonio | 112K | 2020-2025 | Transparència Catalunya |

### Archivos

```
catalunya/
├── contratacion/
│   ├── contractacio_publica.parquet         # 1.3M contratos regulares
│   └── contractacio_menors.parquet          # 866K contratos (3M brutos con histórico)
├── subvenciones/
│   └── raisc_subvenciones.parquet           # 9.6M registros
├── pressupostos/
│   └── pressupostos_*.parquet
├── convenis/
│   └── convenis_*.parquet
├── rrhh/
│   └── rrhh_*.parquet
└── patrimoni/
    └── patrimoni_*.parquet
```

### 🆕 Contratos menores Catalunya (contractaciopublica.cat)

Dataset de **866.024 registros únicos** (3.025.588 brutos) de contratación pública del sector público catalán, extraído del portal [Contractació Pública de Catalunya](https://contractaciopublica.cat).

**Metodología de extracción:**

La API tiene un límite de 10.000 registros por consulta. Para obtener el dataset completo se usa una estrategia de segmentación multidimensional recursiva:

1. Segmentar por `faseVigent` (22 fases del proceso de contratación)
2. Si un segmento supera 10K → sub-segmentar por `ambit` (5 sectores)
3. Si aún supera 10K → sub-segmentar por `tipusContracte` (8 tipos)
4. Si aún supera 10K → sub-segmentar por `procedimentAdjudicacio` (12 procedimientos)
5. Si aún supera 10K → sub-segmentar por `organ` (órgano contratante individual)
6. Fallback final: paginación ASC+DESC para capturar hasta 20K registros por segmento

Incluye todas las fases del proceso: publicación previa, licitación, adjudicación, formalización, ejecución, y contratos agregados (menores). El dataset incluye JSON completo de la API con todos los campos anidados.

**Estadísticas de extracción:**

| Métrica | Valor |
|---------|-------|
| Registros brutos | 3.025.588 |
| Registros únicos | 866.024 |
| Segmentos procesados | 22 |
| Sub-segmentaciones | 181 |
| Peticiones API | ~73.000 |
| Tiempo | ~2.5 horas |

---

## 🍊 Valencia

Datos del portal [Dades Obertes GVA](https://dadesobertes.gva.es) (CKAN API).

| Categoría | Archivos | Registros | Contenido |
|-----------|----------|-----------|-----------|
| Contratación | 13 | 246K | REGCON 2014-2025 + DANA |
| Subvenciones | 52 | 2.2M | Ayudas 2022-2025 + DANA |
| Presupuestos | 4 | 346K | Ejecución 2024-2025 |
| Convenios | 5 | 8K | 2018-2022 |
| Lobbies (REGIA) | 7 | 11K | Único en España 🌟 |
| Empleo | 42 | 888K | ERE/ERTE 2000-2025, DANA |
| Paro | 283 | 2.6M | Estadísticas LABORA |
| Siniestralidad | 10 | 570K | Accidentes 2015-2024 |
| Patrimonio | 3 | 9K | Inmuebles GVA |
| Entidades | 2 | 94K | Locales + Asociaciones |
| Territorio | 1 | 4K | Centros docentes |
| Turismo | 16 | 383K | Hoteles, VUT, campings... |
| Sanidad | 8 | 189K | Mapa sanitario |
| Transporte | 7 | 993K | Bus interurbano GTFS |

### Archivos

```
valencia/
├── contratacion/          # 13 archivos, 42 MB
├── subvenciones/          # 52 archivos, 26 MB
├── presupuestos/          # 4 archivos, 7 MB
├── convenios/             # 5 archivos, 2 MB
├── lobbies/               # 7 archivos, 0.4 MB  🌟 REGIA
├── empleo/                # 42 archivos, 13 MB
├── paro/                  # 283 archivos, 17 MB
├── siniestralidad/        # 10 archivos, 0.6 MB
├── patrimonio/            # 3 archivos, 0.4 MB
├── entidades/             # 2 archivos, 4 MB
├── territorio/            # 1 archivo, 0.4 MB
├── turismo/               # 16 archivos, 17 MB
├── sanidad/               # 8 archivos, 6 MB
└── transporte/            # 7 archivos, 21 MB
```

### 🌟 Datos únicos de Valencia

- **REGIA**: Registro de lobbies único en España (grupos de interés, actividades de influencia)
- **DANA**: Datasets específicos de la catástrofe (contratos, subvenciones, ERTE)
- **ERE/ERTE histórico**: 25 años de datos (2000-2025)
- **Siniestralidad laboral**: 10 años de accidentes de trabajo

---

## 📥 Uso

```python
import pandas as pd

# Nacional - PLACSP
df_nacional = pd.read_parquet('nacional/licitaciones_espana.parquet')

# Catalunya - Contratos menores
df_cat_menors = pd.read_parquet('catalunya/contratacion/contractacio_menors.parquet')

# Catalunya - Subvenciones
df_cat_subv = pd.read_parquet('catalunya/subvenciones/raisc_subvenciones.parquet')

# Valencia - Contratación
df_val = pd.read_parquet('valencia/contratacion/')

# Valencia - Lobbies REGIA
df_lobbies = pd.read_parquet('valencia/lobbies/')

# Cargar múltiples archivos de una carpeta
import glob
files = glob.glob('valencia/subvenciones/*.parquet')
df_subv = pd.concat([pd.read_parquet(f) for f in files])
```

### Ejemplos de análisis

```python
# Top adjudicatarios nacional
df_nacional.groupby('adjudicatario')['importe_sin_iva'].sum().nlargest(10)

# Contratos menores Catalunya por órgano
df_cat_menors.groupby('organContractant')['pressupostAdjudicacio'].sum().nlargest(10)

# Evolución ERE/ERTE Valencia (2000-2025)
df_erte = pd.read_parquet('valencia/empleo/')
df_erte.groupby('año')['expedientes'].sum().plot()

# Lobbies por sector
df_regia = pd.read_parquet('valencia/lobbies/')
df_regia['sector'].value_counts()
```

---

## 🔧 Scripts de extracción

Todos los scripts están en la carpeta `scripts/`.

| Script | Fuente | Descripción |
|--------|--------|-------------|
| `licitaciones.py` | PLACSP | Extrae datos nacionales de ATOM/XML |
| `ccaa_catalunya.py` | Socrata | Descarga datos de Transparència Catalunya |
| `ccaa_catalunya_parquet.py` | - | Convierte CSV de Catalunya a Parquet |
| `ccaa_catalunya_contractacio_publica.py` | contractaciopublica.cat | Scraper completo con segmentación recursiva para sortear el límite de 10K registros de la API. Incluye guardado incremental por fase, reanudación con `--resume`, y reintentos con backoff exponencial para errores 502/503 |
| `ccaa_valencia.py` | CKAN | Descarga datos de Dades Obertes GVA |
| `ccaa_valencia_parquet.py` | - | Convierte CSV de Valencia a Parquet |

### Uso de los scripts

```bash
# Nacional
python scripts/licitaciones.py

# Catalunya - Transparència
python scripts/ccaa_catalunya.py

# Catalunya - Contractació Pública (scraper con segmentación)
python scripts/ccaa_catalunya_contractacio_publica.py --output contractacio_menors.parquet
python scripts/ccaa_catalunya_contractacio_publica.py --output contractacio_menors.parquet --resume  # Reanudar si se interrumpe

# Valencia
python scripts/ccaa_valencia.py
```

---

## 🔄 Actualización

| Fuente | Frecuencia |
|--------|------------|
| PLACSP | Mensual |
| Catalunya | Variable (depende del dataset) |
| Valencia | Diaria/Mensual (depende del dataset) |

---

## 📋 Requisitos

```bash
pip install pandas pyarrow requests aiohttp tqdm
```

---

## 📄 Licencia

Datos públicos del Gobierno de España y CCAA - [Licencia de Reutilización](https://datos.gob.es/es/aviso-legal)

---

## 🔗 Fuentes

| Portal | URL |
|--------|-----|
| PLACSP | https://contrataciondelsectorpublico.gob.es/ |
| Catalunya - Transparència | https://analisi.transparenciacatalunya.cat/ |
| Catalunya - Contractació Pública | https://contractaciopublica.cat/ |
| Valencia | https://dadesobertes.gva.es/ |
| BQuant Finance | https://bquantfinance.com |

---

## 📈 Próximas CCAA

- [ ] Euskadi
- [ ] Andalucía
- [ ] Madrid

---

⭐ Si te resulta útil, dale una estrella al repo

[@Gsnchez](https://twitter.com/Gsnchez) | [BQuant Finance](https://bquantfinance.com)
