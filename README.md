# PetroPI Dashboard

Static production and reservoir surveillance dashboard for well-test data.

## Use

Open the published [PetroPI dashboard](https://alashkhariabdullah95-maker.github.io/petropi-dashboard/) or serve this folder locally:

```sh
python -m http.server 8000
```

Then open `http://localhost:8000`. The dashboard includes demo data; use **Upload XLSX** to load a workbook. It reads a worksheet named `WellTests`, or the first worksheet if that name is absent.

Required columns are `Well`, `Field`, `Date`, `Oil Rate`, `Water Rate`, `Pr`, and `Pwf`. `Gas Rate` is optional. Pressure is in kPa and rates are in m³/day. The dashboard calculates PI and Vogel qmax, flags invalid rows, and can export filtered data and a validation report as XLSX files.

## GitHub Pages

The workflow in `.github/workflows/pages.yml` publishes the repository root to GitHub Pages whenever `main` is updated. After the first deployment completes, the published link above will serve the dashboard.

## Spreadsheet library

`xlsx.full.min.js` is the bundled SheetJS library. Its license is included in `SheetJS-LICENSE.txt`.
