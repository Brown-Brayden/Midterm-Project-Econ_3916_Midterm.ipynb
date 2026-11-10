# Midterm-Project-Econ_3916_Midterm.ipynb
# Health Spending and Life Expectancy Across Countries, 2000–2022

ECON 3916 Midterm Project: Econometric Foundation

## Question

Across countries from 2000 to 2022, is higher health spending per person associated with longer life expectancy once income and urbanization are held constant?

## Headline finding

Holding income, urbanization and the year constant, **a doubling of health spending per person goes with 1.09 more years of life expectancy** (95% CI 0.37 to 1.80, standard errors clustered by country).

- **Without controls,** a doubling of spending goes with 2.89 more years.
- **Adding income cuts the spending coefficient by about half (52%).** Richer countries both spend more on health and live longer for other reasons.
- **Income matters more than the health budget:** doubling GDP per person goes with 2.75 more years.
- **The result is robust to outliers.** Dropping influential observations, or any single country, keeps the spending coefficient between 1.33 and 1.74, against 1.57 in the full sample.

This is a **conditional association, not a causal effect.** Omitted factors such as schooling, disease burden and the quality of government, plus reverse causality (longer-lived populations need more care), probably push it upward. The report explains why.

## Data

Source: World Bank, [World Development Indicators](https://databank.worldbank.org/source/world-development-indicators). I pulled the four series through the WDI API on 2026-11-09.

| Variable | WDI code | Unit |
|---|---|---|
| Life expectancy at birth (outcome) | `SP.DYN.LE00.IN` | years |
| Current health expenditure per person (main explanatory variable) | `SH.XPD.CHEX.PC.CD` | current US$ |
| GDP per person, PPP | `NY.GDP.PCAP.PP.KD` | constant international $ |
| Urban population | `SP.URB.TOTL.IN.ZS` | % of total population |

- **Coverage:** all 217 economies × 23 years = 4,991 country-years, with no regional or income-group aggregates. The analysis uses the 4,222 country-years from 186 countries that have all four variables.
- **Health spending origin:** the series comes from the WHO Global Health Expenditure Database.
- **Missing data:** the gaps are mostly small territories that aren't WHO members (Guam, Gibraltar, the Channel Islands and others), plus a few closed or conflict-affected economies (North Korea, Venezuela, South Sudan).
- **A data caveat:** the Central African Republic has implausibly low life expectancy values, including 14.7 years in 2009. They are kept in the sample and flagged, and the influence checks show they don't drive the result.

No data file is needed. The notebook downloads the series from the World Bank API when it runs. If you'd rather use a DataBank CSV export of the same four series, save it as `data/wdi_health_lifeexp_2000_2022.csv` and the notebook will read that instead.

## How to run

1. Open `Econ_3916_Midterm.ipynb` in Google Colab.
2. Choose *Runtime › Restart and run all*. The notebook needs an internet connection to download the data.
3. Everything prints top to bottom: tables, figures, regressions and diagnostics. The last code cell, *Numbers my report quotes*, prints every number used in the report. Figures are also saved as PNGs in a `figures/` folder.

It uses only packages Colab already has: `numpy`, `pandas`, `matplotlib`, `statsmodels` and `requests`.

## Method

The main model is OLS of life expectancy on log health spending per person, log GDP per person, urban share and a dummy for each year. Standard errors are clustered by country. Three simpler models build up to it, so the reader can see what each control does.

Diagnostics:
- residual plots;
- a Breusch–Pagan test, with heteroskedasticity-robust and clustered standard errors compared;
- variance inflation factors;
- Cook's distance, with a refit that drops one country at a time.

## Repository contents

| File | What it is |
|---|---|
| `Econ_3916_Midterm.ipynb` | The Colab notebook with all the analysis |
| `Econ_3916_Midterm_Report.pdf` | The written report (6–8 pages) |
| `README.md` | This file |
