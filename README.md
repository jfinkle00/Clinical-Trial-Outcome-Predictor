# 🧬 Clinical Trial Outcome Predictor

**Predicting clinical trial success using machine learning and survival analysis**

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://python.org)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## Overview

Clinical trials are the backbone of drug development, yet **over 50% fail to complete**. Each terminated trial represents $20-40 million in lost investment and delayed patient access to treatments.

This project builds machine learning models to predict whether a clinical trial will successfully complete or terminate early, using data from [ClinicalTrials.gov](https://clinicaltrials.gov). By identifying at-risk trials early, pharmaceutical companies and research institutions can allocate resources more effectively and intervene before costly failures.

### Key Features

- **Real-world data**: Pulls 6,000+ trials directly from the ClinicalTrials.gov API
- **Multiple ML models**: Logistic Regression, Random Forest, Gradient Boosting, SVM
- **Interpretable results**: SHAP values explain which factors drive predictions
- **Survival analysis**: Kaplan-Meier curves and log-rank tests for time-to-event analysis
- **Class imbalance handling**: Balanced class weights for robust minority class prediction

---

## Results Summary

| Model | AUC | Accuracy | Precision | Recall |
|-------|-----|----------|-----------|--------|
| Gradient Boosting | 0.74 | 0.71 | 0.85 | 0.74 |
| Random Forest | 0.73 | 0.70 | 0.84 | 0.73 |
| Logistic Regression | 0.71 | 0.68 | 0.82 | 0.72 |
| SVM | 0.70 | 0.67 | 0.81 | 0.71 |

*Results may vary slightly based on API data at time of execution*

---

## Key Findings

### 1. Trial Phase Impact
- **Phase 4** trials have the highest completion rates (~85%)
- **Phase 2/3** trials are riskiest due to efficacy uncertainty
- Early-phase trials fail more often from safety signals

### 2. Sponsor Type Matters
- **Industry-sponsored** trials complete at higher rates than academic
- NIH/government trials show intermediate success rates
- Resource availability is a key differentiator

### 3. Study Design Factors
- Larger enrollment → higher termination risk (recruitment challenges)
- Randomized designs → better completion rates
- Multi-country trials face coordination challenges

### 4. Top Predictive Features (SHAP Analysis)
1. Enrollment size (log-transformed)
2. Trial phase
3. Sponsor type
4. Number of study sites
5. Randomization status

---

## Project Structure

```
Clinical-Trial-Outcome-Predictor/
│
├── Clinical_Trial_Outcome_Predictor.ipynb   # Main analysis notebook
├── README.md                                 # This file
├── requirements.txt                          # Python dependencies
│
├── data/
│   └── clinical_trials_raw.csv              # Cached API data
│
├── figures/
│   ├── 01_outcome_distribution.png
│   ├── 02_completion_by_phase.png
│   ├── 03_completion_by_sponsor.png
│   ├── ...
│   ├── 12_shap_summary.png                  # SHAP feature importance
│   ├── 13_shap_importance.png
│   ├── 14_km_by_phase.png                   # Kaplan-Meier curves
│   └── 15_km_by_sponsor.png
│
└── model_metrics_summary.csv                 # Model performance comparison
```

---

## Installation

### Prerequisites
- Python 3.9+
- Jupyter Notebook or JupyterLab

### Setup

```bash
# Clone the repository
git clone https://github.com/jfinkle00/Clinical-Trial-Outcome-Predictor.git
cd Clinical-Trial-Outcome-Predictor

# Create virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Launch Jupyter
jupyter notebook Clinical_Trial_Outcome_Predictor.ipynb
```

---

## Requirements

```
pandas>=1.5.0
numpy>=1.23.0
matplotlib>=3.6.0
seaborn>=0.12.0
scikit-learn>=1.1.0
shap>=0.41.0
lifelines>=0.27.0
requests>=2.28.0
```

---

## Methodology

### Data Collection
- Source: ClinicalTrials.gov API v2
- Filters: Interventional trials with COMPLETED or TERMINATED status
- Sample: ~4,000 completed + ~2,000 terminated trials

### Feature Engineering
**Numeric Features:**
- Enrollment size (raw and log-transformed)
- Number of arms, interventions, conditions
- Number of sites and countries
- Collaborator count

**Categorical Features (One-Hot Encoded):**
- Trial phase (1, 2, 3, 4, N/A)
- Sponsor type (Industry, NIH, Academic, Other)
- Intervention type (Drug, Biological, Device, Behavioral)
- Masking level (Open, Single, Double, Triple+)

**Binary Features:**
- Is randomized
- Is US-based trial
- FDA-regulated drug/device
- Age group inclusion (children, adults, older adults)

### Model Training
- 80/20 train-test split with stratification
- 5-fold cross-validation for hyperparameter assessment
- Class-weighted models to handle imbalance (~70/30 split)
- Metrics: AUC-ROC, Accuracy, Precision, Recall, F1

### Interpretability
- **SHAP (SHapley Additive exPlanations)**: Feature contribution analysis
- **Feature Importance**: Random Forest Gini importance
- **Partial Dependence**: Relationship between features and predictions

### Survival Analysis
- **Kaplan-Meier Estimator**: Non-parametric survival curves
- **Log-Rank Test**: Statistical comparison between groups
- **Time Variable**: Duration from trial start to completion/termination

---

## Business Applications

| Use Case | Stakeholder | Value |
|----------|-------------|-------|
| Portfolio Risk Assessment | Pharma Executives | Prioritize trials with highest success probability |
| Resource Allocation | Project Managers | Focus support on at-risk trials |
| Go/No-Go Decisions | R&D Leadership | Data-driven trial advancement decisions |
| Trial Design Optimization | Clinical Operations | Design trials with better completion likelihood |
| Due Diligence | Investors/BD | Evaluate acquisition targets' pipeline risk |

---

## Limitations & Future Work

### Current Limitations
- Historical data only (no prospective validation)
- Limited to trial-level features (no molecule/indication complexity)
- Binary outcome (doesn't distinguish termination reasons)

### Future Enhancements
- [ ] Add therapeutic area-specific models
- [ ] Incorporate preclinical data for early-stage predictions
- [ ] Build REST API for real-time predictions
- [ ] Deploy as interactive Streamlit dashboard
- [ ] Add Cox proportional hazards modeling

---

## References

1. ClinicalTrials.gov API Documentation: https://clinicaltrials.gov/api/gui
2. Wong CH, Siah KW, Lo AW. "Estimation of clinical trial success rates and related parameters." *Biostatistics*. 2019
3. Lundberg SM, Lee SI. "A Unified Approach to Interpreting Model Predictions." *NeurIPS*. 2017

---

## Author

**Jason Finkle**  
Data Scientist | M.S. Data Science, American University

[![LinkedIn](https://img.shields.io/badge/LinkedIn-jason--finkle-blue)](https://linkedin.com/in/jason-finkle)
[![GitHub](https://img.shields.io/badge/GitHub-jfinkle00-black)](https://github.com/jfinkle00)

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

*Built with data from ClinicalTrials.gov, a resource provided by the U.S. National Library of Medicine.*
