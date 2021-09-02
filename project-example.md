# 1. General Information
## 1.01 Standard version
0.3
## 1.10 Name
Home insurance claims classifier.
## 1.20 Purpose
For a better client experience and an improved claim processing time, predict at First Notification of Loss if a claim will be complex to handle, or simple. 
## 1.30 Date
21.06.2021

## 1.40 Contributors
John Doe and Jane Rey. 

## 1.50 Source code
The source code is available at this address: http://www.intranet/project/dir

# 2. Initial Data 
## 2.10 Sample
| Claim Id   | Age | Gender | Loan Amount  | ...|
| ------------- |-------------|-------------|-------------|-------------|
| 2938           | 38 |M|74000
| 2939      | 42      |   F |123000
| 2940      | 53      |   F |85000
## 2.20 Size
Columns: 19
Rows: 215 412
## 2.30 Variables description

| Column name   | Description | Example |
| ------------- |-------------|-------------|
| Age           | The age of the client |34
| Gender      | The gender of the client      |   M |
| Loan Amount       | Amount of the client loan     |   83000 |

## 2.40 Dataset unique signature
At this step, the dataset unique signature is:
```
abece2ef84645c61499cb4b74f552daa205380666b1ab03bbfa2fcdab91b11b6
```

# 3. Data Quality

## 3.10 Columns

### 3.10.10 Proportion of missing values by column
| Feature   | Missing value percentage |
| ------------- |-------------|
| Age           | 11% |
| Gender      | 20%      |
| Loan Amount      | 4%      |

### 3.10.20 Proportion of columns filled in
89.47% of columns have been filled in

### 3.10.30 Columns containing only one value (NaN included)
2 columns have been dropped because they were containing only 1 value

The 15 remaining columns are:
Age
Loan Amount
Gender
...
target

## 3.20 Rows

### 3.20.10 Number of duplicate rows
617 duplicate rows have been dropped.
214 795 rows remaining.

### 3.20.20 Number of rows where all fields are missing
No row is entirely empty.
214 795 rows remaining.

### 3.20.30 Number of rows where less than 2 fields are filled in
Every row contains more than 2 non empty fields.
214 795 rows remaining.

# 4. Data Preparation

## 4.10 Rows filtering
No row filtering
214 795 rows remaining.

## 4.20 Columns filtering
The 15 remaining columns are:
Age
Loan Amount
Gender
...
target

## 4.30 Missing values handling
For categorical variables: replacement by the most frequent value.
For numerical variables: replacement by variable median.

## 4.40 Dataset unique signature
```
87b01d1b7bc1b930348dd2d6d7c6d8c00fd5fa5179badc3a8d0a5eb8ab878eae
```

# 5. Features engineering

## 5.10 Created variables
2 variables have been created: 
- Age of the insured person at loan subscription, in months (loan subscription date - insured birth date)
- Loan seniority at claim creation date, in months (claim creation date - loan subscription date)

## 5.20 Target creation
The target is computed like this: if claim processing time (settlement date - creation date) is superior to 3 weeks, it is considered as complex (target = 1), else it is simple (target = 0).
The target rate is: 6.61%

## 5.30 Dataset unique signature
At this step, the dataset unique signature is:
```
bdb4e3721bd9ea1db352b8672a2facb61058380869f09bd35bb0072695d86a4d
```

# 6. Model Description

## 6.10 Used algorithm
We used the GradientBoosting algorithm (scikit-learn 0.20.2) with the following parameters: 

```python
{'nthread': 4, 'objective': 'binary:logistic', 'eval_metric': 'logloss', 'colsample_bytree': 1, 'silent': 1, 'subsample': 0.8, 'learning_rate': 0.2, 'max_depth': 8, 'min_child_weight': 8, 'lambda': 1, 'alpha': 1}
```
The version of xgboost is '1.0.0'

The version of scikit-learn is '0.23.2'

The dataset was split in train and test parts (90/10)

## 6.11 Environmental impact
Servers used were hosted in île-de-france.
A cumulative of 5440 seconds (1.5 hours) of computation was
performed.
Total emissions are estimated to be 9.05e-03 kgCO2eq.
It represents 0.29 tree-days.


