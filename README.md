# Data Reconciliation Tool

A browser-based tool for analysts who need to compare two data sources and find out exactly what does not match. Drop in two files, map the columns, and get a clear breakdown of missing rows, value differences, and formula discrepancies. Everything runs locally in your browser — nothing is uploaded anywhere.

---

## The problem it solves

Comparing two spreadsheets that should match is one of the most common and most painful tasks in data work. VLOOKUP gets you partway there. This tool does the full job — missing rows, value differences, and formula integrity checks — in one place, with a report you can export.

---

## How it works

1. Drop in Source A and Source B — Excel or CSV
2. If the file has multiple sheets, pick which one to use
3. Select the key column that links rows across both sources (e.g. ID, Reference Number)
4. Choose which columns to compare, or leave it to auto-match columns with the same name
5. Hit Run reconciliation
6. Review the results across four tabs — value differences, formula differences, missing rows, and matched rows
7. Export discrepancies as a CSV or copy a summary to paste into an email or report

---

## What it checks

**Value differences**
Rows that exist in both sources but have different values in one or more columns. Shown side by side with the Source A and Source B values highlighted.

**Formula differences (Excel only)**
Compares the actual formula strings behind each cell, not just the displayed values. Flags three types of issue:

- Formula in one source, hardcoded value in the other — the most common integrity risk. Someone may have manually overwritten a formula.
- Different formula strings — both cells have formulas but the logic differs. Values may still match today but the underlying calculation has diverged.

This tab only appears when at least one source is an Excel file. CSV files have no formulas to compare.

**Missing in Source B**
Rows present in Source A that have no matching key in Source B.

**Missing in Source A**
Rows present in Source B that have no matching key in Source A.

**Matched**
Rows that reconcile cleanly across both sources.

---

## Supported file formats

- `.xlsx` — fully supported, including formula comparison
- `.xlsm` — fully supported
- `.csv` — fully supported for value comparison (no formula comparison, since CSV has no formula layer)

---

## Caveats

**Formula comparison is on the raw formula string.**
Two formulas that produce the same result but are written differently will show as a difference. For example `=SUM(B2:B10)` and `=B2+B3+B4+B5+B6+B7+B8+B9+B10` compute the same thing but would be flagged. In a DQA context this is the right behaviour — if the formula logic has changed, you want to know about it.

**Shared formulas show as the base formula.**
Excel stores repeated formulas (e.g. the same formula copied down a column) as a single shared formula to save space. The tool reads the base formula string and marks shared variants. This means the formula shown may not reflect the exact cell reference for every row, but it still accurately identifies whether a formula exists and whether the base logic differs.

**Formula results only reflect the last saved calculation.**
If an Excel file was saved before formulas recalculated (for example immediately after a data refresh), the displayed values may be stale. The formula strings themselves are always accurate regardless.

**Key matching is case-insensitive.**
Row matching normalises key values to lowercase before comparing. So `REC001` and `rec001` are treated as the same record.

**Value comparison is also case-insensitive.**
`Active` and `ACTIVE` are treated as a match. If you need case-sensitive comparison, note this is not currently supported.

**Large files may be slow.**
The tool reads up to 10,000 rows per sheet. Files with many columns and tens of thousands of rows will take a few seconds to process. Everything still runs locally — there is no server timeout to worry about.

**Showing up to 500 rows per tab.**
The missing rows tabs display a maximum of 500 rows in the UI to keep the browser responsive. The CSV export always contains all discrepancies regardless of this limit.

**No session memory.**
Closing the tab clears everything. Export your results before closing.

---

## Privacy

Your data never leaves your computer. Both files are read entirely within the browser using JSZip for Excel files and the FileReader API for CSV. Nothing is sent to any server. The tool works fully offline after the JSZip library loads on first open.

---

## Running the tool

**Locally**
1. Download `data-reconciliation.html`
2. Double-click to open in any browser
3. No install, no login, no internet needed after first load

**Hosted on GitHub Pages**
1. Upload to a GitHub repo as `index.html`
2. Go to Settings, Pages, Deploy from branch, main, root, Save
3. Share the link

---

## Tech stack

- Plain HTML, CSS, and JavaScript — one file, no framework, no build step
- [JSZip](https://stuk.github.io/jszip/) for reading Excel files locally in the browser
- No backend, no database, no cookies, no tracking

---

## Licence

MIT — free to use, modify, and share.
