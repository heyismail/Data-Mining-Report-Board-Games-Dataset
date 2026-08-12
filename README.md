# 🎲 Data Mining Report — Board Games Dataset

A comprehensive **Data Mining I project** analyzing a BoardGameGeek dataset using data preprocessing, dimensionality reduction, clustering, classification, regression, and pattern mining techniques.

> **Academic Year:** 2025/2026
> **Project:** Data Mining I — Analyzing Board Games Data
> **Authors:** Asad Ali, Alissia Biliotti, Ismail Mohammed

---

## 📌 Project Overview

This project investigates a large board-game dataset containing information about game characteristics, complexity, player requirements, playtime, age suitability, popularity, ratings, categories, and community engagement.

The objective was to apply a wide range of **data mining and machine learning techniques** to understand the structure of the dataset, handle data quality issues, discover meaningful groups and relationships, and evaluate predictive models.

The analysis covers:

* Data understanding and preprocessing
* Missing-value analysis and imputation
* Outlier analysis
* Feature transformation
* Principal Component Analysis (PCA)
* Centroid-based clustering
* Density-based clustering
* Hierarchical clustering
* Classification
* Regression
* Frequent pattern mining
* Association rule mining
* FP-Growth

The complete academic analysis is documented in the project report included in this repository.

---

## 📊 Dataset

The dataset contains:

* **21,925 board games**
* **41 original features**

The data includes variables describing:

* Publication year
* Manufacturer and community complexity
* Minimum and maximum players
* Recommended age
* Playtime
* User ratings
* Ownership and demand
* Community voting
* Game categories
* Game characteristics and other attributes

The dataset contains substantial skewness, missing values, default zero values, categorical attributes, and legitimate extreme observations, making it suitable for a comprehensive data-mining study.

---

## 🧹 Data Preprocessing

A major part of the project focused on preparing the dataset for analysis while preserving as much information as possible.

### Features Removed

Several attributes were removed because they were not useful for quantitative modelling or contained insufficient information:

* `BGGId`
* `Description`
* `ImagePath`
* `GoodPlayers`
* `BestPlayers`
* `NumComments`

`Description` was excluded because the project did not involve text mining, while `NumComments` contained only zero values.

### Missing Values

Different imputation strategies were evaluated depending on the characteristics of each variable.

Examples include:

* **YearPublished:** Family-based median followed by global median
* **GameWeight / ComWeight:** Grouped median
* **NumWeightVotes:** Linear Regression
* **MfgAgeRec:** Decision Tree
* **ComAgeRec:** Linear Regression
* **MinPlayers:** Mode/median-based approach
* **MaxPlayers:** KNN and hybrid consistency checks
* **Playtime variables:** Decision Tree and Linear Regression approaches

Multiple algorithms were tested rather than applying a single imputation strategy to the entire dataset.

### Outliers

Extreme values were generally preserved when they represented legitimate board-game characteristics.

For example, highly popular games and games with unusually long playtimes were retained.

Values violating theoretical limits were treated differently. For example, `ComWeight` values above the theoretical maximum were capped at **5.0**.

Popularity variables were also log-transformed to reduce the influence of their highly skewed distributions.

---

## 🔬 Feature Transformation & PCA

Highly skewed variables were transformed using logarithmic transformations, while other numerical features were standardized.

The categorical `Rating` feature was converted into numerical values:

| Rating | Value |
| ------ | ----: |
| Low    |     1 |
| Medium |     2 |
| High   |     3 |

After preprocessing, the project worked with **38 numerical features**.

### Principal Component Analysis

PCA was used to investigate dimensionality and variance.

**17 principal components captured approximately 90% of the total variance.**

The first five principal components explained:

| Component | Variance |
| --------- | -------: |
| PC1       |   19.76% |
| PC2       |   14.26% |
| PC3       |    6.10% |
| PC4       |    5.90% |
| PC5       |    5.50% |

PCA was primarily used for dimensionality analysis and visualization. Clustering itself was performed on the transformed feature space rather than directly on the PCA representation.

---

# 🔎 Clustering

Several clustering approaches were evaluated to understand the natural structure of the board-game dataset.

## K-Means

K-Means clustering was performed using the transformed numerical features.

The optimal number of clusters was investigated using:

* SSE / elbow analysis
* Average silhouette scores

Different configurations including **K = 4, 6, and 8** were examined.

The analysis also explored **K = 8**, which provided a stronger silhouette result in the experiments.

