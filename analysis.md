# 🏦 Loan Approval Analytics Report

## AI-Powered Loan Approval Prediction System - Data Insights

---

## 📊 Executive Summary

| Metric | Value |
|--------|-------|
| **Total Applications** | 50,000 |
| **Total Approved** | 27,523 |
| **Overall Approval Rate** | 55.05% |
| **Average Loan Amount** | $37,532.42 |
| **Average Interest Rate** | 14.64% |

> [!IMPORTANT]
> The overall approval rate of **55.05%** indicates a balanced risk approach, with nearly half of applications being rejected based on creditworthiness criteria.

<details>
<summary>📝 SQL Query</summary>

```sql
-- Executive Summary KPIs
SELECT 
    SUM(total_applications) AS total_applications,
    SUM(approved_count) AS total_approved,
    ROUND(SUM(approved_count) / SUM(total_applications) * 100, 2) AS overall_approval_rate_pct,
    ROUND(AVG(avg_loan_amount), 2) AS avg_loan_amount,
    ROUND(AVG(avg_interest_rate) * 100, 2) AS avg_interest_rate_pct
FROM gold_loan_analytics;
```

</details>

---

## 📈 Key Findings

### 1. Credit Tier Analysis

Credit score is the **strongest predictor** of loan approval. Applications with "Excellent" credit have an approval rate of **89.23%**, while "Very Poor" credit drops to just **13.82%**.

![Applications by Credit Tier](Applications%20by%20Credit%20Tier.png)

| Credit Tier | Applications | Approved | Approval Rate |
|-------------|-------------|----------|---------------|
| Excellent | 2,591 | 2,312 | **89.23%** |
| Good | 7,104 | 6,020 | **84.74%** |
| Fair | 13,417 | 10,069 | **75.05%** |
| Poor | 14,395 | 7,395 | **51.37%** |
| Very Poor | 12,493 | 1,727 | **13.82%** |

<details>
<summary>📝 SQL Query</summary>

```sql
-- Approval Rate by Credit Tier
SELECT 
    credit_tier,
    SUM(total_applications) AS applications,
    SUM(approved_count) AS approved,
    ROUND(SUM(approved_count) / SUM(total_applications) * 100, 2) AS approval_rate_pct
FROM gold_loan_analytics
GROUP BY credit_tier
ORDER BY 
    CASE credit_tier 
        WHEN 'Excellent' THEN 1 
        WHEN 'Good' THEN 2 
        WHEN 'Fair' THEN 3 
        WHEN 'Poor' THEN 4 
        ELSE 5 
    END;
```

</details>

> [!NOTE]
> The majority of applications (26,888 or 53.8%) come from "Poor" and "Very Poor" credit tiers, highlighting the need for financial education and credit improvement programs.

---

### 2. Income Bracket Performance

Higher income correlates strongly with higher approval rates. The "Very High" income bracket achieves **82.91%** approval, while "Low" income sees only **45.66%**.

![Applications by Income Bracket](Applications%20by%20Income%20Bracket.png)

| Income Bracket | Applications | Avg DTI Ratio | Approval Rate |
|----------------|-------------|---------------|---------------|
| Low | 15,056 | 0.2879 | **45.66%** |
| Lower Middle | 21,260 | 0.2838 | **54.88%** |
| Upper Middle | 9,860 | 0.2906 | **62.73%** |
| High | 3,625 | 0.2952 | **72.55%** |
| Very High | 199 | 0.2691 | **82.91%** |

<details>
<summary>📝 SQL Query</summary>

```sql
-- Income Bracket Analysis with DTI
SELECT 
    income_bracket,
    SUM(total_applications) AS applications,
    ROUND(AVG(avg_dti_ratio), 4) AS avg_dti_ratio,
    ROUND(SUM(approved_count) / SUM(total_applications) * 100, 2) AS approval_rate_pct
FROM gold_loan_analytics
GROUP BY income_bracket
ORDER BY 
    CASE income_bracket 
        WHEN 'Low' THEN 1 
        WHEN 'Lower_Middle' THEN 2 
        WHEN 'Upper_Middle' THEN 3 
        WHEN 'High' THEN 4 
        ELSE 5 
    END;
```

</details>

> [!TIP]
> Despite higher incomes showing higher debt-to-income (DTI) ratios, they maintain better approval rates due to stronger overall financial stability.

---

### 3. Loan Purpose Analysis

**Education loans** have the highest approval rate at **67.51%**, while **Debt Consolidation** has the lowest at **36.65%**.

![Loan Applications Distribution by Purpose](Loan%20Applications%20Distribution%20by%20Purpose.png)

| Loan Purpose | Applications | Approved | Avg Loan Amount | Avg Risk Score | Approval Rate |
|--------------|-------------|----------|-----------------|----------------|---------------|
| Personal | 12,429 | 7,560 | $37,560.33 | 9.55 | **60.83%** |
| Education | 10,134 | 6,841 | $39,932.76 | 8.92 | **67.51%** |
| Medical | 7,598 | 4,019 | $37,564.69 | 8.68 | **52.90%** |
| Home Improvement | 7,453 | 3,989 | $35,260.77 | 9.61 | **53.52%** |
| Business | 7,469 | 3,312 | $37,864.88 | 9.04 | **44.34%** |
| Debt Consolidation | 4,917 | 1,802 | $37,032.47 | 9.09 | **36.65%** |

<details>
<summary>📝 SQL Query</summary>

```sql
-- Loan Purpose Analysis
SELECT 
    loan_intent,
    SUM(total_applications) AS total_apps,
    SUM(approved_count) AS approved,
    ROUND(AVG(avg_loan_amount), 2) AS avg_loan_amount,
    ROUND(AVG(avg_risk_score), 2) AS avg_risk_score,
    ROUND(SUM(approved_count) / SUM(total_applications) * 100, 2) AS approval_rate_pct
FROM gold_loan_analytics
GROUP BY loan_intent
ORDER BY total_apps DESC;
```

