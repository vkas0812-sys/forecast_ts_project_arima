# Time Series Analysis Notebook — Google Colab Guide

This README explains how to run **`01_time_series_project_colab.ipynb`** in **Google Colab** from start to finish. It uses a univariate time series with a datetime column and builds forecasting models such as Naïve, AR, MA, ARMA, ARIMA/SARIMA, optional Auto‑ARIMA, and a SARIMA→GARCH volatility add‑on. The notebook evaluates models on **80/20** and **90/10** chronological splits using RMSE and includes residual diagnostics and forecast plots.

## 1) Open in Colab
1. Go to https://colab.research.google.com
2. Choose **File → Upload notebook** and select `01_time_series_project_colab.ipynb`.
3. Click **Connect** to start a runtime.

## 2) Runtime and Python
- Colab’s default Python (>=3.9) works.
- GPU/TPU is not required. CPU is fine.

## 3) Install packages (run the setup cells)
In the notebook there are cells that install or pin dependencies. Run them in order before anything else:
- Upgrade `pip`.
- Install time‑series packages with compatible versions (e.g., `scipy`, `statsmodels`, `pmdarima`, `sktime`).
- Install `arch` for GARCH.
After the install cells finish, use **Runtime → Restart runtime** and then **Run all** from the top.

If you prefer manual installs, you can add a cell like:
```
!pip install --upgrade pip
!pip install --upgrade --force-reinstall scipy==1.11.4 statsmodels==0.14.2 pmdarima==2.0.4 sktime
!pip install arch
```
Then restart the runtime.

## 4) Prepare your dataset
The notebook expects a single time‑series table with:
- A date/time column named `date` (parseable to datetime).
- A numeric target column. If your column is named `qty`, the notebook will rename it to `Value` automatically.

Typical flow:
1. Place your file (e.g., `data giao hang update-2.xlsx`) on your local computer.
2. In Colab, run the **Upload** cell; a file chooser will appear.
3. The code will read it with `pd.read_excel(file_name)`. It converts `date` to datetime, sorts by time, and sets it as the index.

**Assumptions and tips**
- Use a consistent frequency (e.g., daily, weekly, or monthly). The notebook attempts to infer frequency when needed.
- Remove duplicate timestamps; ensure the series is numeric.
- If your column names differ, rename them accordingly before or after loading.

## 5) Run the analysis cells in order
1. **Setup & EDA**: library imports, environment checks, summary stats, line plots.
2. **Stationarity**: ADF and related checks; optional transformations (log/diff/Box‑Cox) if needed.
3. **Train–Test splits**: two scenarios — **80/20** and **90/10** (chronological, no shuffle).
4. **Baselines**: Naïve and (if present) seasonal‑naïve.
5. **Classical models**: AR, MA, ARMA, ARIMA with small grid searches and diagnostics.
6. **Auto‑ARIMA (optional)**: quick selection + residual diagnostics and forecast plot.
7. **SARIMA ranking**: try seasonal orders, compare AIC/AICc/BIC and test residuals.
8. **Diagnostics**: Ljung–Box, Jarque–Bera, ARCH LM, ACF/PACF, QQ plots, histograms.
9. **SARIMA → GARCH (optional)**: fit GARCH(1,1) on SARIMA residuals to model conditional volatility and compare predictive intervals.
10. **Comparison & conclusion**: RMSE tables for both splits and brief selection rationale.

## 6) Outputs you should expect
- Cleaned series preview and plots.
- ACF/PACF and stationarity test results.
- RMSE for each model on 80/20 and 90/10 splits.
- Auto‑ARIMA summary and quick forecast.
- SARIMA and SARIMA→GARCH residual diagnostics and forecast intervals.
- A final comparison table of top models based on RMSE (with AIC tie‑breaks).

## 7) Common issues and fixes
- **`ImportError` for `pmdarima` or `statsmodels`**: re‑run the install cell above and restart the runtime.
- **Version conflicts**: stick to the pinned versions shown in the setup cell.
- **Date parsing errors**: ensure your `date` column is in a standard format and unique per row.
- **Non‑numeric target**: convert to numeric and handle missing values before modeling.

## 8) Reproducibility
- The notebook uses deterministic splits based on time order.
- If you add stochastic models (e.g., neural networks), set seeds for full reproducibility.

## 9) Folder structure (suggested)
```
/project-root
├── 01_time_series_project_colab.ipynb
├── data/                       # your time-series files (optional)
└── outputs/                    # saved figures or tables (optional)
```

## 10) License and credits
Use this notebook freely for study and internal projects. Cite libraries as appropriate.
