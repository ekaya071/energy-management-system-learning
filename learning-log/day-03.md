# Day 03 - EV1 Data Analysis: Timestamp Gap Investigation

## Objective

The goal of this task was to analyze the exported EV1 measurement data from the DMS system, understand the measurement intervals, identify possible gaps/missing data points, and prepare the dataset for further visualization and analysis.

---

# 1. Loading and Inspecting the Dataset

The exported CSV file was loaded using pandas.

```python
import pandas as pd

df = pd.read_csv("EV1_data.csv")
```

After loading the dataset, the structure of the dataframe was checked.

```python
df.head()
```

This command displays the first rows of the dataset and helps to understand:

* available columns
* timestamp format
* measurement values
* general data structure

The number of rows and columns was checked using:

```python
df.shape
```

---

# 2. Checking Available Columns

The available variables in the dataset were inspected using:

```python
df.columns
```

This command returns the names of all columns and helps identify available measurement signals for further visualization.

---

# 3. Checking Missing Values

Possible missing values were investigated using:

```python
df.isnull().sum()
```

This calculates the number of missing entries for each column.

The purpose was to distinguish between:

* missing values inside measurements
* missing time periods between measurements

---

# 4. Timestamp Preparation and Time Difference Calculation

The timestamp column was converted into datetime format:

```python
df["timestamp_utc"] = pd.to_datetime(df["timestamp_utc"])
```

To investigate measurement intervals, the difference between consecutive timestamps was calculated:

```python
df["time_diff_seconds"] = (
    df["timestamp_utc"].diff().dt.total_seconds()
)
```

Explanation:

* `.diff()` calculates the difference between consecutive timestamps.
* `.dt.total_seconds()` converts the time difference into seconds.

The created column represents the measurement interval between two consecutive data points.

Example:

| Timestamp Difference                       | time_diff_seconds |
| ------------------------------------------ | ----------------: |
| Previous measurement → Current measurement |       105 seconds |

---

# 5. Statistical Analysis of Measurement Intervals

Basic statistics were calculated to understand the distribution:

```python
df["time_diff_seconds"].describe()
```

The calculated values were:

| Metric             |         Value |
| ------------------ | ------------: |
| Minimum            |    0.013824 s |
| Maximum            | 3829.643461 s |
| Mean               |   52.494745 s |
| Standard deviation |   65.012004 s |
| Median             |   25.372508 s |

Additional statistics were calculated:

```python
df["time_diff_seconds"].median()
```

for the median value.

```python
df["time_diff_seconds"].std()
```

for the standard deviation.

The difference between mean and median indicates that some very large intervals influence the average value.

---

# 6. Sampling Interval Distribution Analysis

To understand the most common measurement intervals, the values were rounded and counted:

```python
df["time_diff_seconds"].round(0).value_counts().head(20)
```

Explanation:

* `round(0)` groups similar intervals by rounding to the nearest second.
* `value_counts()` counts how many times each interval occurs.
* `head(20)` shows the 20 most frequent intervals.

The most common intervals were:

| Interval | Occurrences |
| -------- | ----------: |
| 3 s      |        4669 |
| 105 s    |        1482 |
| 104 s    |        1039 |
| 107 s    |        1021 |
| 106 s    |         822 |

This indicates that the dataset contains multiple sampling modes instead of one fixed measurement interval.

---

# 7. Searching for Potential Gaps

Large time intervals were filtered using:

```python
df[df["time_diff_seconds"] > 500]
```

The purpose of this filtering was to find unusual measurement delays.

Detected potential gaps:

| Index | time_diff_seconds |
| ----- | ----------------: |
| 2268  |           3829.64 |
| 2684  |           1001.13 |
| 2685  |           1001.05 |
| 2686  |           1001.08 |
| 4161  |           1014.39 |
| 4162  |           1014.17 |
| 4163  |           1015.84 |
| 4164  |            612.53 |

---

# 8. Investigating Gap Locations

To understand whether these values represent real missing data, neighboring rows were inspected.

The index locations were stored:

```python
gap_indices = df[df["time_diff_seconds"] > 500].index
```

Each potential gap was investigated using:

```python
for idx in gap_indices:
    print(df.loc[idx-1:idx+1, 
    ["timestamp_utc", "time_diff_seconds"]])
```

Explanation:

* `idx` represents the dataframe index of the potential gap.
* `idx-1` shows the previous measurement.
* `idx+1` shows the following measurement.
* This helps determine the behavior before and after the gap.

---

# 9. Gap Interpretation

The interval at index 2268 showed:

```
~3 seconds
      ↓
3829 seconds
      ↓
~3 seconds
```

This behavior suggests a strong possibility of missing data.

However, the intervals around 1000 seconds appeared repeatedly:

```
1001 s → 1001 s → 1001 s
```

and

```
1014 s → 1014 s → 1015 s
```

Because these intervals occur consistently, they may represent another measurement mode rather than missing data.

Therefore, a simple rule such as:

```python
time_diff_seconds > threshold = gap
```

may create false detections.

---

# Next Steps

* Visualize available EV1 measurements using Python.
* Plot measurement signals over time.
* Mark identified potential gaps on the plots.
* Investigate possible gap filling methods for paper publication.

Possible approaches:

* Linear interpolation
* Forward fill
* Model-based estimation depending on the physical signal behavior
