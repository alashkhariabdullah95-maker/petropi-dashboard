# PetroPI Dashboard

Static production and reservoir surveillance dashboard for well-test data.

## Use

Download and extract the repository ZIP, then open `index.html` in your browser. The Excel library is embedded in the HTML, so it works without a separate script or internet connection.

Alternatively, serve this folder locally:

```sh
python -m http.server 8000
```

Then open `http://localhost:8000`. The dashboard includes demo data; use **Upload Excel** to load a workbook.

Required columns are `Well`, `Date`, `Oil Rate`, `Water Rate`, `Pr` (reservoir pressure), and `Pwf` (flowing pressure). `Field` and `Gas Rate` are optional; missing fields are labeled `Unassigned`. Header matching ignores capitalization, spacing, punctuation, and common units. The dashboard scans worksheets and the first 50 rows for a matching header, preferring a sheet named `WellTests`. It reads `.xlsx`, `.xls`, and `.xlsm` workbook data (macros are not run). Pressure is in kPa and rates are in m³/day. The dashboard calculates PI and Vogel qmax, flags invalid rows, and can export filtered data and a validation report as XLSX files.

## GitHub Pages

The workflow in `.github/workflows/pages.yml` publishes the repository root to GitHub Pages whenever `main` is updated. After the first deployment completes, the published link above will serve the dashboard.

## Spreadsheet library

The SheetJS library is embedded in `index.html`; its original bundle is also included as `xlsx.full.min.js`. Its license is included in `SheetJS-LICENSE.txt`.
