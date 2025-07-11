# perg-signal-detection-ml

# 🧠 Détection automatique d’altération du signal PERG via Machine Learning

## 🎯 Objectif

Le but de ce projet est de simuler un **cas réel en bioinformatique médicale** en développant un pipeline complet de classification basé sur le signal PERG (Pattern Electroretinogram).  
Nous cherchons à détecter automatiquement si un signal est altéré ou non, en lien avec des pathologies ophtalmiques héréditaires ou acquises, comme l’atrophie optique, la rétinopathie toxique ou la maladie de Stargardt.

---

## 🧪 Données utilisées

- **Source** : [PERG-IOBA Dataset](https://physionet.org/content/perg-ioba-dataset/1.0.0/)
- **Contenu** :
  - Signaux bruts des deux yeux (`RE_1`, `LE_1`)
  - Métadonnées : âge, sexe, diagnostic clinique
- **Label** :
  - `0` = signal normal
  - `1` = signal altéré associé à une pathologie affectant le nerf optique ou la rétine

---

## 🧬 Extraction de features

Pour chaque œil (RE, LE), les features suivantes sont extraites :

- **Statistiques classiques** : moyenne, écart-type, min, max, amplitude
- **Moments statistiques** : asymétrie (`skewness`), aplatissement (`kurtosis`)
- **Signaux cliniques** : amplitude et latence de P50 et N95
- **Analyse spectrale** : fréquence dominante FFT, énergie spectrale
- **Données démographiques** : âge, sexe (encodé)

---

## ⚙️ Modélisation

### 📌 Méthodologie :

1. **Séparation des données** :
   - Entraînement sur un jeu équilibré (50 % positifs, 50 % négatifs)
   - Évaluation sur deux configurations :
     - ✅ Jeu de test équilibré pour comparaison des modèles
     - 🌍 Jeu de test simulant la **prévalence réelle** (~2 %) pour évaluation réaliste

2. **Validation croisée (5 folds)** :
   - Moyennes des performances calculées pour chaque modèle

3. **Optimisation du seuil de décision** :
   - Recherche d’un **seuil permettant une sensibilité ≥ 0.8**
   - Crucial dans le domaine médical pour éviter les faux négatifs

---

## 📈 Résultats

### 🔁 Validation croisée (5-fold)

| Modèle              | Recall (mean) | F1-score (mean) | Balanced Acc (mean) | ROC AUC (mean) |
|---------------------|---------------|------------------|----------------------|----------------|
| Logistic Regression | 0.735         | 0.698            | 0.632                | 0.699          |
| Random Forest       | 0.698         | 0.687            | 0.637                | 0.697          |
| XGBoost             | 0.668         | 0.663            | 0.618                | 0.672          |
| Decision Tree       | 0.653         | 0.630            | 0.590                | 0.601          |

### 🧪 Évaluation sur jeu équilibré (test 50/50)

| Modèle              | Sensibilité | Spécificité | F1-score | Balanced Acc | ROC AUC |
|---------------------|-------------|-------------|----------|---------------|----------|
| Logistic Regression | 0.773       | 0.591       | 0.708    | 0.682         | 0.727    |
| Random Forest       | 0.682       | 0.591       | 0.652    | 0.636         | 0.768    |
| XGBoost             | 0.591       | 0.545       | 0.578    | 0.568         | 0.680    |
| Decision Tree       | 0.636       | 0.455       | 0.583    | 0.545         | 0.545    |

### 🩺 Ajustement du seuil pour Recall ≥ 0.8

| Modèle              | Seuil  | Sensibilité | Spécificité | F1-score | Balanced Acc | AUC     |
|---------------------|--------|-------------|--------------|----------|----------------|---------|
| Random Forest       | 0.46   | 0.818       | 0.591        | 0.735    | 0.705          | 0.768   |
| Logistic Regression | 0.42   | 0.818       | 0.409        | 0.679    | 0.614          | 0.727   |
| XGBoost             | 0.25   | 0.818       | 0.364        | 0.667    | 0.591          | 0.680   |

---

## 📊 Analyse des matrices de confusion

Chaque modèle a été évalué sur un test équilibré avec 50 % de cas positifs :


📍 Exemple - Random Forest
TP = 27 | FN = 6 | TN = 23 | FP = 10


---

## 💡 Remarques bioinformatiques

- 🔬 Le choix d’un **seuil adapté** est fondamental en santé : on privilégie une **haute sensibilité** (ne pas rater de patients malades).
- 🌍 L’évaluation finale respecte aussi une **prévalence réaliste (~2%)**, permettant d’estimer les performances en conditions réelles.
- 🎓 Ce projet n’a pas pour but d’atteindre un modèle cliniquement validé, mais de **montrer un pipeline rigoureux** en apprentissage supervisé appliqué à des données médicales.

---

## ✅ Conclusion

Ce projet démontre l’applicabilité du machine learning pour l’analyse de signaux électrophysiologiques.  
Il illustre un **cas d’usage complet** allant de l’extraction de features à l’évaluation rigoureuse, avec un focus sur les contraintes propres à la **bioinformatique médicale**.
