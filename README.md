# London Bike Rides Analytics: Python & Interactive Tableau Dashboard

[![Tableau Public](https://img.shields.io/badge/Tableau_Public-View_Dashboard-E97627?style=flat&logo=tableau)](https://public.tableau.com/app/profile/phyo.paing8212/viz/LondonBikeRideAnalysis_17881494251050/Dashboard1)
[![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data_Analysis-150458?style=flat&logo=pandas)](https://pandas.pydata.org/)

---

## Project Overview

* Live Dashboard: [View on Tableau Public](https://public.tableau.com/app/profile/phyo.paing8212/viz/LondonBikeRideAnalysis_17881494251050/Dashboard1)
* Tools and Technologies: Python (pandas), Microsoft Excel, Tableau Desktop / Public
* Dataset Scope: 17,414 hourly records of London bike-share rides alongside weather measurements, from 4 January 2015 to 3 January 2017 (19,905,972 rides in total).
* Source: *[add the original dataset link here]*

---

## Business Problem

A bike-share operator needs to know when and in what conditions riders use the bikes, so the fleet can be placed, staffed and serviced at the right times. The raw feed is hard to use as it comes: columns have cryptic names, weather and season are stored as numeric codes, and daily ride counts swing widely. This project cleans the data with Python and presents it in a Tableau dashboard that separates the underlying demand trend from day-to-day weather noise.

---

## Business Questions

| # | Business question | Tableau view |
|---|---|---|
| 1 | How does ride demand change over time once daily noise is smoothed out? | Moving Average |
| 2 | How many rides happen in any period I select? | Total Rides |
| 3 | In which temperature and wind conditions do most rides happen? | Heat Map |
| 4 | At what hours of the day is demand highest? | Hour |
| 5 | How does the weather affect ride volume? | Weather |

---

## Key Insights

1. **Demand follows a strong seasonal cycle.** With the default 30-day window, the moving average of daily rides peaks at about 39,200 on 16 August 2016 and falls to about 16,200 on 17 January 2016 (both after the first 30 days of data).
2. **Rides cluster around the commute hours.** The busiest hours are 08:00 (2.09 million rides), 17:00 (2.06 million) and 18:00 (1.91 million). Together they carry 6.06 million of the 19.9 million rides, which is 30.4%. The quietest hour is 04:00 (52,859 rides).
3. **Most rides happen in mild, light-wind conditions.** In the heat map, 39.9% of all rides fall in hours between 12.5 °C and 22.4 °C with wind below 21.4 kph. Every one of the eight busiest cells has wind below 21.4 kph.
4. **Fair weather carries most of the volume, and bad weather cuts demand sharply.** Clear (7.15 million rides) and scattered clouds (6.04 million) make up 66.2% of all rides. On average, an hour with scattered clouds sees 1,496 rides, against 713 in rain, 583 in a thunderstorm and 251 in snowfall (the overall average is 1,143 per hour).

---

## Recommendations

1. **Have bikes and staff ready before the 08:00 and 17:00 to 18:00 peaks.** About 30% of all rides happen in those three hours.
2. **Schedule servicing and fleet maintenance for the low-demand winter months.** The 30-day average drops to about 16,200 rides a day in January, against about 39,200 in August.
3. **Use the weather forecast to adjust fleet deployment.** Average hourly rides fall to about two-thirds of the overall average in rain, about half in thunderstorms and about a fifth in snow.

---

## Dashboard Design

| View | Chart | What it shows |
|---|---|---|
| Total Rides | Number card | Sum of rides between the selected start and end dates |
| Moving Average | Line chart | Moving average of rides, with the selected period highlighted |
| Heat Map | Heat map | Rides by temperature (bins of 2.49 °C) and wind speed (bins of 3.57 kph) for the selected period |
| Hour | Bar chart | Rides by hour of day |
| Weather | Bar chart | Rides by weather condition |

* **Custom parameters:** The moving average window is adjustable. Period can be day, week or month, and duration defaults to 30.
* **Dynamic time brushing:** Selecting a period on the timeline updates the Total Rides card and the heat map. For example, 13 March 2015 to 6 August 2015 gives 4,547,359 rides.

---

## Data Pipeline & Technical Implementation

### 1. Data Engineering & Transformation (Python)

The preprocessing notebook (`london_bikes.ipynb`) reads the raw file `london_merged.csv` and exports an analytics-ready table to `london_bikes_clean.xlsx` (sheet `Data`):

* Schema Standardization: Mapped technical abbreviations to intuitive business labels (`timestamp` -> `time`, `cnt` -> `count`, `t1` -> `temp_real_C`, `t2` -> `temp_feels_like_C`, `hum` -> `humidity_percent`, `wind_speed` -> `wind_speed_kph`, `weather_code` -> `weather`).
* Metric Normalization: Scaled humidity into decimal percentages (`humidity_percent / 100`) so BI tools can aggregate and format it.
* Categorical Decoding: Transformed coded integers into clear descriptive labels:
  * Seasons: `0.0` (spring), `1.0` (summer), `2.0` (autumn), `3.0` (winter).
  * Weather Codes: Decoded into 7 states (`1.0`: Clear, `2.0`: Scattered clouds, `3.0`: Broken clouds, `4.0`: Cloudy, `7.0`: Rain, `10.0`: Rain with thunderstorm, `26.0`: Snowfall).

### 2. Business Intelligence & Visual Analytics (Tableau)

* Moving average built with a table calculation (`WINDOW_AVG`) driven by two parameters.
* Timeline selection feeds a date-range calculation, so the card and heat map respond to the chosen period.
* Heat map of temperature against wind speed, with ride volume shown as color and label.

---

## Data Notes

* **The data has gaps.** There are 17,414 hourly records against 17,544 possible hours in the period, so 130 hours are missing. The last record is 3 January 2017, so January 2017 is a partial month.
* **The heat map shows where most rides happen, which also reflects how often each condition occurs.** Average rides per hour keep rising with temperature, from about 555 below 2.5 °C to between 2,418 and 2,853 above 22.4 °C. Mild hours dominate the totals because about three-quarters of the hours in the data fall between 7.5 °C and 22.4 °C.
* **Rare weather types have small totals.** Snowfall has 60 hourly records and rain with thunderstorm has 14, so their low totals partly reflect how seldom they occur.

---

## How to Open

1. Open `london_bike_rides.twbx` in Tableau Desktop, or use the Tableau Public link above. The packaged workbook includes its data extract.
2. To rerun the cleaning, open `london_bikes.ipynb` and update the file paths at the top and bottom, which currently point to the author's computer.

---

## Repository Contents

| File | Description |
|---|---|
| `london_merged.csv` | Raw source data |
| `london_bikes.ipynb` | Python cleaning notebook |
| `london_bikes_clean.xlsx` | Cleaned data used by the dashboard (sheet `Data`) |
| `london_bike_rides.twbx` | Packaged Tableau workbook |
| `README.md` | Project documentation |
