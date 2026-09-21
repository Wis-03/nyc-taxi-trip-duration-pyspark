# Data

The dataset is **not** included in this repository (files exceed GitHub's
100 MB limit and raw data does not belong in a code repo).

## How to get it

This project uses the **New York City Taxi Trip Duration** dataset
(Kaggle competition).

1. Download from Kaggle: https://www.kaggle.com/c/nyc-taxi-trip-duration/data
2. Place the extracted files in this `data/` folder:

```
data/
├── train.csv
├── test.csv
└── sample_submission.csv
```

The notebook reads the CSVs from the working directory, so either run it
from `data/` or adjust the paths in the loading cell.

### Dataset summary
- `train.csv` — 1,458,644 trips with `trip_duration` (target, in seconds)
- `test.csv`  — trips to predict
- Columns: `id, vendor_id, pickup_datetime, dropoff_datetime,
  passenger_count, pickup/dropoff longitude & latitude,
  store_and_fwd_flag, trip_duration`
