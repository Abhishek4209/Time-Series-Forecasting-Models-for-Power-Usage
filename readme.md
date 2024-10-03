Here's a well-structured and organized version of the `README.md` file for your multi-step LSTM time-series forecasting project on power usage:

---

# Multi-Step LSTM Time-Series Forecasting Models for Power Usage

## 1. Project Overview

This project aims to forecast daily household power consumption for the next 7 days using a multi-step Long Short-Term Memory (LSTM) model. The time-series data, collected from a household in France, is used to build and compare both univariate and multivariate LSTM models for future power consumption predictions.

## 2. Data Source

The dataset is sourced from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/individual+household+electric+power+consumption), consisting of household electric power consumption recorded at one-minute intervals over four years.

## 3. Data Description

The dataset comprises 2,075,259 observations with the following variables:

| Variable                | Description                                                          |
|-------------------------|----------------------------------------------------------------------|
| `Datetime`              | Timestamp (yyyy-mm-dd hh:mm:ss)                                       |
| `Global_active_power`    | Total active power consumed by the household (kW)                     |
| `Global_reactive_power`  | Total reactive power consumed by the household (kW)                   |
| `Voltage`               | Average voltage (V)                                                   |
| `Global_intensity`       | Average current intensity (A)                                         |
| `Sub_metering_1`         | Active energy consumed by the kitchen (Wh)                            |
| `Sub_metering_2`         | Active energy consumed by the laundry (Wh)                            |
| `Sub_metering_3`         | Active energy consumed by the climate control systems (Wh)            |
| `Sub_metering_4`         | Active energy consumed (Wh), calculated as the difference between the total energy consumption and the sum of the other sub meters |

## 4. Business Problem

**Objective:** Forecast household power consumption for the upcoming 7 days based on historical data.

## 5. Exploratory Data Analysis (EDA)

### 5.1 Initial Data Exploration

- The dataset consists of 8 features (including the timestamp) with 2,075,259 observations.
- Column names were cleaned and standardized (e.g., lowercase conversion).
- Missing values (`?`) were replaced with NaN and imputed using the previous day's value at the same time.
- A new feature `sub_metering_4` was calculated using the formula:  
  `sub_metering_4 = global_active_power * 1000 / 60 - (sub_metering_1 + sub_metering_2 + sub_metering_3)`
- The dataset covers 1441 days (from 2006-12-16 to 2010-11-26).

### 5.2 Data Visualization

#### Histogram Analysis:
- The target variable `global_active_power` (converted to daily consumption) shows a near-normal distribution with a slight positive skew.

#### Time Series Analysis:
- The target variable, `global_active_power`, shows seasonality, with some variability in variance.  
- The time series is stationary, but the first 5 months were excluded due to potential noise.

#### Correlation Analysis:
- A correlation heatmap revealed high multicollinearity.  
- The feature `global_intensity` was dropped due to strong correlation with other features.

### 5.3 Test for Stationarity

- The Augmented Dickey-Fuller (ADF) test confirmed that the time series is stationary.

## 6. Time-Series Forecasting Models

### 6.1 Data Preprocessing

- The dataset was split into a 75% training set (1082 days) and a 25% test set (360 days).
- Data was scaled using the mean and standard deviation from the training set.
- A sliding window approach was used to create input-output sequences:  
  - 14 days of past observations were used to forecast the next 7 days (i.e., each sample consists of 14 time steps for input X and 7 time steps for output y).
  - This resulted in 1062 training samples and 340 test samples.

### 6.2 Model Types

Four LSTM models were built:

#### 1. Univariate Models
   - Predict based on the `global_active_power` feature only.

#### 2. Multivariate Models
   - Use multiple features for prediction, including `sub_metering_1`, `sub_metering_2`, and `sub_metering_3`.

For each analysis type (univariate/multivariate), two LSTM architectures were implemented:

- **Vector Output Model (Stacked LSTM):** This model produces a direct multi-step forecast using a sequence of LSTM layers.
- **Encoder-Decoder Model:** This model uses an encoder to summarize past observations and a decoder to predict future time steps.

### 6.3 Model Training

- The models were trained with a loss function to minimize prediction error, and learning curves were plotted to observe the training and validation loss over time.
- Number of nodes were adjusted to reduce overfitting.

### 6.4 Model Evaluation

- Model performance was evaluated using the **Root Mean Squared Error (RMSE)** metric, which measures the deviation between predicted and actual values.
- Univariate models slightly outperformed multivariate models under the given configurations, but **model hyperparameters were not fully tuned**.

### 6.5 Forecasting Results

- Forecasts for the test set were plotted against actual data to assess the model's prediction accuracy.
  
## 7. Conclusion

- LSTM models successfully forecast power consumption for the next 7 days, with univariate models performing better under the current setup.
- Future improvements could include model hyperparameter tuning, experimenting with different architectures, and incorporating external variables like weather data to improve forecasting accuracy.

---

### Dependencies
- Python 3.12
- Pandas, NumPy
- Matplotlib, Seaborn for visualization
- scikit-learn for preprocessing

---

## References
- [Household Power Consumption Dataset](https://archive.ics.uci.edu/ml/datasets/individual+household+electric+power+consumption)

--- 

This version is streamlined, clear, and includes all the relevant details, making it easier for readers to understand your project flow from data analysis to modeling and evaluation.
