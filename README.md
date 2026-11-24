
# BioTheraX: Explainable Multi-Omics Machine Learning for Precision Breast Cancer Treatment Selection

<p align="center">
  <strong>An Explainable AI Framework for Multi-Omics-Driven Breast Cancer Treatment Classification</strong>
</p>

## Overview

**BioTheraX** is an explainable machine learning research framework designed to investigate precision breast cancer treatment selection through the integration of clinical, genomic, and molecular data.

The framework utilizes the METABRIC (Molecular Taxonomy of Breast Cancer International Consortium) dataset, combining patient clinical characteristics, RNA gene-expression profiles, and mutation information to classify patients into eight distinct treatment categories.

Unlike conventional black-box prediction systems, BioTheraX incorporates **Explainable Artificial Intelligence (XAI)** through SHAP (SHapley Additive exPlanations), providing global feature-importance insights and patient-level explanations of model predictions.

The project explores how multi-omics data integration and interpretable machine learning can support transparent, data-driven approaches to breast cancer treatment research.

**Research Status:** Accepted at IEEE ICCIT 2026 (publication forthcoming).

## Key Features

- **Multi-Omics Integration:** Combines clinical characteristics, gene-expression measurements, and genetic mutation profiles.
- **Advanced Data Preprocessing:** Implements KNN imputation, categorical encoding, outlier analysis, and feature scaling.
- **Biological Feature Engineering:** Constructs mutation burden indicators, biological pathway activity scores, tumour burden metrics, and prognostic indicators.
- **Consensus-Based Feature Selection:** Combines correlation analysis, mutual information, Random Forest importance, and L1 regularization.
- **Dimensionality Reduction:** Explores PCA, Kernel PCA, Truncated SVD, ICA, t-SNE, and UMAP.
- **Class Imbalance Handling:** Applies SMOTE exclusively to the training data to prevent synthetic-sample leakage into the test set.
- **Comparative Model Evaluation:** Evaluates XGBoost, LightGBM, CatBoost, and Random Forest across multiple feature configurations.
- **Explainable AI:** Uses SHAP for global feature importance, individual prediction explanations, and feature interaction analysis.
- **Comprehensive Evaluation:** Generates confusion matrices, ROC curves, precision-recall curves, correlation heatmaps, and dimensionality-reduction visualizations.

## Dataset

BioTheraX utilizes the **METABRIC RNA and Mutation dataset**, containing clinical and molecular information from breast cancer patients.

| Dataset Characteristics | Description |
|---|---|
| Original patients | 1,904 |
| Original data columns | 693 |
| Patients retained after filtering | 1,282 |
| Features available after initial cleaning | 688 |
| Features after engineering | 704 |
| Treatment categories | 8 |

The dataset incorporates:

- Clinical characteristics, including age, tumour stage, tumour size, and hormone receptor status.
- RNA gene-expression measurements.
- Mutation records for cancer-associated genes, including BRCA1, BRCA2, TP53, PIK3CA, and PTEN.

**Dataset:** METABRIC RNA and Mutation.

Refer to the project notebook for the dataset download configuration and source information.

## Treatment Classification Categories

The framework defines eight treatment categories based on recorded treatment histories.

| Treatment Category | Description |
|---|---|
| Hormone_Radio | Hormone therapy + Radiotherapy |
| Hormone_Only | Hormone therapy |
| No_Treatment | No recorded systemic treatment |
| Radiotherapy_Only | Radiotherapy |
| All_Treatments | Chemotherapy + Hormone therapy + Radiotherapy |
| Chemo_Radio | Chemotherapy + Radiotherapy |
| Chemotherapy_Only | Chemotherapy |
| Chemo_Hormone | Chemotherapy + Hormone therapy |

## Methodology

The research pipeline consists of the following stages.

### 1. Data Acquisition and Exploration

- Load the METABRIC RNA and Mutation dataset.
- Examine data distributions, feature types, missing values, and treatment categories.
- Identify relevant clinical and molecular variables.

### 2. Data Cleaning and Preprocessing

- Remove duplicate records.
- Filter patient records according to the project's outcome-selection criteria.
- Handle missing numerical values using KNN imputation.
- Handle missing categorical values using mode imputation.
- Encode categorical variables and prepare numerical features.

### 3. Biological Feature Engineering

Sixteen additional features are constructed to capture clinically and biologically relevant information, including:

- Total mutation burden.
- MMP pathway activity.
- MAPK pathway activity.
- STAT pathway activity.
- TGF-beta pathway activity.
- Age-group indicators.
- Tumour burden score.
- High-risk indicator based on the Nottingham Prognostic Index.
- Long-term survivor indicator.
- Combined cancer-related pathway activation score.

### 4. Feature Selection

Four complementary feature-selection approaches are explored:

1. Correlation-based filtering.
2. Mutual Information.
3. Random Forest feature importance.
4. L1 Regularization (LASSO).

A consensus-based selection strategy identifies features supported by multiple selection methods.

### 5. Dimensionality Reduction

Multiple dimensionality-reduction techniques are investigated:

- Principal Component Analysis (PCA).
- Kernel PCA.
- Truncated Singular Value Decomposition (SVD).
- Independent Component Analysis (ICA).
- t-SNE.
- Uniform Manifold Approximation and Projection (UMAP).

