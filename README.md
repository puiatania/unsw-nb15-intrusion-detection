# Network Intrusion Detection using Machine Learning and Deep Learning (UNSW-NB15)

An academic project for the **AI in Cybersecurity** course, developed as a **Google Colab notebook**. The project explores binary network intrusion detection using the UNSW-NB15 dataset and compares supervised classification, classical anomaly detection, and a deep learning autoencoder.

## Project overview

The objective is to distinguish **normal traffic (`0`)** from **attack traffic (`1`)** using network-flow features. The notebook covers data exploration, preprocessing, model training, hyperparameter and threshold tuning, and evaluation on the dataset's predefined test split.

The experiments use **175,341 training records** and **82,332 test records**. Although the dataset includes attack categories, the prediction task in this project is **binary classification**.

## Approaches explored

- **Preprocessing and exploratory analysis:** data integrity checks, class distributions, numerical summaries, feature correlations, one-hot encoding of `proto`, `service`, and `state`, and numerical scaling.
- **Supervised learning:** Logistic Regression, Random Forest, Gaussian Naive Bayes, Decision Tree, Extra Trees, Gradient Boosting, and a calibrated Linear SVM. Random Forest and Logistic Regression are also evaluated with SMOTE.
- **Classical anomaly detection:** Isolation Forest, One-Class SVM, Local Outlier Factor (LOF), k-nearest-neighbor distance, PCA reconstruction error, and Elliptic Envelope. These models are trained on **normal traffic only**.
- **Deep learning:** an autoencoder trained to reconstruct normal traffic, using reconstruction error to identify anomalies.
- **Model evaluation and tuning:** confusion matrices, precision, recall, F1-score, ROC/PR analysis, cross-validation, and validation-based threshold selection.

## Results

Selected results from the held-out UNSW-NB15 test set (with **attack** as the positive class):

| Model | Accuracy | Attack precision | Attack recall | Attack F1 |
| --- | ---: | ---: | ---: | ---: |
| Random Forest + SMOTE | 88.40% | 83.67% | 98.08% | 90.30% |
| Autoencoder | 84.13% | 86.38% | 84.51% | 85.43% |
| LOF (novelty detection) | 81.63% | 88.73% | 76.33% | 82.07% |
| Isolation Forest (p95 threshold) | 59.36% | 85.85% | 31.35% | 45.93% |

In the project report, **Random Forest + SMOTE** is selected for the supervised scenario, while the **autoencoder** is selected for the normal-only anomaly-detection scenario. The comparison illustrates the trade-off between detecting attacks and limiting false alarms. These figures describe the dataset experiments and do not establish performance on live network traffic.

For the full methodology, detailed results, and discussion, see [Project_Report.pdf](Project_Report.pdf).

## Repository structure

```
.
├── README.md
├── anomaly_detection.ipynb
├── datasets/
│   ├── UNSW_NB15_training-set.csv
│   ├── UNSW_NB15_testing-set.csv
│   └── NUSW-NB15_features.csv
└── Project_Report.pdf
```

## Running the notebook in Google Colab

The project was developed in **Google Colab** and is intended to be run there, **cell by cell**.

1. Open `anomaly_detection.ipynb` in [Google Colab](https://colab.research.google.com/) (for example, by uploading the notebook or opening it from your GitHub repository).
2. Download the three CSV files from the repository's `datasets/` folder and upload them using the **Files** panel in Colab.
3. Place the CSV files directly in the Colab `/content/` directory. **Do not leave them nested under `/content/datasets/` unless you also update the notebook paths.** The notebook currently expects:

   ```python
   TRAIN_PATH = "/content/UNSW_NB15_training-set.csv"
   TEST_PATH  = "/content/UNSW_NB15_testing-set.csv"
   FEAT_PATH  = "/content/NUSW-NB15_features.csv"
   ```

4. Run the cells in order. If dependencies are unavailable in your runtime, the setup cell includes a commented installation command for `imbalanced-learn` and `tensorflow`.

**Note:** Colab file uploads are temporary; after a runtime reset, the CSV files may need to be uploaded again. Training and hyperparameter tuning can be computationally intensive.

## Tools and libraries

- Python, Jupyter Notebook / Google Colab
- NumPy, pandas, Matplotlib
- scikit-learn, imbalanced-learn (SMOTE)
- TensorFlow / Keras

## Dataset and documentation

This project uses the **UNSW-NB15** network intrusion detection dataset, including its predefined training and testing splits and feature metadata. The dataset was supplied for the course assignment; the original assignment also provides a [dataset folder](https://drive.google.com/drive/folders/1tYI7T0dzBtKVRvKHtTqWx1W-2Ls5ajSC).

The accompanying [project report](Project_Report.pdf) documents the methodology, experimental setup, evaluation results, model selection, limitations, and references.

## Scope

This repository presents a **course experiment**, not a production intrusion detection service. The results are specific to the chosen dataset splits, preprocessing, model configurations, and decision thresholds.