</details>

> [!WARNING]
> Debt Consolidation loans have the **highest rejection rate** (63.35%), likely due to applicants already having existing debt problems.

---

### 4. Employment Type Analysis

Surprisingly, employment status shows minimal impact on approval rates, with all categories hovering around **54-57%**.

![Loan Applications by Employment Type](Loan%20Applications%20by%20Employment%20Type.png)

| Employment Status | Applications | Avg Credit Score | Approval Rate |
|-------------------|-------------|------------------|---------------|
| Employed | 34,971 | 674 | **54.36%** |
| Self-Employed | 10,179 | 672 | **56.91%** |
| Student | 4,850 | 666 | **56.10%** |

<details>
<summary>📝 SQL Query</summary>

```sql
-- Employment Type Analysis
SELECT 
    occupation_status,
    SUM(total_applications) AS applications,
    ROUND(AVG(avg_credit_score), 0) AS avg_credit_score,
    ROUND(SUM(approved_count) / SUM(total_applications) * 100, 2) AS approval_rate_pct
FROM gold_loan_analytics
GROUP BY occupation_status
ORDER BY applications DESC;
```

</details>

> [!NOTE]
> Self-employed applicants slightly outperform employed workers, suggesting the model doesn't penalize entrepreneurship.

---

### 5. Credit Tier × Loan Purpose Heatmap Analysis

This cross-analysis reveals critical patterns:

![Applications by Loan Purpose and Credit Tier](Applications%20by%20Loan%20Purpose%20and%20Credit%20Tier.png)

<details>
<summary>📝 SQL Query</summary>

```sql
-- Credit Tier × Loan Purpose Cross-Analysis (Heatmap Data)
SELECT 
    credit_tier,
    loan_intent,
    SUM(total_applications) AS applications,
    ROUND(SUM(approved_count) / SUM(total_applications) * 100, 2) AS approval_rate_pct
FROM gold_loan_analytics
GROUP BY credit_tier, loan_intent
ORDER BY credit_tier, loan_intent;
```

</details>

#### Top Performing Segments (Approval Rate > 85%)
| Credit Tier | Loan Purpose | Applications | Approval Rate |
|-------------|--------------|-------------|---------------|
| Excellent | Personal | 658 | **90.73%** |
| Good | Education | 1,434 | **89.68%** |
| Excellent | Medical | 403 | **89.83%** |
| Excellent | Debt Consolidation | 246 | **88.62%** |
| Excellent | Education | 490 | **88.78%** |
| Excellent | Home Improvement | 390 | **88.72%** |
| Excellent | Business | 404 | **87.62%** |
| Good | Personal | 1,709 | **87.95%** |
| Fair | Education | 2,739 | **85.76%** |
| Good | Medical | 1,102 | **85.30%** |

#### Lowest Performing Segments (Approval Rate < 25%)
| Credit Tier | Loan Purpose | Applications | Approval Rate |
|-------------|--------------|-------------|---------------|
| Very Poor | Debt Consolidation | 1,237 | **1.78%** |
| Very Poor | Business | 1,825 | **3.78%** |
| Very Poor | Home Improvement | 1,800 | **9.67%** |
| Very Poor | Medical | 1,927 | **9.70%** |
| Very Poor | Personal | 3,132 | **17.69%** |
| Poor | Debt Consolidation | 1,369 | **23.45%** |

---

## 🎯 Strategic Insights

### Risk Segmentation

```
HIGH APPROVAL (>70%)          MEDIUM RISK (40-70%)         HIGH REJECTION (<40%)
├── Excellent Credit          ├── Poor Credit              ├── Very Poor Credit
├── Good Credit               │   ├── Education (70.71%)   │   ├── All purposes
├── High Income               │   ├── Personal (60.97%)    ├── Debt Consolidation
└── Education Loans           └── Fair Credit              │   └── Any credit tier
                                  └── Medical, Business    └── Business + Poor Credit
```

### Key Recommendations

1. **Target Education Loans**: Highest approval rate with lowest risk scores
2. **Caution on Debt Consolidation**: This segment has the highest rejection and may indicate existing financial distress
3. **Credit Score Improvement Programs**: With 53.8% of applicants in Poor/Very Poor tiers, there's significant opportunity for credit counseling
4. **Income-Based Pricing**: Consider tiered interest rates based on income brackets

---

## 📁 Data Sources

| File | Description |
|------|-------------|
| [SummaryKPI.csv](SummaryKPI.csv) | Overall KPI metrics |
| [Credit_Tier_Approval_Rates_Analysis.csv](Credit_Tier_Approval_Rates_Analysis.csv) | Credit tier breakdown |
| [Income_Bracket_Analysis_with_Approval_Rates_and_DTI.csv](Income_Bracket_Analysis_with_Approval_Rates_and_DTI.csv) | Income analysis |
| [Loan_Purpose_Analysis.csv](Loan_Purpose_Analysis.csv) | Loan intent breakdown |
| [Employment_Type_and_Loan_Approval_Analysis_2026_02_01_9____.csv](Employment_Type_and_Loan_Approval_Analysis_2026_02_01_9____.csv) | Employment type analysis |
| [Heatmap_Analysis_of_Loan_Applications_and_Approval_Rate____.csv](Heatmap_Analysis_of_Loan_Applications_and_Approval_Rate____.csv) | Cross-tabulation data |

---

*Generated from the Loan Approval Prediction Pipeline — Gold Analytics Layer*
