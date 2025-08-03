# DataWrangling-Using-Polars
This project provides a comprehensive guide to analyzing weather data using **Polars**, a high-performance DataFrame library written in Rust. It leverages Polars' speed and efficiency to perform exploratory data analysis (EDA) on a real-world weather dataset for Toronto, Canada, spanning from February 2, 2000, to May 3, 2025.

## Table of Contents

- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Objectives](#objectives)
- [Setup](#setup)
- [Key Features](#key-features)

  - [Basic EDA Techniques](#basic-eda-techniques)
  - [Advanced EDA Techniques](#advanced-eda-techniques)
  - [Lazy Evaluation](#lazy-evaluation)
- [Results](#results)

## Project Overview

This project demonstrates how to use **Polars** to perform end-to-end data analysis on a weather dataset. It covers essential data manipulation tasks like filtering, grouping, joining, and visualizing data, while showcasing Polars' unique features such as lazy evaluation and its Expression API for efficient and readable code. The tutorial is hands-on, with clear code snippets and explanations, making it accessible for users new to Polars or data analysis in general.

The dataset is sourced from the [Open-Meteo Historical Weather API](https://open-meteo.com/) for Toronto, Canada, and includes hourly weather measurements such as temperature, precipitation, wind speed, snow depth, and solar radiation.

## Dataset

The dataset contains hourly weather data for Toronto, Canada (latitude: 43.7001, longitude: -79.4163) from February 2, 2000, to May 3, 2025, aligned to the America/New_York timezone. It includes the following columns:

- `date`: Timestamp of the observation
- `temperature_2m`: Temperature at 2 meters (°C)
- `apparent_temperature`: "Feels-like" temperature (°C)
- `precipitation`: Precipitation amount (mm)
- `wind_speed_10m`: Wind speed at 10 meters (km/h)
- `snow_depth`: Snow depth (m)
- `sunshine_duration`: Duration of sunshine (seconds)
- `direct_radiation`: Direct solar radiation (W/m²)
- `wind_gusts_10m`: Wind gusts at 10 meters (km/h)

The dataset is available as a CSV file hosted at:
`https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/cgBu1y94dmPAnw4HfZPeOg/hourly-weather-data.csv`

## Objectives

The project aims to :

- Load and preprocess a CSV dataset into a Polars DataFrame
- Handle missing values and outliers
- Filter rows and select columns based on conditions
- Create new columns through feature engineering
- Perform aggregations and grouping
- Join DataFrames
- Use advanced techniques like rolling averages, percent changes, ranking, and categorization
- Leverage Polars' lazy evaluation for optimized performance
- Understand and apply Polars' Expression API for chained transformations

## Setup

To run this project, you need Python 3.6+ and the following libraries:

- `polars`: For data manipulation
- (Optional) `matplotlib` and `seaborn` for visualization (not covered in this notebook but mentioned for potential extensions)

### Installation

Clone this repository and install the required dependencies:

```bash
git clone https://github.com/alyy10/weather-analysis-polars.git
cd weather-analysis-polars
pip install polars
```

### Running the Notebook

1. Ensure you have Jupyter installed (`pip install jupyter`).
2. Launch Jupyter Notebook:
   ```bash
   jupyter notebook
   ```
3. Open `Polars.ipynb` and run the cells sequentially to see the analysis in action.

## Key Features

The notebook is divided into sections covering basic and advanced EDA techniques, lazy evaluation, and exercises to reinforce learning.

### Basic EDA Techniques

- **Data Loading**: Loads the weather dataset into a Polars DataFrame and inspects its structure.
- **Handling Missing Data**: Identifies and removes null values (e.g., 76 nulls in `snow_depth`).
- **Outlier Detection**: Uses the Interquartile Range (IQR) method to filter outliers in `wind_gusts_10m`, reducing the dataset from 221,352 to 218,320 rows.
- **Filtering Rows**: Filters for extreme conditions, such as temperatures below -15°C (2,400 hours identified).
- **Selecting Columns**: Extracts specific columns like `date`, `temperature_2m`, `precipitation`, and `wind_gusts_10m` for focused analysis.

### Advanced EDA Techniques

- **Chained Transformations**: Combines multiple operations (e.g., creating a `feels_like_diff` column, filtering, and sorting) to identify days where the apparent temperature significantly differs from the actual temperature (180,392 rows where `feels_like_diff` > 0).
- **Time-Based Grouping**: Groups data by day to calculate daily averages for temperature, total precipitation, and sunshine duration (9,224 days summarized).
- **Rolling Window**: Computes a 3-hour rolling average for temperature to smooth out fluctuations.
- **Percent Change**: Calculates hour-to-hour percentage changes in wind gusts to detect sudden weather shifts.
- **Ranking**: Ranks hours by direct solar radiation to identify the sunniest periods.
- **Conditional Categorization**: Creates a `weather_type` column to label weather as "Rainy," "Snowy," "Hot," or "Clear" based on conditions.
- **Pivot-Style Aggregation**: Computes average temperature by hour of the day to reveal daily temperature cycles.

### Lazy Evaluation

Demonstrates Polars' lazy evaluation by constructing a query plan to filter temperatures above 25°C and selecting only `date` and `temperature_2m`. The query is optimized and executed only when `.collect()` is called, resulting in 8,043 rows of hot weather data.

## Results

Key outputs from the notebook include:

- **Dataset Overview**: The dataset has 221,352 rows and 9 columns.
- **Missing Data**: Only `snow_depth` has 76 null values, which are removed for clean analysis.
- **Outliers**: Filtering `wind_gusts_10m` outliers reduced the maximum gust from 119.88 km/h to 61.20 km/h.
- **Freezing Hours**: 2,400 hours had temperatures below -15°C.
- **Feels-Like Difference**: December 24, 2022, at 01:00 had the largest temperature vs. apparent temperature difference (11.07°C).
- **Daily Summary**: Daily averages show temperature, precipitation, and sunshine trends over the 25-year period.
- **Hourly Trends**: The warmest hour on average is around 20:00 (11.31°C), and the coldest is around 04:00 (7.41°C).
- **Lazy Evaluation**: Filtering for temperatures above 25°C efficiently returns 8,043 records.

For detailed results, refer to the output cells in `Polars-101.ipynb`.
