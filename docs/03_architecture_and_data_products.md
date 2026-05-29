# Architecture and Data Products

## Purpose

This document describes the high-level architecture and data products for the **Fabric AI Customer Retention Platform** proof of concept.

The architecture is designed to support three primary outcomes:

1. Data engineering foundation in Microsoft Fabric
2. ML-based inactivity or re-engagement risk scoring
3. AI Agent exploration grounded in curated customer risk data

## High-Level Architecture

```text
Raw Transaction CSV
        |
        v
Fabric Lakehouse - Bronze
Raw transaction history
        |
        v
Fabric Lakehouse - Silver
Cleaned and validated transactions
        |
        v
Fabric Lakehouse - Gold
Curated customer behavior data products
        |
        +-----------------------------+
        |                             |
        v                             v
ML Feature Table              Lightweight BI / Validation View
Customer-level features       Risk distribution, top customers
        |
        v
ML Model + Scoring Notebook
Inactivity / re-engagement risk
        |
        v
AI-Ready Customer Risk Profile
Risk score, category, reason codes, explanations
        |
        v
Fabric Data Agent
Natural-language exploration for Customer Success
```

## Architecture Summary

The POC starts with a raw historical transaction CSV. The data is ingested into a Microsoft Fabric Lakehouse and organized using a lightweight medallion architecture.

The Bronze layer stores the raw source data. The Silver layer stores cleaned, standardized, and validated transactions. The Gold layer contains curated data products designed for specific consumers, including ML, AI Agent exploration, and lightweight validation reporting.

The ML workflow uses a customer-level feature table to train and score a baseline inactivity or re-engagement risk model. The model output is combined with business-friendly reason codes and explanation text to create an AI-ready customer risk profile.

The Fabric Data Agent is grounded in the curated risk profile, allowing Customer Success users to ask natural-language questions about customer risk, re-engagement priority, risk drivers, and customer segments.

## Medallion Layers

### Bronze Layer

The Bronze layer stores the raw transaction data as ingested.

Example table:

- `bronze_online_retail_transactions`

Purpose:

- Preserve source data
- Support reproducibility
- Avoid manual source modifications
- Provide traceability into downstream transformations

### Silver Layer

The Silver layer stores cleaned and standardized transaction data.

Example table:

- `silver_transactions_cleaned`

Typical transformations:

- Standardize column names
- Parse invoice dates
- Validate customer IDs
- Calculate revenue
- Handle nulls
- Review returns or negative quantities
- Normalize country values where appropriate
- Add processing metadata

### Gold Layer

The Gold layer stores curated data products.

Example tables:

- `gold_customer_purchase_summary`
- `ml_customer_retention_features`
- `ml_customer_retention_predictions`
- `ai_customer_risk_profile`
- `gold_retention_validation_summary`

The Gold layer is intentionally separated into purpose-built data products rather than a single generic reporting model.

## Data Product Design

### 1. Customer Purchase Summary

Example table:

- `gold_customer_purchase_summary`

Purpose:

Provide a customer-level behavioral summary that can support validation, reporting, and feature engineering.

Example fields:

| Field | Description |
|---|---|
| CustomerID | Customer identifier |
| Country | Customer country |
| FirstPurchaseDate | First observed purchase date |
| LastPurchaseDate | Most recent observed purchase date |
| PurchaseCount | Count of unique purchases/invoices |
| TotalSpend | Total customer spend |
| AvgOrderValue | Average spend per order |
| ProductDiversity | Count of distinct products purchased |
| ActivePurchaseSpanDays | Days between first and last purchase |
| DaysSinceLastPurchase | Days since most recent purchase |

### 2. ML Customer Retention Features

Example table:

- `ml_customer_retention_features`

Purpose:

Provide a flattened customer-level feature table for model training and scoring.

Example fields:

| Field | Description |
|---|---|
| CustomerID | Customer identifier |
| Country | Customer country or encoded country feature |
| DaysSinceLastPurchase | Recency feature |
| PurchaseCount | Frequency feature |
| TotalSpend | Monetary value feature |
| AvgOrderValue | Average customer order value |
| AvgDaysBetweenPurchases | Purchase cadence feature |
| ProductDiversity | Breadth of purchased products |
| RecentPurchaseCount | Purchase count in recent period |
| HistoricalPurchaseCount | Purchase count before recent period |
| RecentSpend | Spend in recent period |
| HistoricalSpend | Spend before recent period |
| InactivityRiskProxyLabel | Inferred label for model training |

### 3. ML Customer Retention Predictions

Example table:

- `ml_customer_retention_predictions`

Purpose:

Store customer-level model scoring output.

Example fields:

| Field | Description |
|---|---|
| CustomerID | Customer identifier |
| ScoreDate | Date model scoring was performed |
| RiskScore | Model-generated risk score or probability |
| PredictedRiskClass | Model-generated class |
| ModelName | Name of model used for scoring |
| ModelVersion | Model version or run identifier |
| FeatureSnapshotDate | Date of feature snapshot |

