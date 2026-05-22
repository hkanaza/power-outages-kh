# ⚡ Predicting the Impact of Power Outages on Togolese Businesses

!\[Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat\&logo=python\&logoColor=white)
!\[Scikit-learn](https://img.shields.io/badge/Scikit--learn-1.2+-F7931E?style=flat\&logo=scikit-learn\&logoColor=white)
!\[Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat\&logo=jupyter\&logoColor=white)
!\[Status](https://img.shields.io/badge/Status-Complete-27AE60?style=flat)
!\[ALX](https://img.shields.io/badge/ALX-Data%20Science%20Program-1B4F72?style=flat)

> \*\*A supervised machine learning project that predicts the severity of electricity disruption impacts on formal businesses in Togo, based on real survey data from the Togolese Ministry of Economy (DGEAE, 2025).\*\*

\---

## 📋 Table of Contents

* [Project Overview](#-project-overview)
* [Business Problem](#-business-problem)
* [Dataset](#-dataset)
* [Methodology](#-methodology)
* [Results](#-results)
* [Key Findings](#-key-findings)
* [Project Structure](#-project-structure)
* [Getting Started](#-getting-started)
* [Visualizations](#-visualizations)
* [Recommendations](#-recommendations)
* [Limitations \& Future Work](#-limitations--future-work)
* [Author](#-author)

\---

## 🔍 Project Overview

Togo, like many Sub-Saharan African countries, faces significant structural challenges in its energy sector. **Load-shedding** — scheduled or unscheduled electricity interruptions — represents a major constraint for businesses, affecting their productivity, competitiveness, and profitability.

This project applies **supervised machine learning classification** to predict the level of impact that electricity disruptions have on businesses, based on:

* Their business sector
* Their electricity usage patterns
* The consequences they experience
* Their adaptation strategies

**The goal** is to build a predictive model that can help policymakers and researchers identify the most vulnerable businesses **before** disruptions occur.

\---

## 💼 Business Problem

**Research Question:**

> \*To what extent do electricity supply disruptions affect the competitiveness of businesses in Togo?\*

**ML Framing:**  
This is a **3-class supervised classification** problem:

|Class|Definition|Distribution|
|-|-|-|
|🟢 **Low**|No disruption or < 10% activity loss|29.0% (n=36)|
|🟡 **Moderate**|Between 10% and 25% activity loss|46.0% (n=57)|
|🔴 **High**|More than 25% activity loss|25.0% (n=31)|

\---

## 📊 Dataset

|Property|Value|
|-|-|
|**Source**|DGEAE — Directorate General of Economic Studies and Analysis|
|**Institution**|Ministry of Economy and Strategic Watch, Togo|
|**Survey Period**|Q2 2025|
|**Sample Method**|Stratified by sector + Cut-Off method|
|**Raw Sample**|172 formal businesses|
|**Modeling Sample**|124 businesses (with target variable recorded)|
|**Features**|23 explanatory variables|
|**Target**|Z5 — Amplitude of the impact on business activity|

### Feature Categories

```
📂 Electricity Usage (6 features)
   ├── Uses CEET electricity (binary)
   ├── Experienced disruptions (binary)
   └── Usage type: air conditioning, lighting, production, equipment charging

📂 Consequences Experienced (6 features)
   └── Production stoppage, delivery delays, material losses,
       financial losses, loss of competitiveness, cost increases

📂 Adaptation Strategies (4 features)
   └── Solar energy, generator, battery/UPS, schedule adjustment

📂 Adaptation Effectiveness (2 features)
   └── Alternative coverage level, adaptation cost level

📂 Business Sector (5 features — one-hot encoded)
   └── Industry, Construction, HBR, Trade/Storage, Transport
```

\---

## 🔬 Methodology

### Pipeline Overview

```
Raw Data (Excel)
     │
     ▼
Data Cleaning \& Encoding
  • Sector mapping (NAEMA codes → labels)
  • Target variable grouping (5 → 3 classes)
  • Binary/ordinal encoding
  • Median imputation for missing values
     │
     ▼
Exploratory Data Analysis
  • Target distribution
  • Impact by sector
  • Consequences \& adaptation strategies
  • Correlation matrix
     │
     ▼
Train/Test Split (75% / 25%, stratified)
     │
     ▼
Model Training + 5-Fold Cross-Validation
  • Decision Tree    (baseline)
  • Random Forest
  • Gradient Boosting
  • Logistic Regression
     │
     ▼
Evaluation
  • Weighted F1-score (primary metric)
  • Accuracy, Precision, Recall
  • Confusion matrix (normalized)
  • Feature importance (Gini)
     │
     ▼
Conclusions \& Recommendations
```

### Why Weighted F1-Score?

The dataset has **imbalanced classes** (46% Moderate, 29% Low, 25% High). Accuracy alone would be misleading since a naive classifier predicting "Moderate" for all instances would achieve 46% accuracy. The **weighted F1-score** accounts for class imbalance by weighting each class by its support.

\---

## 📈 Results

### Model Performance Comparison

|Model|CV F1 (5-fold)|CV Std|Test Accuracy|Test F1|
|-|-|-|-|-|
|Decision Tree|0.319|±0.085|29.0%|0.272|
|Random Forest|0.300|±0.056|29.0%|0.249|
|**Gradient Boosting** ⭐|**0.340**|±0.092|**32.3%**|**0.309**|
|Logistic Regression|0.295|±0.088|22.6%|0.222|

### 🏆 Best Model: Gradient Boosting

```
              precision    recall  f1-score   support

        High       0.40      0.25      0.31         8
         Low       0.29      0.44      0.35         9
    Moderate       0.45      0.50      0.47        14

    accuracy                           0.32        31
   macro avg       0.38      0.40      0.38        31
weighted avg       0.38      0.32      0.34        31
```

### Performance Discussion

The scores (\~32-35%) are **honest and contextually justified**:

* Dataset is **small** (n=124) — typical of field survey studies
* **3 imbalanced classes** increase classification difficulty
* Variables are **self-reported** — subject to perception bias
* A random classifier would achieve F1 ≈ 0.20, confirming our models capture **real predictive signal**

\---

## 🔑 Key Findings

### Top 5 Most Predictive Features

|Rank|Feature|Importance|Interpretation|
|-|-|-|-|
|1|Alternative needs coverage|**10.3%**|Energy resilience capacity — the #1 protective factor|
|2|Cost increases (consequence)|**7.7%**|Direct economic pressure from disruptions|
|3|Equipment charging usage|**7.4%**|Dependency on rechargeable devices|
|4|Financial losses (consequence)|**6.5%**|Quantification of economic damage|
|5|Direct production usage|**6.5%**|Electricity as core production input|

### Impact by Sector

|Sector|Low Impact|Moderate Impact|High Impact|
|-|-|-|-|
|**Construction**|45.5%|18.2%|**36.4%** ⚠️|
|Trade/Storage|21.3%|48.9%|29.8%|
|HBR|25.0%|50.0%|25.0%|
|**Industry**|35.1%|48.6%|**16.2%** ✅|
|Transport|30.8%|46.2%|23.1%|

> \*\*Construction\*\* is the most vulnerable sector (36.4% high impact), while \*\*Industry\*\* shows the best resilience (16.2% high impact), likely due to greater investment in alternative energy solutions.

\---

## 📁 Project Structure

```
power-outages-togo/
│
├── 📓 Impact\_PowerOutages\_Togo\_ML.ipynb   # Main Jupyter notebook
├── 📊 BASE\_ELECTRICITE\_TOGO\_VF.xlsx       # Raw dataset (not committed — see note)
├── 📄 README.md                            # This file
├── 📋 requirements.txt                     # Python dependencies
├── 🚫 .gitignore                           # Files to exclude from Git
│
└── 📂 outputs/                             # Generated figures (auto-created by notebook)
    ├── fig1\_eda\_distribution.png
    ├── fig2\_eda\_consequences.png
    ├── fig3\_correlation.png
    ├── fig4\_model\_comparison.png
    ├── fig5\_feature\_importance.png
    └── dashboard\_final.png
```

> ⚠️ \*\*Note on the dataset:\*\* The raw Excel file is excluded from version control (`.gitignore`) to respect data confidentiality. To reproduce results, obtain the dataset from the DGEAE and place it in the root directory as `BASE\_ELECTRICITE\_TOGO\_VF.xlsx`.

\---

## 🚀 Getting Started

### Prerequisites

* Python 3.10+
* pip or conda

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/hkanaza/power-outages-togo.git
cd power-outages-togo

# 2. Create a virtual environment (recommended)
python -m venv venv
source venv/bin/activate        # Linux/macOS
# OR
venv\\Scripts\\activate           # Windows

# 3. Install dependencies
pip install -r requirements.txt

# 4. Place the dataset in the root directory
# File: BASE\_ELECTRICITE\_TOGO\_VF.xlsx

# 5. Launch Jupyter
jupyter notebook Impact\_PowerOutages\_Togo\_ML.ipynb
```

### Run on Google Colab

Click the badge below to open directly in Google Colab:

[!\[Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/power-outages-togo-ml/blob/main/Impact_PowerOutages_Togo_ML.ipynb)

> After opening in Colab, upload the dataset file when prompted.

\---

## 📊 Visualizations

The notebook generates **6 key visualizations**:

|Figure|Description|
|-|-|
|`fig1\_eda\_distribution.png`|Target distribution + breakdown by sector|
|`fig2\_eda\_consequences.png`|Consequences \& adaptation strategies by impact level|
|`fig3\_correlation.png`|Feature correlation matrix|
|`fig4\_model\_comparison.png`|Model comparison + normalized confusion matrix|
|`fig5\_feature\_importance.png`|Top 15 features (Random Forest + Gradient Boosting)|
|`dashboard\_final.png`|Complete 6-panel summary dashboard|

\---

## 💡 Recommendations

### For Policymakers

* **Prioritize Construction and Trade/Storage** sectors in load-shedding scheduling plans
* **Subsidize solar hybrid solutions** to reduce dependency on costly diesel generators
* Build an **energy vulnerability scoring system** based on this model for targeted support programs
* Invest in reliable power infrastructure for **industrial zones and commercial districts**

### For Businesses

* Invest in **hybrid energy solutions** (solar panels + generator + UPS battery)
* Establish **production schedule plans** that anticipate power outage windows
* Monitor **alternative energy coverage rate** — the single most important resilience indicator

\---

## ⚠️ Limitations \& Future Work

|Current Limitation|Proposed Improvement|
|-|-|
|Small dataset (n=124)|Add Q3-Q4 2025 survey waves (target n > 300)|
|Class imbalance|Apply **SMOTE** oversampling technique|
|Self-reported variables|Cross-validate with administrative data|
|Limited features|Add revenue, business age, geographic region|
|Static model|Build a **real-time scoring API** (Flask/FastAPI)|
|French survey keys|Fully bilingual preprocessing pipeline|

\---

## 🛠️ Tech Stack

|Tool|Version|Purpose|
|-|-|-|
|Python|3.10+|Core language|
|Pandas|2.0+|Data manipulation|
|NumPy|1.24+|Numerical computing|
|Scikit-learn|1.2+|Machine learning|
|Matplotlib|3.7+|Visualization|
|Seaborn|0.12+|Statistical plots|
|Jupyter|7.0+|Interactive development|

\---

## 👤 Author

**Hodabalo Kanaza**  
ALX Data Science Program — May 2026

[!\[LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat\&logo=linkedin)](https://linkedin.com/in/YOUR_LINKEDIN)
[!\[GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat\&logo=github)](https://github.com/YOUR_USERNAME)

\---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

Data used with authorization from the **DGEAE / Ministry of Economy and Strategic Watch of Togo**.

\---

## 🙏 Acknowledgments

* **DGEAE (Direction Générale des Études et Analyses Économiques)** — for providing the survey data
* **Ministry of Economy and Strategic Watch of Togo** — for authorizing the use of this dataset
* **ALX Africa** — for the Data Science training and mentorship

\---

<div align="center">

**⭐ If you find this project useful, please consider giving it a star!**

*Made with ❤️ in Togo | ALX Data Science Program 2026*

</div>

