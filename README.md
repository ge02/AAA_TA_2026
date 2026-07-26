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

The analysis combines several public data sources:

- **Chicago Taxi Trips** ([Chicago Data Portal](https://data.cityofchicago.org/Transportation/Taxi-Trips-2024-/ajtu-isnz/about_data)) — ~6.8M raw trips from 2025, one
  trip per row with timestamps, spatial identifiers, fares and distances.
- **Weather** ([Open-Meteo API](https://open-meteo.com/en/docs)) — hourly Chicago weather (temperature,
  precipitation, sunshine).
- **Points of Interest** ([OpenStreetMap](https://www.openstreetmap.org/) / [Overpass API](https://wiki.openstreetmap.org/wiki/Overpass_API)) — nine categories
  (airports, stations, stadiums, restaurants, bars, hotels, hospitals,
  universities, attractions), aggregated into distance/density features.
- **Geometry** ([Chicago Data Portal](https://data.cityofchicago.org/Facilities-Geographic-Boundaries/Boundaries-Community-Areas-Map/cauq-8yn6)) — boundaries for Chicago's community areas and census tracts.

There are three ways to use this repo using different data sources:

1. **End to End Reproduction:** Run each notebook including all in `01_Preprocessing` this fetches all datasets fresh and directly from the sources and performs all cleaning an preprocessing steps. 
This process takes **several hours** due to many API calls and multiple Gigabytes of data downloading from the internet.

2. **Recommended:** Get all data sets including our final preprocessed data from Sciebo in the `data_parquet` folder: https://uni-koeln.sciebo.de/s/z5xgdGosd9y9c4e (Password: AAA2026) 
Put each data set in the repective folder or just replace everything with the sciebo data. The folder `data` in the repo route must keep the name. 
Then each notebook starting from `02_Descriptive_Analysis` runs using our preprocessed data.

3. **Fast check if everything compiles:** Just run all notebooks starting from `02_Descriptive_Analysis`, except `03_Predictive_Analysis` without adding any data sets. This method uses sampled datasets that are pushed to the git repository. This does not gurantee senseful outcomes of the notebooks, but serves as a quick check if all notebooks compile using a fraction of the dataset. (see `00_Sample_Data.ipynb`)

## Getting Started

The project is managed with [`uv`](https://github.com/astral-sh/uv) and rendered
with [Quarto](https://quarto.org/). Quick start:

```bash
uv sync
uv run python -m ipykernel install --user --name aaa-team-02 --display-name "Python (AAA Team 02)"
uv run quarto render report.qmd --to pdf
```

## Report

The reports relies on linked graphs from notebooks. To get the exact replication of our intended report we recommend to render it using the notebook outputs pushed to this git repo. If the notebooks are run using a different data sources (such as Sample Data), graphs render differently. If notebooks are not run to the end or if they are missing some outputs the report may not render correctly. 


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
  01_Preprocessing/             # Fetching, cleaning, merging, aggregation can be skipped if using Sciebo Data or sample data
  02_Descriptive_Analysis/      # Descriptive data analysis
  03_Predictive_Analysis/       # SVM and NN modeling
  04_Reinforcement_Learning/
data/                           # Datasets 
assets/                         # Figures used in the report
docs/                           # Rendered output (report.pdf)
```

## Team

Niklas Eichholz · Anthony Ge · Yannick Herrmann · Hendrik Mehl
