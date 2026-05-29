# POC Scope and Success Criteria

## Purpose

This document defines the scope for the initial two-week proof of concept for the **Fabric AI Customer Retention Platform**.

The purpose of the POC is to demonstrate how Microsoft Fabric can be used to transform historical transaction data into curated customer retention data products that support machine learning, explainable risk scoring, and an AI-assisted Customer Success experience.

## Guiding Principle

The use of information drives the design.

This POC is intentionally scoped around the decisions Customer Success needs to make, rather than starting with technology for its own sake. The solution should help Customer Success identify which customers may need review, understand why they are prioritized, and explore risk patterns using a grounded AI experience.

## In-Scope

### 1. Data Ingestion

The POC will ingest the available historical transaction CSV into Microsoft Fabric.

Expected output:

- Raw transaction data stored in a Bronze layer
- Source schema documented
- No manual modification of the source file

### 2. Data Cleaning and Standardization

The POC will create a cleaned transaction dataset suitable for downstream feature engineering and analytics.

Cleaning may include:

- Standardized column names
- Parsed invoice dates
- Validated customer identifiers
- Revenue calculation
- Handling of null values
- Handling of negative quantity or return/cancellation-like records
- Basic duplicate review
- Data quality observations

Expected output:

- `silver_transactions_cleaned`

### 3. Customer-Level Feature Engineering

The POC will create a customer-level ML feature table derived from transaction behavior.

Example features:

- First purchase date
- Last purchase date
- Days since last purchase
- Purchase count
- Total spend
- Average order value
- Purchase frequency
- Average days between purchases
- Product diversity
- Country
- Recent activity indicators
- Inactivity or re-engagement risk proxy label

Expected output:

- `ml_customer_retention_features`

### 4. Baseline ML Model

The POC will train and score a baseline machine learning model that estimates customer inactivity or re-engagement risk.

The model will not be positioned as a production churn prediction model because the dataset does not contain confirmed churn labels.

Expected output:

- ML training notebook
- Feature table used for training/scoring
- Basic model evaluation metrics
- Customer-level scored output

### 5. Explainable Risk Profile

The POC will create an explainable customer risk profile for Customer Success.

The risk profile should include:

- Customer ID
- Country
- Risk score
- Risk category
- Re-engagement priority
- Last purchase date
- Days since last purchase
- Total spend
- Purchase count
- Average order value
- Key reason codes
- Plain-English explanation text

Expected output:

- `ai_customer_risk_profile`

### 6. Fabric Data Agent Experience

The POC will configure a Fabric Data Agent or equivalent conversational experience grounded in curated customer retention data.

The agent should help Customer Success users ask questions such as:

- Which customers have the highest re-engagement priority?
- Why is a specific customer high risk?
- Which countries have the most high-risk customers?
- What are the most common risk reasons?
- Which high-value customers have not purchased recently?

Expected output:

- Configured agent experience
- Agent instructions / grounding rules
- Validated demo questions

### 7. Lightweight Validation Report

The POC will include lightweight validation outputs to help review the ML and risk results.

This may be a notebook output, simple Power BI page, or basic summary report.

Example validation views:

- Risk category distribution
- Top at-risk customers
- Sample customer explanations
- Model evaluation metrics
- Data quality summary

Expected output:

- Lightweight validation artifact
- Not a full executive dashboard

### 8. Documentation

The POC will include documentation covering:

- Customer scenario and requirements
- POC scope and success criteria
- Architecture and data products
- Assumptions, risks, and limitations
- Future-state recommendations

## Out of Scope

### 1. True Churn Prediction

A production churn prediction model is out of scope.

The dataset does not include confirmed churn labels, cancellation dates, subscription status, support tickets, satisfaction scores, or account manager notes. The POC will instead use an inferred inactivity or re-engagement risk proxy based on transaction behavior.

### 2. Automated Customer Actions

Automated outreach, discounts, retention campaigns, or customer interventions are out of scope.

The POC output is intended to support human-in-the-loop exploration and prioritization by Customer Success.

### 3. Real-Time or Streaming Analysis

Real-time ingestion and scoring are out of scope.