The metric "tree-days" corresponds to the number of days a
mature tree needs to absorb this quantity of CO2.
On average, a tree absorbs 11kgCO2/year.

## 6.20 Metrics
We used the Log Loss metric. 

## 6.30 Validation strategy
We chose hyperparameters and variable with a 3-fold cross-validation. 

## 6.40 Performance

| Metric   | Value |
| ------------- |-------------|
| Log-Loss           | 0.187 |
| AUC      | 0.902      |
| Accuracy      | 0.934      |
| Threshold           | 0.5 |
| F1 score      | 0.206      |
| Precision      | 0.535      |
| Recall           | 0.128 |

Confusion matrix:

|    | P | N |
| -------- |----|----|
| P     | 2465 | 20 |
| N      | 157 | 23 |


Lift at 10%: 2.06

## 6.41 Performance over protected groups
Protected features "['Gender', 'Age']" used in dataset.

The following metrics are computed with a threshold = 0.5

Feature: Gender

Distribution of predictions per subgroup

|        | 0      | 1     |
| -------|--------|-------|
| global | 98.39% | 1.61% |
| F      | 98.31% | 1.69% |
| M      | 98.46% | 1.54% |


Performances of the model per subgroup

|        | ratio  | logloss | auc  | accuracy | f1_score  | precision | recall  | adversarial_proportions |
| -------|--------|---------|--------|---------|--------|---------|--------|---------|
| global | 100% | 0.19   | 0.9  | 0.93 | 0.21  | 0.53 | 0.13  | 0.0% |
| F      | 48.93% | 0.18   | 0.91  | 0.93 | 0.2  | 0.5 | 0.12  | 0.0% |
| M      | 51.07% | 0.19   | 0.89  | 0.94 | 0.21  | 0.57 | 0.13  | 0.0% |

Feature: Age

Distribution of predictions per subgroup

|        | 0      | 1     |
| -------|--------|-------|
| global | 98.39% | 1.61% |
| 0_0-18yr      | 100.0% | 0.0% |
| 1_18-30yr     | 94.43% | 5.57% |
| 2_30-45yr     | 96.97% | 3.03% |
| 3_45-60yr     | 100.0% | 0.0% |
| 4_over_60yr   | 100.0% | 0.0% |


Performances of the model per subgroup

|             | ratio| logloss | auc  |accuracy|f1_score| precision | recall  | adversarial_proportions |
| ------------|------|---------|------|-------|--------|---------|--------|---------|
| global      | 100%   | 0.19  | 0.9   | 0.93 | 0.21  | 0.53 | 0.13  | 7.05% |
| 0_0-18yr    | 18.2%  | 0.04  | 0.94  | 1.0  | 0.0   | 0.0  | 0.0   | 0.0% |
| 1_18-30yr   | 16.85% | 0.43  | 0.81  | 0.82 | 0.28  | 0.64 | 0.18  | 9.35% |
| 2_30-45yr   | 22.33% | 0.35  | 0.77  | 0.86 | 0.15  | 0.39 | 0.09  | 13.45% |
| 3_45-60yr   | 22.89% | 0.1   | 0.83  | 0.98 | 0.0   | 0.0  | 0.0   | 7.54% |
| 4_over_60yr | 19.74% | 0.04  | 0.92  | 1.0  | 0.0   | 0.0  | 0.0   | 3.8% |


The percentage of adversarial examples corresponds to the percentage
of instances for which the prediction of the model can be modified by
changing only the considered feature.

In other words, these are instances for which, if the considered
feature were worth something else, all other things being equal, then
the model prediction would be different.

## 6.50 Model unique signature
Trained model unique signature is:
```
a5b4e3721bd9ea1db352b8672a2facb61058380869f09bd35bb0072695d88cbb
```

# 7. Model Audit

## 7.10 Variable importance
![](./img/chart3.png)

## 7.20 Learning curve
![](./img/chart4.png)

## 7.30 Probability distribution

## 7.40 Descriptive statistics
![](./img/chart1.png)
![](./img/chart2.png)

## 7.50 Stability over time
![](./img/chart5.png)
