# PetroPI Dashboard

Static production and reservoir surveillance dashboard for well-test data.

## Use

Download and extract the repository ZIP, then open `index.html` in your browser. The Excel library is embedded in the HTML, so it works without a separate script or internet connection.

Alternatively, serve this folder locally:

```sh
python -m http.server 8000
```

Then open `http://localhost:8000`. The dashboard includes demo data; use **Upload Excel** to load a workbook.

The **Excel template** uses seven sheets, with input rows on **Well Test**. Use `Well Name`, `Date`, `Well Test Rate` (total liquid in m³/day), `BSW%` (enter `25` for 25%), and `Pr` (kPa). PetroPI calculates water rate as total liquid × BSW% / 100 and oil rate as the remainder; it does not depend on Excel's calculated `BSW` or `1-BSW` cells. The other sheets are reference information and do not supply test inputs automatically.

Workbooks with separate `Oil Rate` and `Water Rate` columns still work with `Well`, `Date`, `Pr`, and measured Pwf or the estimation columns below. `Field` and `Gas Rate` are optional. The dashboard scans the first 50 rows of worksheets for matching headers and reads `.xlsx`, `.xls`, and `.xlsm` (macros are not run).

## Flowing bottomhole pressure (Pwf)

The Pwf columns are to the right of `ESP Depth` on **Well Test**. Leave `Measured Pwf (kPa)` blank to estimate it. On each row, provide `Well Type`, `Target MD (m)`, `Flowing Pressure Gradient (kPa/m MD)`, and the applicable reference:

| Well Type | Reference pressure (kPa) | Reference MD (m) |
|---|---|---|
| `FF` (naturally flowing) | `THP` (wellhead tubing pressure) | Wellhead, blank or zero |
| `ESP` (electric submersible pump) | `PIP` (pump intake pressure) | Pump intake MD |
| `GL` (gas lift) | `Injection-Point Tubing Pressure (kPa)` | Injection point MD |

`Pwf = reference pressure + flowing pressure gradient × (target MD − reference MD)`. Use measured depths along the wellbore from the same datum. The gradient must be per meter of measured depth for that interval, not per meter of TVD. PetroPI does not assume the original `Perf. Depth` and `ESP Depth` columns are MD; copy values to `Target MD (m)` and `Reference MD (m)` only if they are confirmed to be measured depths. The target should be the reservoir pressure reference depth, typically the perforation midpoint if `Pr` is reported there. Use a consistent pressure basis for `Pr`, measured Pwf, and reference pressures; use absolute pressures for Vogel qmax. For GL, use tubing pressure at the active injection point, not annulus or surface injection pressure. Supply the average flowing gradient for the specific interval (below the intake for ESP and below the injection point for GL); the app does not model pump head or multiphase flow.

The dashboard labels measured and estimated values separately, uses the selected Pwf for PI and Vogel qmax, and flags missing inputs and `Pwf ≥ Pr`. A nonblank measured Pwf takes precedence; an invalid measurement is flagged rather than silently replaced. The XLSX export retains the estimate inputs, result, and source. These are screening estimates; verify gradients and results before operational use.

## GitHub Pages

The workflow in `.github/workflows/pages.yml` publishes the repository root to GitHub Pages whenever `main` is updated. After the first deployment completes, the published link above will serve the dashboard.

## Spreadsheet library

The SheetJS library is embedded in `index.html`; its original bundle is also included as `xlsx.full.min.js`. Its license is included in `SheetJS-LICENSE.txt`.
