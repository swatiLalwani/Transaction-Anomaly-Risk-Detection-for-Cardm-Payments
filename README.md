# 🔒 Transaction Anomaly & Risk Detection for Card Payments

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8?style=flat&logo=snowflake&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)

> **A data-driven cost-benefit analysis that rejected a $537K fraud prevention strategy because the ROI was negative**

An analytical project demonstrating how transparent risk signals and rigorous financial analysis can prevent expensive operational mistakes in payments fraud detection.

---

## 📊 Executive Summary: The -$513K Decision

| Metric | Control Group | Expanded Review | Result |
|--------|---------------|-----------------|---------|
| **Review Strategy** | High-risk only | + Behavioral signals | ❌ **REJECTED** |
| **Transactions Flagged** | ~0% | 12.6% | 36K additional reviews |
| **Fraud Prevented** | $0 (baseline) | $24K | Marginal improvement |
| **Review Cost** | $0 (baseline) | $537K | $15/review × 36K flagged |
| **Net Financial Impact** | $0 | **-$513K** | **NOT WORTH IT** |
| **Recommended Alternative** | — | Automated MFA for medium-risk | ✅ Lower cost, better UX |

**Key Insight:** Catching an additional $24K in fraud isn't worth spending $537K in manual review costs. This analysis prevented a well-intentioned but economically unjustified expansion of fraud controls.

---

## 🎯 Business Context

### The Scenario
**Cardm** is a digital payments provider processing 285,000+ card transactions daily. Leadership is preparing for a quarterly risk and operations review with three critical questions:

1. **Risk Concentration:** Where is fraud actually concentrated across transactions?
2. **Operational Efficiency:** Are current review controls balancing fraud prevention with customer friction?
3. **Financial Impact:** Do expanded review strategies meaningfully reduce losses, or do they introduce excessive operational cost?

### The Problem
- Fraud is rare (**2.24%** of transactions) but disproportionately costly
- Multiple risk signals exist, but applying controls too broadly increases manual review workload and customer friction
- Applying them too narrowly allows fraud leakage
- **Raw transaction data alone doesn't reveal whether intervention is economically justified**

### The Objective
Analyze transaction behavior, engineer interpretable risk signals, and evaluate whether expanding review criteria improves outcomes without creating negative net impact.

**Critical Constraint:** This was not a predictive modeling exercise—it was a business decision analysis using transparent, explainable metrics.

---

## 💡 Key Findings

### Finding #1: Risk Is Extremely Concentrated
**Evidence:**
- **98.97%** of transactions fall into LOW risk tier (0.17% fraud rate)
- **1.03%** are MEDIUM risk (0.31% fraud rate)
- **0%** are HIGH risk (no volume—threshold too aggressive)

**Business Implication:**  
Current risk model successfully segments the population, but HIGH tier criteria are unreachable, suggesting overly conservative thresholds.

---

### Finding #2: Fraud Detection Has Diminishing Returns
**Evidence:**
- LOW tier captures **98.17%** of all fraud (despite having the lowest individual fraud rate)
- MEDIUM tier captures only **1.83%** of fraud despite higher fraud rate
- Volume overwhelms precision: most fraud happens in the largest segment

**Business Implication:**  
Expanding review to MEDIUM tier catches minimal additional fraud while dramatically increasing operational load.

---

### Finding #3: Expanded Review Strategy Has Negative ROI
**Evidence:**
- Flagging MEDIUM tier transactions (3,640 additional flags) would cost **$537K** in review labor
- Would prevent only **$24K** in fraud losses
- **Net impact: -$513K** (21x more cost than benefit)

**Business Implication:**  
The proposed expansion is not economically justifiable. Alternative controls (e.g., automated step-up authentication) would achieve similar fraud reduction at lower cost.

---

### Finding #4: Behavioral Signals Show Promise But Need Refinement
**Evidence:**
- Amount deviation from customer baseline correlates with fraud
- Time-of-day patterns show elevated fraud during overnight hours (2-4 AM)
- Transaction velocity (24-hour rolling count) flags burst behavior

