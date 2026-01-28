# 🇪🇸 Licitaciones y Contratación Pública de España

**El dataset más completo de contratación pública española en formato abierto.**

## ⚠️ Importante: ¿Por qué hay una carpeta "Catalunya"?

**No es una división política, es una ampliación de datos.**

| Carpeta | Fuente | Qué contiene |
|---------|--------|--------------|
| `nacional/` | PLACSP (Hacienda) | Licitaciones de **TODA España** (incluida Catalunya) |
| `catalunya/` | Portal Transparència | Datos **ADICIONALES** que solo publica la Generalitat |

La carpeta `catalunya/` **NO duplica** los datos nacionales. Contiene información que **no existe en PLACSP**:

- 📊 **9.6 millones de subvenciones** (registro RAISC)
- 💰 **Presupuestos detallados** de la Generalitat (2014-2025)
- 📝 **Convenios** de colaboración
- 🏛️ **Entidades** del sector público catalán
- 👔 **Retribuciones** de altos cargos y funcionarios
- 🗺️ **Datos territoriales** (municipios, comarcas)
- 📋 **Datos específicos de Barcelona** (contratos menores, contratistas)

**Si solo te interesan las licitaciones → usa `nacional/`**  
**Si quieres subvenciones, presupuestos, convenios de Catalunya → usa `catalunya/`**

---

## 📊 Resumen de datos

| Fuente | Registros | Período | Cobertura |
|--------|-----------|---------|-----------|
| **PLACSP** (Nacional) | 8.7M | 2012-2026 | Todas las CCAA |
| **Catalunya** (adicional) | 17.6M | 2014-2025 | Solo Generalitat |
| **Total** | **26.3M** | | |

---

## 📂 Estructura del repositorio

```
licitaciones-espana/
│
├── nacional/                                      # 🇪🇸 TODA ESPAÑA
│   ├── licitaciones_espana.parquet                # 626 MB - Última versión
│   ├── licitaciones_completo_2012_2026.parquet    # 762 MB - Historial
│   └── licitaciones.py                            # Script extracción
│
├── catalunya/                                     # 🏛️ DATOS ADICIONALES
│   │
│   ├── contratacion/                              # Complementa PLACSP
│   │   ├── contratos_registro.parquet             # ⭐ 461 MB (3.4M reg)
│   │   ├── publicaciones_pscp.parquet             # 414 MB (1.6M reg)
│   │   ├── adjudicaciones_generalitat.parquet
│   │   ├── contratacion_programada.parquet
│   │   ├── contratos_covid.parquet
│   │   ├── fase_ejecucion.parquet
│   │   ├── resoluciones_tribunal.parquet
│   │   ├── contratistas_bcn.parquet               # Barcelona
│   │   ├── contratos_menores_bcn.parquet          # Barcelona
│   │   ├── modificaciones_bcn.parquet             # Barcelona
│   │   ├── perfil_contratante_bcn.parquet         # Barcelona
│   │   └── resumen_trimestral_bcn.parquet         # Barcelona
│   │
│   ├── subvenciones/                              # 🆕 NO EXISTE EN PLACSP
│   │   ├── raisc_concesiones.parquet              # ⭐ 119 MB (9.6M reg)
│   │   ├── raisc_convocatorias.parquet
│   │   └── convocatorias_subvenciones.parquet
│   │
│   ├── presupuestos/                              # 🆕 NO EXISTE EN PLACSP
│   │   ├── ejecucion_gastos.parquet               # 2014-2025
│   │   ├── ejecucion_ingresos.parquet
│   │   └── presupuestos_aprobados.parquet
│   │
│   ├── entidades/                                 # 🆕 NO EXISTE EN PLACSP
│   │   ├── ens_locals.parquet
│   │   ├── sector_publico_generalitat.parquet
│   │   ├── ajuntaments.parquet
│   │   ├── ajuntaments_lista.parquet
│   │   ├── codigos_departamentos.parquet
│   │   └── composicio_plens.parquet
│   │
│   ├── convenios/                                 # 🆕 NO EXISTE EN PLACSP
│   │   └── convenios.parquet
│   │
│   ├── rrhh/                                      # 🆕 NO EXISTE EN PLACSP
│   │   ├── altos_cargos.parquet
│   │   ├── retribuciones_funcionarios.parquet
│   │   ├── retribuciones_laboral.parquet
│   │   ├── taules_retributives.parquet
│   │   ├── convocatorias_personal.parquet
│   │   └── enunciats_examens.parquet
│   │
│   ├── territorio/
│   │   ├── municipis_catalunya.parquet
│   │   └── municipis_espanya.parquet
│   │
│   └── README.md
│
├── scripts/
│   ├── ccaa_cataluna.py                           # Descarga CSVs
│   └── ccaa_cataluna_parquet.py                   # Conversión Parquet
│
└── README.md
```

