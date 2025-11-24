# Quorel V2 – Project Report

## 1. Cover Page

**Project Title:** Quorel V2 – Real-Time Explainable Credit Intelligence Platform  
**Domain:** Financial Analytics / Explainable AI  
**Technology Stack:** Python, Flask, SQLite, scikit-learn, statsmodels, yfinance, HTML/CSS/JS  
**Institution:** *[Your Institute Name]*  
**Course / Program:** *[Course Name (e.g., B.Tech CSE)]*  
**Team Members:**  
- Name 1 (Reg. No. / Roll No.)  
- Name 2 (Reg. No. / Roll No.)  
- Name 3 (Reg. No. / Roll No.)  

**Guide / Mentor:** *[Faculty / Mentor Name]*  
**Academic Year:** *[Year–Year]*

---

## 2. Introduction

Modern financial markets are increasingly data-driven. Analysts and investors rely on a mix of quantitative indicators, qualitative news, and historical trends to gauge the risk and creditworthiness of securities. Traditional credit scoring systems and ratings agencies often operate like **black boxes**: they produce a score or rating, but do not clearly explain the underlying reasoning. This lack of transparency can lead to mistrust and difficulty in auditing decisions.

**Quorel V2** is a prototype of a **real-time, explainable credit intelligence platform**. It focuses on listed stocks and uses easily accessible data sources—market prices, volumes, and public news—to generate a **0–100 creditworthiness-style score**. Crucially, the system emphasizes **explainability**, combining feature importance, sentiment analysis, and forecasting into human-readable narratives and visual dashboards.

---

## 3. Problem Statement

Traditional credit risk models and scoring methods:

- Provide scores or ratings without exposing clear, interpretable reasoning.
- Are slow to update when new market information or news arrives.
- Often require expensive data feeds and proprietary infrastructure.

This leads to several issues:

- **Limited transparency:** Analysts and regulators cannot easily understand why a particular issuer or stock is considered high or low risk.
- **Reduced trust:** Investors may be hesitant to rely on black-box scores for significant financial decisions.
- **Slow responsiveness:** New information (earnings surprises, macro shocks, breaking news) may not be quickly incorporated in a visible, explainable way.

**Objective:** Build a system that produces **real-time, explainable, and evidence-backed creditworthiness scores** for stocks using publicly available data, and presents these insights through a user-friendly web dashboard.

---

## 4. Functional Requirements

- **User Management**
  - Users shall be able to sign up with an email and password.
  - Users shall receive an email verification link to activate their account.
  - Users shall be able to log in and log out securely.
  - Users shall be able to reset their password via an email-based reset link.

- **Score Generation**
  - The system shall allow the user to select or input a stock ticker symbol.
  - The system shall fetch recent OHLCV data for the ticker from Yahoo Finance.
  - The system shall clean and preprocess the data, computing engineered features.
  - The system shall run a machine learning model to produce a base score between 0 and 100.
  - The system shall apply adjustments based on volatility, liquidity, and news sentiment to obtain a final score.

- **News & Sentiment Analysis**
  - The system shall fetch recent news headlines for the selected ticker from RSS feeds and/or websites.
  - The system shall extract events and perform sentiment analysis (positive, neutral, negative).
  - The system shall display a breakdown of sentiment counts and reflect sentiment in the score.

- **Explainability & Visualization**
  - The system shall display feature importances for the scoring model.
  - The system shall generate a plain-language explanation summarizing the key drivers of the score.
  - The system shall plot price history with moving averages.
  - The system shall plot sentiment distribution for the ticker.

- **Forecasting**
  - The system shall forecast short-term future prices using ARIMA/SARIMAX models.
  - The system shall forecast short-term score trends using historical scores.
  - The system shall provide forecast information in both visual and textual form.

- **Peer Comparison**
  - The system shall identify peer tickers based on industry/sector.
  - The system shall compute correlations between the main ticker and its peers.
  - The system shall display peer scores and a peer comparison chart.

- **APIs**
  - The system shall expose REST-style endpoints for programmatic access to scores, events, score history, and price forecasts.

---

## 5. Non-functional Requirements

- **Performance**
  - Score computation for a given ticker should typically complete within a few seconds, assuming normal network latency.

- **Scalability (Prototype Level)**
  - The system should handle multiple user requests sequentially without crashing.
  - The design should allow future migration from SQLite to a more scalable database if needed.

- **Security**
  - Passwords shall be stored using secure hashing (Bcrypt).
  - Sensitive routes (dashboard, scoring pages) shall be accessible only to authenticated users.
  - Email verification and password reset tokens shall be time-bound and single-use.

- **Reliability & Robustness**
  - The system shall handle missing or incomplete OHLCV data gracefully.
  - If news or sentiment data is unavailable, the system shall still produce a score with appropriate messaging.

