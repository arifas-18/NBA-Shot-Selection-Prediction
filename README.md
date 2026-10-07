# NBA Shot Selection Prediction

## Project Overview

This project analyzes and predicts NBA shot outcomes using historical field-goal attempt data from Kobe Bryant's NBA career.

The objective is to understand the circumstances associated with successful and unsuccessful shots and develop Machine Learning classification models that predict whether a basketball shot will be made or missed.

The project combines Exploratory Data Analysis (EDA), Feature Engineering, Data Preprocessing, Machine Learning, Hyperparameter Tuning, Cross-Validation, Model Evaluation, and Basketball Strategy Analysis.

---

## Problem Statement

The objective of this project is to build a Machine Learning classification model that predicts whether a basketball shot will be made or missed based on the circumstances under which the shot was attempted.

The target variable is:

`shot_made_flag`

- `0` = Shot Missed
- `1` = Shot Made

This is a supervised binary classification problem.

---

## Dataset

The dataset contains information about field-goal attempts made by Kobe Bryant during his NBA career.

Each row represents one shot attempt.

The dataset contains information related to:

- Shot type
- Action type
- Court location
- Shot distance
- Shot zone
- Game period
- Time remaining
- Opponent
- Season
- Playoff status
- Game context
- Shot outcome

The target variable is `shot_made_flag`.

### Dataset Preparation

The original dataset contains both shots with known outcomes and shots with unknown outcomes.

After separating the target:

- Modeling data: 25,697 records
- Unknown shots for prediction: 5,000 records

The 25,697 known-outcome records were used for model development, while the 5,000 unknown shots were preserved for final prediction.

---

## Business Understanding

Shot selection plays an important role in basketball strategy.

By analyzing historical shot attempts, Machine Learning can help identify patterns related to shot success.

This project can help stakeholders:

- Understand which types of shots are more successful.
- Identify favorable and unfavorable shooting locations.
- Study the influence of shot distance.
- Analyze shooting performance during different game periods.
- Examine performance against different opponents.
- Develop more effective offensive strategies.
- Predict whether a future shot is likely to be successful.

---

## Project Workflow

Data Loading
↓
Data Understanding
↓
Exploratory Data Analysis
↓
Data Cleaning
↓
Feature Engineering
↓
Categorical Encoding
↓
Numerical Scaling
↓
Stratified Train-Test Split
↓
Baseline Models
↓
Hyperparameter Tuning
↓
XGBoost
↓
Cross-Validation
↓
Model Comparison
↓
Final Model Selection
↓
Unknown Shot Prediction
↓
Basketball Strategy Analysis

---

## Exploratory Data Analysis

Exploratory Data Analysis was performed to understand the structure of the dataset and identify patterns related to shot success.

The analysis focused on:

- Shot outcome
- Shot distance
- Shot type
- Action type
- Court location
- Shot zone
- Game period
- Time remaining
- Playoff status
- Season
- Opponent

Visualizations were created to understand distributions, relationships, and patterns in the data.

---

## Data Preprocessing

Several preprocessing and feature engineering techniques were applied before training the Machine Learning models.

### 1. Separate Known and Unknown Outcomes

Rows with known `shot_made_flag` values were used for model development.

Rows with missing target values were preserved separately for final prediction.

### 2. Target Conversion

The target variable was converted into integer format:

`0 = Missed`

`1 = Made`

### 3. Time Feature Engineering

Minutes and seconds remaining were combined into:

`seconds_remaining_in_period`

### 4. Clutch Feature

A new feature called:

`is_clutch`

was created to identify shots attempted during the final 30 seconds of a period.

### 5. Date Feature Engineering

The `game_date` column was converted into a date format and the following features were extracted:

- `game_year`
- `game_month`

### 6. Identifier Removal

Identifier and constant columns were removed, including:

- `shot_id`
- `game_id`
- `game_event_id`
- `team_id`
- `team_name`
- `game_date`

### 7. Redundant Feature Removal

The location correlation analysis was used to identify redundant variables.

The `lat` and `lon` variables were removed while retaining useful court-location information such as `loc_x` and `loc_y`.

### 8. Home/Away Feature

Information from the `matchup` column was used to create:

`is_home`

This identifies whether the shot was taken during a home game.

---

## Train-Test Split

A stratified 80:20 train-test split was performed.

Stratification helps maintain approximately the same proportion of made and missed shots in both training and testing datasets.

The processed data contained:

- Training samples: 20,557
- Testing samples: 5,140
- Processed features: 144

---

## Preprocessing Pipeline

A `ColumnTransformer` was used to process numerical and categorical features.

### Numerical Features

Numerical features were standardized using:

`StandardScaler`

### Categorical Features

Categorical features were encoded using:

`OneHotEncoder`

with:

`handle_unknown="ignore"`

This preprocessing pipeline ensured that the numerical and categorical features were transformed appropriately before model training.

---

## Machine Learning Models

The following classification algorithms were implemented:

1. Logistic Regression
2. Decision Tree
3. Random Forest
4. K-Nearest Neighbors (KNN)
5. Linear Support Vector Machine (SVM)
6. XGBoost

Both baseline and optimized versions of the models were evaluated.

---

## Baseline Model Performance

The baseline models produced the following results:

