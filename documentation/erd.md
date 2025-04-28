# Entity-Relationship Diagram

```markdown
[raw_wdi] 
    - country_code (PK)
    - indicator_code (PK)
    - country_name
    - indicator_name
    - YR2000 ~ YR2023
    |
    |--(UNPIVOT and CLEAN via DuckDB)-->
    |
[clean_wdi] 
    - country_code (PK, FK from raw_wdi)
    - indicator_code (PK, FK from raw_wdi)
    - country_name
    - indicator_name
    - year (PK)
    - value
    |
    |--(Pivot to Wide Format)-->
    |
[wide_format_data.csv]
    - country_code
    - country_name
    - year
    - NY_GDP_PCAP_KD (GDP per capita)
    - SL_EMP_TOTL_SP_ZS (Employment Rate)
    - NY_GDP_MKTP_KD_ZG (GDP Growth)
    |
    |--(Regression Analysis)--> [regression_results.csv]
    |
    |--(Panel Regression)--> 
    |     [panel_results_NY_GDP_PCAP_KD.csv]
    |     [panel_results_NY_GDP_MKTP_KD_ZG.csv]
    |     [panel_results_SL_EMP_TOTL_SP_ZS.csv]
    |
    |--(Time Series Forecasting)-->
    |     [forecast_USA_NY_GDP_PCAP_KD.csv]
    |     [forecast_CHN_NY_GDP_PCAP_KD.csv]
    |     [forecast_IND_NY_GDP_PCAP_KD.csv]
    |     [forecast_USA_SL_EMP_TOTL_SP_ZS.csv]
    |     ...
    |
    |--(Machine Learning Modeling)-->
    |     [ml_models_comparison_NY_GDP_PCAP_KD.csv]
    |     [ml_models_comparison_NY_GDP_MKTP_KD_ZG.csv]
    |     [ml_models_comparison_SL_EMP_TOTL_SP_ZS.csv]
    |
    |--(Clustering Analysis)-->
          [cluster_countries_Average_of_all_years.csv]
          [cluster_countries_Data_from_2023.csv]
```

## Explanation

* `raw_wdi`: Original wide table (economic indicators expanded by year)
* `clean_wdi`: Cleaned long table (each row is country-indicator-year-value)
* `wide_format_data.csv`: Pivoted wide table from `clean_wdi`, used for modeling
* Subsequent model output tables, such as `panel_results_xxx.csv`, `forecast_xxx.csv`, `ml_models_comparison_xxx.csv`, `cluster_countries_xxx.csv`, are results obtained by applying different modeling methods based on `wide_format_data`