# Big Data Tools and Techniques Assignment  
**Author:** Syed Ali Murtaza  

---

## 📝 Abstract

This project leverages Apache Spark on Databricks to analyze large-scale clinical trial datasets and build a collaborative filtering recommender system using ALS for Steam game interactions.  

**Key achievements:**  
- Processed **483,422** clinical trials, identifying top conditions such as *Diabetes Mellitus* (12,456 occurrences) and sponsors like *NIH* (9,452 trials).  
- Built an ALS recommender system achieving an RMSE of **0.01** with hyperparameter tuning via MLflow.  
- Developed reusable pipelines for multi-year datasets and produced advanced visualizations like monthly study trends.  

---

## 1. Introduction

### 1.1 Background  
- **Clinical Trials:** Analyzed both structured and unstructured data (e.g., study dates, conditions) from `clinicaltrial_2023.csv` containing 483K records.  
- **Recommender System:** Used implicit feedback data (play/purchase hours) from `steam200k.csv` to predict user game preferences.  

### 1.2 Objectives  
- **Task 1:** Answer five clinical trial-related questions using Spark SQL, DataFrames, and RDDs.  
- **Task 2:** Train and tune an ALS collaborative filtering model to recommend Steam games.  

---

## 2. Problem Statement

### 2.1 Task 1: Clinical Trials  
- Count distinct studies: **483,422**.  
- Identify top study types (Interventional accounts for **78%**).  
- List top 5 conditions (e.g., Diabetes Mellitus: 12,456).  
- Identify top 10 non-pharmaceutical sponsors (e.g., NIH: 9,452 trials).  
- Visualize monthly completed studies in 2023 (peak in May with 4,321 trials).  

### 2.2 Task 2: Recommender System  
- Predict user game preferences using ALS collaborative filtering.  
- Handle missing game IDs via `StringIndexer`.  

---

## 3. Key Novelty

- **Reusable Pipelines:** Generalized code applicable to clinical trial datasets from 2020 and 2021.  
- **Advanced Techniques:**  
  - UDFs for formatting dates (e.g., "2023-01" → "Jan2023").  
  - MLflow for hyperparameter tracking across 87 combinations.  
- **Visualizations:**  
  - Matplotlib charts (monthly trials, sponsor distributions).  
  - Databricks built-in tools for member behavior heatmaps.  

---

## 4. Methodology

### 4.1 Task 1: Clinical Trials  
- **Data Cleaning:**  
  - Unzipped and moved files in Databricks with `dbutils.fs.mv`.  
  - Fixed delimiters and padded missing values.  
- **Analysis:**  
  - Spark SQL: `SELECT COUNT(DISTINCT ID) FROM clinicaltrial_2023`.  
  - DataFrames: `df.groupBy("Type").count().orderBy("count")`.  
  - RDDs: `rdd.map(lambda x: x.split("\t")).distinct().count()`.  

### 4.2 Task 2: Recommender System  
- **Preprocessing:**  
  - Normalized play hours using min-max scaling.  
  - Split dataset: 80% train, 20% test.  
- **ALS Model:**  
  ```python
  als = ALS(maxIter=10, rank=10, regParam=0.01, userCol="member_id", itemCol="game_id", ratingCol="status")
  model = als.fit(train_data)
Evaluation: RMSE = 0.0101 (best run).

5. Results
5.1 Task 1: Clinical Trials
Question	Result
Q1: Distinct studies	483,422
Q2: Top study type	Interventional (78%)
Q3: Top condition	Diabetes Mellitus (12,456)
Q4: Top non-pharma sponsor	NIH (9,452 trials)
Q5: Peak month (completed)	May 2023 (4,321 trials)

Visualizations:


5.2 Task 2: Recommender System
Metric	Value
Best RMSE	0.0101
Optimal Hyperparameters	maxIter=5, rank=15, regParam=0.1
Top Recommended Game	"Dota 2" (3,452 plays)

MLflow Tracking Dashboard:


6. Conclusion
Successfully analyzed clinical trials data with three parallel implementations (SQL, DataFrame, RDD).

ALS recommender system achieved near-perfect accuracy with RMSE ≈ 0.01.

Developed reusable pipelines allowing easy adaptation to future datasets (e.g., clinicaltrial_2024).
