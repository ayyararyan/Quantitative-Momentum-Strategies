# Quantitative Momentum Strategies

## Project overview and citation
This repository accompanies research exploring momentum, reversals, and liquidity effects in Indian equity markets, providing notebook-based workflows for reproducing portfolio construction and factor analysis across BSE and NSE universes. Please cite the underlying paper when using this code or derived results:

Chui, A., Ranganathan, K., Rohit, A., & Veeraraghavan, M. (2023). *Momentum, reversals and liquidity: Indian evidence*. *Pacific-Basin Finance Journal*, 102193.

## Dataset requirements
The notebook expects local market- and factor-level datasets to be available in the project root (same directory as the notebook). Filenames are referenced directly in the code, so keep the following names and column structures:

### Equity return panels
- **BSE_.csv** (BSE universe) and **NSE_.csv** (NSE universe)
  - Expected columns include: `permno` (unique firm ID), `Date` (monthly period), `ret` (percentage returns), `Market Capitalization`, `turn` (turnover), `company_name`, `co_nic_code` (industry code), `nse_dummy`, `owner_gp_name`, and `penny` (flag for penny-stock handling). Additional columns may be preserved but should not conflict with these names.
- **andy_bse_merged.csv** (Compustat-style merged file)
  - Explicitly loaded with columns: `permno`, `Date`, `ret`, `Market Capitalization`, `turn`, `company_name`, `co_nic_code`, `nse_dummy`, and `owner_gp_name`. The notebook also derives `nic_code_ini` from `co_nic_code` and filters on `penny`, so ensure those fields are present or derivable.
- **andy_bse_lookup.xlsx**
  - Auxiliary lookup sheet (`Sheet2`) used for exclusions when running with the Compustat-merged data option.

### Risk-free series
- **Jayant Verma Monthly.csv**
  - Should contain monthly risk-free returns with columns `date` (YYYY-MM format) and `RF_`. The notebook converts `date` to a month-end index for matching against portfolio returns.

Place all files in the repository root or adjust notebook paths accordingly. Ensure dates are parseable by pandas and numeric fields are stored as numbers (returns in percentages are converted to decimals within the notebook).

## Environment setup
- **Python**: 3.10+ recommended.
- **Core packages**: `pandas`, `numpy`, `scipy` (including `winsorize`), `matplotlib`, and `pandas.tseries.offsets` utilities. Install via:
  ```bash
  pip install pandas numpy scipy matplotlib
  ```
- Optional: Jupyter Notebook/Lab for interactive execution.

## Quick start
1. Install dependencies in a virtual environment: `python -m venv .venv && source .venv/bin/activate && pip install pandas numpy scipy matplotlib`.
2. Place the required CSV and Excel files in the repository root using the filenames noted in *Dataset requirements*.
3. Launch the notebook: `jupyter notebook "Quantitative Momentum Strategies.ipynb"`, then choose the data source (`ex` flag for BSE/NSE/Compustat) and run cells sequentially.
4. Future script/CLI usage: encapsulate notebook logic in a Python module that loads the same datasets from the project root and exposes parameters (e.g., exchange selection, liquidity filter, winsorization) via CLI flags; run with `python -m qms.cli --ex bse --liquidity liquid` once implemented.