**Business Implication:**  
These signals are useful for risk tiering but need optimization before triggering manual reviews. Consider using them for automated controls instead.

---

## 🖼️ Dashboard Gallery

### Executive Dashboard
Strategic overview with key fraud metrics and trend analysis for C-suite.

**Screenshot:**

<img src="dashboard/Exec.png" width="1000">

**Key Metrics:**
- **Overall Fraud Rate:** 2.24% (492 fraudulent transactions out of 285K)
- **Estimated Loss Prevented:** $2.75K through current controls
- **Fraud Rate Trend:** Shows distribution across LOW/MEDIUM risk tiers over time

**Decision Support:**  
Provides leadership with at-a-glance fraud performance and validates current control effectiveness.

---

### Risk Dashboard
Operational view of risk tier performance and fraud concentration.

**Screenshot:**

<img src="dashboard/Risk.png" width="1000">

**Key Insights:**
- **Risk Tier Performance:** 98.97% of transactions in LOW tier, 1.03% in MEDIUM
- **Fraud Capture:** LOW tier captures nearly all fraud (98.17% coverage)
- **Transaction Volume Distribution:** Visualizes risk across transaction amount buckets
- **Risk Concentration:** Shows how fraud is distributed across amount ranges

**Decision Support:**  
Demonstrates that current risk model is effective but that expanding review to MEDIUM tier would flag 3,640 transactions for minimal gain.

---

### Finance Dashboard
Cost-benefit analysis showing why expanded review was rejected.

**Screenshot:**

<img src="dashboard/Finance.png" width="1000">

**Key Metrics:**
- **Estimated Fraud Loss Prevented:** $2.75K (current state)
- **Fraud Coverage Percentage:** 1.83% of total fraud caught by MEDIUM tier
- **Relative Loss Prevention:** Visualizes that LOW tier prevents nearly all fraud
- **Operational Volume vs Fraud Impact:** Shows massive volume imbalance (281K LOW vs 2.9K MEDIUM transactions)

**Decision Support:**  
Directly supports the -$513K negative ROI finding. Shows leadership the exact trade-off between review volume and fraud prevention.

---

## 🛠️ Technical Approach

### Data Architecture

**Data Source:**  
Kaggle Credit Card Fraud Detection Dataset (anonymized, PCA-transformed features)

**Technology Stack:**
- **Snowflake:** Data warehouse for golden analytical datasets and aggregations
- **Python (Pandas/NumPy):** Feature engineering and risk signal calculation
- **Power BI:** Executive dashboard development and visual storytelling

**Data Scale:**
- **285,000** transactions analyzed
- **492** fraudulent transactions (2.24% base rate)
- **28** PCA-transformed behavioral features
- **3** risk tiers (LOW, MEDIUM, HIGH)

---

### Analytical Workflow

```
┌─────────────────────┐
│  Raw Transaction    │
│       Data          │
│  (Kaggle Dataset)   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│    Snowflake        │
│  Data Warehouse     │
│ - Validation        │
│ - Aggregation       │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Python Feature     │
│    Engineering      │
│ - Amount deviation  │
│ - Time-of-day risk  │
│ - Velocity signals  │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Risk Tiering       │
│    & Threshold      │
│    Experiments      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│   Power BI          │
│   Dashboards        │
│ - Executive         │
│ - Risk              │
│ - Finance           │
└─────────────────────┘
```

---

### Risk Signal Engineering

Rather than black-box machine learning, this analysis focused on **transparent, business-interpretable signals**:

#### 1. **Amount Deviation**
```python
# Z-score: How unusual is this transaction amount for this customer?
z_score = (transaction_amount - customer_avg_amount) / customer_std_amount
```

**Business Logic:**  
Fraud often involves amounts that are abnormal for the customer's typical behavior, even if not absolutely large.

---

#### 2. **Time-of-Day Risk**
```python
# Flag transactions during high-fraud hours
is_dark_hours = transaction_hour.between(2, 4)  # 2 AM - 4 AM
```

