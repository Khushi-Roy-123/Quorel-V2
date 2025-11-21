# Quorel - Real-Time Explainable Credit Intelligence Platform

## Live Demo
Access the live platform here: [https://quorel-v2.onrender.com/](https://quorel-v2.onrender.com/)

## Problem Statement
Traditional credit scoring models operate as "black boxes," providing scores without clear explanations for how those scores were derived. This lack of transparency leads to distrust from investors and regulators, as they cannot fully understand or validate the scoring process. There is a pressing need for a real-time, explainable, and evidence-backed credit intelligence system that continuously processes multiple data sources and offers transparent, actionable insights.

## Solution Overview
Quorel addresses this issue by building a real-time credit intelligence platform that ingests financial, operational, macroeconomic, and unstructured data from diverse sources. It generates creditworthiness scores for issuers and asset classes that update faster than traditional ratings. Crucially, Quorel provides detailed explanations for each score, breaking down feature contributions, highlighting trends, and giving plain-language summaries that non-technical stakeholders can understand. All insights are accessible via an interactive analyst-friendly web dashboard equipped with visualization tools and alerting systems.

## Key Features
- **Multi-source, Continuous Data Ingestion:** Collects and normalizes structured and unstructured data in near real-time, ensuring up-to-date credit risk views.
- **Explainable Credit Scoring:** Uses advanced AI models with built-in explainability techniques (e.g., SHAP) to provide feature-level contribution breakdowns for each score.
- **Trend Analysis:** Differentiates short-term and long-term trends in creditworthiness to inform timely decisions.
- **Interactive Dashboard:** Enables analysts to visualize score trends, explore feature importance, apply filters, and investigate score changes with rich, user-friendly charts.
- **Alerts and Notifications:** Flags sudden or unusual score variations for immediate attention.
- **Scalable & Fault-Tolerant Architecture:** Designed to handle dozens of issuers across sectors with resilience to data source interruptions.
- **Automated MLOps:** Supports scheduled data refreshes, model retraining, and deployment workflows for continuous improvement.

## How to Run Quorel on Your Device

### Prerequisites
- Python 3.8 or higher installed
- Docker (optional but recommended for containerized setup)
- Clone the repository and install required packages

### Installation Steps
1. Clone the Quorel repository:
    git clone https://github.com/Khushi-Roy-123/Quorel-V2.git
    cd Quorel-V2

2. Install Python dependencies:
   pip install -r requirements.txt


3. Run the application locally:
   python main.py

## Summary
Quorel provides a transformative solution to the credit scoring inefficiencies by merging rapid, AI-powered credit risk evaluations with human-centered explainability and transparency. Its scalable system architecture and intuitive dashboards empower analysts and regulatory bodies alike to make better-informed, trustable credit decisions in dynamic financial environments.

---

