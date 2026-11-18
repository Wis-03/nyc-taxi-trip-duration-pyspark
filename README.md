# NYC Taxi Trip Duration — PySpark

Predicting the duration of New York City taxi trips at scale with **Apache Spark (PySpark)**:
distributed data cleaning, geospatial & temporal feature engineering, and a comparison of
three regression models on ~1.46 M trips.

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![PySpark](https://img.shields.io/badge/Apache%20Spark-MLlib-orange)
![License](https://img.shields.io/badge/License-MIT-green)

---

## Problem

Given a taxi trip's origin, destination, timestamp and metadata, predict its **duration**
(in seconds). Based on the Kaggle *New York City Taxi Trip Duration* competition
(training set: **1,458,644** trips).

## Approach

The full pipeline is in [`notebooks/nyc_taxi_trip_duration.ipynb`](notebooks/nyc_taxi_trip_duration.ipynb).

1. **EDA** — distributions, correlations, and outlier discovery
   (e.g. a 979-hour trip; durations range from 1 s to 3.5 M s).
2. **Cleaning** — remove duration and coordinate outliers, drop temporally
   incoherent trips (dropoff before pickup).
3. **Feature engineering**
   - *Geospatial*: Haversine distance, Manhattan distance, bearing/direction,
     proximity to JFK and LaGuardia airports.
   - *Temporal*: pickup hour, day of week, month, weekend flag, rush-hour flag.
   - `store_and_fwd_flag` encoded to binary.
4. **Target transform** — model `log(trip_duration)` to handle right-skew,
   then exponentiate predictions back to seconds.
5. **ML pipeline** — `VectorAssembler` → `StandardScaler` → regressor,
   with **train/validation split** and **`CrossValidator`** for hyperparameter tuning.
6. **Models compared** — Linear Regression, Random Forest, Gradient-Boosted Trees.
7. **Evaluation** — RMSE, MAE and R² (`RegressionEvaluator`), plus a test-set
   submission in Kaggle format.

## Results

Metrics on the validation set (predictions back-transformed to seconds):

| Model | RMSE (s) | MAE (s) | R² |
|---|---|---|---|
| Linear Regression | 483 | 316 | 0.445 |
| **Best model (tuned tree-based)** | **≈ 341** | **≈ 214** | **0.724** |

The tuned tree-based model reduces validation **RMSE by approximately 29%**
(from 483 s to ≈ 341 s) and **MAE by approximately 32%** (from 316 s to ≈ 214 s)
compared with the linear baseline. Geospatial features, including distance and
proximity to airports, provide important predictive signal
(distance ↔ duration correlation ≈ 0.77 in the project analysis).

## Tech stack

`PySpark` (Spark SQL + MLlib) · `pandas` · `NumPy` · `matplotlib` · `seaborn` · `folium`

## Repository structure

```
nyc-taxi-trip-duration-pyspark/
├── notebooks/
│   └── nyc_taxi_trip_duration.ipynb   # full pipeline (EDA → features → models)
├── data/
│   └── README.md                      # how to download the dataset (not versioned)
├── requirements.txt
├── .gitignore
└── README.md
```

## Getting started

```bash
git clone https://github.com/Wis-03/nyc-taxi-trip-duration-pyspark.git
cd nyc-taxi-trip-duration-pyspark

python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# Download the data into data/  (see data/README.md), then:
jupyter notebook notebooks/nyc_taxi_trip_duration.ipynb
```

> **Reproducibility note:** Results above are reported from the project's validation run;
> training outcomes may vary by environment and split. Follow the notebook to reproduce
> the preprocessing, feature engineering, cross-validation, and evaluation.
>
> Interactive folium maps in the notebook were stripped from the committed
> outputs to keep it lightweight — re-run those cells to regenerate them.

## License

Released under the MIT License — see [`LICENSE`](LICENSE).
