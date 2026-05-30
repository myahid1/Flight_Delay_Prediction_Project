# Flight Delay Prediction Project: Final Report

## 1. Project Overview

This project explores whether scheduled flight information can be used to predict whether a flight will arrive 15 minutes or more late.

The target variable is `ARR_DEL15`, where:

* `0` = the flight was not delayed by 15 minutes or more
* `1` = the flight was delayed by 15 minutes or more

The project uses flight on-time performance data from the U.S. Bureau of Transportation Statistics. The dataset used in this project contains January 2025 flight records.

The main objective was not to build a perfect flight delay prediction system, but to understand which flight-related factors are associated with delay risk and to compare different machine learning models for this classification task.

## 2. Dataset Preparation

The original dataset contained 539,747 flight records and 21 columns.

Cancelled and diverted flights were removed because they do not have a normal arrival delay outcome. After this cleaning step, the final dataset contained 522,269 completed flights with no missing values.

A new `ROUTE` feature was also created by combining the origin and destination airports. For example:

```text
JFK-LAX
LAX-JFK
ORD-PHX
```

This allowed route-level delay patterns to be analysed and used in modelling.

## 3. Target Variable Distribution

After cleaning the dataset, the target variable showed the following distribution:

```text
Not delayed: 424,139 flights, around 81.21%
Delayed:      98,130 flights, around 18.79%
```

This means the dataset is somewhat imbalanced, with most flights not delayed. Because of this, accuracy alone is not enough to evaluate the models. A model could achieve high accuracy by simply predicting most flights as not delayed.

For this reason, precision, recall, F1-score, and the confusion matrix were also used to evaluate model performance.

## 4. Key Findings

The main findings from this project are:

1. **The dataset was somewhat imbalanced**

After removing cancelled and diverted flights, around 81% of completed flights were not delayed, while around 19% arrived 15 minutes or more late.

2. **Delay rates varied across time, airline, airport, and route**

Flights scheduled later in the day generally had higher delay rates than early morning flights. Delay rates also differed across airlines, origin airports, destination airports, and routes.

3. **Accuracy alone was not a reliable evaluation metric**

The baseline model achieved around 81% accuracy by predicting every flight as not delayed, but it failed to identify any delayed flights. This showed that recall, precision, F1-score, and the confusion matrix were more useful for evaluating the models.

4. **The GridSearch Tuned Decision Tree was selected as the best model**

The best model achieved around 70% accuracy, 34% precision, 60% recall, and a 43% F1-score for delayed flights. It was selected because it gave the best balance between identifying delayed flights and reducing false positives.

5. **The model is better suited as a delay-risk screening tool**

The model detected around 60% of actual delayed flights, but many predicted delays were false positives. This means it can help flag flights with higher delay risk, but it should not be treated as a precise delay prediction system.

6. **Date-related features were highly important**

Feature importance showed that `DAY_OF_MONTH` and `DAY_OF_WEEK` were among the most important predictors. This suggests that January-specific conditions, such as weather, holidays, or temporary disruptions, may have influenced the model.

## 5. Exploratory Data Analysis

Several delay patterns were explored before building the models.

### 5.1 Delay Rate by Airline

Delay rates varied across airlines. In the dataset, F9, OH, B6, and G4 had some of the highest delay rates, while HA, WN, and YX had lower delay rates.

However, this does not necessarily mean that one airline is operationally better or worse than another. Airline delay rates may also be affected by route mix, airport congestion, weather, flight timing, and other external factors.

### 5.2 Delay Rate by Departure Time

A clear pattern appeared when analysing scheduled departure time blocks. Early morning flights had lower delay rates, while afternoon and evening flights had higher delay rates.

This suggests that delays may build up throughout the day. If earlier flights are delayed, aircraft and crew may arrive late for later flights.

### 5.3 Delay Rate by Day of Week

Delay rates also differed by day of week. In this dataset, Monday and Sunday had the highest delay rates, while Wednesday had the lowest delay rate.

This may reflect weekly travel patterns, airport congestion, or January-specific travel conditions.

### 5.4 Delay Rate by Airport

Origin and destination airports also showed different delay rates. Some airports had noticeably higher delay rates than others.