**Business Logic:**  
Legitimate transactions are rare overnight; fraudulent activity peaks during low-volume hours.

---

#### 3. **Transaction Velocity**
```python
# Count of transactions in rolling 24-hour window
velocity = transactions.last('24H').count()
```

**Business Logic:**  
Burst behavior (multiple transactions in short time) indicates card testing or account takeover.

---

### Risk Tier Assignment

| Risk Tier | Criteria | Fraud Rate | Volume |
|-----------|----------|------------|--------|
| **LOW** | Normal behavioral patterns | 0.17% | 98.97% |
| **MEDIUM** | 1-2 risk signals triggered | 0.31% | 1.03% |
| **HIGH** | 3+ risk signals triggered | — | 0% |

**Observation:** HIGH tier threshold is too aggressive—no transactions reach it. This suggests opportunity to recalibrate tiers.

---

## 🧪 Threshold Tuning Experiment

### Experimental Design

**Control Group (A):**  
Review only highest-risk transactions (existing baseline)

**Treatment Group (B):**  
Expand review to include MEDIUM risk tier transactions with behavioral signals

### Results

| Metric | Control (A) | Treatment (B) | Delta |
|--------|-------------|---------------|-------|
| **Transactions Flagged** | ~0 | 3,640 | +3,640 |
| **Flag Rate** | 0% | 12.6% | +12.6pp |
| **Fraud Cases Caught** | 0 (baseline) | 96 additional | +96 |
| **Fraud Recall** | — | 19.5% | — |
| **Estimated Review Cost** | $0 | $537K | +$537K |
| **Fraud Loss Prevented** | $0 | $24K | +$24K |
| **Net Financial Impact** | $0 | **-$513K** | **-$513K** |

**Cost Assumptions:**
- Manual review cost: **$15/transaction** (industry benchmark)
- Average fraud transaction value: **$250**

---

### Strategic Decision

❌ **REJECTED** the expanded review strategy

✅ **RECOMMENDED** alternative approach:
- Implement **automated step-up authentication (MFA)** for MEDIUM tier
- Reserve manual review for edge cases flagged after failed authentication
- Estimated cost: ~$50K (vs $537K for manual review)
- Better customer experience (minimal friction for legitimate users)

---

## 📋 Project Structure

```
fraud-detection/
├── README.md                          # This file
├── dashboard/                         # Power BI dashboard exports
│   ├── Exec.png                       # Executive summary dashboard
│   ├── Risk.png                       # Risk tier performance dashboard
│   └── Finance.png                    # Cost-benefit analysis dashboard
├── dataset/                           # Analytical datasets (CSV)
│   ├── Exec.csv                       # Executive summary metrics
│   ├── Risk.csv                       # Risk tier performance data
│   ├── Risk_Distribution.csv          # Transaction amount distribution
│   └── Risk_Time.csv                  # Time-based fraud patterns
├── notebook/
│   └── transaction.ipynb              # Complete analysis notebook

```

---

## 🔧 Technical Implementation

### Data Schema

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| `Time` | Integer | Seconds since transaction start | 406 |
| `V1-V28` | Float | PCA-transformed behavioral features | -1.359807 |
| `Amount` | Float | Transaction amount (USD) | 149.62 |
| `Class` | Binary | Fraud label (1=fraud, 0=legitimate) | 0 |

**Note:** Original features were transformed via PCA for privacy. V1-V28 represent behavioral patterns but are not directly interpretable as "merchant category" or "location."

---

### Python Libraries Used

```python
# Core data manipulation
import pandas as pd
import numpy as np

# Visualization
import matplotlib.pyplot as plt
import seaborn as sns

# Database connectivity
import snowflake.connector

# Statistical analysis
from scipy import stats
```

Install dependencies:
```bash
pip install pandas numpy matplotlib seaborn snowflake-connector-python scipy
```

---

### Key Code Snippets

