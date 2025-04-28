# Codebook

This document describes all variables used in the project’s main datasets, their meaning, units, origin, and includes the core data-fetch and data-cleaning code snippets.

---

## 1. Cleaned Long-Format Table: **wdi_long_clean.csv**

**Location:** `data/clean/wdi_long_clean.csv`  
**Description:** This “long” table is produced by UNPIVOTing the raw wide-format CSV into rows of (country, indicator, year, value).

| Variable         | Description                                      | Unit                          | Source                                                         |
|------------------|--------------------------------------------------|-------------------------------|----------------------------------------------------------------|
| country_code     | ISO-2 or ISO-3 country code                      | string                        | Derived from raw “country” column in economic_dev_2000_2023.csv |
| indicator_code   | World Bank indicator code (e.g. NY.GDP.PCAP.KD)   | string                        | Derived from raw “series” column                                 |
| country_name     | Country name label                               | string                        | Taken from raw “Country” column                                  |
| indicator_name   | Indicator name label                             | string                        | Taken from raw “Series” column                                   |
| year             | Observation year                                 | year                          | Extracted from wide-table columns YR2000…YR2023 during UNPIVOT    |
| value            | Indicator value for that country-year            | varies by indicator (USD or %)| UNPIVOT result of the YR#### columns                             |

**Core cleaning SQL (DuckDB):**  
```sql
CREATE OR REPLACE TABLE raw_wdi AS
SELECT
  country         AS country_code,
  series          AS indicator_code,
  "Country"       AS country_name,
  "Series"        AS indicator_name,
  YR2000, YR2001, …, YR2023
FROM read_csv_auto('data/raw/economic_dev_2000_2023.csv');

CREATE OR REPLACE TABLE clean_wdi AS
SELECT
  country_code,
  indicator_code,
  country_name,
  indicator_name,
  CAST(REPLACE(col, 'YR', '') AS INTEGER) AS year,
  value
FROM raw_wdi
UNPIVOT (
  value FOR col IN (YR2000, YR2001, …, YR2023)
)
WHERE value IS NOT NULL
ORDER BY country_code, indicator_code, year;
```

---

## 2. Raw Wide-Format Table: **economic_dev_2000_2023.csv**

**Location:** `data/raw/economic_dev_2000_2023.csv`  
**Description:** Pulled directly from the World Bank WDI API; contains three indicators for USA, CHN, IND over years 2000–2023.

| Variable | Description                              | Unit                        | Source Script                |
|----------|------------------------------------------|-----------------------------|------------------------------|
| country  | Country code (ISO-3, e.g. USA, CHN, IND) | string                      | fetch_and_save_wdi output    |
| series   | Indicator code (e.g. NY.GDP.PCAP.KD)     | string                      | fetch_and_save_wdi output    |
| Country  | Country name (English)                   | string                      | fetch_and_save_wdi output    |
| Series   | Indicator name (English)                 | string                      | fetch_and_save_wdi output    |
| YR2000   | Value of the indicator in year 2000      | varies by indicator (USD/%) | fetch_and_save_wdi output    |
| YR2001   | Value of the indicator in year 2001      | varies by indicator         | fetch_and_save_wdi output    |
| …        | …                                        | …                           | …                            |
| YR2023   | Value of the indicator in year 2023      | varies by indicator         | fetch_and_save_wdi output    |

**Core data-fetch Python code (wbgapi):**  
```python
import wbgapi as wb
import pandas as pd

df = wb.data.DataFrame(
    ["NY.GDP.PCAP.KD","SL.EMP.TOTL.SP.ZS","NY.GDP.MKTP.KD.ZG"],
    economy=["USA","CHN","IND"],
    time=list(range(2000,2024)),
    labels=True
).reset_index().rename(columns={"economy":"country","time":"year"})

df.to_csv("data/raw/economic_dev_2000_2023.csv", index=False, encoding="utf-8-sig")
```

---

### Notes

- The UNPIVOT step transforms columns YR2000…YR2023 into a unified `year` + `value` format, enabling panel-data analysis.
- Only the primary tables and fields used in the report are documented; any additional derived tables can follow this same template.