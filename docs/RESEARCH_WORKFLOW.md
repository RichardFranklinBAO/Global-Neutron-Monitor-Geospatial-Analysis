# Research workflow and input/output handoffs

The eight-stage process below documents the original exploratory project. **This portfolio is not verified as an end-to-end reproduction.** Original R scripts are kept close to their source form; Earth Engine identifiers and local paths have been redacted from the shared S3 notebook.

## Inputs

- NMDB NEST station time series, exported by year as CSV with UTC time and station codes. Original notes refer to 12-hour sampling from 2000 through a 2026 extraction horizon.
- `dataset/row/nmdb_stations_final.csv`: station / lat / lon / alt_m; other fields may include cutoff_GV and notes.
- `dataset/inputs/s7/test.ANS`: CARI-7A output generated externally by the researcher; not bundled.
- `dataset/inputs/s8/nmdb_stations_final.csv`: coordinate table for global map.
- ERA5 export files retrieved using an authorized Earth Engine / Google Cloud account.

## Stages

1. **S1 — Mapping:** read station metadata, validate longitude/latitude bounds, detect duplicate station codes and plot on an R basemap.
2. **S2 — NMDB cleaning and merging:** combine the yearly time-series exports, standardize UTC timestamps, compute observational coverage and create a selected-station coordinate lookup. Output examples: `dataset/outcomes/from_s2/nmdb_merged_2000_2026.csv`, `nmdb_selected_stations_latlon.csv`.
3. **S3 — ERA5 point sampling:** form Earth Engine geographic point features from station coordinates, select ECMWF/ERA5 hourly variables, sample each station location and export via Google Cloud Storage. Configure your own `EE_PROJECT_ID` and `EE_EXPORT_BUCKET`. Outputs go in `dataset/outcomes/from_s3/`.
4. **S4 — Weather aggregation:** join yearly ERA5 point exports and aggregate measurements to station-day periods `h00_12` and `h13_23`. Expects annual file names such as `era5_points_2000.csv`.
5. **S5 — Station altitude:** add station altitude `alt_m` as a metadata row above the NMDB wide-format time series. This is a legacy schema choice.
6. **S6 — Time-series integration:** reshape NMDB observations and join location/weather data by station, date and period. Export `station_CODE_weather_space_merged.csv` files in `dataset/outcomes/from_s6/`.
7. **S7 — CARI comparison:** parse the CARI `.ANS` file, average station readings to daily values, match by station and date, fit linear regressions, save per-station R² and visualization files in `dataset/outcomes/from_s7/`.
8. **S8 — World map:** merge `r2_results.csv` and station coordinates, render global location markers with color = R² and marker size = paired observation count.

## Setup

For Python notebooks:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Open notebooks from within the `scripts/` folder to retain relative paths.

R packages:

```r
install.packages(c("rmarkdown", "knitr", "here", "readr", "dplyr",
                   "stringr", "purrr", "tidyr", "lubridate",
                   "ggplot2", "maps", "ggrepel", "tibble"))
```

To render an individual R Markdown stage after providing its required datasets:

```bash
Rscript -e 'rmarkdown::render("scripts/S1 Creating My Map.Rmd")'
```

The original work uses annual files for S4, and S3's public notebook retains a template for yearly exports. Large historical Earth Engine exports can exceed resource quotas, so test a short time span before launching full retrieval. Google Cloud credentials and any necessary payment arrangements must be configured by the person rerunning the work.

## Current reproduction boundary

The portfolio includes scripts and a representative project map but **not** station metadata tables, raw NMDB data, ERA5 exports, complete CARI inputs, original Cloud credentials, or a working one-command test suite. Some source scripts have local assumptions. The S7 notebook has been stripped of large embedded output payloads before publication, while historical study figures are documented in the README.