- **Usability**
  - The web UI shall be intuitive for non-technical analysts.
  - Explanations shall use plain, understandable language.

- **Maintainability**
  - The codebase shall be modularly structured (data pipeline, scoring engine, utils, templates).
  - Configuration (e.g., credentials) should be easily externalizable via environment variables.

---

## 6. System Architecture

At a high level, Quorel V2 follows a **layered, modular architecture**:

- **Presentation Layer (Frontend)**
  - Flask routes render Jinja2 templates (HTML/CSS/JS) for dashboards, forms, and charts.

- **Application Layer (Flask App)**
  - Handles HTTP requests, user authentication, session management, and routing.
  - Coordinates calls to data ingestion, preprocessing, ML models, and visualization utilities.

- **Data & ML Layer**
  - **Data Pipeline:** Ingests market and news data, cleans and engineers features.
  - **Scoring Engine:** Contains the machine learning model (Decision Tree) and score logic.
  - **Forecasting:** ARIMA and SARIMAX models for price and score predictions.
  - **NLU & Sentiment:** Extracts events and sentiment from news.

- **Data Storage Layer**
  - SQLite databases (`users.db`, `score_history.db`) store user credentials, admin info, alert configurations, and historical scores.

External dependencies include Yahoo Finance (for OHLCV data) and news sources (RSS/HTML) for headlines.

---

## 7. Design Diagrams

> Note: The following descriptions explain each diagram type. In your final document, you can include drawn diagrams (using tools like Lucidchart, Draw.io, or hand-drawn scanned images) based on these descriptions.

### 7.1 Use Case Diagram

**Actors:**
- User (Analyst / Investor)
- Admin (optional specialized role)

**Main Use Cases:**
- Sign Up / Verify Email
- Log In / Log Out
- Reset Password
- View Dashboard
- Generate Score for Ticker
- View News & Events
- View Peer Comparison
- View Glossary
- Access APIs (System-to-System Actor)

The Use Case Diagram should show the User interacting with these use cases via the web application boundary.

### 7.2 Workflow Diagram (High-Level Flow)

Typical workflow for generating and viewing a score:

1. User logs into the system.
2. User selects or enters a stock ticker.
3. System fetches OHLCV data and news.
4. System cleans data and engineers features.
5. ML model produces base score and feature importances.
6. System applies sentiment and risk adjustments.
7. System stores final score in history and generates visualizations.
8. Dashboard displays score, explanation, plots, and sentiment.

### 7.3 Sequence Diagram (Score Request)

Objects:
- User
- Browser / UI
- Flask App
- Data Pipeline (Ingest + Preprocess)
- Scoring Engine
- Forecasting Module
- Database

**Sequence (simplified):**
1. User → Browser: Request `/score?ticker=XYZ`.
2. Browser → Flask App: HTTP GET request.
3. Flask App → Data Pipeline: Fetch & preprocess market data.
4. Data Pipeline → Flask App: Cleaned dataframe with features.
5. Flask App → Scoring Engine: Fit model & compute score/importances.
6. Flask App → Forecasting Module: Compute price/score forecasts.
7. Flask App → Database: Store final score in `score_history`.
8. Flask App → Browser: Rendered `score.html` with plots and explanation.

### 7.4 Class/Component Diagram

Main components/classes:

- `app.py` (Flask application)
  - Routes: auth, dashboard, score, events, peers, APIs.
- `data_pipeline.ingest`
  - Functions: `fetch_yahoo_finance`, `fetch_news_headlines`.
- `data_pipeline.preprocess`
  - Functions: `clean_financial_data`, `feature_engineering`.
- `scoring_engine.model.CreditScoringModel`
  - Methods: `fit`, `score`.
- `scoring_engine.peer_comparison`
  - Functions: `get_peers`.
- `utils.explain`
  - Function: `make_plain_language_explanation`.
- `utils.plots`
  - Functions: plotting utilities for feature importances, price, sentiment.

The diagram can show these as components with dependencies between them.

### 7.5 ER Diagram (Data Storage)

Main entities (SQLite):

- `users`
  - `id`, `email`, `password_hash`, `is_verified`, `verification_token`, `reset_token`, `reset_expiry`.

- `admins`
  - `id`, `email`, `designation`.

- `score_history`
  - `id`, `ticker`, `time`, `score`.

- `top_scores` (optional summary table)
  - `ticker`, `score`, `updated_at`.

- `ultra_notice` (alert/watchlist configuration)
  - `id`, `email`, `ticker1`, `ticker2`, `ticker3`, `threshold`, `direction`, `active`.

The ER Diagram should show relationships such as `users` (1)–(N) `ultra_notice`. Other tables are mostly independent.

---

## 8. Design Decisions & Rationale

