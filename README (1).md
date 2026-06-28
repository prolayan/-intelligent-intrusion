# Intelligent Intrusion Detection using UNSW-NB15

A machine learning pipeline for **network intrusion detection**, built on the [UNSW-NB15 dataset](https://www.kaggle.com/datasets/mrwellsdavid/unsw-nb15). The project covers feature selection, model comparison, and evaluation for classifying network traffic as **normal** or **attack**.

## 📊 Dataset

- **Source:** [UNSW-NB15 (Kaggle)](https://www.kaggle.com/datasets/mrwellsdavid/unsw-nb15)
- **Description:** A modern network traffic dataset created by the Australian Centre for Cyber Security (ACCS), containing a mix of real-world normal activity and synthetic attack behavior. It includes 49 features describing flow, packet, and connection-level characteristics, with a binary `label` column (0 = normal, 1 = attack).
- **Access:** Downloaded automatically at runtime via [`kagglehub`](https://github.com/Kaggle/kagglehub).

## 🧠 Project Pipeline

1. **Data Loading**
   Dataset is pulled via `kagglehub.dataset_download()` and loaded into a Pandas DataFrame (`UNSW_NB15_training-set.csv`).

2. **Preprocessing**
   - One-hot encoding of categorical features (`pd.get_dummies`)
   - Removal of identifier columns (`id`)

3. **Feature Selection**
   Two independent approaches are used to identify the most informative features:
   - **Random Forest Feature Importance** — ranks features by their contribution to a trained `RandomForestClassifier`.
   - **Linear SVM Coefficients** — ranks features by the absolute weight of a `LinearSVC` model trained on standardized data.

   Each feature subset (top 10) is validated by training a fresh Random Forest and comparing accuracy.

4. **Model Training & Comparison**
   Using the top 10 Random Forest features, five classifiers are trained and benchmarked:
   - Logistic Regression
   - Decision Tree
   - Random Forest
   - Support Vector Machine (SVM, RBF kernel)
   - K-Nearest Neighbors (KNN)

   The best-performing model is selected automatically based on test accuracy.

5. **Evaluation**
   For the best model:
   - Confusion Matrix
   - ROC Curve & AUC
   - Accuracy, Precision, Recall, F1-Score
   - Bar chart summarizing all metrics

6. **Outlier Handling & Correlation Analysis**
   - IQR-based outlier removal on key traffic features (`sbytes`, `dbytes`, `rate`, `sload`, `dload`)
   - Feature scaling with `StandardScaler` and boxplot visualization before/after
   - Correlation heatmap across top traffic-related features (e.g., `sttl`, `dttl`, `ct_state_ttl`, `ct_dst_src_ltm`, `ct_srv_src`)

## 🛠️ Tech Stack

| Category          | Tools |
|--------------------|-------|
| Language           | Python |
| Data Handling      | Pandas, NumPy |
| Modeling           | scikit-learn (RandomForest, SVM, Logistic Regression, Decision Tree, KNN) |
| Visualization      | Matplotlib, Seaborn |
| Dataset Access     | KaggleHub |

## 🚀 Getting Started

### Requirements
```bash
pip install kagglehub pandas scikit-learn matplotlib seaborn numpy
```

### Run
```bash
python intelligent_intrusion.py
```

The script will automatically download the dataset, run the full pipeline, and display all evaluation plots.

## 📈 Results

The pipeline reports accuracy, precision, recall, and F1-score for each of the five models, then highlights the best-performing one with a confusion matrix, ROC curve, and metric summary chart.

> *Note: Exact scores depend on the dataset split and version downloaded via KaggleHub.*

## 📁 Notes

- The notebook was originally developed in **Google Colab**.
- Some print statements are in Arabic (e.g., file paths, dataset shape) reflecting the original development environment — functionality is unaffected.

## 📜 License

This project is for educational and research purposes. Refer to the [UNSW-NB15 dataset license](https://www.kaggle.com/datasets/mrwellsdavid/unsw-nb15) for data usage terms.
