# Saudi Labor Market Analysis (2021–2025)
### Analyzing Saudization Progress Through GASTAT, GOSI, and KAPSARC Data

---

## The Question

Vision 2030 set explicit targets for Saudi workforce participation, private sector
employment, and female inclusion. This project uses five years of quarterly labor
market data to assess whether the headline numbers reflect structural change — or
whether they measure something narrower than the policy intended.

---

## Data Sources

| Source | What it covers | Format |
|---|---|---|
| GASTAT Labor Market Survey | Employment, unemployment, participation, wages — quarterly Q1 2021–Q4 2025 | 20 Excel files |
| GOSI Open Data | Saudi/Non-Saudi contributor split by sector | Zip of quarterly xlsx files |
| KAPSARC Administrative Records | GOSI contributor headcounts 2017–2025 | CSV |

---

## Project Structure

```
gastat-labor-market/
├── data/
│   ├── raw/               ← GASTAT quarterly Excel files (not tracked)
│   └── supplementary/     ← GOSI zip + KAPSARC CSV (not tracked)
├── notebooks/
│   ├── 01_extract.ipynb   ← Extraction pipeline → 9 clean CSVs
│   └── 02_analyze.ipynb   ← Analysis and visualization
└── output/                ← Generated CSVs (not tracked)
```

---

## What the Extraction Notebook Does

`01_extract.ipynb` loops 20 quarterly GASTAT Excel files and handles two
real-world extraction challenges:

- **Inconsistent file layouts** — GASTAT's 2023 files use a different sheet
  structure than earlier and later files. The pipeline detects the layout and
  adjusts extraction logic accordingly.
- **Sheet position shifts** — A new education sheet was added from Q2 2021,
  shifting occupation and economic activity sheets by one index position.
  The pipeline searches by Arabic header label rather than hardcoded index.

**Outputs (9 CSVs):**

| File | Rows | Coverage |
|---|---|---|
| `combined_main.csv` | 80 | Employment, unemployment, participation, wages — all 20 quarters |
| `sector_employment.csv` | 100 | Public vs private split — all 20 quarters |
| `occupation_employment.csv` | 220 | 11 occupation groups — all 20 quarters |
| `economic_activity_employment.csv` | 460 | 23 economic activities — all 20 quarters |
| `wages_by_sector.csv` | 38 | Wages by sector — 2024–2025 (GASTAT introduced this breakdown in 2024) |
| `working_hours.csv` | 20 | Avg working hours by nationality — all 20 quarters |
| `private_sector_willingness.csv` | 16 | Saudi unemployed willing to work in private sector — Q1 2022–Q4 2025 |
| `gosi_headcount.csv` | — | GOSI contributor headcounts by nationality — 2017–2025 |
| `gosi_sector_split.csv` | — | Saudi/Non-Saudi % by sector — 2025–2026 |

---

## Key Findings

**The headline numbers moved in the right direction.**
Saudi unemployment fell from 12.2% to 7.2%. Employment rose from 42.4% to 45.9%.
Female participation held above the Vision 2030 target of 30% for the entire period.

**The GOSI headcount data adds a layer the survey rates don't show.**
Between 2021 and 2024, for every Saudi who entered formal employment, roughly
3 to 4 Non-Saudi workers were added alongside them — 680,000 Saudis versus
2.3 million Non-Saudis. Part of this reflects rapid non-oil GDP expansion.
The employment rate gap is narrowing because more Saudis are working, not because
the expatriate workforce is shrinking.

**The occupational and sector split is more nuanced at the sector level.**
Aggregate data shows Saudis concentrated in professional and technical roles,
Non-Saudis in operational and elementary ones. At the sector level, both groups
are present in retail, hospitality, and transport — and the Non-Saudi share in
those sectors has been growing relative to the Saudi share since 2021.

**93% of Saudi unemployed reported willingness to work in the private sector**
in every quarter since Q1 2022. That figure did not move. The commonly cited
explanation for slow Saudization — that Saudis resist private sector work —
is not supported by the data.

**The wage gap widened.** Saudi average monthly wages grew from SAR 10,404 to
SAR 11,103. Non-Saudi wages grew from SAR 3,892 to SAR 4,131. The absolute gap
increased by SAR 460. Note: wages received by employees are not equivalent to
total employer cost, which includes GOSI contributions, visa and sponsorship fees,
and Nitaqat compliance costs — none of which are captured in this dataset.

---

## Power BI Dashboard (In Progress)

An interactive Power BI dashboard is being built on top of the extracted CSVs,
covering employment trends, sector splits, wage gaps, and the GOSI cross-validation
in a format designed for non-technical stakeholders.

---

## Tools

Python · pandas · openpyxl · Plotly · Jupyter Notebook

---

## Running the Project

1. Place the 20 GASTAT quarterly Excel files in `data/raw/`
2. Place `gosi.zip` and `labor-market-administrative-records-data.csv` in `data/supplementary/`
3. Run `01_extract.ipynb` top to bottom — this generates all CSVs in `output/`
4. Run `02_analyze.ipynb` top to bottom — this produces all charts and analysis

**Data sources:**
- GASTAT: [Statistical Reports](https://www.stats.gov.sa/en/statistics-tabs?tab=436312&category=417515)
- GOSI: [Open Data Portal](https://www.gosi.gov.sa/ar/StatisticsAndData/OpenedData/download)
- KAPSARC: [Labor Market Administrative Records](https://datasource.kapsarc.org/explore/assets/labor-market-administrative-records-data/)
