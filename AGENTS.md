# AGENTS.md

## Repo shape

- Single static file: `index.html` (LabFluids, Spanish-language hydrostatic pressure simulator). No build step, package manager, tests, or linter — verify changes by opening the file in a browser.
- `Guía de Aprendizaje y Manual de Usuario.odt` is a binary LibreOffice/Word document (the user manual). Never edit it as text.
- UI text, comments, and identifiers are in Spanish (`presion`, `profundidad`, `líquido`). Keep new code consistent.

## Gotchas

- External CDNs only: Chart.js (`cdn.jsdelivr.net/npm/chart.js`) and Google Fonts (Inter, Space Grotesk). No bundler or vendored copies; chart and fonts need internet.
- Physics model: `Ph = densidad * g * h`; `Patm = 101.325` kPa for absolute pressure; derived outputs `F = Ph·A`, `Fe = ρ·g·V`, float/sink by comparing `ρ_obj` vs `ρ_líq`. `maxProf` (default 10 m) drives the depth slider, ruler, and chart x-axis — they must stay in sync.
- Fluids are **data-driven**: presets live in the `LIQUIDOS` array (`nombre`, `densidad`, `color`); the `<select>`, the poster chips, tank color, and chart colors all read `option.dataset.nombre` / `option.dataset.color`. There are no per-fluid CSS classes.
- Chart has three kinds of datasets, distinguished by flags: `esCurva` (live theoretical line), `esActual` (current-state point), `esScatter` (registered rows, created on demand per fluid name). When filtering/clearing, preserve `esCurva` and `esActual`.
- Chart.js uses `type: 'line'` with `scales.x.type: 'linear'` (numeric `{x,y}` data). Live updates call `chart.update('none')`; only meaningful transitions pass `chart.update()` for animation.
- Custom fluids are added at runtime via the form (`dataset.custom = '1'`); they exist only in memory (no localStorage/backend). Registration appends to the table/chart only.
- Number display uses `toLocaleString('es-ES')` (comma decimals). Never write formatted strings into `input[type=number].value` — use the raw number.
