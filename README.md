# 🎬 Cinema Audience Forecasting Challenge

## 📌 Project Overview

This project focuses on predicting **cinema audience counts** using historical booking and visit data. The primary objective is to build a **robust regression model** capable of forecasting future attendance by leveraging time-series patterns, theater metadata, and historical booking trends.

---

## Dataset Description

The project uses multiple datasets to provide a comprehensive view of cinema operations:

- **`booknow_visits.csv`**  
  Contains the target variable `audience_count` along with historical show dates.

- **`booknow_booking.csv` & `cinePOS_booking.csv`**  
  Detailed transactional booking records, including ticket counts and booking timestamps.

- **`booknow_theaters.csv` & `cinePOS_theaters.csv`**  
  Theater metadata such as genre/type (Drama, Comedy, etc.), location area, and geographical coordinates.

- **`date_info.csv`**  
  Maps calendar dates to corresponding days of the week.

- **`movie_theater_id_relation.csv`**  
  A linkage file used to align theater IDs between the two booking systems.

---

## Exploratory Data Analysis (EDA)

Key insights derived from exploratory data analysis:

- **Weekly Seasonality**  
  Audience counts peak significantly on **Saturdays and Sundays**, indicating strong weekend demand.

- **Growth Trend**  
  A noticeable increase in audience demand begins around **July 2023**.

- **Right-Skewed Distribution**  
  Most shows attract between **0–100 viewers**, with a long right tail.

- **Booking Correlation**  
  A moderate positive correlation (**≈ 0.45**) exists between total tickets booked and final audience count.

---

## Feature Engineering

The following features were engineered to capture temporal dependencies and local trends:

- **Temporal Features**
  - Day of week
  - Month
  - Quarter
  - Indian holiday indicator

- **Lag Features**
  - Previous audience counts at lags of **1, 3, 7, 14, and 21 days**

- **Rolling Statistics**
  - Rolling mean and standard deviation over **7, 14, and 21-day windows**

- **Exponentially Weighted Means (EWM)**
  - Decaying historical averages to emphasize recent trends

---

## Model Evaluation & Performance

Multiple regression models were evaluated using the validation score:

| Model | Validation Score |
|------|------------------|
| **LightGBM (Tuned)** | **0.494** |
| XGBoost (Tuned) | 0.491 |
| Ridge Regression | 0.485 |
| Random Forest | 0.453 |

### Final Model

**LightGBM (LGBM)** was selected as the final model due to its superior generalization capability on time-series data and consistent performance across validation splits.

---

## Iterative Forecasting Approach

Since future dates require historical values that do not yet exist, an **iterative forecasting strategy** was implemented:

1. Predict the audience count for the next day
2. Append the prediction to the historical dataset
3. Recompute lag features and rolling statistics
4. Repeat for the entire test horizon

This ensures realistic forecasting without data leakage.

