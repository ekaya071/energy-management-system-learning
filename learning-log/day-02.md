# Day 02 - EV1 Data Export and Initial Gap Analysis

## Objective

The goal of this task is to analyze vehicle measurement data exported from the DMS (Data Management System), identify possible data gaps, and prepare the dataset for further analysis.

---

## 1. DMS Data Export

The provided script: 60_Measurement_Data/00_Continuous_Logging_DMS/unified_vehicle_booking_dataset.py 
was executed successfully.

Generated CSV file: EV1_20260714_000000_20260721_000000.csv
  
The exported dataset contains:

- 11,519 measurements
- 16 attributes

---

## 2. Initial Data Analysis with Python

Created a Python analysis script: 02_python_scripts/analyze_EV1_data.py
  
Used pandas to load and inspect the CSV file.

```python
import pandas as pd

df = pd.read_csv(file_path)
df.head()
df.shape
df.columns 
Results:

Checked first rows of the dataset
Verified number of rows and columns
Reviewed available attributes

  df.isnull().sum()
  Result:

No missing values were detected in the exported CSV.
  The goal was to investigate whether measurement timestamps have constant intervals.

Convert timestamps

The timestamp column was initially read as a string.

Converted it into datetime format: df["timestamp_utc"] = pd.to_datetime(df["timestamp_utc"])
  This allows time calculations.

Calculate measurement intervals

Calculated the difference between consecutive measurements:
time_differences = df["timestamp_utc"].diff()
  Converted the time differences into seconds:
time_differences_seconds = time_differences.dt.total_seconds()
  time_differences_seconds.mean()
time_differences_seconds.min()
time_differences_seconds.max()
  Results:

Average interval: ~52.5 seconds
Minimum interval: ~0.014 seconds
Maximum interval: ~3829 seconds (~64 minutes)

The measurement intervals are not constant.

Observed patterns:

Frequent intervals around 2–3 seconds
Repeated intervals around 107–115 seconds
One large gap candidate of approximately 64 minutes

Further investigation is needed to determine whether these gaps are actual missing data points or expected logging behavior.