### 4. AI Customer Risk Profile

Example table:

- `ai_customer_risk_profile`

Purpose:

Provide a business-friendly and AI-ready data product for Customer Success exploration and Fabric Data Agent grounding.

Example fields:

| Field | Description |
|---|---|
| CustomerID | Customer identifier |
| Country | Customer country |
| RiskScore | ML or combined risk score |
| RiskCategory | Low, Medium, or High |
| ReEngagementPriority | Business-friendly priority ranking |
| LastPurchaseDate | Most recent purchase date |
| DaysSinceLastPurchase | Key recency metric |
| TotalSpend | Total customer spend |
| PurchaseCount | Historical purchase count |
| AvgOrderValue | Average order value |
| ReasonCode1 | Primary reason customer was flagged |
| ReasonCode2 | Secondary reason customer was flagged |
| ExplanationText | Plain-English explanation of risk |
| AgentSafeSummary | Short summary designed for AI Agent responses |

## Risk Language

Because the dataset does not contain confirmed churn or cancellation records, the solution should avoid definitive churn language.

Preferred language:

- Inactivity risk
- Re-engagement priority
- Retention risk indicator
- Customer review priority
- Based on available transaction history

Avoid language:

- Guaranteed churn
- Confirmed cancellation risk
- Customer will churn
- Definitive churn prediction

## Explainability Approach

The model score alone is not sufficient for Customer Success users. Each scored customer should also have business-readable explanation fields.

Example reason codes:

- `HIGH_VALUE_INACTIVE`
- `LONG_RECENCY_GAP`
- `DECLINING_PURCHASE_FREQUENCY`
- `LOW_RECENT_ACTIVITY`
- `HISTORICALLY_FREQUENT_NOW_INACTIVE`

Example explanation text:

> Customer 17850 is marked High Re-Engagement Priority because they have not purchased in 145 days, compared to their prior average purchase cadence of 32 days, and they have historically generated high total spend.

## Fabric Data Agent Grounding

The Fabric Data Agent should be grounded only in curated data products, especially:

- `ai_customer_risk_profile`
- `gold_retention_validation_summary`
- Optionally, a semantic model built on top of the curated risk data

The agent should not be grounded directly in raw Bronze data.

## Agent Instruction Example

```text
You are a Customer Retention Data Agent for the Fabric AI Customer Retention Platform POC.

Answer questions only using the curated customer retention risk profile and approved validation summaries.

Use cautious language such as "based on available transaction history" or "this customer is flagged for review."

Do not claim that a customer will churn. Do not reference support tickets, satisfaction scores, cancellation reasons, account manager notes, or product usage telemetry because those data sources are not included in this POC.

When explaining customer risk, use the available risk score, risk category, re-engagement priority, reason codes, and customer behavior metrics.
```

## Validated Agent Questions

The following questions should be tested during the demo:

- Which customers have the highest re-engagement priority?
- Why is Customer `<CustomerID>` marked high risk?
- Which countries have the most high-risk customers?
- What are the most common risk reasons?
- Which high-value customers have not purchased recently?
- Summarize the top retention risks based on available transaction history.
- What data is this risk score based on?
- What are the limitations of this POC?

## Minimal BI / Validation View

Although a full executive dashboard is out of scope, a lightweight validation artifact should be included.

Suggested visuals or notebook outputs:

- Risk category distribution
- Top 10 high-priority customers
- Total customers by risk category
- Average days since last purchase by risk category
- Sample risk explanations
- Basic model evaluation metrics

## Repository Alignment

Recommended repo structure:

```text
fabric-ai-customer-retention-platform/
│
├── README.md
├── docs/
│   ├── 00_documentation_index.md
│   ├── 01_customer_scenario_and_requirements.md
│   ├── 02_poc_scope_and_success_criteria.md
│   └── 03_architecture_and_data_products.md
│
├── data/
│   └── README.md
│
├── notebooks/
│   ├── 01_ingest_bronze.ipynb
│   ├── 02_clean_silver.ipynb
│   ├── 03_feature_engineering.ipynb
│   ├── 04_train_score_model.ipynb
│   └── 05_create_ai_risk_profile.ipynb
│
├── agent/
│   ├── agent_instructions.md
│   └── validated_questions.md
│
├── validation/
│   └── README.md
│
└── src/
    └── README.md
```

## Design Rationale

This architecture demonstrates that dimensional modeling and BI skills are still valuable, but the project intentionally prioritizes AI and data engineering outcomes.

The solution does not simply create a dashboard from a CSV.

It creates a governed data flow from raw data to curated AI-ready data products:

```text
CSV -> Lakehouse -> Features -> ML Score -> Explainable Risk Profile -> Data Agent
```

This makes the project more aligned to AI Engineering, Data Engineering, and Architect-level interview discussions.
