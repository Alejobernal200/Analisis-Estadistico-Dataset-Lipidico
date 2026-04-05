# LIPID MAPS® Partial Spectra DB — Statistical Dashboard

> Interactive lipidomics data analysis dashboard built with vanilla HTML, CSS and Plotly.js — no frameworks, no dependencies, no build step.

![Dashboard Preview](https://img.shields.io/badge/status-active-brightgreen) ![Language](https://img.shields.io/badge/language-HTML%20%2F%20CSS%20%2F%20JS-blue) ![Plotly](https://img.shields.io/badge/Plotly.js-v3.4.0-informational) ![License](https://img.shields.io/badge/license-MIT-lightgrey) ![i18n](https://img.shields.io/badge/i18n-ES%20%2F%20EN-orange)

---

## Descripción / Description

Este artefacto es un **dashboard de análisis estadístico interactivo** desarrollado sobre la base de datos [LIPID MAPS® Partial Spectra DB](https://www.lipidmaps.org/databases/partial_db/overview), una colección especializada de espectros de masas de lípidos poco representados en la investigación lipidómica convencional.

El dashboard permite explorar visualmente **458 registros LC-MS/MS** distribuidos en 6 clases lipídicas, con filtros interactivos, visualizaciones múltiples y soporte bilingüe (español / inglés).

> **This artifact is an interactive statistical analysis dashboard** built on the LIPID MAPS® Partial Spectra DB — a curated collection of partial MS/MS spectra for underrepresented lipids, including epilipids and species from less-studied organisms.

---

## Características principales / Key Features

| Característica | Descripción |
|---|---|
| **Treemap** | Distribución proporcional de registros por clase lipídica |
| **Histograma + KDE** | Densidad espectral m/z con estimador de kernel (Freedman-Diaconis) |
| **Heatmap** | Cobertura de modos de ionización (aductos) por clase |
| **Gráfico de burbuja** | m/z promedio vs. especies moleculares únicas |
| **Barras horizontales** | Top 8 subclases por frecuencia de registros |
| **KPI Cards** | Métricas clave: total de registros, clases, rango m/z, especie más frecuente |
| **Filtros interactivos** | Filtrado dinámico por clase lipídica — actualiza todos los gráficos simultáneamente |
| **Panel de Insights** | Narrativa analítica estructurada en 3 actos (Panorama / Patrones / Recomendación) |
| **Bilingüe ES/EN** | Toggle de idioma completo: UI, gráficos, ejes, leyendas y panel narrativo |
| **Exportar CSV** | Descarga del resumen estadístico en formato CSV (con BOM UTF-8) |

---

## Estructura del proyecto

```
lipid-maps-dashboard/
│
├── index.html          # Archivo único — todo el dashboard está aquí
└── README.md           # Este archivo
```

El proyecto es intencionalmente **monolítico**: un solo archivo HTML autocontenido que incluye:
- Plotly.js v3.4.0 (embebido / minificado)
- Estilos CSS (design system con variables CSS)
- Datos del dataset (objeto `DATA` en JavaScript)
- Lógica de gráficos, filtros, traducción y exportación

---

## Dataset

**Fuente:** [LIPID MAPS® Partial Spectra Database](https://www.lipidmaps.org/databases/partial_db/overview)

| Clase | Código | N registros | % del total |
|---|---|---|---|
| Sacarolípidos / Saccharolipids | SL01 | 234 | 51.1% |
| Glicerolípidos / Glycerolipids | GL03 + GL05 | 84 | 18.3% |
| Esfingolípidos / Sphingolipids | SP05 | 79 | 17.2% |
| Glicerofosfolípidos / Glycerophospholipids | GP01 | 34 | 7.4% |
| Lípidos esterólicos / Sterol lipids | ST01 | 27 | 5.9% |
| **TOTAL** | — | **458** | **100%** |

**Características del dataset:**
- Rango m/z: `507.38 – 2193.53 Da`
- Media m/z: `1239.22 Da` · Mediana: `1132.03 Da`
- Completitud: `100%` (sin valores faltantes)
- Estrategia de adquisición: `LC-MS/MS`
- Especies moleculares únicas: `407`

---

## Uso / Usage

No requiere instalación ni servidor. Simplemente abre el archivo en cualquier navegador moderno:

```bash
# Clonar el repositorio
git clone https://github.com/tu-usuario/lipid-maps-dashboard.git

# Abrir directamente en el navegador
open index.html
# o en Windows:
start index.html
```

>  Compatible con Chrome, Firefox, Safari y Edge modernos.
>  No requiere Node.js, npm, Python ni ningún servidor.

---

## Cómo funciona / How it works

### Arquitectura

```
index.html
├── <style>          → Design system (CSS variables, layout, componentes)
├── <script>         → Plotly.js v3.4.0 (embebido)
└── <body>
    ├── Header        → Logo, metadatos, botón de idioma
    ├── KPI Grid      → 4 tarjetas de métricas clave
    ├── Filter Bar    → Botones de clase + exportar CSV
    ├── Charts Grid   → 5 visualizaciones Plotly
    ├── Story Panel   → Narrativa analítica (3 actos)
    ├── About Panel   → Info de la base de datos + tabla
    └── <script>      → DATA, lógica de gráficos, filtros, i18n
```

### Flujo de datos

```
DATA (objeto JS)
    │
    ├── applyFilter(cls)        → Filtra y re-renderiza todos los gráficos
    │       ├── renderTreemap()
    │       ├── renderHistogram()  → KDE calculado en tiempo real (Freedman-Diaconis)
    │       ├── renderHeatmap()
    │       ├── renderBubble()
    │       └── renderBars()
    │
    ├── toggleLanguage()        → Alterna ES ↔ EN
    │       ├── applyTranslation()  → Actualiza DOM completo
    │       └── re-render todos los gráficos con nuevas etiquetas
    │
    └── exportCSV()             → Genera y descarga CSV del resumen
```

### Sistema de traducción (i18n)

El sistema de internacionalización está implementado 100% en JavaScript nativo, sin librerías externas. El objeto `translations` contiene todas las cadenas de texto en ambos idiomas:

```javascript
const translations = {
  es: { /* ~60 claves */ },
  en: { /* ~60 claves */ }
};

function toggleLanguage() {
  currentLang = currentLang === 'es' ? 'en' : 'es';
  applyTranslation();   // Actualiza el DOM
  // Re-renderiza gráficos con ejes y leyendas traducidas
}
```

Lo que se traduce al cambiar de idioma:
- Título de página y header
- Etiquetas y subtextos de KPI cards
- Títulos, badges y subtítulos de gráficos
- Ejes X/Y de todos los gráficos (Plotly)
- Leyendas y nombres de series
- Panel de insights (narrativa completa)
- Barra de filtros y botón de exportar
- Panel "Acerca de" — descripción, tags, tabla de categorías
- Nombre del archivo CSV exportado

### Cálculo del histograma (KDE)

El histograma no usa el binning automático de Plotly. Se implementa manualmente con la regla de **Freedman-Diaconis**:

```
bin_width = 2 × IQR × n^(-1/3)
```

El KDE (Kernel Density Estimator) usa un kernel gaussiano con ancho de banda según la regla de **Silverman**:

```
h = 1.06 × σ × n^(-0.2)
```

La curva KDE se escala en unidades de conteo (misma escala que el histograma) para superponerse correctamente.

---

## Design System

El dashboard usa un sistema de variables CSS inspirado en interfaces de análisis de datos científicos:

```css
:root {
  --bg:      #0D1B2A;   /* Fondo principal */
  --panel:   #132338;   /* Paneles y cards */
  --accent:  #00C8FF;   /* Azul cian — color primario */
  --accent2: #00FF9C;   /* Verde — secundario */
  --accent3: #FF6B6B;   /* Rojo — alertas */
  --text:    #FFFFFF;
  --text2:   #A8C0D6;   /* Texto secundario */
  --grid:    #1E3A5F;   /* Bordes y líneas de grilla */
}
```

**Tipografías:**
- `Space Grotesk` — UI principal (pesos 300–700)
- `JetBrains Mono` — datos numéricos y código

---

## Contexto científico / Scientific context

**LIPID MAPS® Partial Spectra DB** es una iniciativa del consorcio [EpiLipidNET](https://www.epilipidnet.eu/) y el equipo LIPID MAPS®. Se enfoca en:

- **Epilípidos**: lípidos modificados post-transcripcionalmente por procesos enzimáticos o ambientales, con subestructuras detectables por MS/MS pero aún no completamente caracterizadas.
- **Lípidos de organismos poco estudiados**: especies lipídicas de bacterias, hongos y organismos marinos sin representación en bases de datos convencionales.
- **Anotación parcial**: espectros con información estructural incompleta que requieren complemento experimental para elucidación total.

**Contribuidores del dataset:**
Zhixu Ni · Laura Goracci · Paul Kennedy · Miguel Gijón · María Fedorova · equipo LIPID MAPS®

---

## Autor / Author

**Alejandro Bernal**
Dashboard desarrollado el 05/04/2026
Asistencia analítica: [Claude AI](https://claude.ai) · Anthropic

---

## Referencias / References

- LIPID MAPS® Partial Spectra Database: https://www.lipidmaps.org/databases/partial_db/overview
- Plotly.js: https://plotly.com/javascript/
- EpiLipidNET: https://www.epilipidnet.eu/
- Freedman, D. & Diaconis, P. (1981). *On the histogram as a density estimator: L2 theory.* Zeitschrift für Wahrscheinlichkeitstheorie.
- Silverman, B.W. (1986). *Density Estimation for Statistics and Data Analysis.* Chapman & Hall.

---

## Licencia / License

Este proyecto es de uso libre para fines académicos y de investigación. Los datos pertenecen a LIPID MAPS® y están sujetos a sus términos de uso ([lipidmaps.org](https://www.lipidmaps.org)).

```
MIT License — free to use, modify and distribute with attribution.
```
