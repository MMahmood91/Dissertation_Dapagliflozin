# Generic Dapagliflozin Adoption in West Midlands GP Practices

Analysis code for the MSc Business Analytics dissertation *Generic Dapagliflozin Adoption in West Midlands GP Practices: Prescribing Variation and Savings after Patent Invalidation* (Mifta Mahmood, Aston University, 2026).

The study looks at the generic dapagliflozin share of all SGLT2 inhibitor items in the three West Midlands ICBs (Birmingham and Solihull, Black Country, and Coventry and Warwickshire) from January 2025 to June 2026. September 2025, the first full month of generic supply, is the break point.

## Repository layout

```
notebooks/   the seven analysis notebooks, with all outputs saved
results/     the cleaned ICB data, the postcode lookup and the aggregated result tables (CSV)
figures/     the charts used in the dissertation (PNG)
README.md    this file
```

The notebooks read and save files in their own folder. The CSV and PNG files were moved into `results/` and `figures/` afterwards to make them easier to browse. To re-run a notebook, put it in the same folder as the files it reads.

## Notebooks and run order

| Notebook (in `notebooks/`) | What it does | Main outputs (in `results/` and `figures/`) |
|---|---|---|
| `01_OpenPrescribing_SubICB_cleaned.ipynb` | Cleans the monthly ICB-level data | `op_wm_cleaned.csv` |
| `02_ITS_Analysis.ipynb` | Interrupted time series for each ICB (RQ1, RQ2) | `ITS_summary.csv`, `ITS_newey_west_results.csv`, `ITS_second_break.csv`, `ttest_results.csv`, `ITS_results.png` |
| `03a_Postcode_LSOA_Lookup.ipynb` | Looks up the LSOA and district for each practice postcode | `postcode_lsoa_lookup.csv` |
| `03_GP_Practice_Analysis.ipynb` | Builds the practice-level dataset | `practice_generic_share_by_icb.png` |
| `04_Regression_Model.ipynb` | Practice-level regression, robustness checks and anomaly detection (RQ3) | `regression_results.csv`, `model_comparison_results.csv`, diagnostic charts |
| `05a_EPD_Extraction.ipynb` | Extracts SGLT2 rows from the monthly English Prescribing Dataset files | (practice-level extract, not published) |
| `05_EPD_Cost_Savings.ipynb` | Prices and the three savings scenarios (RQ4) | `monthly_savings.csv`, `icb_savings.csv`, `district_savings.csv`, `savings_summary.csv`, two charts |

Run them in this order: 01, 02, 03a, 03, 04, 05a, 05. All outputs are saved in the notebooks, so you can review them without running anything.

Notebook 03a calls the postcodes.io API. Notebook 05a reads 18 EPD files of about 7 GB each and takes around 15 minutes.

## Data

**In this repository:** the cleaned ICB-level data, the postcode lookup and all aggregated results (ICB, district and summary level).

**Not in this repository:** four practice-level files.

| File | Made by | What it holds |
|---|---|---|
| `practice_master_lookup.csv` | Notebook 03 | One row per practice: code, name, ICB, district, postcode, IMD, diabetes prevalence and list size |
| `practice_level_regression_data.csv` | Notebook 03 | The regression dataset: each practice's generic share and predictors |
| `epd_sglt2_extract.csv` | Notebook 05a | SGLT2 items, quantities and costs by practice and month |
| `outlier_practices.csv` | Notebook 04 | The 22 practices flagged as faster or slower than expected |

These files name individual GP practices or hold their codes. The ethics section of the dissertation (Section 3.7) keeps practices unidentified, so they are not published here. Some practices are flagged as faster or slower than expected, and a flag is a prompt for clinical review, not a judgement on a practice. Listing them by name could be read as a league table of poor care. Examiners can have these files on request.

**Raw sources (all public):**

- OpenPrescribing, dapagliflozin measure (sub-ICB and practice level): https://openprescribing.net/measure/dapagliflozin/
- NHSBSA English Prescribing Dataset (EPD) with SNOMED code, January 2025 to June 2026: https://opendata.nhsbsa.net/dataset/english-prescribing-dataset-epd-with-snomed-code
- Quality and Outcomes Framework 2024-25 (prevalence and geography mapping files): https://digital.nhs.uk/data-and-information/publications/statistical/quality-and-outcomes-framework-achievement-prevalence-and-exceptions-data/2024-25
- English Indices of Deprivation 2019 (Ministry of Housing, Communities and Local Government): https://www.gov.uk/government/statistics/english-indices-of-deprivation-2019
- NHS ODS GP practice file (epraccur): https://digital.nhs.uk/services/organisation-data-service/export-data-files/csv-downloads/gp-and-gp-practice-related-data
- postcodes.io: https://postcodes.io

To re-run the notebooks, put the raw files in the folders the notebooks name (`OpenPrescribing_Data/`, `QOF_2024_25/`, `ONS_data/` and `EPD_Data/`), in the same folder as the notebooks.

## Software

Python 3.14 with pandas, numpy, matplotlib, statsmodels, scipy, scikit-learn and requests.

## Use of AI

The code was written with help from Claude (Anthropic), a generative AI assistant, which helped to write and debug it. I ran and checked all the code myself, and the outputs shown are from my own runs. Each notebook ends with a full statement.