---

## 🔍 Detalle por fuente

### Nacional (PLACSP) - `nacional/`

Datos de la **Plataforma de Contratación del Sector Público** del Ministerio de Hacienda.

| Dataset | Registros | Descripción |
|---------|-----------|-------------|
| Licitaciones | 3.6M | Contratos de todas las CCAA |
| Agregación CCAA | 1.7M | Contratos agregados autonómicos |
| Contratos menores | 3.3M | Contratos < 15.000€ |
| Encargos medios propios | 14.7K | Encargos a entidades propias |
| Consultas preliminares | 3.7K | CPM |
| **Total** | **8.7M** | **2012-2026** |

**Campos principales:** expediente, objeto, órgano_contratante, tipo_contrato, procedimiento, importe_sin_iva, importe_adjudicacion, adjudicatario, nif_adjudicatario, cpv, nuts, fecha_publicacion, fecha_adjudicacion, estado...

### Catalunya - `catalunya/`

Datos del **Portal de Transparència de Catalunya** y **Open Data Barcelona**.

#### Contratación (complementa PLACSP)
| Dataset | Registros | Descripción |
|---------|-----------|-------------|
| contratos_registro | 3.4M | Registro público contratos |
| publicaciones_pscp | 1.6M | Ciclo completo licitación |
| adjudicaciones_generalitat | 200K | Adjudicaciones |
| fase_ejecucion | 80K | Contratos en ejecución |
| contratacion_programada | 15K | Contratación planificada |
| contratos_covid | 5K | Emergencia COVID-19 |
| resoluciones_tribunal | 2K | Tribunal de Contratos |
| *_bcn | 180K | Datos Ayuntamiento Barcelona |

#### Subvenciones (EXCLUSIVO)
| Dataset | Registros | Descripción |
|---------|-----------|-------------|
| raisc_concesiones | **9.6M** | Todas las subvenciones Catalunya |
| raisc_convocatorias | 50K | Convocatorias |
| convocatorias_subvenciones | 30K | Convocatorias activas |

#### Presupuestos (EXCLUSIVO)
| Dataset | Registros | Descripción |
|---------|-----------|-------------|
| ejecucion_gastos | 2M | Ejecución presupuestaria gastos |
| ejecucion_ingresos | 300K | Ejecución presupuestaria ingresos |
| presupuestos_aprobados | 200K | Presupuestos aprobados |

#### Entidades (EXCLUSIVO)
| Dataset | Descripción |
|---------|-------------|
| ens_locals | Todos los entes locales de Catalunya |
| sector_publico_generalitat | Entidades sector público |
| ajuntaments | Datos de ayuntamientos |
| composicio_plens | Composición plenos municipales |

#### Otros (EXCLUSIVO)
| Dataset | Descripción |
|---------|-------------|
| convenios | Convenios de colaboración |
| altos_cargos | Retribuciones altos cargos |
| retribuciones_* | Tablas salariales funcionarios |

---

## 📥 Uso rápido

