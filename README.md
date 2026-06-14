# Predictive Policing in England and Wales: Forecasting Police Demand for Patrol Allocation

## Overview

This repository contains the implementation of a predictive policing framework designed to forecast crime demand and to allocate efficiently police patrols across England and Wales.

The framework predicts monthly crime demand at Lower Layer Super Output Area (LSOA) level using a HistGradientBoostingRegressor model. Predicted demand is then used to allocate patrol resources using weighted K-Medoids clustering and spatial patrol area generation.

## Data

The model uses:

* `street.parquet` – historical street-level crime data obtained from police.uk.
* `LSOA_2021_EW_BSC.geojson` – LSOA boundary dataset used for spatial visualisation.

The file `street.parquet` is not included in this repository because of its size (5.7 GB).

Place the required files in:

```text
data/street.parquet
data/LSOA_2021_EW_BSC.geojson
```

before running the notebook.

## Methodology

The workflow consists of:

1. Crime data loading and preprocessing.
2. Crime type grouping.
3. Monthly LSOA demand aggregation.
4. Feature engineering.
5. HistGradientBoostingRegressor training.
6. Model evaluation using MAE, RMSE and R².
7. Crime demand forecasting for 2027.
8. Hotspot identification.
9. Patrol resource matching.
10. Weighted K-Medoids patrol allocation.
11. Patrol area visualisation.
12. Interactive map generation.

## Crime Groups

### Police Officer Group

* Vehicle Crime
* Other Theft
* Robbery
* Public Order Offences
* Other Crime
* Burglary
* Violence and Sexual Offences

### PCSO and Police Officer Group

* Anti-Social Behaviour
* Shoplifting
* Bicycle Theft
* Theft from the Person
* Criminal Damage and Arson

## Model Performance

### Crime Group 1

* MAE: 2.58
* RMSE: 3.847
* R²: 0.866

### Crime Group 2

* MAE: 1.893
* RMSE: 3.464
* R²: 0.870

These results indicate that the model explains approximately 87% of the variation in crime demand.

## Running the Project

Install dependencies:

```bash
pip install -r requirements.txt
```

Open:

```bash
jupyter notebook pred_2027.ipynb
```

Run all notebook cells from top to bottom.

## Outputs

The notebook generates:

* Crime demand forecasts
* Hotspot datasets
* Interactive hotspot map
* Patrol allocation datasets
* Interactive allocation map

## Reproducibility

All preprocessing, modelling, evaluation, forecasting, hotspot selection, patrol allocation and visualisation steps are fully documented in the notebook.
