# Copilot instructions for generos-musicales

## Project overview

This is a browser-only, single-page D3 network visualization. The UI, styles, CSV parsing, validation, and rendering are all implemented in `index.html`; Tailwind CSS and D3 are loaded from CDNs.

On page startup, the app fetches its default CSVs from `data/Points.1793384852180.csv` and `data/Links.1793384846440.csv`. The header's **Cargar otros archivos** button opens file inputs that can replace the nodes or links CSV individually. The app expects the first two columns of the nodes file to contain ID and category, and the first two columns of the links file to contain source and target; optional values follow by position.

The main data flow is:

`default fetch or file input -> state.rawNodesText/rawLinksText -> parseData() -> updateUI() -> renderVisualization()`

`parseData()` detects comma or semicolon delimiters, checks link endpoints against node IDs, records missing references and isolated nodes, and preserves original CSV rows for tooltips. `renderVisualization()` draws the SVG graph and supports force-directed (`red`), radial, and concentric (`concentrico`) layouts.

## Build, test, and lint

There is no package manifest, build script, automated test runner, or lint configuration. Serve the repository root over HTTP so the default CSV `fetch()` requests work:

```bash
python3 -m http.server 8000
```

Open `http://localhost:8000/`. For a focused manual check, confirm the default graph loads, use **Cargar otros archivos** to replace each CSV, and switch between all three layouts. No single-test command exists.

## Repository-specific conventions

- Keep the app lightweight and browser-only; there is no module or framework setup.
- The default data paths are relative to the repository root. Keep the matching CSV files under `data/` and update startup fetches if their names change.
- CSV columns are interpreted by position, not header names. Preserve the original rows and headers because tooltips display their values.
- Incomplete link endpoints are skipped and missing node references are shown as warnings; retain this validation behavior when changing parsing.
- The interface is Spanish-language, including the manual upload dialog and validation messages.
- Appearance controls live in the sidebar: background and link colors are global, while node colors are selected per category in the legend. Keep those controls and the SVG/legend colors in sync.
- Layout changes should preserve the existing D3 simulation and fixed-position radial/concentric rendering paths.
