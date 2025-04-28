## QTM 350 Final Project: World Bank WDI Analysis  

This repository contains a fully reproducible workflow for analyzing World Bank World Development Indicators (WDI) data. We perform SQL-based cleaning, Python-based modeling (regression, panel, time-series, machine learning, clustering), and assemble results in a Quarto report.

---

### Repository structure  

```
├── data/  
│   ├── raw/                # Original wide-format CSV from WDI API  
│   │   └── economic_dev_2000_2023.csv  
│   ├── clean/              # Long-format cleaned data  
│   │   └── wdi_long_clean.csv  
│   └── tmp/                # DuckDB database file  
│       └── wdi.duckdb  
│  
├── documentation/          # Project documentation  
│   ├── codebook.md         # Variable definitions and origins  
│   └── erd.drawio          # Entity-relationship diagram (Draw.io format)  
│  
├── figures/  
│   ├── exploratory/        # Descriptive plots & tables  
│   └── modeling/           # Modeling outputs (regression, ARIMA, ML, clustering)  
│  
├── models/                 # CSV outputs of model results  
│   ├── wide_format_data.csv  
│   ├── regression_results.csv  
│   ├── panel_results_*.csv  
│   ├── forecast_* .csv  
│   ├── ml_models_comparison_*.csv  
│   └── cluster_countries_*.csv  
│  
├── scripts/  
│   ├── qtm350_final_project.ipynb   # Data fetch, cleaning, modeling pipeline  
│   └── qtm350_final_report.qmd      # Quarto report source  
│  
├── _quarto.yml             # Quarto project configuration  
└── README.md               # This file  
```

---

## Usage

1. **Generate data, models, and figures**  
   Run the Jupyter notebook in `scripts/qtm350_final_project.ipynb`.  
   - This executes the WDI API pull, DuckDB cleaning/UNPIVOT, exploratory analysis, and all modeling steps.  
   - It populates `data/clean/`, `figures/`, and `models/` with required CSVs and PNGs.   

2. **Render the report**  
   In the `scripts/` folder, execute:  
   ```bash
   quarto render qtm350_final_report.qmd --to html,pdf
   ```  
   This produces a fully reproducible HTML and PDF report with navigation bar, including Introduction, Data Description, Data Analysis, Results & Discussion, and Conclusion sections .

---

## Prerequisites

- **Python 3.8+** with packages: `wbgapi`, `pandas`, `duckdb`, `matplotlib`, `seaborn`, `scikit-learn`, `statsmodels`, `pmdarima`.  
- **Quarto** installed for rendering the report.  
- **Git** for version control; each group member’s contributions are tracked via commits and lines of code .

---

## Project Requirements

- Use **SQL** (via DuckDB) for data cleaning and descriptive statistics.  
- Use **Python** for modeling (regression, panel, ARIMA, ML, clustering).  
- Provide a **documentation/** folder containing a codebook and ERD.  
- Host on **GitHub** with meaningful commits and clear README.  
- Submit **Quarto**-generated HTML and PDF that render correctly on GitHub. 