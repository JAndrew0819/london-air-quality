# Mini-Project 1: Air Quality Analysis



This project investigates whether London's air quality improves on weekends compared to weekdays, using hourly pollution data from the OpenWeather API covering late 2020 to end of 2025.

## Project Overview

The research question is: does London's air clean up on weekends? The project collects hourly air pollution data for central London, transforms it into a clean tabular format with weekday/weekend labels, and produces two analytical insights addressing different aspects of the question.

I chose to collect all available data from the API (27 November 2020 onwards) to maximise the number of weekday-weekend comparisons. This includes the COVID lockdown period (2020-2021), which is noted as a limitation in NB03. The location is central London (51.5074, -0.1278), roughly around Westminster, chosen as a reasonable proxy for "London's air." All eight available pollutants (CO, NO, NO2, O3, SO2, PM2.5, PM10, NH3) were kept through NB02 so NB03 could compare which ones differ most. Weekend was defined as Saturday and Sunday only (pandas `dayofweek >= 5`), excluding Friday evening and public holidays for simplicity. Hourly granularity was preserved in the CSV so NB03 could do both overall and hourly analysis. For categories, I used OpenWeather's built-in AQI (1=Good to 5=Very Poor) rather than custom thresholds, since custom levels would require citing official government and scientific standards for each pollutant and the built-in AQI provides a good and grounded proxy for the air quality.

## How to Run

Packages needed: `requests`, `python-dotenv`, `pandas`, `numpy`, `matplotlib`, `seaborn`, `jinja2` (Python 3.10+).

1. Clone the repository.
2. Create a `.env` file in the repository root with your OpenWeather API key:
   ```
   API_KEY=your_api_key_here
   ```
   Free keys are available at [OpenWeather](https://home.openweathermap.org/users/sign_up).
3. Run the notebooks in order from the `notebooks/` folder: NB01 then NB02 then NB03. Each reads the output of the previous one.
4. The `.env` file is `.gitignore`-d and will not be committed.

The `data/` folder already contains the collected data, so you can skip NB01 and start from NB02 if you do not have an API key. NB03 can also be run directly from the saved CSV.

Repository structure:

```
├── data/
│   ├── london_air_pollution_2020_2025.json
│   └── london_air_quality_2020_2025.csv
├── figures/
│   ├── no2-hourly-weekday-vs-weekend.png
│   └── no2-hourly-weekday-vs-weekend.svg
├── notebooks/
│   ├── NB01-Data-Collection.ipynb
│   ├── NB02-Data-Transformation.ipynb
│   └── NB03-Data-Analysis.ipynb
├── README.md
└── STUDY-CONSENT.md
```

## Data Sources

Hourly air pollution data was collected from the [OpenWeather Air Pollution API](https://openweathermap.org/api/air-pollution), specifically the `/air_pollution/history` endpoint. Each hourly record includes the Air Quality Index (AQI, 1-5 scale) and concentrations for eight pollutants: CO, NO, NO2, O3, SO2, PM2.5, PM10, and NH3. The data covers central London (51.5074, -0.1278) from 27 November 2020 to late December 2025, totalling approximately 44,000 hourly observations.

## Results

London's air does clean up on weekends, but the effect depends on the pollutant and the time of day.

The styled DataFrame comparison shows that traffic-linked gases have clear weekend drops: NO falls 23%, SO2 17%, and NO2 13%. However, particulate matter and CO barely change, suggesting these come from non-traffic sources. Ozone rises 5% on weekends because less NO from traffic means less ozone gets destroyed. The overall AQI barely shifts (1.65 vs 1.63), staying in the "Good" band on both days.

The hourly NO2 line plot reveals that not all commuting hours are equal, the evening rush (7-8pm) creates a gap of about 5 µg/m³ between weekdays and weekends, nearly double the morning rush gap of 3 µg/m³. By midnight the two lines converge completely, confirming the difference is driven by traffic rather than other sources.