The available data is static historical transaction data. The POC will use batch processing and batch scoring.

### 4. Full Executive BI Solution

A full Power BI executive dashboard or enterprise dimensional model is out of scope.

The POC will include only lightweight validation views because the primary objective is to demonstrate AI and data engineering capabilities, including feature engineering, ML scoring, explainability, and a Fabric Data Agent experience.

### 5. Production MLOps

Full production MLOps is out of scope.

The initial POC will not include automated retraining, model drift monitoring, approval workflows, production model registry governance, or full operational monitoring.

### 6. Enterprise Security and Governance Hardening

Full production-grade security architecture is out of scope.

Security and governance considerations will be documented, but the POC will not implement a complete enterprise security model, data access model, or compliance framework.

## Acceptance Criteria

The POC will be considered successful if the following criteria are met.

### Data Engineering

- The source transaction CSV is ingested into Fabric.
- A cleaned and standardized transaction dataset is created.
- Key data quality considerations are documented.
- Customer-level behavioral features are produced.

### Machine Learning

- A customer-level ML feature table is created.
- An inactivity or re-engagement risk proxy is defined and documented.
- A baseline ML model is trained and scored.
- Basic model evaluation metrics are captured.
- Model limitations are clearly documented.

### Explainability

- Customer risk categories are created.
- Reason codes are generated.
- Plain-English customer risk explanations are produced.
- The output avoids unsupported claims about true churn, support issues, satisfaction, or cancellation reasons.

### AI Agent

- The Fabric Data Agent is grounded in curated customer risk data.
- The agent is constrained to answer from approved data products.
- A set of validated demo questions is documented.
- The agent uses cautious language such as “based on available transaction history.”

### Demo Readiness

- The project can be demonstrated end-to-end.
- The demo shows data ingestion, feature creation, ML scoring, customer risk profile output, and AI Agent exploration.
- The limitations and future-state roadmap are clearly explained.

## Assumptions

- The source transaction dataset has a stable schema for the POC.
- The dataset contains at least one year of historical transaction data.
- The dataset includes enough repeat customer behavior to support retention feature engineering.
- Customer ID is populated well enough to create customer-level features.
- Historical inactivity can be used as a proxy for re-engagement or retention risk.
- The POC uses static batch data, not live operational feeds.
- The Data Agent will be grounded only in curated Fabric data products.
- The POC is intended for exploration and demonstration, not production decision automation.

## Risks

| Risk | Impact | Mitigation |
|---|---|---|
| Poor or incomplete data quality | May limit feature reliability and model usefulness | Perform data quality checks and document limitations |
| Missing true churn labels | Model cannot be positioned as confirmed churn prediction | Use inactivity/re-engagement risk language |
| Stakeholder over-trust in model output | Users may treat score as definitive | Use reason codes, limitations, and human-in-the-loop framing |
| Unsupported AI Agent responses | Agent may answer beyond available data | Ground agent only in curated data and define clear instructions |
| Two-week timeline | Limits production hardening | Prioritize demoable AI/data workflow over full production design |
| Static historical data | Cannot demonstrate real-time scoring | Clearly scope solution as batch POC |
| Too few repeat customers | May reduce value of frequency/cadence features | Validate customer behavior distribution early |

## Dependencies

- Access to Microsoft Fabric workspace and required features
- Access to the source transaction dataset
- Ability to create Lakehouse tables and notebooks
- Ability to create or simulate ML experiment/model artifacts
- Ability to configure a Fabric Data Agent or equivalent conversational experience
- Agreement on inactivity or re-engagement risk proxy definition
- Agreement on curated fields exposed to the AI Agent
- Time-boxed review of risk explanations and demo questions

## Future Phase Opportunities

Future phases could include:

- Incorporating confirmed churn/cancellation data
- Adding support tickets and customer satisfaction scores
- Adding product usage telemetry
- Adding account manager notes or customer interaction history
- Developing richer next-best-action recommendations
- Implementing production MLOps and model monitoring
- Automating retraining and scoring pipelines
- Implementing CI/CD across dev/test/prod workspaces
- Expanding Power BI reporting for leadership and Customer Success
- Adding row-level security and enterprise governance controls
