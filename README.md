# Comparing Statistical Approaches for Network Inference in Noisy Biological Time-Series Data
**Full project write-up:** [View PDF](project_writeup.pdf)

## Overview
This project evaluates how different statistical modeling assumptions affect gene regulatory network inference when working with short, noisy biological time-series data. Rather than optimizing a single predictive model, the focus is on comparing inference strategies and understanding the limits of what conclusions can be supported under realistic experimental constraints.

## Problem
Gene regulatory network inference is often used for hypothesis generation rather than definitive conclusions. In practice, biological experiments frequently produce short and noisy time-series datasets due to cost, experimental complexity, or biological limitations. This raises an important analytical question: **how much does model choice matter when data quality and quantity are constrained?**

## Data
- Time-series gene expression data sampled every 10 minutes  
- 5 genes with a short observation window  
- A small reference regulatory network used for evaluation  
- Dataset size reflects common limitations in biological experiments  

## Approach
Instead of tuning a single model for performance, I compared two common statistical approaches to network inference with different assumptions:

### 1. Gaussian Graphical Model (Graphical Lasso)
- Modeled gene expression as a multivariate Gaussian distribution  
- Used L1-regularized precision matrix estimation to infer conditional dependencies  
- Extended to a time-lagged formulation to introduce directionality  
- Ranked inferred interactions using absolute partial correlations  

**Goal:** Establish a baseline for gene–gene dependency inference without explicit temporal modeling.

### 2. Vector Autoregressive Model (VAR(1) with L1 Regularization)
- Modeled gene expression dynamics as:  
  \[
  x(t+1) = A x(t) + \varepsilon(t)
  \]
- Each coefficient represents a directed, time-lagged influence  
- Applied L1 regularization to encourage sparsity and interpretability  
- Ranked inferred edges by coefficient magnitude  

**Goal:** Explicitly model temporal effects and improve directed network inference.

## Evaluation
- Compared inferred edges against a known reference regulatory network  
- Constructed ROC curves and computed AUC values  
- Interpreted performance **comparatively rather than absolutely**, given the small sample size  

| Model | AUC |
|------|-----|
| Graphical Lasso | ~0.44 |
| VAR(1) Lasso | ~0.58 |

## Results & Insights
- The VAR(1) model consistently outperformed the Gaussian Graphical Model, suggesting that incorporating temporal structure improves recovery of directed interactions.
- However, overall performance remained limited due to:
  - Small network size  
  - Short time series  
  - Measurement noise  
- These results highlight that **model choice alone cannot overcome fundamental data limitations**.

## Key Takeaway
In short, incorporating temporal structure can modestly improve network inference under constrained conditions, but inferred networks should be treated as hypothesis-generating tools rather than definitive representations of regulatory relationships. Careful interpretation is essential when working with small, noisy time-series data.

## Tools
Python, NumPy, SciPy, pandas, scikit-learn, matplotlib

## How to Run
```bash
pip install -r requirements.txt
python main_analysis.py
