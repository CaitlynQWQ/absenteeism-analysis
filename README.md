# Absenteeism Analysis & Prediction

This project performs a logistic regression analysis on employee absenteeism data using **Python**, **SQL**, and **Jupyter Notebook**. The goal is to identify key factors behind employee absences and predict the likelihood of high absenteeism. The full pipeline includes data preprocessing, model training, prediction, and SQL integration for deployment.

---

## Project Overview

- **Goal:** Predict which employees are at higher risk of being excessively absent.
- **Tech Stack:** Python (pandas, scikit-learn), MySQL (via `pymysql`), Jupyter
- **Model:** Logistic Regression (`sklearn.linear_model.LogisticRegression`)
- **Deployment:** Model and Scaler are saved via `pickle`, results optionally written to MySQL

---

## Data Preprocessing

- **Original Dataset:** `Absenteeism_data.xlsx`
- **Key Features:**
  - Categorical features like `Reason for Absence` and `Education` were transformed:
    - `Reason for Absence`: 28 codes grouped into 4 binary flags:
      - `Reason_1`: serious medical conditions (ICD 1–14)
      - `Reason_2`: pregnancy and perinatal issues (ICD 15–17)
      - `Reason_3`: temporary symptoms and external causes (ICD 18–21)
      - `Reason_4`: administrative/unspecified (ICD 22–28)
    - `Education`: Recoded into a binary feature:
      - `0` = High School
      - `1` = Graduate or above

- **Feature Scaling:** Applied only to numeric features using a custom `CustomScaler` class wrapping `StandardScaler`

---

## Model Summary

- **Algorithm:** Logistic Regression
- **Train Accuracy:** ~77%
- **Test Accuracy:** ~75%
- **Outputs:**
  - `Probability`: Model's predicted probability of high absenteeism
  - `Prediction`: Final binary classification based on threshold = 0.5

---

## SQL Integration

The final predictions are written into a MySQL table named `predicted_outputs` with the following schema:

```sql
CREATE TABLE predicted_outputs (
  reason_1 BIT NOT NULL,
  reason_2 BIT NOT NULL,
  reason_3 BIT NOT NULL,
  reason_4 BIT NOT NULL,
  month_value INT NOT NULL,
  transportation_expense INT NOT NULL,
  age INT NOT NULL,
  body_mass_index INT NOT NULL,
  education BIT NOT NULL,
  children INT NOT NULL,
  pets INT NOT NULL,
  probability FLOAT NOT NULL,
  prediction BIT NOT NULL
);
```
---
## Project Structure
```bash
absenteeism-analysis/
├── 01.Data_Preprocessing.ipynb             # Cleaning, transformation
├── 02.Modeling_Logistic_Regression.ipynb   # Logistic regression training & evaluation
├── 03.Integration_Connect_Python_SQL.ipynb # Connect and write predictions to MySQL
├── absenteeism_module.py                   # Custom model & scaler classes
├── Absenteeism_data.xlsx                   # Raw data
├── Absenteeism_preprocessed_01.xlsx        # Cleaned data (optional reference)
├── model                                   # Pickled model (.pkl)
├── scaler                                  # Pickled scaler (.pkl)
├── SQLquery.sql                            # SQL table schema
└── README.md                               # You’re here!

```
---
## How to Run
Clone the repository:
```git
git clone https://github.com/CaitlynQWQ/absenteeism-analysis.git

cd absenteeism-analysis
```
Open the notebooks in JupyterLab or VS Code and execute in order:

  01.Data_Preprocessing.ipynb
  
  02.Modeling_Logistic_Regression.ipynb
  
  03.Integration_Connect_Python_SQL.ipynb

Ensure MySQL server is running and predicted_outputs database/table is created.