## Bisecting K-Means

Bisecting K-Means was compared against standard K-Means.

Although the same number of clusters could be used, the resulting cluster distributions differed, demonstrating the effect of the clustering strategy.

## X-Means

X-Means was used to automatically investigate the appropriate number of clusters.

The analysis identified **K = 8** as the best configuration according to the evaluated silhouette scores.

---

## Density-Based Clustering

### DBSCAN

DBSCAN was applied using:

* `MinPts = 76`
* `Epsilon ≈ 1.68326`

The initial analysis identified **8 clusters**.

Approximately **33.9% of the observations** were initially classified as noise.

A second DBSCAN analysis was performed on the noise points, reducing the number of noise observations and increasing the number of detected clusters to **16**.

This demonstrated that iterative density-based clustering could reveal additional, weaker structures.

### OPTICS

OPTICS was subsequently applied to investigate density structures in more detail.

The analysis produced:

* **31 clusters**
* Approximately **2,572 noise points**

The reachability plot revealed multiple dense regions and cluster structures.

---

## Hierarchical Clustering

Hierarchical clustering was evaluated using:

* Single Linkage
* Complete Linkage
* Average Linkage
* Ward Linkage

Because running hierarchical clustering on the entire dataset exceeded available memory resources, a **50% stratified sample** was used.

Average Linkage achieved the highest reported silhouette score:

> **Silhouette = 0.892 with K = 4**

However, Ward Linkage produced a more balanced distribution of observations across clusters.

---

## HDBSCAN

Hierarchical Density-Based Spatial Clustering of Applications with Noise (HDBSCAN) was used as a final density-based approach.

The resulting analysis identified:

* **18 clusters**
* **1,133 noise points**
* Approximately **97.8% membership probability** for cluster assignments

Overall, HDBSCAN produced highly stable clusters compared with the earlier DBSCAN and OPTICS experiments.

---

# 🤖 Classification

The `Rating` variable was selected as the classification target.

The three classes were encoded as:

* `1` → Low
* `2` → Medium
* `3` → High

The project compared several classification algorithms.

## K-Nearest Neighbours

Different methods were used to select an appropriate value of K, including:

* Elbow analysis
* Stratified cross-validation
* K-Fold cross-validation

The final evaluation used **K = 10**.

## Naive Bayes

Gaussian Naive Bayes was selected because the features were numerical.

Naive Bayes performed worse than KNN and Decision Trees.

Binning continuous features and applying alternative Naive Bayes approaches did not improve performance.

## Decision Trees

Decision Trees were evaluated using both:

* Gini Index
* Entropy

The Gini-based tree was selected for subsequent analysis.

Decision Trees produced the strongest overall classification performance among the evaluated approaches and also performed well under different misclassification-cost scenarios.

---

# 📈 Regression

Regression experiments were conducted for several targets.

### Category Prediction

Linear Regression and Lasso Regression were investigated for categorical/binary targets.

### Predicting Ownership

`NumOwned` was selected as a continuous target.

The analysis identified a strong relationship between ownership and other popularity variables, particularly:

* `NumUserRatings`
* `NumWeightVotes`
* `NumWish`
* `NumWant`

Using multiple independent variables improved predictive performance compared with the single-feature model.

### Rating Prediction

Linear Regression was also experimentally applied to the encoded `Rating` variable.

The resulting:

* **RMSE:** 0.5777
* **MAE:** 0.48

indicated that predictions were, on average, roughly half a rating class away from the encoded target.

---

# 🧩 Pattern Mining

Pattern mining was used to discover relationships between board-game characteristics.

Two major approaches were explored:

* Apriori
* FP-Growth

Numerical features were discretized into bins before being converted into transaction-style data.

---

## Apriori

Different support and confidence thresholds were tested.

The analysis showed that:

* Higher support values reveal dominant patterns.
* Lower support values reveal more specialized or niche combinations.
* Increasing confidence substantially reduces the number of generated rules.

Several interesting relationships were discovered between:

* Game complexity
* Rating
* Ownership
* Demand
* Playtime
* Age groups
* Language difficulty
* Categories

For example, one high-lift association showed a strong relationship between long playtime, high rating, many weight votes, and heavy game weight/high demand.

---

## FP-Growth

FP-Growth was used to investigate frequent patterns and association rules, particularly among categorical game characteristics.

At different confidence levels, FP-Growth produced strong associations involving categories such as:

* Children
* Party
* War
* Strategy

