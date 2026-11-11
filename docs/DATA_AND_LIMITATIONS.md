# Data provenance and limitations

## Public sources and references

- [NMDB / NEST](https://www.nmdb.eu/nest/): neutron-monitor observation data; follow provider acknowledgments and distribution terms.
- [ERA5 Hourly in Google Earth Engine](https://developers.google.com/earth-engine/datasets/catalog/ECMWF_ERA5_HOURLY): gridded reanalysis used for geographic point extraction; observe required ECMWF/Copernicus attribution.
- [FAA CARI-7 / CARI-7A](https://www.faa.gov/data_research/research/med_humanfacs/aeromedical/radiobiology/cari7): separate cosmic radiation modeling program. The project depended on an externally generated `.ANS` output file.
- Copeland, K. (2017), *CARI-7A: Development and Validation*, Radiation Protection Dosimetry 175(4), 419–431, DOI: 10.1093/rpd/ncw369. Reference only; the copyrighted paper is not redistributed.

## Project evidence

The original source records include code for latitude/longitude checks, Google Earth Engine geographic-point sampling, ERA5 exports, station/date integration, station-wise linear regression and the final world map. The original README recorded 57 station codes, 49 observed and 47 with valid coordinates; the archived S7 notebook's saved execution printed 288,557 matched rows at 47 stations. These are historical records, **not independently rerun validations**.

## Limitations

1. **Unfinished study:** the workflow is not validated end-to-end; the full set of data files is unavailable in this public repository.
2. **Missing source files:** large NMDB, ERA5 and CARI tables are not included; source reuse and licensing must be checked.
3. **Statistical scope:** R² from in-sample station-level OLS is a descriptive association, not a causal claim nor conclusive physical-model validation. Common trends, temporal autocorrelation, dataset coverage and incompatible measurement units can affect interpretation.
4. **Geographic scope:** the code documents GIS-style point extraction and mapped visualization, not advanced ArcGIS/QGIS geoprocessing or spatial econometric estimation.
5. **Software configuration:** the shared S3 notebook uses environment variables in place of the original private Google Cloud resources. Users must authenticate and set up their own infrastructure.
6. **Data/schema handoffs:** some original scripts depend on specific file naming conventions and pre-prepared station tables. Reproduction requires additional integration work.
7. **Historical date spans:** `2000_2026` in a file name denotes the intended download period; it does not imply complete observations through 2026 for each station.
8. **Altitude and geography:** geographic point sampling of gridded reanalysis does not imply point-level physical measurement or that aviation-altitude CARI rates are on the same absolute scale as ground-based detector counts.

## Future improvements

Implement a provider-compliant data subset and checksums, add table/schema validation between the eight stages, automate a reduced-size working example, assess time-blocked generalization and sensitivity of R² to station coverage, and systematically document geographic and altitude effects.
