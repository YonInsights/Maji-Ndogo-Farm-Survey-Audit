# 📊 Module 2: Validating Maji Ndogo's Statistical Consistency

Part of the **[Maji Ndogo Agricultural Data Pipeline & Statistical Audit](https://github.com/YonInsights/Maji-Ndogo-Farm-Survey-Audit)** portfolio project.

---

## 📌 Executive Summary
In this module, the audit shifts from exploratory profiling to **inferential statistics**. Rather than using arbitrary tolerance thresholds (e.g., ±1.5%), we apply statistical hypotheses to evaluate whether discrepancies between field survey observations and weather station IoT sensor readings are statistically significant or attributable to natural random variation.

---

## 🔬 Key Statistical Analyses & Results

### 1. Sample vs. Population Metrics & Bessel's Correction (Challenge 1)
- **Population (5,654 fields):**
  - $\mu = 0.5344$
  - $\sigma^2 = 0.012498$ ($\text{ddof}=0$)
- **Sample ($n=50$, seed 42):**
  - $\bar{x} = 0.5305$ (differs by only $\sim 0.73\%$ from population mean)
  - $s^2 = 0.010990$ ($\text{ddof}=1$, Bessel's correction applied to eliminate bias)

### 2. Central Limit Theorem (CLT) Verification (Challenge 2)
- Simulated **1,000 independent samples** of $n=30$ fields each.
- **Empirical Mean of Sample Means:** $0.5343$ (matches $\mu=0.5344$)
- **Empirical Standard Deviation:** $0.0207$
- **Theoretical Standard Error:** $\frac{\sigma}{\sqrt{30}} = \frac{0.1118}{\sqrt{30}} \approx 0.0204$
- **Takeaway:** Demonstrates that the sampling distribution of sample means follows a normal distribution centered at the population mean, validating parametric inference for downstream tests.

### 3. Parametric Confidence Intervals (Challenge 3)
- **Standardized Yield (All 5,654 fields):**
  - Mean: $0.5344$, **95% CI:** $[0.5315, 0.5373]$ (narrow width $\approx 0.0058$ indicates tight measurement precision).
- **Rainfall at Weather Station 0 ($n=1,375$ fields):**
  - Mean: $1522.89\text{ mm}$, **95% CI:** $[1499.83\text{ mm}, 1545.96\text{ mm}]$
  - **Audit Finding:** The published station mean of **$1575.95\text{ mm}$ falls outside the 95% CI**, providing statistical evidence of a potential discrepancy.

### 4. Hypothesis Testing — Station 0 (Challenge 4)
- **Rainfall One-Sample t-Test:**
  - $H_0: \mu = 1575.95\text{ mm}$ vs. $H_1: \mu \neq 1575.95\text{ mm}$
  - $t = -4.51238,\; p = 0.00001$
  - **Decision:** **Reject $H_0$** — discrepancy is statistically significant.
- **Temperature One-Sample t-Test:**
  - $H_0: \mu = 13.4039^\circ\text{C}$ vs. $H_1: \mu \neq 13.4039^\circ\text{C}$
  - $t = -0.48144,\; p = 0.63028$
  - **Decision:** **Fail to Reject $H_0$** — temperature measurements are consistent with sensor readings.

### 5. Automated Multi-Station Testing Suite (Challenge 5)
Implemented `rainfall_ttest_results(df, station_means, alpha=0.05)`:
```text
Significant difference in Rainfall at Station 0 (P-value: 0.00001 ≤ 0.05). H₀ rejected.
No significant difference in Rainfall at Station 1 (P-value: 0.06741 > 0.05). H₀ not rejected.
Significant difference in Rainfall at Station 2 (P-value: 0.00542 ≤ 0.05). H₀ rejected.
Significant difference in Rainfall at Station 3 (P-value: 0.00549 ≤ 0.05). H₀ rejected.
Significant difference in Rainfall at Station 4 (P-value: 0.00064 ≤ 0.05). H₀ rejected.
```

### 6. Type I / Type II Error Evaluation (Challenge 6)
- **Error Plausibility:** With $\alpha=0.05$, the probability of 4 simultaneous independent Type I errors is $0.05^4 \approx 6 \times 10^{-6}$ ($< 1$ in $160,000$). Even with Bonferroni correction ($\alpha = 0.01$), 3 of 4 rejections persist.
- **Audit Recommendations:** Investigate elevation differences, sensor calibration logs, and satellite precipitation data.

---

## 🏃 How to Run

From the root directory:
```bash
cd module_2
marimo edit notebook.py
```