PCA reduces the 704-dimensional feature representation to 426 components while retaining approximately 95.04% of the total variance.

t-SNE and UMAP are primarily used for visualization.

### 6. Train-Test Splitting and Class Imbalance Handling

To maintain an unbiased evaluation, the dataset is divided before applying any oversampling technique.

- **Training Set (80%):** Used for model learning and training.
- **Testing Set (20%):** Reserved exclusively for final evaluation on unseen patient records.

The split is stratified to preserve treatment-class proportions.

**SMOTE (Synthetic Minority Over-sampling Technique)** is subsequently applied exclusively to the training set to address class imbalance across the eight treatment categories.

The test set is kept untouched, ensuring that synthetic samples do not introduce information leakage into the evaluation process.

### 7. Model Training and Comparison

Four machine learning algorithms are evaluated:

| Model | Description |
|---|---|
| XGBoost | Extreme Gradient Boosting |
| LightGBM | Light Gradient Boosting Machine |
| CatBoost | Categorical Boosting |
| Random Forest | Ensemble-based decision trees |

Each model is investigated using three feature configurations:

- All Features (704 features).
- Consensus Features.
- Top 50 Features.

This produces twelve model-feature combinations for comparative evaluation.

### 8. Model Evaluation

Performance is assessed using:

- Accuracy
- Precision
- Recall
- F1-Score
- ROC-AUC
- Log Loss

F1-Score is used as the primary metric for model ranking.

### 9. Explainability with SHAP

SHAP is incorporated to improve model transparency through:

- **Global Interpretability:** Identifies the most influential features across predictions.
- **Local Interpretability:** Explains how individual clinical and molecular features contribute to a specific prediction.
- **Feature Interaction Analysis:** Investigates how combinations of biological and clinical variables influence model outputs.

## Experimental Results

The original experimental analysis reported the following best-performing configuration:

### CatBoost + Top 50 Features

| Evaluation Metric | Originally Reported Performance |
|---|---:|
| Test Accuracy | 89.16% |
| Test F1-Score | 88.91% |

The original analysis identified CatBoost with the Top 50 features as the best-performing configuration across eight treatment categories.

**Evaluation note:** These figures are from the original experimental analysis. They should not be interpreted as results from the corrected train-test splitting and training-only SMOTE workflow until the updated pipeline has been executed and the test metrics have been recalculated.

## Technology Stack

| Category | Technologies |
|---|---|
| Programming Language | Python |
| Data Processing | Pandas, NumPy |
| Machine Learning | Scikit-learn |
| Gradient Boosting | XGBoost, LightGBM, CatBoost |
| Explainable AI | SHAP |
| Feature Optimization | Optuna |
| Imbalanced Learning | Imbalanced-learn |
| Dimensionality Reduction | PCA, UMAP, t-SNE, SVD, ICA |
| Visualization | Matplotlib, Seaborn, Plotly |

## Repository Structure

The following is a suggested organization for the project.

```text
BioTheraX/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── data/
│   └── README.md
│
├── notebooks/
│   └── BioTheraX_Analysis.ipynb
│
├── src/
│   ├── preprocessing.py
│   ├── feature_engineering.py
│   ├── feature_selection.py
│   ├── model_training.py
│   └── explainability.py
│
├── models/
│
├── results/
│   ├── metrics/
│   └── visualizations/
│
└── reports/
```

*The structure above is illustrative. Actual filenames and directories may differ.*

## Installation and Usage

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY
```

### 2. Create a Virtual Environment

```bash
python -m venv venv
```

Activate it on Windows:

```bash
venv\Scripts\activate
```

On Linux/macOS:

```bash
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

If a requirements file is not available, install the required packages:

```bash
pip install pandas numpy scikit-learn xgboost lightgbm catboost shap optuna umap-learn imbalanced-learn matplotlib seaborn plotly
```

### 4. Run the Analysis

Open the project notebook in Jupyter Notebook or VS Code:

```bash
jupyter notebook
```

Run the notebook cells sequentially to execute the preprocessing, feature engineering, feature selection, model comparison, evaluation, and explainability pipeline.

## Research Significance

BioTheraX explores the intersection of:

- Precision Oncology.
- Multi-Omics Data Integration.
- Machine Learning for Healthcare.
- Explainable Artificial Intelligence.
- Data-Driven Clinical Decision Support.

By combining molecular and clinical information with interpretable machine learning, the project investigates how complex biological patterns can be translated into more understandable model predictions.

## Limitations and Responsible Use

BioTheraX is an experimental research and decision-support prototype developed using retrospective public dataset information.

Its predictions represent learned associations in the available dataset and should not be interpreted as clinically validated treatment recommendations.

Further validation using independent patient cohorts, prospective clinical data, and rigorous clinical evaluation is necessary before considering any real-world medical application.

**Final treatment decisions must always be made by qualified medical professionals in consultation with patients.**

## Research Publication

**Conference:** IEEE International Conference on Computer and Information Technology (ICCIT 2026)

**Status:** Accepted; publication forthcoming.

## Author

**Abu Hurayra Mahbe**

Computer Science and Engineering  
United International University

---

<p align="center">
  <strong>BioTheraX — Towards More Transparent and Interpretable Multi-Omics-Driven Precision Oncology.</strong>
</p>
