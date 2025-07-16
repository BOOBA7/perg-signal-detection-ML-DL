# 🧠 PERG Signal Classifier – ML & DL Pipeline

> Automated detection of pattern electroretinogram (PERG) signal alterations using machine learning and deep learning approaches.

---

## 📁 Project Overview

This project aims to develop a robust classification pipeline for detecting functional abnormalities in PERG signals, potentially linked to early retinal or optic nerve dysfunction (e.g., glaucoma, retinal dystrophies).

### 🔬 Objective

To simulate a clinically relevant diagnostic aid through:

- Classical ML models trained on handcrafted FFT-derived features  
- Deep learning models leveraging combined signal and clinical metadata representations  

---

## 🧪 Dataset Summary

**Source**: [PERG-IOBA Dataset (PhysioNet)](https://physionet.org/content/perg-ioba-dataset/1.0.0/)  
**Total Participants**: 304 unique patients (collected from 2003–2022)  
**Records Used in Final Modeling**: 242 labeled samples with complete signals and metadata  

### Signal Characteristics

- Channels: `RE_1`, `LE_1`  
- Sampling rate: 300 Hz  
- Duration: 500 ms segments  
- Format: CSV (per participant)

### Clinical Metadata

- Age (in years)  
- Sex (`Male` / `Female`)  
- Diagnosis (binary: 0 = Normal, 1 = Pathological)

---

## ⚙️ Feature Engineering – FFT-Based Pipeline

For each signal (`RE_1`, `LE_1`), we compute descriptive statistics from the Fast Fourier Transform (FFT):

- Mean amplitude  
- Standard deviation  
- Minimum and maximum amplitude  

**Final feature vector per sample**:

- 4 FFT features from `RE_1`  
- 4 FFT features from `LE_1`  
- 2 clinical metadata: Age, Sex (one-hot encoded)

**Stored in**:

- `fft_features.npy`  
- `metadata_features.npy`  
- `labels.npy`

---

## 🤖 Classical Machine Learning Models

Trained on a 10-dimensional feature vector:

- Logistic Regression  
- Decision Tree  
- Random Forest ✅ **Best performance (AUC ≈ 0.77)**  
- XGBoost  

### Evaluation Methodology

- **Balanced 50/50 split**: Equal number of Normal and Pathological cases  
- **Prevalence-based split (~2% Normal)**: Simulates clinical distribution  
- **Threshold tuning**:  
  - F1-score maximization  
  - Clinical constraint: sensitivity (recall) ≥ 0.8

---

## 🧬 Deep Learning Model – Keras (TensorFlow Backend)

### Architecture

- **Input A**: FFT features → BatchNorm → Dense(128 → 64 → 32)  
- **Input B**: Metadata → BatchNorm → Dense(64 → 32)  
- **Fusion**: Concatenate → Dense(64 → 32) → Output (sigmoid)

### Training

- Loss: Binary Crossentropy  
- Optimizer: Adam  
- Class Weights: Balanced  
- Callbacks: `EarlyStopping`, `ReduceLROnPlateau`, `TensorBoard`

### Evaluation

- ROC curve, confusion matrix  
- F1-score, sensitivity, specificity vs threshold  
- Multi-threshold grid search from **t = 0.1 to 0.9**

---

## 📊 Results Summary

| Model         | AUC    | Sensitivity | Specificity | F1-score | Accuracy | Threshold |
|---------------|--------|-------------|-------------|----------|----------|-----------|
| Random Forest | **0.77** | 0.70        | 0.70        | 0.69     | **0.71** | 0.50      |
| Deep Learning | 0.66   | **0.93**     | 0.33        | **0.76** | 0.67     | 0.40      |

### Interpretation

- The **Random Forest** model is optimal for balanced performance.  
- The **Deep Learning** model favors **high sensitivity**, making it suitable for early screening use cases.  
- The **optimal threshold** was chosen based on F1 maximization and recall constraint.

---

## ✅ Good Machine Learning Practices (GMLP)

- ✅ Stratified training/testing splits (balanced and prevalence-based)  
- ✅ Explainable input features (FFT-derived + demographics)  
- ✅ Transparent DL architecture and reproducible training parameters  
- ✅ ROC- and F1-based threshold selection  
- ✅ Pipeline reproducibility (seed fixing, logging, versioning)  
- ✅ Ground truth derived from expert-validated clinical tags

---

## 📦 Output Artifacts

- `perg_concat_model.keras` — Trained DL model (Keras format)  
- `.npy` files: features and labels  
- Threshold exploration and confusion matrix plots  
- TensorBoard logs available in `/logs/`  

---

## 🧾 Regulatory Context – Simulated SaMD Submission

> This project simulates a **Software as a Medical Device (SaMD)** regulatory framework.  
> For **academic demonstration only** – Not intended or approved for clinical use.

For formal structure, see: [`FDA_SUBMISSION_SIMULATED.md`](FDA_SUBMISSION_SIMULATED.md)

---

## 🔭 Future Directions

- Develop CNN- or LSTM-based models for time-series representation  
- Conduct prospective external validation on multi-institutional datasets  
- Real-time integration with PERG acquisition systems (API/device-level)  
- Deploy browser-based or embedded decision support interface for clinicians

---

## 👨‍🔬 Author

**Anis Boubala**  
Veterinarian & Bioinformatics Enthusiast  
📎 LinkedIn: [linkedin.com/in/anisboubala](https://www.linkedin.com/in/anisboubala)

---

## 🤝 Acknowledgements

This project was designed, implemented, and documented with the support of AI-based tools including **ChatGPT (OpenAI)** for technical refinement, regulatory structuring, and code optimization.
