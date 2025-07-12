# 🧠 PERG Signal Classification via Machine Learning & Deep Learning

This project investigates the use of both traditional **Machine Learning (ML)** and **Deep Learning (DL)** approaches to classify **Pattern Electroretinogram (PERG)** signals as either *healthy* or *pathological*, with a focus on translational relevance in ophthalmologic diagnostics.

---

## 📚 Project Highlights

- **Signal source**: PERG-IOBA dataset ([PhysioNet](https://physionet.org/content/perg-ioba-dataset/1.0.0/))
- **Preprocessing**:
  - Raw PERG signals (`RE_1`, `LE_1`)
  - Zero-phase filtering & NaN cleaning
- **Feature engineering**:
  - Frequency-domain features (FFT)
  - Demographic metadata (age, sex)
- **Modeling approaches**:
  - **Traditional ML**: Logistic Regression, Decision Tree, Random Forest, XGBoost
  - **Deep Learning**: Fully connected model using concatenated FFT + metadata
- **Evaluation**:
  - Stratified cross-validation
  - Realistic prevalence test set (~2% prevalence)
  - Balanced test set (50/50)
  - Multi-threshold ROC analysis (e.g., Youden index, F1-optimized threshold)

---

## 🔬 Key Findings

| Metric              | Deep Learning Model | Machine Learning Model |
|---------------------|---------------------|------------------------|
| AUC                 | 0.66                | ~0.75 – 0.80           |
| Accuracy            | 61–67%              | 68–75%                 |
| Sensitivity (TPR)   | **High** (↑ recall) | Balanced               |
| Specificity (TNR)   | **Low**             | Balanced               |
| Overdiagnosis Risk  | Yes                 | No                     |
| Clinical alignment  | ⚠️ Weak              | ✅ Strong               |

> **Conclusion**: ML models yielded better overall diagnostic alignment, particularly in minimizing false positives. DL models achieved strong recall but risk over-referral.

---

## 🩺 Clinical Context

PERG is an electrophysiological test sensitive to **retinal ganglion cell dysfunction**, commonly used in **glaucoma diagnosis**.  
In clinical practice, high false positive rates (low specificity) may lead to unnecessary patient anxiety, additional testing, and resource overuse.  
This highlights the importance of balancing **sensitivity and specificity**, not just maximizing AUC or accuracy.

---

## 📦 Repository Structure

.
├── data/ # CSV signals and metadata (external)
├── notebooks/ # Jupyter workflows (preprocessing, training, analysis)
├── models/ # Saved DL .keras and ML .pkl models
├── scripts/ # Python modules for feature extraction, evaluation
└── README.md # This file


---

## ⚙️ Requirements

- Python ≥ 3.10
- NumPy, Pandas, Scikit-learn, TensorFlow ≥ 2.14
- Seaborn, Matplotlib
- XGBoost (for ML benchmark)

---

## 🚀 How to Run

1. Clone the repo & prepare the dataset (`/data`)
2. Run the notebooks:
   - `1_preprocessing.ipynb` → clean & extract features
   - `2_ml_training.ipynb` → traditional ML
   - `3_dl_training.ipynb` → Deep Learning model
3. Evaluate predictions (via `evaluate_predictions()`)

---

## 📌 Limitations

- Small sample size (~280 patients)
- High class imbalance in real-world test
- PERG signals are inherently noisy
- No image/fundus data used
- Not validated in a clinical pipeline

---

## ⚠️ Disclaimer

This project is for **educational and demonstration purposes only**.  
It is **not** FDA-cleared or approved for **clinical or diagnostic** use.  
Models trained here should **not** be deployed for real-world decision-making without further validation, clinical oversight, and regulatory approval.

---

## 👤 Author

Anis Boubala  
🧬 Bioinformatics Portfolio — July 2025  
For questions: [LinkedIn](https://www.linkedin.com)