```python
import pandas as pd

# === LICITACIONES DE TODA ESPAÑA ===
df = pd.read_parquet('nacional/licitaciones_espana.parquet')

# Filtrar por comunidad autónoma
df_cat = df[df['nuts'].str.startswith('ES51', na=False)]  # Catalunya
df_mad = df[df['nuts'].str.startswith('ES30', na=False)]  # Madrid

# === SUBVENCIONES CATALUNYA (no existe a nivel nacional) ===
df_subv = pd.read_parquet('catalunya/subvenciones/raisc_concesiones.parquet')

# === PRESUPUESTOS CATALUNYA ===
df_pres = pd.read_parquet('catalunya/presupuestos/ejecucion_gastos.parquet')

# === CARGAR SOLO COLUMNAS ESPECÍFICAS (más rápido) ===
df = pd.read_parquet('nacional/licitaciones_espana.parquet',
                     columns=['expediente', 'objeto', 'importe_sin_iva', 'adjudicatario'])
```

---

## 🔧 Fuentes y metodología

### Nacional (PLACSP)

**Fuente:** [Datos Abiertos - Ministerio de Hacienda](https://www.hacienda.gob.es/es-ES/GobiernoAbierto/Datos%20Abiertos/Paginas/licitaciones_plataforma_contratacion.aspx)

**Proceso:**
1. Descarga de 78 archivos ZIP (~15 GB)
2. Parsing de XML en formato CODICE (estándar europeo)
3. Conversión a Parquet

### Catalunya

**Fuentes:**
- [Portal Transparència Catalunya](https://analisi.transparenciacatalunya.cat/) (API Socrata)
- [Open Data Barcelona](https://opendata-ajuntament.barcelona.cat/) (API CKAN)

**Proceso:**
1. Descarga vía API pública: `https://analisi.transparenciacatalunya.cat/api/views/{ID}/rows.csv`
2. Consolidación de archivos anuales (Barcelona)
3. Conversión a Parquet (90% reducción de tamaño)

**No hay scraping.** Todo se obtiene de APIs públicas oficiales. Los scripts están disponibles en `scripts/` para replicar el proceso.

---

## 🔄 Actualización

### Nacional
```bash
cd nacional
python licitaciones.py  # Descarga y procesa (~2h)
```

### Catalunya
```bash
cd scripts
python ccaa_cataluna.py           # Descarga CSVs (~12 GB, ~30min)
python ccaa_cataluna_parquet.py   # Convierte a Parquet (~5min)
```

---

## 📋 Requisitos

```bash
pip install pandas pyarrow requests
```

---

## ❓ FAQ

**¿Los datos de Catalunya están duplicados con los nacionales?**  
En contratación hay solapamiento parcial. Pero subvenciones, presupuestos, convenios, RRHH y entidades son EXCLUSIVOS de Catalunya.

**¿Por qué Catalunya y no otras CCAA?**  
Catalunya tiene el portal de datos abiertos más completo de España. Otras CCAA están en desarrollo para futuras versiones.

**¿Con qué frecuencia se actualizan?**  
PLACSP: mensual. Catalunya: variable según dataset.

**¿Puedo usar estos datos comercialmente?**  
Sí. Son datos públicos bajo licencia de reutilización.

---

## 📄 Licencia

Datos públicos del Gobierno de España y Generalitat de Catalunya.
- [Licencia de Reutilización - datos.gob.es](https://datos.gob.es/es/aviso-legal)

---

## 🔗 Enlaces

- [Plataforma de Contratación del Sector Público](https://contrataciondelsectorpublico.gob.es/)
- [Portal Transparència Catalunya](https://analisi.transparenciacatalunya.cat/)
- [Open Data Barcelona](https://opendata-ajuntament.barcelona.cat/)
- [BQuant Finance](https://bquantfinance.com)
- [Newsletter BQuant Fund Lab](https://bquantfinance.substack.com)

---

⭐ **Si te resulta útil, dale una estrella al repo**

🐛 **¿Problemas o sugerencias?** Abre un issue
