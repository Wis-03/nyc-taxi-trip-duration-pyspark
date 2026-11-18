# Data

The Kaggle dataset is not included in this repository. Download it from the [NYC Taxi Trip Duration competition](https://www.kaggle.com/c/nyc-taxi-trip-duration/data), extract the CSV files, and place them in this folder:

```text
data/
├── train.csv
├── test.csv
└── sample_submission.csv
```

The training file contains 1,458,644 trips and the `trip_duration` target in seconds. The test file is used to generate predictions; the sample submission documents Kaggle's expected output format.

From the repository root, download and extract the Kaggle files into `data/`, install the requirements, and open `notebooks/nyc_taxi_trip_duration.ipynb`. The notebook locates `train.csv` and `test.csv` automatically when the kernel starts in the repository root, `notebooks/`, or `data/`. It does not require a machine-specific path.
