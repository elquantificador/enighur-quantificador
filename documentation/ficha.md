# Ficha del artículo

Completa esta ficha al crear el repositorio y mantenla al día. Es el único
lugar con datos propios del artículo; el README es igual en todos los
repositorios.

## Identificación

- **Título:** Ingresos y gastos de los ecuatorianos, doce años después
- **Descripción del repositorio:** Repositorio para el articulo de El Quantificador: Ingresos y gastos de los ecuatorianos, doce años después
- **Autoría:** Daniel Sánchez Pazmiño
- **Fecha de publicación:** 2026-06-05
- **Artículo publicado:** https://elquantificador.org/post/economia/2026-06-01-enighur-ingresos-gastos-ecuador/
- **Categoría:** Economía
- **Edición:** Enero-Junio 2026

## Estado

Publicado el 2026-06-05; el sitio registra una actualización el 2026-10-01. El
título publicado es "Ingresos y gastos de los ecuatorianos, doce años después".
El texto del artículo está en `final_version/index.Rmd`; el `index.Rmd` de la
raíz es el front matter de otro artículo (embarazo infantil) y no corresponde a
este; lleva un comentario HTML que lo aclara y ningún script lo usa. El README
anterior menciona `article/index.Rmd`, carpeta que no existe.

## Pregunta y alcance

El artículo pregunta cuánto ganan los hogares de Ecuador y en qué gastan su
dinero. Usa la ENIGHUR 2024-2025 del INEC (recolección de diciembre de 2024 a
noviembre de 2025), con cobertura nacional, y compara puntualmente con la
ENIGHUR 2011-2012. La unidad de análisis es el hogar, desagregado por quintil
de ingreso, provincia y región.

## Requisitos

- R: no documentada en el repositorio.
- Paquetes de R: cowplot, dplyr, ggplot2, haven, htmltools, htmlwidgets,
  janitor, knitr, openxlsx, plotly, readr, readxl, scales, tidyverse. Los
  scripts de `scripts/` los verifican con `ensure_packages()` y no los
  instalan. Para renderizar `report/report.qmd` hace falta además Quarto.

## Fuentes de datos