The analysis found that FP-Growth was particularly useful for discovering categorical associations.

It was also investigated as a possible technique for filling missing categorical values.

---

# 🧠 Key Findings

The project produced several important conclusions:

### 1. Data preprocessing is critical

The original dataset contained substantial missing values, default zero values, skewed distributions, and inconsistent observations.

Different variables required different imputation strategies.

### 2. Legitimate outliers should not automatically be removed

Many extreme values represented genuine characteristics of board games, such as highly popular games or unusually long games.

Therefore, most outliers were retained.

### 3. PCA successfully reduced dimensionality

Although the original feature space did not exhibit a severe curse-of-dimensionality problem, PCA reduced the representation to **17 components covering approximately 90% of the variance**.

### 4. Decision Trees performed strongly

Decision Trees consistently performed well in the experiments, particularly for classification and several missing-value imputation tasks.

### 5. Linear Regression was effective for some numerical variables

Linear Regression provided strong results for predicting variables such as `ComAgeRec` and `NumWeightVotes`, depending on the selected feature set.

### 6. Density-based methods revealed complex structures

DBSCAN, OPTICS, and HDBSCAN identified different structures and noise distributions.

HDBSCAN produced particularly stable clusters with relatively few noise points.

### 7. Pattern mining revealed meaningful relationships

Apriori and FP-Growth uncovered relationships involving:

* Age groups
* Game complexity
* Ratings
* Ownership
* Demand
* Playtime
* Categories

However, pattern mining was not always effective for missing-value imputation because rule coverage could be limited.

---

# 🛠️ Techniques Used

| Area                     | Techniques                                                 |
| ------------------------ | ---------------------------------------------------------- |
| Data Preparation         | Missing-value analysis, imputation, outlier handling       |
| Transformation           | Log transformation, standardization, discretization        |
| Dimensionality Reduction | PCA                                                        |
| Clustering               | K-Means, Bisecting K-Means, X-Means                        |
| Density Clustering       | DBSCAN, OPTICS, HDBSCAN                                    |
| Hierarchical Clustering  | Single, Complete, Average, Ward Linkage                    |
| Classification           | KNN, Gaussian Naive Bayes, Decision Trees                  |
| Regression               | Linear Regression, Lasso                                   |
| Pattern Mining           | Apriori, FP-Growth                                         |
| Evaluation               | MAE, RMSE, R², Accuracy, Precision, Recall, F1, Silhouette |

---

# 📁 Repository Contents

The repository currently contains the project data and final report.

```text
Data-Mining-Report-Board-Games-Dataset/
│
├── Data_Compressed.zip
│   └── Dataset / project data
│
├── Final Report.pdf
│   └── Complete project report
│
└── README.md
    └── Project documentation
```

---

# 📄 Project Report

The complete analysis, methodology, experiments, visualizations, and results are available in:

**`Final Report.pdf`**

The report contains the full discussion of preprocessing, clustering, classification, regression, and pattern mining experiments.

---

# 👥 Authors

**Asad Ali**
**Alissia Biliotti**
**Ismail Mohammed**

**Academic Year:** 2025/2026

---

# 🎓 Project Context

This project was completed as part of **Data Mining I**.

The main goal was not only to apply individual algorithms, but to conduct an end-to-end data-mining investigation:

```text
Raw Dataset
     │
     ▼
Data Understanding
     │
     ▼
Data Quality Assessment
     │
     ▼
Missing Value Treatment
     │
     ▼
Feature Transformation
     │
     ├──────────────► PCA
     │
     ├──────────────► Clustering
     │
     ├──────────────► Classification
     │
     ├──────────────► Regression
     │
     └──────────────► Pattern Mining
                         │
                         ▼
                 Association Rules
                         │
                         ▼
                    Insights
```

---

## ⭐ Conclusion

This project demonstrates a complete data-mining workflow on a large and heterogeneous board-game dataset.

The analysis showed that careful preprocessing and feature engineering are essential before applying machine-learning techniques. Different algorithms were effective for different tasks: **Decision Trees performed strongly for classification and selected imputation problems, while Linear Regression was effective for several numerical prediction tasks.**

Clustering methods revealed multiple structures within the dataset, while Apriori and FP-Growth uncovered associations between game characteristics, demographics, complexity, ratings, popularity, and demand.

Overall, the processed dataset provides a foundation for further predictive modelling and exploratory analysis of board-game characteristics and user engagement.
