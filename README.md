# London Bike Sharing Analytics: Python ETL & Interactive Tableau Dashboard

[![Tableau Public](https://img.shields.io/badge/Tableau_Public-View_Dashboard-E97627?style=flat&logo=tableau)](https://public.tableau.com/app/profile/phyo.paing8212/viz/LondonBikeRideAnalysis_17881494251050/Dashboard1)
[![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data_Analysis-150458?style=flat&logo=pandas)](https://pandas.pydata.org/)

---

## Project Overview

* Live Dashboard: [View on Tableau Public](https://public.tableau.com/app/profile/phyo.paing8212/viz/LondonBikeRideAnalysis_17881494251050/Dashboard1)
* Tools and Technologies: Python (`pandas`, `openpyxl`), Microsoft Excel, Tableau Desktop / Public
* Dataset Scope: 17,414 hourly records tracking London bike-share journeys alongside localized weather metrics.
* Business Objective: Raw municipal bike-share feeds present cryptic numerical codes, unformatted metrics, and high short-term variance. This project implements an automated Python ETL pipeline to standardize and clean the data, feeding an interactive Tableau dashboard designed to distinguish underlying demand trajectories from day-to-day weather fluctuations and highlight operating sweet spots for fleet management.

---

## Key Metrics & Business Insights

* Dynamic Ride Tracking: Automatically computes total ride volume for any user-selected time segment via brush filtering (e.g., 4,547,359 rides between March 13, 2015, and August 6, 2015).
* Commuter "Sweet Spot": Highest ride density concentrates between 12.5°C and 22.4°C with gentle-to-moderate wind speeds under 21.4 kph.
* Weather & Seasonal Elasticity: Clear and scattered-cloud conditions account for the primary share of commuter volume, while adverse conditions (thunderstorms, snowfall) lead to sharp operational drop-offs.

---

## Data Pipeline & Technical Implementation

### 1. Data Engineering & Transformation (Python)
The preprocessing pipeline (`london_bikes.ipynb`) cleans and enriches the raw source data into an analytics-ready schema exported to `london_bikes_clean.xlsx` (Sheet: `'Data'`):

* Schema Standardization: Mapped technical abbreviations to intuitive business labels (`timestamp` -> `time`, `cnt` -> `count`, `t1` -> `temp_real_C`, `t2` -> `temp_feels_like_C`, `hum` -> `humidity_percent`, `wind_speed` -> `wind_speed_kph`, `weather_code` -> `weather`).
* Metric Normalization: Scaled integer humidity figures into decimal percentages (`humidity_percent / 100`) to enable native aggregation and formatting in BI tools.
* Categorical Decoding: Transformed coded integers into clear descriptive labels:
  * Seasons: `0.0` (Spring), `1.0` (Summer), `2.0` (Autumn), `3.0` (Winter).
  * Weather Codes: Decoded into 7 explicit states (`1.0`: Clear, `2.0`: Scattered clouds, `3.0`: Broken clouds, `4.0`: Cloudy, `7.0`: Rain, `10.0`: Rain with thunderstorm, `26.0`: Snowfall).

### 2. Business Intelligence & Visual Analytics (Tableau)
* Custom Parameter Controls: Built user-adjustable parameters to adjust moving average window durations (e.g., 30-day moving average), smoothing seasonality to isolate macro trends.
* Dynamic Time Brushing: Configured interactive dashboard filter actions on the timeline that instantly update the top KPI card and heat map dimensions.
* Two-Dimensional Climate Matrix: Developed a cross-tabulated heat map plotting real temperature (°C) against wind speed (kph) to evaluate ride volume distributions across varying conditions.
