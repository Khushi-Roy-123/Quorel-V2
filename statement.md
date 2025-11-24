# Quorel V2 – Project Statement

## Problem Statement

Traditional credit scoring and market risk models often behave like **black boxes**. They generate scores or ratings without making it clear **why** a particular issuer, stock, or asset class is considered more or less risky. This lack of transparency:

- Reduces trust for analysts, investors, and regulators.
- Makes it difficult to justify or audit decisions.
- Fails to incorporate real-time signals from markets and news in an interpretable way.

There is a need for a **real-time, explainable, and evidence-backed credit intelligence system** that continuously ingests market and news data, produces consistent scores, and clearly explains the drivers behind those scores.

---

## Scope of the Project

The scope of Quorel V2 includes:

- **Data ingestion**
  - Fetch OHLCV price and volume data for listed stocks from Yahoo Finance.
  - Collect recent market and business news headlines via RSS feeds and simple scraping.

- **Data preprocessing & feature engineering**
  - Clean and normalize OHLCV data to handle missing or inconsistent fields.
  - Compute derived features such as returns, volatility, price range, and momentum.

- **Scoring engine**
  - Train an interpretable machine learning model (Decision Tree) on engineered features.
  - Convert model output to a **0–100 creditworthiness-style score** for stocks.
  - Incorporate news sentiment and market risk factors (volatility, liquidity) into the final score.

- **Explainability & visualization**
  - Expose feature importances and top drivers per ticker.
  - Generate human-readable narratives explaining how the score was formed.
  - Provide charts for feature importance, price trends, moving averages, and sentiment.

- **Forecasting**
  - Use ARIMA/SARIMAX models to forecast prices and scores over a short horizon.
  - Use forecast trends as a small adjustment to current scores and commentary.

- **Web application & APIs**
  - Build a secure Flask-based web dashboard for analysts.
  - Implement REST-style endpoints for scores, events, history, and forecasts.

Out of scope (for this version):

- Production-grade data pipelines (Kafka, Spark, etc.).
- Complex credit portfolio modeling or regulatory capital calculations.
- Full institutional user management and RBAC beyond basic admin support.

---

## Target Users

Quorel V2 is designed primarily for:

- **Sell-side and buy-side analysts**
  - Need a fast, interpretable view of stock-level risk and momentum.
  - Want to see not just the score but also the **reasons** behind it.

- **Individual / retail investors (advanced)**
  - Interested in a clearer, dashboard-style explanation of risk signals.
  - Want a more transparent alternative to purely black-box ratings.

- **Risk and compliance teams (prototype usage)**
  - Evaluating explainable AI approaches for internal credit and market risk models.
  - Exploring how sentiment and short-term market dynamics can be integrated.

- **Academic / student projects**
  - Using the platform as a reference implementation for explainable ML in finance.

---

## High-Level Features

- **Real-time score generation**
  - On-demand scoring for any supported stock ticker.
  - Score range: 0–100, summarizing short-term creditworthiness / risk.

- **Explainable ML model**
  - Decision Tree classifier over engineered OHLCV features.
  - Feature importance visualization and textual explanation.

- **News & sentiment integration**
  - Aggregation of recent finance/market headlines.
  - Event extraction and sentiment scoring.
  - Sentiment-based positive/negative adjustments to the base score.

- **Market risk adjustments**
  - Volatility-based penalties using recent return volatility.
  - Liquidity-based penalties using relative volume changes.

- **Forecast-driven insights**
  - ARIMA-based price forecast and ARIMA-based score forecast using historical score data.
  - SARIMAX model with exogenous volatility and sentiment factors.
  - Small forecast-based adjustment to the score and narrative.

- **Interactive web dashboard**
  - Login-protected analyst dashboard (signup, verification, login, reset).
  - Score page with visuals and explanations for a selected ticker.
  - Events page summarizing recent news events and sentiment.
  - Peer comparison view with correlations and influence ratings.
  - Glossary page explaining key financial and modeling concepts.

- **API access**
  - JSON endpoints for:
    - Current score and explanation for a ticker.
    - Recent events and sentiment.
    - Historical score timeline per ticker.
    - Price forecast series for charting.

This document is intended as a **high-level project statement** to accompany the main README and support reports, presentations, or academic documentation.
