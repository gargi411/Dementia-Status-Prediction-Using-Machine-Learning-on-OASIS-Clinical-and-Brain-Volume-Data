# Dementia-Status-Prediction-Using-Machine-Learning-on-OASIS-Clinical-and-Brain-Volume-Data

# Dementia Status Prediction Using Machine Learning on OASIS Clinical and Brain-Volume Data: A KDD-Based Predictive Analytics Study

**Course/Project:** Predictive Analytics (CA3, Individual Academic Project)  
**Student Academic Report & Notebook**

---

## SECTION 1: PROJECT TITLE AND ABSTRACT

### Abstract
This study presents an end-to-end predictive modeling workflow following the classic **Knowledge Discovery in Databases (KDD)** framework to classify cognitive/dementia status. Leveraging tabular data from the Open Access Series of Imaging Studies (OASIS), we explore demographic, clinical, and structural brain-volume features. Recognizing the critical challenge of target leakage, we explicitly exclude clinical diagnostic measurements such as the Clinical Dementia Rating (CDR) and Mini-Mental State Examination (MMSE) from our primary predictive feature space to evaluate the actual predictive utility of pure structural and demographic profiles. Furthermore, to prevent data leakage in repeated longitudinal measurements, we adopt a strict subject-wise group-splitting strategy. Baseline and advanced estimators, including Dummy Classifiers, Logistic Regression, Decision Trees, and Random Forests, are trained and validated. This educational project rigorously evaluates classification limits, highlights the dangers of data leakage, and provides structured clinical context regarding the boundaries of tabular machine learning in automated medical diagnostic assistance.

### Keywords
Predictive Analytics, KDD, OASIS Dataset, Dementia Status, Subject-Wise Splitting, Target Leakage, Structural Brain Volume.

### Problem Statement & Motivation
Dementia represents an escalating global healthcare crisis, requiring objective, accessible, and early classification strategies. Clinical assessments such as the CDR score provide high-accuracy indicators but rely heavily on expert semi-structured interviews. This study explores the feasibility of employing purely demographic metrics (Age, Education, Socioeconomic Status, Gender) and non-invasive structural brain measurements (Estimated Total Intracranial Volume [eTIV], Normalized Whole Brain Volume [nWBV], and Atlas Scaling Factor [ASF]) to predict dementia classification.

### Objectives
1. Implement a structured, reproducible 18-section academic notebook following the **KDD methodology**.
2. Design and deploy an automated data-preprocessing and machine learning pipeline using scikit-learn.
3. Enforce a robust subject-wise split to guarantee that multiple records from the same subject do not contaminate both train and test splits.
4. Mitigate feature-based target leakage by explicitly identifying and dropping confounding clinical variables.
5. Compare multiple estimators and perform a rigorous error and limitation analysis.


## SECTION 2: KDD PROCESS OVERVIEW

The Knowledge Discovery in Databases (KDD) process is a structured sequence of stages designed to turn raw data into actionable knowledge. The mapping of this project to the KDD stages is detailed below:

1. **Data Selection:** Extracting and parsing the primary `oasis_longitudinal.csv` and secondary `oasis_cross-sectional.csv` datasets.
2. **Data Preprocessing & Cleaning:** Resolving missing attributes (imputation based on training subsets), handling outliers, correcting types, and validating categorical encodings.
3. **Data Transformation:** Feature scaling of structural metrics (eTIV, nWBV, ASF), nominal encoding, and subsetting to exclude identifier attributes and clinical leakages.
4. **Data Mining:** Fitting multiple classification algorithms (Dummy Classifier, Logistic Regression, Decision Tree, Random Forest).
5. **Evaluation & Interpretation:** Evaluating generalization via group-aware metrics (Accuracy, F1-Score, ROC-AUC, Recall, Precision) and analyzing confusion matrices and feature importance.

### KDD Workflow Diagram
```
+---------------------+      +------------------------+      +---------------------------+
|   1. Data Selection | ---> | 2. Preprocessing       | ---> | 3. Transformation         |
|   - Load CSVs       |      |    - Group Imputation  |      |    - Scale brain metrics  |
|   - Target check    |      |    - Zero Leakage split|      |    - Exclude CDR, MMSE    |
+---------------------+      +------------------------+      +---------------------------+
                                                                           |
+---------------------+      +------------------------+                    |
| 6. Knowledge &      | <--- | 5. Evaluation          | <--- | 4. Data Mining            | <+
|    Presentation     |      |    - Multi-metric Table|      |    - Logistic Reg, Trees  |
|    - Final Report   |      |    - Feature Importance|      |    - Group-split validation|
+---------------------+      +------------------------+      +---------------------------+
```

## SECTION 3: PROBLEM UNDERSTANDING

### Real-world Problem Context
Dementia is characterized by a progressive decline in cognitive function, commonly linked to structural brain atrophy. Accurate classification of cognitive impairment stages helps identify candidates for targeted clinical therapies.

### Prediction Target & Features
- **Target variable:** Typically the `Group` column (representing Normal, Demented, or Converted classes) in the OASIS longitudinal set.
- **Demographics:** Gender (`M/F`), Age, Education (`EDUC`), Socioeconomic Status (`SES`).
- **Neuroimaging (Structural Volumetrics):** Estimated Total Intracranial Volume (`eTIV`), Normalized Whole Brain Volume (`nWBV`), Atlas Scaling Factor (`ASF`).
- **Leakage Red Flags (To be Excluded):** Mini-Mental State Examination (`MMSE`) and Clinical Dementia Rating (`CDR`). These metrics represent direct cognitive assessments closely aligned with the diagnostic target definition itself; their inclusion leads to trivial, high-accuracy classification models that fail to generalize to subjects lacking extensive clinician screening.

### Scope and Clinical Boundaries
**CRITICAL CLINICAL DISCLAIMER:** This model is designed strictly for academic, educational, and exploratory evaluation of machine learning behaviors on tabular clinical indicators. It is **not** a clinical diagnostic system and must never be used to replace professional medical assessment, diagnostic scans, or qualified neurological consultations.


## SECTION 4: DATA COLLECTION AND DATA UNDERSTANDING

We start by designing an interactive data loading and collection sequence. If the files `oasis_longitudinal.csv` and `oasis_cross-sectional.csv` are not found locally, the user will be prompted to upload them or provide a path via Google Drive.
