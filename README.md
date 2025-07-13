# 🧠 PERG Signal Classifier (ML & DL Pipeline)

> Automated detection of pattern electroretinogram (PERG) signal alterations using machine learning and deep learning techniques.

---

## 📁 Project Overview

This project aims to develop a classification pipeline for detecting PERG signal abnormalities, potentially linked to early retinal or optic nerve dysfunctions such as glaucoma.

### 🔬 Objective

To simulate a clinically relevant diagnostic aid using:

* Classical ML models trained on handcrafted FFT features
* Deep Learning models leveraging concatenated signal-derived and clinical metadata features

---

## 🧪 Dataset

**Source**: [PERG-IOBA Dataset](https://physionet.org/content/perg-ioba-dataset/1.0.0/) from PhysioNet
**Total Records**: 279 patients with associated clinical diagnosis

### Signal Details

* Channels: `RE_1`, `LE_1`
* Sampling rate: 300 Hz
* Duration: 500 ms segments
* Format: CSV (per patient)

### Metadata

* Age (years)
* Sex (Male/Female)
* Diagnosis label (0 = Normal, 1 = Altered)

---

## ⚙️ Feature Extraction (FFT-Based)

We compute FFT statistics from each signal:

* Mean amplitude
* Standard deviation
* Maximum & minimum amplitude

Concatenated features:

* 8 FFT features (4 from RE\_1 + 4 from LE\_1)
* 2 clinical metadata (Age, Sex)

Stored in:

* `fft_features.npy`
* `metadata_features.npy`
* `labels.npy`

---

## 🤖 Classical ML Models

Trained on the 10-dimensional input vector (FFT + metadata):

* Logistic Regression
* Decision Tree
* Random Forest ✅ Best AUC (\~0.77)
* XGBoost

### Evaluation Strategies

* **Balanced split (50/50)**: For robust metric estimation
* **Prevalence-based split (\~2%)**: For clinical realism
* Threshold optimized using:

  * F1-score maximization
  * Minimum recall (e.g., sensitivity >= 0.8)

---

## 🧬 Deep Learning Model

Implemented in Keras (TensorFlow backend)

### Architecture

* **Input 1**: FFT features → BatchNorm → Dense(128 → 64 → 32)
* **Input 2**: Metadata → BatchNorm → Dense(64 → 32)
* **Fusion**: Concatenate → Dense(64 → 32) → Output (sigmoid)

### Training

* Loss: Binary crossentropy
* Optimizer: Adam
* Class weighting: Balanced
* Callbacks: Early stopping, ReduceLROnPlateau, TensorBoard

### Evaluation

* ROC curve
* Confusion matrix
* Precision/Recall/F1 vs threshold
* Sensitivity-specificity trade-off explored from t=0.1 to 0.9

---

## 📊 Key Results

| Model         | AUC      | Sensitivity | Specificity | F1-score | Accuracy | Threshold |
| ------------- | -------- | ----------- | ----------- | -------- | -------- | --------- |
| Random Forest | **0.77** | 0.70        | 0.70        | 0.69     | **0.71** | 0.50      |
| Deep Learning | 0.66     | **0.93**    | 0.33        | **0.76** | 0.67     | 0.45      |

### Interpretation

* RF is optimal for balanced performance across all metrics.
* DL model prioritizes sensitivity — ideal for early disease detection.
* Final threshold chosen via F1-maximizing and clinical constraint.

---

## ✅ Good ML Practice (GMLP)

* Stratified splits (balanced and real-world prevalence)
* Feature explainability (FFT-based)
* DL interpretability (transparent architecture)
* Reproducible pipeline (seeds, logs, checkpoints)
* Versioning via Git
* Ground truth sourced from ophthalmologist-reviewed clinical metadata

---

## 📦 Output Files

* `perg_concat_model.keras` (saved DL model)
* `.npy` files for features and labels
* Confusion matrix plots and threshold reports
* TensorBoard logs in `logs/`

---

## 🧾 Regulatory Context (Simulated FDA Use)

> This project simulates an FDA submission under the SaMD framework.
> The model is intended for academic demonstration only and is not cleared for clinical use.

---

## 🔭 Future Work

* CNN/LSTM-based signal modeling (time-series)
* Prospective validation on external datasets
* Real-time signal ingestion via PERG hardware API
* Integration into ophthalmology decision support systems

---

## 👨🏻‍🔬 Author

**Anis Boubala**
Veterinarian & Bioinformatics Enthusiast
LinkedIn: [linkedin.com/in/anisboubala](https://www.linkedin.com/in/anisboubala)


