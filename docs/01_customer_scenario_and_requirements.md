# Customer Scenario and Requirements

## Project Name

Fabric AI Customer Retention Platform

## Document Purpose

This document captures the detailed simulated customer scenario and requirements for the project.

A shorter project summary is included in the root `README.md` so interviewers and reviewers can quickly understand the project. This document provides the deeper requirements and business context that would normally come from a lightweight discovery and scoping exercise.

## Scenario Overview

A customer-facing retail/subscription-style business wants to become more proactive in identifying customers who may be at risk of becoming inactive or disengaged.

Today, customer retention analysis is mostly reactive. Business users review historical reporting, exported spreadsheets, and basic customer activity trends after customers have already reduced purchasing activity or stopped purchasing altogether.

Leadership wants to understand whether historical transaction data can be used to create a more proactive customer retention intelligence capability.

The goal of this proof of concept is to simulate a realistic customer engagement where Microsoft Fabric is used to transform raw customer transaction history into curated data products that support machine learning, explainable risk scoring, and a Fabric Data Agent experience for Customer Success users.

## Business Problem

Customer Success does not currently have a consistent, proactive way to identify customers who may need re-engagement.

The business currently has visibility into basic reporting, but lacks:

- A curated customer-level view of purchasing behavior
- A prioritized list of customers for retention outreach
- Explainable indicators showing why a customer may be at risk
- A natural language experience for exploring customer retention risk
- A repeatable data engineering foundation for future AI and ML use cases

## Primary User Group

The initial proof of concept is focused on the **Customer Success** team.

Customer Success users need to understand which customers may require attention before inactivity becomes permanent.

They need decision support that helps answer:

- Which customers should we review first?
- Which customers appear to be slowing down?
- Which high-value customers have not purchased recently?
- Why is a customer considered high priority for re-engagement?
- Which countries or segments show stronger or weaker repeat purchasing behavior?

## Secondary Stakeholders

While Customer Success is the primary user group, the following stakeholders may also benefit from the POC outputs:

- Leadership / Executive Sponsors
- Sales or Account Management
- Data and Analytics Teams
- AI / Data Platform Teams

For the initial POC, these stakeholders are considered consumers of demo outcomes, not primary design personas.

## Available Source Data

For this simulated engagement, the available dataset is a single historical retail transaction CSV.

The dataset includes fields similar to:

| Field | Description |
|---|---|
| Invoice Number | Transaction or invoice identifier |
| Product Code | Product or stock identifier |
| Description | Product description |
| Quantity | Quantity purchased |
| Invoice Date | Date and time of transaction |
| Price | Unit price |
| Customer ID | Customer identifier |
| Country | Customer or transaction country |

## Important Data Limitations

The available dataset does **not** include:

- Confirmed churn or cancellation records
- Subscription status
- Support ticket history
- Customer satisfaction scores
- Product usage telemetry
- Account manager notes
- Customer contact history
- Marketing campaign engagement
- Explicit customer feedback

Because of these limitations, the first version will not be positioned as a production churn prediction system.

Instead, it will estimate **inactivity risk** or **re-engagement priority** using transaction-derived customer behavior.

## Business Objective

Enable the Customer Success team to proactively identify and prioritize customers who may need re-engagement by transforming historical transaction data into explainable customer retention risk indicators, ML-driven risk scores, and an AI-assisted exploration experience.

## Initial POC Objective

Build a Microsoft Fabric-based proof of concept that ingests historical transaction data, engineers customer-level retention features, trains and scores a baseline ML model using an inactivity-risk proxy, creates an explainable customer risk profile, and exposes the results through a Fabric Data Agent experience.

## Key Business Questions

The POC should support exploration of questions such as:

- Who are our highest-value customers?
- Which customers have not purchased recently?
- Which customers used to purchase frequently but appear to be slowing down?
- Which customers should Customer Success review first?
- Which countries have the highest concentration of high-risk customers?
- What are the most common risk reasons?
- Why is a specific customer marked as high re-engagement priority?

## Functional Requirements

The solution should:

1. Ingest the available transaction dataset into Microsoft Fabric.
2. Clean and standardize the transaction data.
3. Create customer-level behavioral features.
4. Define an inactivity or re-engagement risk proxy.
5. Train and score a baseline ML model.
6. Generate risk categories and reason codes.
7. Create an AI-ready customer risk profile.
8. Configure a Fabric Data Agent or equivalent conversational experience grounded in curated data.
9. Provide lightweight validation outputs for reviewing model and risk results.
10. Document assumptions, limitations, and future-state opportunities.

## Non-Functional Requirements

The solution should be:

- Explainable enough for Customer Success users to understand why a customer is flagged
- Scoped appropriately for a two-week POC
- Grounded in curated data rather than raw source data
- Honest about data limitations and proxy-based risk scoring
- Organized in a way that supports portfolio review and technical interview discussion
- Designed as a foundation that could be extended in future phases

## Success Definition

The POC will be successful if it demonstrates a complete retention intelligence workflow:

```text
Raw Transaction Data
    -> Curated Fabric Data Products
    -> ML Feature Engineering
    -> Inactivity / Re-Engagement Risk Scoring
    -> Explainable Customer Risk Profile
    -> Fabric Data Agent Exploration
```

The focus is not to deliver a production churn platform.

The focus is to demonstrate how AI and data engineering capabilities can be combined in Microsoft Fabric to support proactive customer retention analysis.
