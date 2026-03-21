# Data

The analysis in this repository requires four input datasets stored locally in this folder. All datasets are publicly available and can be downloaded from the sources listed below. Raw data files are not included in this repository.

---

## Required Files

### 1. EPA AQS Annual AQI County Summary Files
**Used in:** Section 2a  
**Folder:** `data/epa_pm25/`  
**Files:** `annual_aqi_by_county_2015.csv` through `annual_aqi_by_county_2020.csv` (6 files)  
**Source:** [EPA AQS Airdata — Annual Summary Files](https://aqs.epa.gov/aqsweb/airdata/download_files.html)  
**Access:** Public, no registration required. Under "Annual Summary Data", download the "Annual AQI By County" files for each year 2015–2020.

**Key columns used:**

| Column | Description |
|--------|-------------|
| `State` | State name |
| `County` | County name |
| `Year` | Calendar year |
| `Days.PM2.5` | Number of days PM2.5 was the dominant AQI pollutant |
| `Days.with.AQI` | Total days with valid AQI readings |

**Note:** FIPS codes are not included in these files. They are assigned in the notebook by joining on state and county name using the `tigris` county reference table.

---

### 2. CDC PLACES County Health Data
**Used in:** Section 2b  
**File:** `data/cdc_places.csv`  
**Source:** [CDC PLACES — County Data (GIS Friendly Format)](https://data.cdc.gov/500-Cities-Places/PLACES-County-Data-GIS-Friendly-Format-2022-releas/i46a-9kgh/about_data)  
**Access:** Public, no registration required. Click Export → CSV.

**Key columns used:**

| Column | Description |
|--------|-------------|
| `CountyFIPS` | 5-digit county FIPS code |
| `CASTHMA_CrudePrev` | Adult asthma crude prevalence (%) |
| `COPD_CrudePrev` | COPD crude prevalence (%) |
| `CHD_CrudePrev` | Coronary heart disease crude prevalence (%) |
| `DIABETES_CrudePrev` | Diabetes crude prevalence (%) |

**Note:** CDC PLACES provides a single cross-section of model-based prevalence estimates derived from BRFSS survey data. These are joined to all years of the panel; temporal variation in health outcomes is not available at the county level in this dataset.

---

### 3. Census ACS — Poverty Rate
**Used in:** Section 2c  
**File:** `data/acs_poverty.csv`  
**Source:** [US Census data.census.gov](https://data.census.gov) — Table `B17001`  
**Access:** Public, no registration required. Search `B17001`, set Geography to County → All counties in the United States, download as CSV.

**Key columns used:**

| Column | Description |
|--------|-------------|
| `GEO_ID` | Census geography identifier (format: `0500000US12345`) |
| `NAME` | County name |
| `B17001_001E` | Total population for whom poverty status is determined |
| `B17001_002E` | Population below poverty level |

**Derived variable:** `poverty_rate = B17001_002E / B17001_001E × 100`

---

### 4. Census ACS — Health Insurance Coverage
**Used in:** Section 2c  
**File:** `data/acs_uninsured.csv`  
**Source:** [US Census data.census.gov](https://data.census.gov) — Table `B27010`  
**Access:** Public, no registration required. Search `B27010`, set Geography to County → All counties in the United States, download as CSV.

**Key columns used:**

| Column | Description |
|--------|-------------|
| `GEO_ID` | Census geography identifier |
| `B27010_001E` | Total civilian noninstitutionalized population under 65 |
| `B27010_017E` | Population under 65 with no health insurance coverage |

**Derived variable:** `pct_uninsured = B27010_017E / B27010_001E × 100`

---

### 5. Census ACS — Median Household Income
**Used in:** Section 2c  
**File:** `data/acs_income.csv`  
**Source:** [US Census data.census.gov](https://data.census.gov) — Table `B19013`  
**Access:** Public, no registration required. Search `B19013`, set Geography to County → All counties in the United States, download as CSV.

**Key columns used:**

| Column | Description |
|--------|-------------|
| `GEO_ID` | Census geography identifier |
| `B19013_001E` | Median household income in the past 12 months (2019 inflation-adjusted dollars) |

**Derived variable:** `log_income = log(B19013_001E)` (log-transformed in notebook to reduce right skew)

---

## Notes on ACS Downloads

All three ACS files use the 2019 5-year estimate vintage. When downloading from data.census.gov, Census exports a CSV with a second descriptor row directly below the column headers (the `GEO_ID` of this row reads `"Geography"`). This row is filtered out automatically in the notebook.

The `GEO_ID` column in Census exports follows the format `0500000US12345` — the last five characters are the county FIPS code, which is extracted in the notebook for joining.

---

## Folder Structure

```
data/
├── README.md
├── cdc_places.csv
├── acs_poverty.csv
├── acs_uninsured.csv
├── acs_income.csv
└── epa_pm25/
    ├── annual_aqi_by_county_2015.csv
    ├── annual_aqi_by_county_2016.csv
    ├── annual_aqi_by_county_2017.csv
    ├── annual_aqi_by_county_2018.csv
    ├── annual_aqi_by_county_2019.csv
    └── annual_aqi_by_county_2020.csv
```

---

*industry_projects - Samantha McGarrigle*