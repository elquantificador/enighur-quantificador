# Ficha del artículo

Completa esta ficha al crear el repositorio y mantenla al día. Es el único
lugar con datos propios del artículo; el README es igual en todos los
repositorios.

## Identificación

- **Título:** ENIGHUR 2024-2025: ingresos y gastos de los hogares en Ecuador
- **Descripción del repositorio:** Repositorio para el articulo de El Quantificador: Ingresos y gastos de los ecuatorianos, doce años después
- **Autoría:** [COMPLETAR: nombres, igual que en el sitio]
- **Fecha de publicación:** [COMPLETAR: AAAA-MM-DD]
- **Artículo publicado:** [COMPLETAR: enlace a elquantificador.org]
- **Categoría:** [COMPLETAR: Economía, Política, Salud, Educación o Valores y
  comportamiento]
- **Edición:** [COMPLETAR: por ejemplo, Julio-Diciembre 2026]

## Estado

[COMPLETAR: en preparación, en revisión editorial o publicado. Anota qué falta
para cerrar el artículo.]

## Pregunta y alcance

[COMPLETAR: dos o tres oraciones con la pregunta, el periodo, la cobertura
geográfica y la unidad de análisis.]

## Requisitos

- R [COMPLETAR: versión, por ejemplo 4.5.x].
- Paquetes de R: [COMPLETAR: lista en orden alfabético].

## Fuentes de datos

| Fuente | Institución | Periodo | Acceso | Script |
|---|---|---|---|---|
| [COMPLETAR] | [COMPLETAR] | [COMPLETAR] | [COMPLETAR: descarga automática o manual] | `code/cleaning/00_download_fuente.R` |

Si alguna fuente no permite descarga automática, explica aquí cómo obtenerla y
dónde colocarla en `data/raw/`.

## Salidas

- `outputs/tables/[COMPLETAR].csv`: [COMPLETAR: qué contiene].
- `outputs/graphs/[COMPLETAR].png`: [COMPLETAR: qué muestra].
- `article/images/graficoN.png`: copias numeradas según el orden del texto.

## Cómo replicar en este repositorio

Este repositorio conserva sus instrucciones originales en la sección
"README anterior". Si todavía no tiene `MASTER.R`, sigue esas instrucciones.
Cuando se migre a la estructura estándar, reemplaza esta sección por los pasos
de `MASTER.R`.

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
