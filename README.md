# IBM HR Employee Attrition — Probability & Statistical Analysis

A two-part probabilistic and statistical exploration of the IBM HR Employee Attrition dataset (1,470 employees, 35 variables), applying descriptive statistics, probability theory, and random variable theory to understand workforce attrition and compensation structure.

This is a probability/statistics-focused companion to the [IBM HR Attrition business analysis](https://github.com/TianjinDuan01/ibm-hr-attrition-analysis) (Tableau dashboard + cost modeling for a business audience). Where that repo answers *"who is leaving and why"* for stakeholders, this analysis builds the underlying probabilistic model: joint/conditional probability, Bayes' theorem, random variable distributions, conditional expectation, and covariance/correlation structure.

[Part 1 Report (PDF)](./Part1_Descriptive_Stats_and_Probability.pdf) · [Part 2 Report (PDF)](./Part2_Random_Variables_and_Correlation.pdf) · [Part 1 Source (Rmd)](./part1_analysis.Rmd) · [Part 2 Source (Rmd)](./part2_analysis.Rmd)

---

## Part 1 — Descriptive Statistics & Probability Analysis

### Dataset Overview
Six variables selected across categorical (Attrition, Department, OverTime) and numeric (Age, MonthlyIncome, YearsAtCompany) types to balance individual-level and organizational-level attrition drivers.

| Categorical Variable | Breakdown |
|---|---|
| Attrition | 83.9% stayed / 16.1% left |
| Department | R&D 65.4% · Sales 30.3% · HR 4.3% |
| OverTime | 71.7% standard hours / 28.3% overtime |

| Numeric Variable | Min | Median | Mean | Max |
|---|---|---|---|---|
| Age | 18 | 36 | 36.92 | 60 |
| MonthlyIncome | $1,009 | $4,919 | $6,503 | $19,999 |
| YearsAtCompany | 0 | 5 | 7.01 | 40 |

### Probability Analysis: Attrition vs. OverTime

Using joint probability, conditional probability, and Bayes' theorem on the Attrition × OverTime contingency table:

- **P(Attrition | OverTime = Yes) = 30.5%** vs. **P(Attrition | OverTime = No) = 10.4%** — overtime nearly **triples** attrition risk.
- Among employees who left, **53.6%** had worked overtime, despite overtime workers making up only 28.3% of the workforce — a striking overrepresentation.
- Bayes' theorem confirms the result from the reverse direction: P(Attrition=Yes | OverTime=Yes) = P(OverTime=Yes|Attrition=Yes) × P(Attrition=Yes) / P(OverTime=Yes) ≈ **30.5%**.

### Probability Analysis: Attrition vs. Department

- **Sales has the highest attrition rate (20.6%)**, followed by HR (19.1%) and R&D (13.8%) — despite R&D contributing the most total departures in absolute terms (its size dominates the workforce).
- Sales is disproportionately represented among leavers (38.8% of all departures vs. 30.3% of headcount), pointing to department-specific stressors (quotas, travel, client pressure) rather than a company-wide issue.

### Random Variables: PMF & CDF

**YearsAtCompany (discrete):**
- Mean = 7.01 years, Variance = 37.53, SD = 6.13 — high dispersion relative to the mean indicates two distinct populations: short-tenure employees who cycle through quickly, and long-tenured "career" employees.
- Mode = 5 years (13.3% probability); **52.8% of employees have ≤5 years of tenure**, per the empirical CDF.

**MonthlyIncome (continuous):**
- PDF peak (mode) ≈ $2,883, well below the mean ($6,503) — confirming a right-skewed distribution.
- **50.95% of employees earn ≤ $5,000/month.**
- A Q-Q plot against the normal distribution shows clear deviation at both tails (heavy right tail, firm lower bound), reinforcing that income is not normally distributed.

---

## Part 2 — Continuous Random Variables & Multivariate Relationships

Builds on Part 1 by formally modeling **MonthlyIncome** as a continuous random variable and extending to multivariate relationships with **Age**, **JobLevel**, and **YearsAtCompany**.

### Distribution of Monthly Income

| Mean | SD | Variance | Skewness |
|---|---|---|---|
| $6,502.93 | $4,707.96 | 22,164,857 | 1.37 |

The gap between mean ($6,503) and median ($4,919), combined with skewness of 1.37, confirms a strongly right-skewed distribution more consistent with a **log-normal or gamma** shape than normal — driven by IBM's hierarchical pay structure rather than continuous scaling.

### Conditional Expectation by Job Level

| Job Level | n | Mean Income | SD |
|---|---|---|---|
| 1 | 543 | $2,786.92 | $748.63 |
| 2 | 534 | $5,502.28 | $1,410.03 |
| 3 | 218 | $9,817.25 | $1,806.00 |
| 4 | 106 | $15,503.78 | $1,816.24 |
| 5 | 69 | $19,191.83 | $512.38 |

Job level drives a nearly **7x increase** in expected income from Level 1 to Level 5. Notably, variance is *lowest* at Level 5 despite the highest pay — executive compensation appears tightly standardized (board governance, benchmarking), while mid-level pay shows the most individual variation.

### Covariance & Correlation

| Variable Pair | Covariance | Correlation |
|---|---|---|
| Income & Age | 21,412.2 | 0.498 |
| Income & Years at Company | 14,833.7 | 0.514 |
| Age & Years at Company | — | 0.311 |

Both age and tenure show moderate positive correlation with income (r ≈ 0.50), each explaining roughly 25% of income variance individually. The weak Age–Tenure correlation (0.311) indicates IBM hires experienced professionals at all career stages rather than growing talent exclusively from within.

---

## Methodology

Built in R (R Markdown), using base R and `dplyr`/`ggplot2`. Core techniques across both parts:
- Descriptive statistics (mean, quantiles, frequency tables) for categorical and numeric variables
- Joint, marginal, and conditional probability from contingency tables; Bayes' theorem
- Probability mass function (discrete) and kernel density estimation (continuous), with empirical CDFs
- Conditional expectation via job-level stratification
- Covariance and Pearson correlation; Q-Q plot for normality assessment

## Limitations & Extensions

- Correlation analysis assumes linear relationships; income growth may be non-linear across a career — polynomial or spline regression could capture this better.
- Correlation does not establish causation; observed age/income relationships may reflect cohort effects or survivorship bias rather than within-person growth.
- Categorical factors beyond Department (education field, gender) were excluded from Part 2; regression with dummy variables could surface pay gaps or departmental premiums after controlling for experience.
- Conditional variance (heteroscedasticity across job levels) was observed but not formally tested.

## Files

- `part1_analysis.Rmd` — R Markdown source for Part 1 (data cleaning, descriptive statistics, probability analysis, PMF/CDF)
- `Part1_Descriptive_Stats_and_Probability.pdf` — Part 1 rendered report
- `part2_analysis.Rmd` — R Markdown source for Part 2 (continuous random variables, conditional expectation, covariance/correlation)
- `Part2_Random_Variables_and_Correlation.pdf` — Part 2 rendered report

## Related Work

- [IBM HR Attrition Analysis — Who Is Leaving & Why](https://github.com/TianjinDuan01/ibm-hr-attrition-analysis): a business-facing Tableau dashboard and cost analysis using the same dataset.

## Acknowledgments

Coursework completed for the Statistical Analysis module sequence at Johns Hopkins Carey Business School. Dataset: [IBM HR Analytics: Employee Attrition & Performance](https://www.kaggle.com/code/faressayah/ibm-hr-analytics-employee-attrition-performance) (Kaggle, Sayah 2022).
