# PetroPI Dashboard

Static production and reservoir surveillance dashboard for well-test data.

## Use

Download and extract the repository ZIP, then open `index.html` in your browser. The Excel library is embedded in the HTML, so it works without a separate script or internet connection.

Alternatively, serve this folder locally:

```sh
python -m http.server 8000
```

Then open `http://localhost:8000`. The dashboard includes demo data; use **Upload Excel** to load a workbook.

Required columns are `Well`, `Date`, `Oil Rate`, `Water Rate`, and `Pr` (reservoir pressure), plus `Pwf`/`Measured Pwf (kPa)` or the estimation columns below. `Field` and `Gas Rate` are optional; missing fields are labeled `Unassigned`. The dashboard scans worksheets and the first 50 rows for a matching header, preferring `WellTests`. It reads `.xlsx`, `.xls`, and `.xlsm` workbook data (macros are not run). Rates are in m³/day (gas in Sm³/day).

## Flowing bottomhole pressure (Pwf)

Use **Excel template** for the column names and one example of each well type. Leave `Measured Pwf (kPa)` blank to estimate it. On each row, provide `Well Type`, `Target MD (m)`, `Flowing Pressure Gradient (kPa/m MD)`, and the applicable reference:

| Well Type | Reference pressure (kPa) | Reference MD (m) |
|---|---|---|
| `FF` (naturally flowing) | `Wellhead Tubing Pressure (kPa)` | Wellhead, blank or zero |
| `ESP` (electric submersible pump) | `Pump Intake Pressure (kPa)` | Pump intake MD |
| `GL` (gas lift) | `Injection-Point Tubing Pressure (kPa)` | Injection point MD |

`Pwf = reference pressure + flowing pressure gradient × (target MD − reference MD)`. Use measured depths along the wellbore from the same datum. The gradient must be per meter of measured depth for that interval, not per meter of TVD. When migrating an old template, replace the TVD inputs and gradient with MD-based values; do not just rename the columns. The target should be the reservoir pressure reference depth, typically the perforation midpoint if `Pr` is reported there. Use a consistent pressure basis for `Pr`, measured Pwf, and reference pressures; use absolute pressures for Vogel qmax. For GL, use tubing pressure at the active injection point, not annulus or surface injection pressure. Supply the average flowing gradient for the specific interval (below the intake for ESP and below the injection point for GL); the app does not model pump head or multiphase flow.

The dashboard labels measured and estimated values separately, uses the selected Pwf for PI and Vogel qmax, and flags missing inputs and `Pwf ≥ Pr`. A nonblank measured Pwf takes precedence; an invalid measurement is flagged rather than silently replaced. The XLSX export retains the estimate inputs, result, and source. These are screening estimates; verify gradients and results before operational use.

## GitHub Pages

The workflow in `.github/workflows/pages.yml` publishes the repository root to GitHub Pages whenever `main` is updated. After the first deployment completes, the published link above will serve the dashboard.

## Spreadsheet library

The SheetJS library is embedded in `index.html`; its original bundle is also included as `xlsx.full.min.js`. Its license is included in `SheetJS-LICENSE.txt`.
