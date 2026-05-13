# 📧 Email Spam & Phishing Analysis

Analyse exploratoire, nettoyage et préparation des données d'un dataset d'emails
pour la détection de phishing — première étape d'un pipeline de classification NLP.

---

## 📋 Description

Ce projet analyse un dataset de **18 650 emails** (légitimes vs phishing) afin de :
- Comprendre la distribution des classes
- Identifier les mots-clés caractéristiques des emails de phishing
- Nettoyer et normaliser le texte (URLs, emails, numéros de téléphone...)
- Extraire des features numériques prêtes pour un modèle de Machine Learning

---

## 📁 Structure du projet

```
email_spam_analysis/
├── data_exploration.ipynb    # Notebook principal : EDA + NLP + feature engineering
├── Email.csv                 # Dataset brut (18 650 emails)
├── data_for_use.csv          # Dataset final : texte nettoyé + features numériques
├── numero.csv                # Features numériques uniquement
├── processed_text.txt        # Textes nettoyés (un email par ligne)
├── top_keywords.txt          # Top 20 mots-clés de phishing extraits
└── README.md
```

---

## 📊 Dataset

**Dimensions :** 18 650 emails × 3 colonnes

| Colonne | Description |
|---|---|
| `Email Text` | Corps de l'email (texte brut) |
| `Email Type` | Étiquette — `Safe Email` ou `Phishing Email` |
| `label` | Encodage binaire — 0 = légitime, 1 = phishing |

**Distribution des classes :**

| Classe | Nombre | Pourcentage |
|---|---|---|
| Safe Email | 11 322 | 60.7% |
| Phishing Email | 7 328 | 39.3% |

---

## 🔧 Étapes du notebook

### 1. Exploration initiale
- Chargement et aperçu du dataset (`shape`, `info`, `describe`)
- Détection des valeurs manquantes et doublons
- Distribution des classes (bar chart + pie chart)

### 2. Analyse des mots fréquents (phishing)
- Extraction des mots les plus fréquents dans les emails de phishing
- Suppression des stop words (anglais)
- Visualisation Top 15 mots

### 3. Vectorisation TF-IDF
- `TfidfVectorizer` sur 1000 features
- Visualisation des mots avec le score TF-IDF moyen le plus élevé

### 4. Extraction des mots-clés phishing
- Comparaison fréquence phishing vs légitime
- Sélection des mots surreprésentés dans le phishing (ratio > 2×)
- Export → `top_keywords.txt`

### 5. Preprocessing NLP — classe `TextPrep`
Transformateur compatible scikit-learn qui :
- Convertit en minuscules
- Remplace les URLs → `[URL]`
- Remplace les adresses email → `[EMAIL]`
- Remplace les numéros de téléphone → `[PHONE]`
- Remplace les chiffres → `[NUMBER]`
- Supprime les caractères spéciaux

### 6. Feature Engineering numérique
Pour chaque email, extraction de :

| Feature | Description |
|---|---|
| `keyword_count` | Nombre de mots-clés phishing détectés |
| `text_length` | Longueur totale du texte |
| `word_count` | Nombre de mots |
| `avg_word_length` | Longueur moyenne des mots |
| `url_count` | Nombre d'URLs détectées |
| `email_count` | Nombre d'adresses email |
| `exclamation_count` | Nombre de `!` |
| `question_count` | Nombre de `?` |
| `dollar_count` | Nombre de symboles monétaires (`$`, `£`, `€`) |

### 7. Export des données finales
- `data_for_use.csv` — texte nettoyé + features numériques + label → prêt pour la modélisation
- `numero.csv` — features numériques seules

---

## 🚀 Installation & lancement

```bash
# Cloner le dépôt
git clone https://github.com/<TON_USERNAME>/email_spam_analysis.git
cd email_spam_analysis

# Installer les dépendances
pip install pandas numpy matplotlib seaborn scikit-learn jupyter

# Lancer le notebook
jupyter notebook data_exploration.ipynb
```

---

## 📦 Dépendances

| Librairie | Usage |
|---|---|
| `pandas` | Manipulation des données |
| `numpy` | Calculs numériques |
| `matplotlib` / `seaborn` | Visualisations |
| `scikit-learn` | TF-IDF, classe transformer |
| `re` | Expressions régulières (nettoyage texte) |
| `collections.Counter` | Comptage des mots |

---

## 📈 Résultats clés

**Top mots-clés phishing identifiés** (extrait de `top_keywords.txt`) :
`statements`, `save`, `adobe`, `removed`, `advice`, ...

> Ces mots sont au moins 2× plus fréquents dans les emails de phishing
> que dans les emails légitimes, et apparaissent plus de 20 fois dans le corpus.

---

## 🔜 Étapes suivantes (modélisation)

Le fichier `data_for_use.csv` est prêt pour entraîner des modèles de classification :

```python
X = df_combined[['processed_text'] + numerical_feature_columns]
y = df_combined['label']

# Modèles suggérés : Naive Bayes, Random Forest, Logistic Regression, SVM
```
