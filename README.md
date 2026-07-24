# Forecasting Ride-Hailing Demand & Smart EV Charging

**Advanced Analytics and Applications — Team 02**

This repository contains the full data science project for Team 02: an end-to-end
analysis that helps a car manufacturer enter the US ride-hailing market with a
fully electrified fleet. Using historical **Chicago taxi trips** as a proxy for
ride-hailing demand, we build a pipeline that goes from raw data to demand
forecasts and an intelligent EV charging strategy.

## Motivation

Operating an electric ride-hailing fleet in a city comes down to one recurring
question: **where and when will how much demand occur?** Charging schedules,
vehicle rebalancing, and infrastructure investment all depend on being able to
anticipate demand. Without a reliable forecast, every operational decision is
guesswork. The project tackles this in three stages:

1. **Descriptive analysis** — how taxi demand varies across space and time.
2. **Predictive modeling** — forecasting trip demand per spatio-temporal cell
   using Support Vector Machines and Neural Networks.
3. **Reinforcement learning** — an EV charging agent that learns a cost-minimizing
   home-charging strategy under uncertain next-day energy demand.

## Data

The analysis combines several public data sources (see [data/README.md](data/README.md)
for download instructions — the full datasets live on sciebo, sample data ships
with the repo):

- **Chicago Taxi Trips** ([Chicago Data Portal](https://data.cityofchicago.org/Transportation/Taxi-Trips-2024-/ajtu-isnz/about_data)) — ~6.8M raw trips from 2025, one
  trip per row with timestamps, spatial identifiers, fares and distances.
- **Weather** ([Open-Meteo API](https://open-meteo.com/en/docs)) — hourly Chicago weather (temperature,
  precipitation, sunshine).
- **Points of Interest** ([OpenStreetMap](https://www.openstreetmap.org/) / [Overpass API](https://wiki.openstreetmap.org/wiki/Overpass_API)) — nine categories
  (airports, stations, stadiums, restaurants, bars, hotels, hospitals,
  universities, attractions), aggregated into distance/density features.
- **Geometry** ([Chicago Data Portal](https://data.cityofchicago.org/Facilities-Geographic-Boundaries/Boundaries-Community-Areas-Map/cauq-8yn6)) — boundaries for Chicago's community areas and census tracts.

Trips are cleaned, spatially resolved (community area, census tract, and H3
hexagons at resolutions 6 & 7), merged with weather, and aggregated into
**12 demand datasets** spanning four temporal (1h, 2h, 6h, 24h) and three
spatial resolutions, enriched with temporal, weather and POI features.

## Methods

| Stage | Approach |
|-------|----------|
| Descriptive | Spatial & temporal demand analysis, kernel density estimation, POI/weather visualizations |
| Predictive | Support Vector Machines and Neural Networks across spatio-temporal resolutions, compared on MAE and R² |
| Reinforcement Learning | Monte Carlo control & Q-learning vs. an exact dynamic-programming optimum for EV home charging |

## Repository Structure

```
report.qmd                      # Main Quarto report that stitches the sections together
sections/                       # Report text, one file per chapter (problem → conclusion)
notebooks/                      # All analysis, organized by stage:
  01_Preprocessing/             # Fetching, cleaning, merging, aggregation
  02_Descriptive_Analysis/      # Descriptive data analysis
  03_Predictive_Analysis/       # SVM (and NN) modeling
  04_Reinforcement_Learning/
data/                           # Datasets (download from sciebo; samples included)
assets/                         # Figures used in the report
docs/                           # Rendered output (report.pdf)
```

## Getting Started

The project is managed with [`uv`](https://github.com/astral-sh/uv) and rendered
with [Quarto](https://quarto.org/). Quick start:

```bash
uv sync
uv run python -m ipykernel install --user --name aaa-team-02 --display-name "Python (AAA Team 02)"
uv run quarto render report.qmd --to pdf
```

**Note:**
Note: A small sample of the cleaned data is included in `data/samples` so you can verify the notebooks execute without downloading the full datasets. Just update the path in each notebook's Load Data cell to point at `data/samples`.

For a full run on the entirety of the cleaned data, please download the datasets from [Sciebo (Password: 'AAA2026')](https://uni-koeln.sciebo.de/s/z5xgdGosd9y9c4e) and copy the content of the directory `data_parquet` into `data/` to reproduce the notebooks;
without them, the included sample data is used automatically (except for the model notebooks).

For full details on the toolchain (Quarto, LaTeX, kernels, embedding notebook
cells, rendering formats), see [README_QUARTO.md](README_QUARTO.md).

## Team

Niklas Eichholz · Anthony Ge · Yannick Herrmann · Hendrik Mehl
