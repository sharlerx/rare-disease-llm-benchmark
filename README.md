cat > README.md << 'EOF'
# Evaluating GPT-4o for Rare Disease Diagnosis
### A Stratified Benchmarking Study Across Disease Rarity Tiers

**Author**: Oluwaseyi Omisore  | University of Massachusetts Boston | 2026

---

## Overview

Rare diseases affect over 300 million people worldwide, yet patients wait an
average of 4–7 years for a correct diagnosis. This project asks: can a large
language model help?

I benchmarked GPT-4o on 296 clinical case summaries spanning six disease
rarity tiers — from diseases seen only once in training data to those seen
over 100 times. Contrary to expectations, accuracy did not decrease
monotonically with rarity. Instead, I found a non-monotonic pattern peaking
in the middle tiers.

**Key finding**: GPT-4o achieves 37.5% top-1 accuracy on Very rare diseases
but only 11.4% on Ultra-rare diseases — and these represent two completely
different failure modes requiring different solutions.

📄 [Read the full report](report.pdf)

---

## Results Summary

| Rarity Tier | N | Top-1 Accuracy | Top-5 Accuracy | Gap (pp) |
|---|---|---|---|---|
| Zero-shot (novel) | 28 | 10.7% | 14.3% | 3.6 |
| Ultra-rare (1) | 44 | 11.4% | 11.4% | 0.0 |
| Very rare (2-5) | 56 | 37.5% | 42.9% | 5.4 |
| Rare (6-20) | 56 | 33.9% | 50.0% | 16.1 |
| Moderate (21-100) | 56 | 21.4% | 25.0% | 3.6 |
| Common (100+) | 56 | 28.6% | 33.9% | 5.4 |

Chi-square p=0.011 | Fisher exact p=0.005 (Ultra-rare vs Very rare)

---

## Key Findings

**1. Non-monotonic accuracy pattern**
Performance peaks in the Very rare and Rare tiers, not in the most common
diseases. More training data does not always mean better performance.

**2. Two distinct failure modes**
- Ultra-rare tier: 0.0pp gap between top-1 and top-5 → knowledge failure
  (correct answer not in top 5 at all)
- Rare tier: 16.1pp gap → calibration failure (correct answer present but
  ranked too low)

**3. Error clustering**
Wrong predictions cluster around imprinting disorders and chromosomal
syndromes, suggesting the model confuses mechanistically related diseases.

---

## Project Structure
├── notebooks/
│   ├── 01_data_cleaning_pace.ipynb   # PACE framework data cleaning
│   ├── 02_eda.ipynb                  # Exploratory data analysis
│   ├── 03_benchmarking.ipynb         # GPT-4o benchmark pipeline
│   └── 04_analysis.ipynb             # Statistical analysis & figures
├── data/
│   └── processed/                    # Cleaned datasets with engineered features
├── figures/                          # All publication-ready figures
├── results/                          # Benchmark results and summary tables
└── report.pdf                        # Full research report

---

## How to Reproduce

1. Clone this repository
```bash
git clone https://github.com/sharlerx/rare-disease-llm-benchmark
cd rare-disease-llm-benchmark
```

2. Install dependencies
```bash
pip install pandas numpy matplotlib seaborn scipy openai tqdm
```

3. Download the dataset from Kaggle
- [Evaluate LLMs for Rare Disease Diagnosis](https://www.kaggle.com/)
- Place `train_split_50k_cases.csv` and `Evaluation.csv` in the root folder

4. Get a free GitHub Models API key at github.com/marketplace/models

5. Run notebooks in order (01 → 02 → 03 → 04)

**Total cost: $0.00** using GitHub Models free tier

---

## Dataset

- **Source**: Kaggle — Evaluate LLMs for Rare Disease Diagnosis
- **Training set**: 50,594 cases across 1,393 unique diseases
- **Evaluation set**: 6,915 cases across 1,012 unique diseases
- **Pilot sample**: 296 stratified cases across 6 rarity tiers

---

## Framework

This project follows the **PACE** data science framework:
- **Plan**: Research question, rarity tier definitions, success metrics, ethics
- **Analyze**: Data quality audit, missing values, bias documentation
- **Construct**: Benchmarking pipeline, matching strategy, clean datasets
- **Execute**: Statistical analysis, figures, clinical interpretation

---

## Future Work

- Scale to full 6,915-case evaluation using UMass Boston Gibbs HPC cluster
- Multi-model comparison (Claude, Llama 3, Gemini)
- Retrieval-augmented generation using OMIM/Orphanet for Ultra-rare diseases
- Rarity-aware re-ranking model targeting the Rare tier calibration failure
- Adaptive prompting based on predicted rarity tier

---

## Contact

Oluwaseyi Omisore | University of Massachusetts Boston
GitHub: [@sharlerx](https://github.com/sharlerx)