| Fuente | Institución | Periodo | Acceso | Script |
|---|---|---|---|---|
| ENIGHUR 2024-2025: bases de trabajo (`Bases_trabajo.RData`), bases primarias (`Bases_primarias.RData`) y tabulados (`Tabulados_ENIGHUR_2024-2025.xlsx`) | INEC; https://www.ecuadorencifras.gob.ec/encuesta-nacional-de-ingresos-y-gastos-de-los-hogares-urbanos-y-rurales/ | 2024-12 a 2025-11 | manual | `scripts/01_ingresos_hogares.R`, `02`, `04`, `05`, `06` |
| ENIGHUR 2011-2012: `ENIGHUR11_INGRESOS_H.sav`, `ENIGHUR11_GASTOS_HMO.sav` y `TABULADOS ENIGHUR 2011-2012.xlsx` | INEC; [catálogo ANDA, ENIGHUR 2011-2012](https://anda.inec.gob.ec/anda5/index.php/catalog/291) | 2011-2012 | manual | `scripts/01_ingresos_hogares.R`, `scripts/05_mediana_gasto_componentes_enighur_2025.R` |
| ENEMDU 2025 anual, personas (`BDDenemdu_personas_2025_anual.sav`) | INEC; [ENEMDU Anual](https://www.ecuadorencifras.gob.ec/enemdu-anual/) (archivo `1_BDD_ENEMDU_2025_SPSS.zip`: https://www.ecuadorencifras.gob.ec/documentos/web-inec/EMPLEO/2025/anual/1_BDD_ENEMDU_2025_SPSS.zip) | 2025 | manual | `scripts/03_ingresos_hogares_enemdu_2025.R`, `scripts/04_resumen_ingresos_hogares_2025.R` |

Ningún script descarga datos. Los archivos pesados no están en el
repositorio; hay que colocarlos así, desde la raíz del proyecto:

- `data/enighur/2025/Enighur_Bases_de_datos_R/Bases de trabajo/Bases_trabajo_R/Bases_trabajo.RData`
- `data/enighur/2025/Bases_primarias/Bases_primarias.RData`
- `data/enighur/2025/Tabulados_ENIGHUR_2024-2025.xlsx`
- `data/enighur/2012/required/ENIGHUR11_INGRESOS_H.sav` y
  `ENIGHUR11_GASTOS_HMO.sav` (los scripts aceptan también las rutas originales
  largas de 2012)
- `data/enighur/2012/TABULADOS ENIGHUR 2011-2012.xlsx`
- `data/enemdu/1_BDD_ENEMDU_2025_SPSS/BDDenemdu_personas_2025_anual.sav`

Sí está versionado `data/intermediate/mapeo_categorias_gasto.csv`, el mapeo
propio de las categorías de gasto del INEC (ENIGHUR) a las categorías del
artículo.

## Salidas

- `final_version/images/grafico1` a `grafico6` (`.png` y `.svg`): los seis
  gráficos del artículo, en el orden del texto. `grafico1` es idéntico al
  archivo `output/figures/sankey_ingresos_gastos_2025.png` y `grafico5` a
  `output/figures/gasto_total_mediano_provincia_2025.png`; los demás difieren
  de los archivos de `output/figures/`.
- `final_version/grafico1/output/grafico1.png` y `.svg`: gráfico 1 reproducible
  de forma independiente (descomposición del ingreso monetario mensual del
  hogar promedio), junto con `data/raw/*.rds` y
  `data/processed/grafico1_processed.rds`.
- `output/figures/*.png` y `*.svg`: figuras de trabajo (distribución del
  ingreso, gasto por quintil, gasolina, gasto mediano por provincia,
  alimentos por región, Sankey) y `sankey_ingresos_gastos_interactivo.html`.
- `output/tables/enemdu_2025_ingreso_hogar_summary.xlsx`: resumen del ingreso
  del hogar desde ENEMDU 2025.
- `output/tables/enighur_ingresos_y_gastos_resumen.xlsx`: resumen de ingresos
  y gastos de ENIGHUR.
- `output/tables/productos_alimentos_mas_presentes_region_2025.csv`: productos
  de alimentos más presentes en las compras de los hogares, por región.
- `report/report.qmd`: versión Quarto anterior del artículo, que consume las
  figuras de `output/figures/`.

## Cómo replicar en este repositorio

El repositorio no tiene `MASTER.R`. Los pasos que se toman del README anterior
y de los scripts son:

1. Coloca los archivos de datos en las rutas de la sección Fuentes de datos y
   abre `enighur-quantificador.Rproj` (los scripts se ejecutan desde la raíz).
2. Instala los paquetes de la sección Requisitos.
3. Ejecuta `Rscript scripts/check.R`, la verificación rápida de código y rutas.
4. Ejecuta los scripts en orden numérico según lo que necesites:
   `01_ingresos_hogares.R` (figuras de ingreso, gasto, gasolina, provincias y
   regiones), `02_sankey_ingresos_gastos.R` (Sankey), `03` y `04` (tablas de
   ingreso con ENEMDU 2025 y ENIGHUR 2025), `05` (medianas de gasto por
   componente) y `06_productos_alimentos_region.R` (alimentos por región).
5. Para el gráfico 1 por separado, ejecuta
   `Rscript final_version/grafico1/scripts/01_prepare_data.R` y luego
   `Rscript final_version/grafico1/scripts/02_render_grafico1.R`.
6. El texto publicado está en `final_version/index.Rmd`, que lee
   `final_version/images/` (no uses el `index.Rmd` de la raíz, que es de otro
   artículo). La versión `report/report.qmd` se renderiza con
   `quarto render report/report.qmd`.

Observación: el README anterior menciona
`output/tables/resumen_ingresos_hogares_2025.xlsx` y `scripts/04` lo escribe,
pero ese archivo no está versionado. `scripts/05` escribe solo
`output/tables/enighur_ingresos_y_gastos_resumen.xlsx`.

## README anterior

Texto del README de este repositorio antes de adoptar el README estándar,
conservado sin cambios de contenido para no perder información.

### ENIGHUR 2024-2025: ingresos y gastos de los hogares en Ecuador

Análisis en R de la ENIGHUR 2024-2025, con comparaciones puntuales frente a la ENIGHUR 2011-2012 y una salida editorial en Quarto para *El Quantificador*.

#### Qué hace este repo

Este proyecto construye gráficos, tablas y un artículo corto sobre ingresos y gastos de los hogares ecuatorianos. El flujo principal:

1. Carga bases locales de ENIGHUR 2025 en `.RData`.
2. Reconstruye y compara métricas clave de 2012 y 2025.
3. Exporta figuras y tablas a `output/`.
4. Inserta esos resultados en `report/report.qmd`.

#### Requisitos

- R con los paquetes usados por cada script ya instalados.
- Quarto para renderizar el reporte.
- Archivos locales de datos bajo `data/`.

Los scripts no instalan dependencias automáticamente. Si falta un paquete, el proyecto falla rápido y debes instalarlo manualmente.

#### Estructura

- `scripts/01_ingresos_hogares.R`: script principal para generar varias figuras de ingresos, gasto total, gasolina, provincias y regiones.
- `scripts/02_sankey_ingresos_gastos.R`: genera el Sankey interactivo y sus salidas estáticas.
- `scripts/03_ingresos_hogares_enemdu_2025.R`: resume ingresos del hogar desde ENEMDU 2025.
- `scripts/04_resumen_ingresos_hogares_2025.R`: arma tablas resumen de ingresos de hogares ENIGHUR 2025.
- `scripts/05_mediana_gasto_componentes_enighur_2025.R`: calcula medianas y resúmenes de gasto por componentes.
- `scripts/06_productos_alimentos_region.R`: produce gráfico y tabla de productos alimenticios por región.
- `scripts/utils.R`: temas y helpers compartidos para exportar figuras.
- `scripts/packages.R`: validación y carga de paquetes.
- `scripts/check.R`: smoke check barato para validar código y rutas críticas sin correr todo el análisis.
- `report/report.qmd`: artículo Quarto que consume figuras desde `output/figures/`.
- `output/figures/`: figuras generadas.
- `output/tables/`: tablas generadas.

#### Datos esperados

El repo asume insumos locales grandes que no conviene versionar completos.

Rutas importantes que el proyecto busca:

- `data/enighur/2025/Enighur_Bases_de_datos_R/Bases de trabajo/Bases_trabajo_R/Bases_trabajo.RData`
- `data/enighur/2012/required/ENIGHUR11_INGRESOS_H.sav`
- `data/enighur/2012/required/ENIGHUR11_GASTOS_HMO.sav`

También acepta las rutas originales largas de 2012 si esos archivos no están copiados en `required/`.

#### Flujo recomendado

Primero corre el smoke check:

```powershell
Rscript scripts/check.R
```

Si pasa, ejecuta el análisis o los componentes que necesites:

```powershell
Rscript scripts/01_ingresos_hogares.R
Rscript scripts/02_sankey_ingresos_gastos.R
Rscript scripts/06_productos_alimentos_region.R
```

Para renderizar el artículo:

```powershell
quarto render report/report.qmd
```

#### Salidas principales

El proyecto escribe resultados en `output/`, por ejemplo:

- `output/figures/distribucion_ingreso_2025.png`
- `output/figures/gasto_total_por_quintil_ingreso_2025.png`
- `output/figures/gasolina_promedio_por_quintil_2025.png`
- `output/figures/gasto_total_mediano_provincia_2025.png`
- `output/figures/productos_alimentos_mas_presentes_region_2025.png`
- `output/figures/sankey_ingresos_gastos_interactivo.html`
- `output/tables/resumen_ingresos_hogares_2025.xlsx`
- `output/tables/enemdu_2025_ingreso_hogar_summary.xlsx`
- `output/tables/enighur_ingresos_y_gastos_resumen.xlsx`
- `output/tables/productos_alimentos_mas_presentes_region_2025.csv`

`report/report.qmd` espera encontrar varias de esas figuras antes de renderizar correctamente.

#### Notas operativas

- Ejecuta los scripts desde la raíz del proyecto.
- `scripts/01_ingresos_hogares.R` desactiva el dispositivo gráfico por defecto con `pdf(NULL)` para evitar generar `Rplots.pdf` durante una corrida normal.
- Si falta una base de datos o una figura requerida, los scripts y el reporte muestran errores explícitos con las rutas revisadas.