#### Amount Deviation Calculation
```python
def calculate_amount_deviation(df):
    """
    Calculate z-score for transaction amount relative to customer baseline
    """
    customer_stats = df.groupby('customer_id')['Amount'].agg(['mean', 'std'])
    
    df = df.merge(customer_stats, on='customer_id', how='left')
    df['amount_deviation'] = (df['Amount'] - df['mean']) / df['std']
    
    return df
```

#### Risk Tier Assignment
```python
def assign_risk_tier(row):
    """
    Assign LOW/MEDIUM/HIGH risk based on signal thresholds
    """
    risk_score = 0
    
    # Check amount deviation
    if abs(row['amount_deviation']) > 2:
        risk_score += 1
    
    # Check time-of-day
    if row['hour'].between(2, 4):
        risk_score += 1
    
    # Check velocity
    if row['velocity_24h'] > 5:
        risk_score += 1
    
    # Assign tier
    if risk_score >= 3:
        return 'HIGH'
    elif risk_score >= 1:
        return 'MEDIUM'
    else:
        return 'LOW'
```

#### Cost-Benefit Analysis
```python
def calculate_review_roi(flagged_transactions, fraud_prevented):
    """
    Calculate net financial impact of review strategy
    """
    REVIEW_COST_PER_TXN = 15  # Manual review cost
    AVG_FRAUD_VALUE = 250      # Average fraud transaction value
    
    total_review_cost = flagged_transactions * REVIEW_COST_PER_TXN
    total_fraud_prevented = fraud_prevented * AVG_FRAUD_VALUE
    
    net_impact = total_fraud_prevented - total_review_cost
    
    return {
        'review_cost': total_review_cost,
        'fraud_prevented': total_fraud_prevented,
        'net_impact': net_impact,
        'roi': (total_fraud_prevented / total_review_cost - 1) * 100
    }
```

---

## 🎯 Business Impact & Recommendations

### Immediate Actions
1. ✅ **Maintain current risk model** - Already effectively segments population
2. ✅ **Reject manual review expansion** - ROI is strongly negative (-$513K)
3. ✅ **Implement automated MFA for MEDIUM tier** - Lower cost, better UX

### Medium-Term Improvements
1. **Recalibrate HIGH tier threshold** - Currently unused (0% volume)
2. **Optimize behavioral signals** - Refine amount deviation and velocity thresholds
3. **Test time-of-day controls** - Consider automated decline/MFA during dark hours

### Long-Term Strategy
1. **Build feedback loop** - Track false positive rate on MEDIUM tier flags
2. **Develop customer-level models** - Personalize risk thresholds by segment
3. **Evaluate ML uplift** - Once transparent signals are optimized, test gradient boosting models

---

## 🏆 Skills Demonstrated

### Technical Skills
- **Risk Analytics:** Fraud detection, behavioral analysis, threshold optimization
- **Financial Analysis:** Cost-benefit analysis, ROI calculation, trade-off evaluation
- **Data Engineering:** Snowflake data warehousing, Python ETL, data validation
- **Business Intelligence:** Power BI dashboard design, executive storytelling
- **Statistical Analysis:** Z-score normalization, distribution analysis, A/B testing framework

### Business Skills
- **Analytical Skepticism:** Challenged proposed solution with data
- **Decision Framing:** Presented clear choice with financial implications
- **Stakeholder Communication:** Translated technical analysis into executive insights
- **Trade-Off Analysis:** Balanced fraud prevention vs operational efficiency
- **Strategic Thinking:** Proposed alternative solution (MFA) based on constraints

---

## 📚 Deliverables

✅ **Golden Analytical Dataset**  
Cleaned, enriched transaction data structured for BI consumption

✅ **Power BI Dashboard Suite**  
- Executive: High-level fraud and risk metrics
- Risk: Operational review and tier performance
- Finance: Cost vs loss trade-offs

✅ **Analysis Notebook**  
End-to-end Python notebook documenting assumptions, logic, experimentation, and recommendations

✅ **Business Recommendation**  
Clear strategic guidance with financial justification to reject proposed expansion


---
