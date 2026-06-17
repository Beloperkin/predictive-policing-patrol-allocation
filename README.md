# Predictive Policing in England and Wales: Forecasting Police Demand and Efficient Patrol Allocation

## Project Overview

This repository contains the implementation of a predictive policing framework designed to forecast crime demand and support police patrol allocation across England and Wales.

The project uses historical police-recorded crime data to predict monthly crime demand at Lower Layer Super Output Area (LSOA) level for the year 2027. Predicted demand is then used to identify crime hotspots and allocate patrol resources using spatial clustering techniques.

---

## Scope of This Repository

This notebook contains the implementation for the **Police Officer crime group**, which includes:

* Vehicle Crime
* Other Theft
* Robbery
* Public Order Offences
* Other Crime
* Burglary
* Violence and Sexual Offences

These crime types are aggregated into a single demand measure that is predicted and used for patrol allocation.

---

## Data

The project requires the following datasets:

### Crime Dataset

* `street.parquet`

This file contains historical street-level crime data for England and Wales obtained from the UK Police open data platform.

The file is **not included in this repository** because its size is **5.7 GB**.

The file can be downloaded from the following OneDrive link: https://tuenl-my.sharepoint.com/:u:/g/personal/m_beloperkin_student_tue_nl/IQBfyqiWLDgxRa-J6hNC6m0wAUYN8s7iLxfWFIQ_IExngOo?e=C0asWJ

### Boundary Dataset

* `LSOA_2021_EW_BSC.geojson`

This file contains the geographic boundaries of LSOAs and is used for spatial visualisation and patrol area generation.

Place the files in:

```text
data/street.parquet
data/LSOA_2021_EW_BSC.geojson
```

before running the notebook.

---

## Methodology

The framework consists of several stages:

### 1. Data Loading and Preprocessing

Historical crime records are loaded and filtered to retain only the crime types assigned to the Police Officer group.

### 2. Monthly Demand Aggregation

Crime records are aggregated by:

* Month
* LSOA

This produces a monthly crime demand count for each LSOA.

### 3. Feature Engineering

Several predictive features are created, including:

* Month and year variables
* Lagged demand values
* Rolling averages
* Rolling standard deviations
* Long-term demand averages
* Spatial coordinates

These features provide information about temporal trends and historical crime behaviour.

### 4. Crime Demand Forecasting

A **HistGradientBoostingRegressor** model is trained to predict future crime demand.

The model was selected because it:

* Handles large tabular datasets efficiently
* Captures non-linear relationships
* Provides strong predictive performance
* Is computationally efficient for large-scale forecasting

### 5. Hotspot Identification

For each forecast month in 2027:

* LSOAs are ranked by predicted demand
* The top 30% highest-demand locations are selected
* Only locations with predicted demand greater than zero are retained

These locations are treated as predicted crime hotspots.

### 6. Patrol Type Classification using DBSCAN

DBSCAN is used to distinguish between:

* Dense hotspot clusters
* Isolated hotspot locations

Dense hotspot areas are considered more suitable for **bike patrols** because travel distances are shorter.

More isolated hotspot areas are assigned to **car patrols** because they require faster travel across larger distances.

### 7. Weighted K-Medoids Patrol Allocation

After patrol type classification, weighted K-Medoids is used to allocate patrol centres.

Unlike K-Means, K-Medoids selects **real hotspot locations** as patrol centres instead of creating artificial average coordinates.

Predicted crime demand is used as a weight, ensuring that high-demand areas have greater influence on patrol centre placement.

### 8. Spatial Visualisation

The final patrol allocation is visualised using:

* LSOA boundaries
* Patrol centre markers
* Interactive Folium maps

These maps allow users to explore monthly allocation results across England and Wales.

---

## Model Evaluation

The forecasting model is evaluated using:

### Mean Absolute Error (MAE)

Measures the average prediction error.

### Root Mean Squared Error (RMSE)

Measures prediction error while giving greater importance to larger mistakes.

### Coefficient of Determination (R²)

Measures how much variation in crime demand is explained by the model.

### Results

#### Police Officer Crime Group

| Metric | Value |
| ------ | ----- |
| MAE    | 2.58  |
| RMSE   | 3.847 |
| R²     | 0.866 |

An R² value of 0.866 indicates that approximately 86.6% of the variation in crime demand is explained by the model.

---

## Repository Structure

```text
predictive-policing-patrol-allocation/
│
├── README.md
├── requirements.txt
├── prediction_and_allocation_2027.ipynb
│
├── data/
│   └── LSOA_2021_EW_BSC.geojson
│
└── outputs/
    └── .gitkeep
```

---

## Installation

Install the required Python packages:

```bash
pip install -r requirements.txt
```

---

## Running the Notebook

1. Place the required datasets inside the `data` folder.
2. Open the notebook:

```bash
jupyter notebook prediction_and_allocation_2027.ipynb
```

3. Run all notebook cells from top to bottom.

---

## Outputs

The notebook generates:

* Forecasted crime demand datasets
* Hotspot datasets
* Patrol allocation datasets
* Interactive HTML hotspot maps
* Interactive HTML patrol allocation maps

Output files are stored in the `outputs` folder.

---

## Reproducibility

All preprocessing, feature engineering, model training, evaluation, forecasting, hotspot selection, DBSCAN patrol classification, weighted K-Medoids allocation, and visualisation steps are documented within the notebook.

Running the notebook with the required input files reproduces the complete workflow described in the accompanying report.
