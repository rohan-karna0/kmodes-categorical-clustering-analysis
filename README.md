# K-Modes Categorical Clustering Analysis

## Overview
Clustering categorical data is challenging because categories do not have a natural numeric meaning.  
Algorithms like **K-Means**, which rely on Euclidean distance, often produce misleading or unstable results when applied to purely categorical features.

This project is an **experimental analysis of K-Modes clustering**, an algorithm designed specifically for categorical data.  
Rather than treating clustering as a black box, the focus is on **understanding model behavior, evaluation metrics, and failure cases**.

The objective is not to claim perfect clustering, but to clearly understand **when and why K-Modes works — and when it does not**.

---

## Problem Statement
Many real-world datasets (customer profiles, user attributes, demographic information) are categorical in nature, yet clustering is often applied without proper validation.

This project aims to answer:
- How does K-Modes behave on purely categorical data?
- How should the number of clusters (K) be selected without assumptions?
- How sensitive is K-Modes to different initialization strategies?
- How can clustering quality be evaluated for categorical features?

---

## Datasets Used (Synthetic by Design)
All datasets used in this project are **synthetically generated**.  
This provides full control over randomness and structure, making it easier to study both **success cases and failure cases** in clustering.

### Categorical Features Used
Each data point represents a synthetic user profile composed entirely of categorical attributes.

The dataset contains the following columns:

- **gender_category**: Male, Female  
- **region_category**: North, South, East, West  
- **education_level**: High School, Graduate, Postgraduate, PhD  
- **occupation_type**: Student, Engineer, Manager, Business, Retired  
- **spending_behavior**: Low, Medium, High  

All features are nominal categorical variables with no numeric or ordinal meaning, making the dataset unsuitable for distance-based methods such as K-Means.

These features were intentionally chosen to resemble real-world categorical profiling data while remaining fully synthetic and controlled.

---

### Dataset Variants

#### 1. Randomly Generated Categorical Data
- Categories are assigned randomly across all features
- No underlying structure exists
- Used intentionally to demonstrate **failure cases**
- Confirms that good clustering metrics do not appear by chance

#### 2. Structured Synthetic Categorical Data
- Categories are generated with consistent combinations
- Simulates realistic categorical patterns
- Used to test whether K-Modes can recover meaningful groupings
- Enables controlled comparison across different values of K and initialization strategies

---

## Why K = 7? (Metric-Based Selection)
The value **K = 7** was selected strictly using **quantitative evaluation metrics**, without relying on visual inspection or prior assumptions.

### Cost Evaluation
- K-Modes cost (total categorical mismatches) was computed for multiple values of K
- The **lowest cost was observed at K = 7**
- Indicates tighter and more compact clusters

### Silhouette Evaluation
- A **custom silhouette score based on Hamming distance** was implemented
- The silhouette score **peaked at K = 7**
- Indicates improved intra-cluster similarity and inter-cluster separation

**Conclusion:**  
K = 7 was selected because it simultaneously minimized clustering cost and maximized the silhouette score.

---

## Methodology
- Clustering algorithm: **K-Modes**
- Distance metric: **Hamming distance**
- Initialization strategies compared:
  - Huang
  - Cao
  - Random
- Evaluation metrics:
  - K-Modes cost
  - Custom silhouette score for categorical data
- t-SNE used only for **qualitative visualization**, never for validation or model selection

---

## Key Observations
- On randomly generated categorical data:
  - Clusters were unstable
  - Silhouette scores remained low
  - No meaningful grouping emerged (expected behavior)

- On structured synthetic categorical data:
  - Clear improvement at **K = 7**
  - Huang and Cao initializations produced more stable clusters
  - Random initialization occasionally appeared visually reasonable but performed worse on metrics

This demonstrates why **metric-based validation is essential**, especially when visualizations can be misleading.

---

## Limitations
- Synthetic data may not capture all real-world noise
- High-cardinality categorical features can reduce clustering effectiveness
- Visualization techniques like t-SNE can appear convincing even when clustering quality is poor

---

## Future Work
- Apply the same evaluation framework to real-world business datasets
- Extend to mixed numerical and categorical data using K-Prototypes
- Explore alternative distance measures for categorical clustering
- Improve robustness of evaluation techniques for categorical features

---

## How to Run
```bash
pip install -r requirements.txt
python kmodes_experiment.py