| Model | Training Accuracy | Testing Accuracy | Precision | Recall | F1-Score |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 68.41% | 67.61% | 71.78% | 45.14% | 55.42% |
| Decision Tree | 100.00% | 58.19% | 53.06% | 54.47% | 53.76% |
| Random Forest | 100.00% | 65.19% | 64.95% | 47.75% | 55.04% |
| KNN | 73.88% | 60.45% | 57.03% | 46.01% | 50.93% |
| Linear SVM | 68.43% | 67.63% | 71.79% | 45.18% | 55.46% |

The baseline results showed signs of overfitting in some tree-based models, particularly the Decision Tree and Random Forest.

---

## Hyperparameter Tuning

Hyperparameter tuning was performed to:

- Improve model generalization.
- Reduce overfitting.
- Improve testing performance.
- Improve F1-score.
- Evaluate model stability.
- Identify a suitable final classifier.

`RandomizedSearchCV` was used instead of exhaustive GridSearchCV to efficiently explore different hyperparameter combinations.

F1-score was used as an important optimization metric.

---

## Tuned Model Performance

After hyperparameter tuning, the models achieved:

| Model | Training Accuracy | Testing Accuracy | Precision | Recall | F1-Score |
|---|---:|---:|---:|---:|---:|
| Tuned Logistic Regression | 68.06% | 67.43% | 70.38% | 46.62% | 56.09% |
| Tuned Decision Tree | 68.48% | 67.28% | 70.06% | 46.53% | 55.92% |
| Tuned Random Forest | 81.82% | 67.57% | 70.48% | 46.97% | 56.37% |
| Tuned KNN | 74.15% | 61.15% | 57.97% | 46.97% | 51.89% |
| Tuned Linear SVM | 68.31% | 67.63% | 71.50% | 45.62% | 55.70% |
| Tuned XGBoost | 69.05% | 67.82% | 71.72% | 46.01% | 56.06% |

---

## Final Model

The final model was selected based on test F1-score, testing performance, cross-validation performance, and generalization ability.

### Selected Model: Tuned Random Forest

The Tuned Random Forest achieved:

- Testing Accuracy: **67.57%**
- Precision: **70.48%**
- Recall: **46.97%**
- F1-Score: **56.37%**
- Training Accuracy: **81.82%**
- Accuracy Gap: **14.25%**

The model was selected based primarily on its highest test F1-score among the evaluated models.

---

## Final Classification Report

The final model produced the following classification performance:

| Class | Precision | Recall | F1-Score |
|---|---:|---:|---:|
| Shot Missed (0) | 0.66 | 0.85 | 0.75 |
| Shot Made (1) | 0.72 | 0.46 | 0.56 |

Overall accuracy was approximately **68%**.

The classification results show that the model performs better at identifying missed shots than made shots.

---

## Model Evaluation

The project uses several evaluation metrics:

- Accuracy
- Precision
- Recall
- F1-Score
- Classification Report
- Confusion Matrix
- Cross-Validation Performance
- Training vs Testing Accuracy

The training-testing accuracy gap was also examined to identify possible overfitting.

---

## Cross-Validation

Stratified cross-validation was applied to the strongest candidate models.

Cross-validation was used because a single train-test split provides only one estimate of model performance.

Using multiple validation splits provides a more reliable estimate of model stability and generalization.

---

## Predicting Unknown Shot Outcomes

The project also predicts the outcomes of the 5,000 shots where `shot_made_flag` was originally unknown.

The final model predicted:

- **3,510 shots as missed**
- **1,490 shots as made**

This corresponds to:

- **70.2% predicted missed shots**
- **29.8% predicted made shots**

The predictions were stored along with the corresponding `shot_id`.

---

## Basketball Strategy Analysis

In addition to Machine Learning prediction, the project analyzes shooting patterns from a basketball strategy perspective.

The analysis includes:

### Shot Zone Analysis

Shot success rates are analyzed across different court zones.

### Shot Type Analysis

Different shot types are compared based on:

- Number of attempts
- Number of made shots
- Success rate

### Shot Distance Analysis

Shot distances are grouped into meaningful ranges:

- 0–5
- 6–10
- 11–15
- 16–20
- 21–25
- 26–30
- 30+

This makes the analysis more meaningful than examining individual distances.

---

## Key Challenges

Several challenges were addressed during the project.

### Missing Target Values

The `shot_made_flag` column contained missing values.

Since this column is the prediction target, missing target values were not imputed. Instead, known outcomes were used for model development and unknown outcomes were preserved for final prediction.

### Categorical Variables

The dataset contained categorical features such as:

- Action type
- Shot type
- Opponent
- Shot zone

These variables required appropriate encoding before Machine Learning.

### Redundant Variables

Several variables contained overlapping information.

Correlation analysis and feature engineering were used to reduce unnecessary redundancy.

### Identifier Variables

Variables such as shot ID, game ID, and event ID primarily acted as identifiers and were removed from the modeling features.

### Model Overfitting

Some flexible models achieved very high training accuracy while performing considerably worse on unseen data.

Hyperparameter tuning was therefore used to control model complexity and improve generalization.

### SVM Computational Cost

More complex SVM approaches can be computationally expensive for a dataset of this size, so a Linear SVM approach was used in the final implementation.

---

## Technologies Used

### Programming Language

- Python

### Data Manipulation

- NumPy
- Pandas

### Data Visualization

- Matplotlib
- Seaborn

### Machine Learning

- Scikit-learn

### Gradient Boosting

- XGBoost

### Development Environment

- Jupyter Notebook

---

## Project Structure

NBA-Shot-Selection-Prediction/
│
├── NBA shot project.ipynb
├── README.md
├── requirements.txt
└── data.csv



