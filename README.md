# Flight Delay Prediction Project

## Project Overview

This project uses flight on-time performance data to predict whether a flight will arrive 15 minutes or more late. The goal is to explore how scheduled flight information such as airline, route, departure time, day of week, and distance can be used to estimate flight delay risk.

The target variable is `ARR_DEL15`, where:

* `0` = flight was not delayed by 15 minutes or more
* `1` = flight was delayed by 15 minutes or more

## Dataset

The dataset was obtained from the U.S. Bureau of Transportation Statistics (BTS) Reporting Carrier On-Time Performance dataset.

This project uses January 2025 flight data. Cancelled and diverted flights were removed so that the model focuses only on completed flights with valid arrival delay outcomes.

## Project Structure

```text
Flight_Delay_Prediction_Project/
│
├── data/
│   ├── raw_flight_data.csv
│   └── cleaned_flight_delay_data.csv
│
├── notebooks/
│   ├── 01_data_cleaning_eda.ipynb
│   └── 02_model_building.ipynb
│
├── charts/
│
├── results/
│   ├── model_comparison.csv
│   └── feature_importance.csv
│
├── README.md
├── requirements.txt
└── .gitignore
```

## Methods Used

* Data cleaning
* Exploratory data analysis
* Feature engineering
* One-hot encoding for categorical variables
* Train-test split
* Classification modelling
* Model evaluation using precision, recall, F1-score, and confusion matrix

## Models Tested

* Baseline model
* Logistic Regression
* Decision Tree
* Random Forest
* Gradient Boosting
* Tuned Decision Tree
* GridSearch Tuned Decision Tree

## Key Findings

* Around 19% of completed flights in the dataset arrived 15 minutes or more late.
* Delay rates were higher for later departure time blocks, especially evening flights.
* Delay rates varied by airline, origin airport, destination airport, route, and day of week.
* The baseline model achieved high accuracy by predicting most flights as not delayed, but it failed to identify delayed flights.
* The GridSearch Tuned Decision Tree was selected as the best model because it provided the best balance between precision and recall for delayed flights.
* The best model detected around 60% of actual delayed flights, but it also produced many false positives.
* Feature importance showed that day of month, day of week, scheduled departure time, airline, distance, and airport-related features were important predictors.

## Limitations

* The project only uses one month of data, so the model may not generalise well to other months or seasons.
* Weather data was not included, even though weather can strongly affect flight delays.
* Airport congestion, aircraft rotation, and crew scheduling information were not available.
* The model should be treated as a delay-risk screening tool, not a precise prediction system.
* Some delay patterns may be specific to January 2025.

## How to Run

Clone the repository:
```bash
git clone <your-repository-link>
cd Flight_Delay_Prediction_Project
```

Create and activate a virtual environment:

```bash
python -m venv venv
venv\Scripts\activate
```

Install the required packages:

```bash
pip install -r requirements.txt
```
Run the notebooks in order:

```text
1. notebooks/01_data_cleaning_eda.ipynb
2. notebooks/02_model_building.ipynb
```

## Tools Used

* Python
* Pandas
* NumPy
* Matplotlib
* Scikit-learn
* Jupyter Notebook
