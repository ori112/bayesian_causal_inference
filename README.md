# Bayesian Causal Inference: The Lalonde Job Training Dataset

##  Project Overview
This project uses **Bayesian Causal Inference** to estimate the true impact of the National Supported Work (NSW) demonstration—a job training program from the 1970s.

Using the known **Lalonde Dataset**, this analysis tackles a classic causal paradox where raw data comparisons are misleading due to selection bias and zero-inflated earnings. By implementing a **Two-Part Bayesian Hurdle Model**, we successfully isolate the treatment effect from confounding variables like age, education, and prior earnings.

---

## "The Zero Problem"
Real-world income data is difficult to model for two reasons:
1.  **Zero-Inflation:** 31% of participants earned $0 (unemployed), creating a massive spike that breaks standard "Bell Curve" regressions.
2.  **Selection Bias:** The treated group was fundamentally different from the control group (younger, less educated, lower past earnings).

**The Raw Paradox:**
A naive comparison of means suggests the program **failed**:
* **Control Group Avg Earnings:** ~$6,984
* **Treated Group Avg Earnings:** ~$6,349
* *Implied Effect:* -$635 (The training seemed to hurt participants!)

---

## The Solution: Bayesian Hurdle Model

### 1. The Employment Model (Bernoulli)
* **Question:** Did the program help people find a job?
* **Method:** Logistic Regression (Sigmoid link) predicting the probability of employment ($re78 > 0$).

### 2. The Earnings Model (LogNormal)
* **Question:** For those employed, did the program increase wages?
* **Method:** Log-Linear Regression modeling the positive "tail" of income.

### 3. Confounder Adjustment
Both models simultaneously adjust for 8 confounding variables to block non-causal paths:
* `age`, `education`, `race` (Black/Hispanic), `marriage`, `degree`, `re74`, `re75`.

---

## 📊 Key Findings: The Causal Flip
After adjusting for confounders and correctly modeling the zeros, the results **flipped** from negative to strongly positive.

| Metric | Result | Interpretation |
| :--- | :--- | :--- |
| **Probability of Superiority** | **~99.9%** | We are nearly certain the program was beneficial. |
| **Average Treatment Effect (ATE)** | **+$3,500** | On average, the program increased annual earnings by ~$3,500. |
| **Odds Ratio ($\gamma_w$)** | **1.48x** | Treated participants were **1.5x more likely** to find a job. |
| **Wage Multiplier ($\beta_w$)** | **1.40x** | Employed participants earned **40% higher wages** than peers. |

---

## Dependencies
* python 3.11+
* pymc
* arviz
* pandas & numpy 
* matplotlib & seaborn