- **Flask as Web Framework:**
  - Lightweight and easy to integrate with Python-based data science stack.
  - Simple routing and templating for quick prototyping.

- **SQLite for Storage:**
  - File-based, minimal configuration, suitable for a student/POC project.
  - Easy to bundle with the project and reset if required.

- **Decision Tree Model:**
  - Provides clear feature importances.
  - Simple to train quickly on on-demand data.
  - Good for demonstration of explainable ML, even if not state-of-the-art.

- **ARIMA/SARIMAX for Forecasting:**
  - Well-established time-series models, easy to implement with statsmodels.
  - Allow incorporation of exogenous features (volatility, sentiment).

- **Sentiment from News Headlines:**
  - Provides an unstructured data signal that influences score.
  - Approximates how real analysts factor in qualitative information.

- **Modular Package Structure:**
  - Separation into `data_pipeline`, `scoring_engine`, `utils`, `templates` improves clarity and maintainability.

---

## 9. Implementation Details

- **Programming Language:** Python 3.8+
- **Framework:** Flask
- **Key Libraries:** `pandas`, `numpy`, `scikit-learn`, `statsmodels`, `yfinance`, `feedparser`, `requests`, `BeautifulSoup`, `flask-bcrypt`, `flask-mail`, `matplotlib`.

Implementation highlights:

- `app.py` initializes Flask app, databases, and mail configuration.
- Auth routes implement signup, email verification, login, logout, and password reset.
- Scoring route `/score` orchestrates data ingestion, preprocessing, model fitting, scoring, adjustments, forecasting, and visualization generation.
- API routes provide JSON for integration and charting.
- Templates in `templates/` define the web UI, including dashboard, score page, events page, peer comparison, glossary, and auth views.

---

## 10. Screenshots / Results

*(To be filled with actual screenshots in the final report.)*

Suggested screenshots:

- Landing page / login page.
- Dashboard view after login.
- Score page for a sample ticker (e.g., AAPL) showing score, explanation, and plots.
- Events page showing news headlines and sentiment.
- Peer comparison page with bar chart.
- Example API response (e.g., from `/api/score/AAPL`).

---

## 11. Testing Approach

- **Manual Functional Testing:**
  - Verified complete user flow: signup → verification → login → scoring.
  - Tested error cases: invalid credentials, unverified email, missing ticker data, network issues.

- **API Testing:**
  - Used tools like Postman/`curl` to test `/api/score`, `/api/events`, `/api/score_history`, and `/api/price_forecast`.
  - Checked JSON structure and response times.

- **Module-Level Testing (Informal):**
  - Ran `data_pipeline` functions separately to verify data shapes and columns.
  - Tested `CreditScoringModel` on sample data to validate score range and feature importances.
  - Tested forecasting helpers on sample series to ensure correct output lengths.

---

## 12. Challenges Faced

- Handling missing or inconsistent OHLCV data from external APIs.
- Ensuring the ML model has enough variation in target labels to train properly.
- Dealing with network latency and occasional failures from external data sources (Yahoo Finance, news sites).
- Designing explanations that are accurate yet understandable to non-technical users.
- Balancing prototype complexity with performance (e.g., model retraining on each request vs pre-trained models).

---

## 13. Learnings & Key Takeaways

- Practical experience in building an **end-to-end ML-powered web application**.
- Understanding of how financial time-series features (returns, volatility, momentum) can be engineered and used in models.
- Exposure to **explainable AI** concepts such as feature importance and narrative explanations.
- Insights into integrating **unstructured data** (news + sentiment) with structured market data.
- Experience in designing REST APIs, handling authentication, and managing small-scale databases.

---

## 14. Future Enhancements

- Replace or augment the Decision Tree with more advanced yet explainable models (e.g., Gradient Boosted Trees with SHAP explanations).
- Integrate more reliable and higher-quality data sources (paid APIs, fundamental data, macro indicators).
- Implement real user roles and role-based access control (RBAC).
- Migrate from SQLite to PostgreSQL or another production-grade database.
- Add real-time streaming updates and alerts (e.g., using WebSockets).
- Containerize and orchestrate the system using Docker and Kubernetes for scalable deployment.
- Build a more sophisticated UI/UX with interactive charts and filters.

---

## 15. References

- Yahoo Finance API via `yfinance`: https://github.com/ranaroussi/yfinance  
- statsmodels Time Series Documentation: https://www.statsmodels.org/  
- scikit-learn Documentation: https://scikit-learn.org/  
- Flask Documentation: https://flask.palletsprojects.com/  
- BeautifulSoup (bs4) Documentation: https://www.crummy.com/software/BeautifulSoup/  
- Feedparser Documentation: https://feedparser.readthedocs.io/  
- Official Python Documentation: https://docs.python.org/  
- Additional tutorials and resources on Explainable AI and financial time-series analysis.