However, airport-level results should be interpreted carefully because delay rates may be influenced by route mix, airline mix, weather conditions, and operational factors.

### 5.5 Delay Rate by Distance Group

Distance group showed some variation in delay rates, but the pattern was not strictly increasing or decreasing. Medium-distance flights appeared to have slightly higher delay rates than some longer-distance flights.

This suggests that distance may be related to delay risk, but it is unlikely to be the main factor by itself.

### 5.6 Delay Rate by Route

Route-level analysis showed that some routes had much higher delay rates than the overall dataset average. However, route-level delay rates should be interpreted carefully, especially for routes with smaller sample sizes.

A few delayed flights can have a larger effect on the delay rate when a route has fewer total flights.

## 6. Model Building

The following models were trained and compared:

* Baseline model
* Logistic Regression
* Decision Tree
* Random Forest
* Gradient Boosting
* Tuned Decision Tree
* GridSearch Tuned Decision Tree

Only information that would be available before the flight was used as model features. Columns such as actual arrival time and actual arrival delay were excluded to avoid data leakage.

The main features used included:

* Day of month
* Day of week
* Airline
* Origin airport
* Destination airport
* Scheduled departure time
* Departure time block
* Scheduled arrival time
* Scheduled elapsed time
* Distance
* Distance group
* Route

Categorical variables such as airline, airport, departure time block, and route were one-hot encoded before modelling.

## 7. Model Evaluation

The baseline model predicted the majority class for every flight. Since most flights were not delayed, the baseline achieved around 81% accuracy.

However, the baseline model failed to identify any delayed flights. Its recall and F1-score for delayed flights were both 0. This showed that accuracy alone was misleading for this project.

### Model Comparison

The GridSearch Tuned Decision Tree was selected as the best model because it provided the best overall balance between precision and recall for delayed flights.

The best model achieved approximately:

```text
Accuracy:  70%
Precision: 34%
Recall:    60%
F1-score:  43%
```

This means the model was able to identify around 60% of actual delayed flights. However, many flights predicted as delayed were not actually delayed, which explains the relatively low precision.

## 8. Confusion Matrix

The confusion matrix for the best model was:

```text
                    Predicted Not Delayed    Predicted Delayed

Actual Not Delayed          61,035                  23,793

Actual Delayed               7,747                  11,879
```

The model correctly identified 11,879 delayed flights and 61,035 non-delayed flights.

However, it also produced 23,793 false positives, where flights were predicted as delayed but were not actually delayed. It also missed 7,747 actual delayed flights.

This means the model is more useful as a delay-risk screening tool than as a precise flight delay prediction system.

## 9. Feature Importance

The tuned Decision Tree relied most heavily on the following types of features:

* Day of month
* Day of week
* Scheduled departure time
* Airline
* Distance
* Scheduled arrival time
* Origin and destination airports

The high importance of `DAY_OF_MONTH` suggests that date-specific factors may have strongly influenced delay risk in the January 2025 dataset. This could reflect weather disruptions, holiday travel patterns, or other temporary operational issues.

This is an important limitation because a model trained on one month of data may not generalise well to other months or seasons.

## 10. Limitations

This project has several limitations.

First, the model only uses one month of data. Flight delay patterns can vary by season, weather conditions, holidays, and operational changes, so a larger dataset across multiple months would likely produce a more generalisable model.

Second, the dataset does not include weather data. Weather is an important cause of flight delays, so excluding it limits the model’s predictive power.

Third, the model does not include aircraft rotation, crew scheduling, airport congestion levels, or real-time operational disruptions. These factors can strongly affect whether a flight is delayed.

Fourth, the model produced many false positives. This means it often predicted delay risk even when the flight was not actually delayed.

Because of these limitations, the model should not be treated as a highly accurate flight delay prediction system. It is better understood as a basic delay-risk classification model using scheduled flight information.

## 11. Skills Demonstrated

This project demonstrates the following skills:

* Data cleaning with pandas 
* Exploratory data analysis
* Feature engineering
* Handling imbalanced classification problems
* One-hot encoding categorical variables
* Building machine learning pipelines
* Training and comparing classification models
* Hyperparameter tuning using GridSearchCV
* Model evaluation using precision, recall, F1-score, and confusion matrix
* Interpreting feature importance
* Communicating model limitations clearly