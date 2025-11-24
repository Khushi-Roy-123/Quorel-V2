# Quorel V2 – Real-Time Explainable Credit Intelligence Platform

## Live Demo

Access the live platform here: https://quorel-v2.onrender.com/

## Overview

Quorel V2 is a Flask-based web application that generates an **explainable 0–100 creditworthiness score for stocks** in near real time.

It combines:

- Market data (OHLCV from Yahoo Finance)
- News headlines and sentiment
- A simple but interpretable ML model
- Time-series forecasting (ARIMA / SARIMAX)

to provide **transparent scores, visual dashboards, and narrative explanations** that non-technical stakeholders can understand.

---

## Features

- **Secure analyst dashboard**
  - User signup, email verification, login, and password reset
  - Protected views for scores, events, peers, and glossary

- **Data ingestion & preprocessing**
  - Fetches OHLCV from Yahoo Finance via `yfinance`
  - Robust cleaning: handles missing columns and gaps
  - Feature engineering:
    - Daily returns
    - Relative range (High–Low vs Close)
    - 5‑day volatility
    - 5‑day momentum

- **Explainable scoring engine**
  - `DecisionTreeClassifier` over engineered features
  - 0–100 score from probability of positive next‑day return
  - Feature importances exposed and visualized
  - Plain‑language explanation of top drivers + momentum & volatility context

- **News & sentiment integration**
  - RSS + HTML scraping for finance/markets news
  - NLU-based event extraction and sentiment tagging
  - Sentiment-based score adjustments
  - Sentiment distribution plots (positive / neutral / negative)

- **Forecast-aware insights**
  - ARIMA on prices and historical scores
  - SARIMAX with exogenous features (volatility + sentiment index)
  - Forecast trend adjustment to the current score
  - JSON endpoints for chart-ready forecast data

- **Peer comparison**
  - Static + dynamic peer lookup (industry/sector)
  - 6‑month return correlation between symbol and peers
  - Influence rating (1–5 stars) from correlation strength
  - Peer comparison bar chart

- **APIs for integration**
  - `GET /api/score/<ticker>` – score, probability, importances, explanation
  - `GET /api/events` – news events + sentiment
  - `GET /api/score_history/<ticker>` – time series of scores
  - `GET /api/price_forecast` – actual + forecast prices and confidence bands

---

## Tech Stack

- **Backend**
  - Python 3.8+
  - Flask
  - Flask-Bcrypt
  - Flask-Mail
  - SQLite (`score_history.db`, `users.db`)

- **Data & ML**
  - `pandas`, `numpy`
  - `scikit-learn` (DecisionTreeClassifier, StandardScaler)
  - `statsmodels` (ARIMA, SARIMAX)
  - `yfinance`

- **NLP & News**
  - `feedparser` (RSS)
  - `requests`, `BeautifulSoup` (scraping)
  - Custom NLU (`nlu_events/`) for event + sentiment extraction

- **Visualization**
  - `matplotlib` (Agg backend)
  - Jinja2 templates
  - (Optional frontend charting library in templates, e.g. Chart.js)

- **Deployment**
  - Dockerfile
  - Vercel / Render configuration (`vercel.json`)
  - `requirements.txt` for Python dependencies

---

## Getting Started

### Prerequisites

- Python **3.8 or higher**
- `pip` installed
- (Recommended) Virtual environment (venv)
- Internet access (for market data and news)
- SMTP credentials for email (e.g. Gmail app password)

### 1. Clone the repository

```bash
git clone https://github.com/Khushi-Roy-123/Quorel-V2.git
cd Quorel-V2
```

### 2. Create & activate a virtual environment (optional but recommended)

```bash
python -m venv venv

# Windows (PowerShell)
venv\Scripts\Activate.ps1

# or Windows (cmd)
venv\Scripts\activate.bat
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure environment variables

For development, you can export environment variables or use a `.env` file (with something like `python-dotenv`):

- `FLASK_SECRET_KEY` – Flask secret key
- `MAIL_SERVER` – e.g. `smtp.gmail.com`
- `MAIL_PORT` – e.g. `587`
- `MAIL_USE_TLS` – `true`
- `MAIL_USERNAME` – SMTP user (your email)
- `MAIL_PASSWORD` – SMTP/app password

Example (PowerShell):

```powershell
$env:FLASK_SECRET_KEY = "replace-with-strong-secret"
$env:MAIL_SERVER = "smtp.gmail.com"
$env:MAIL_PORT = "587"
$env:MAIL_USE_TLS = "true"
$env:MAIL_USERNAME = "your-email@gmail.com"
$env:MAIL_PASSWORD = "your-app-password"
```

> **Note:** The repository currently contains hard-coded credentials in `app.py` for demo purposes.  
> For any public or production use, move all secrets into environment variables and remove them from the code.

### 5. Run the app

```bash
python app.py
```

By default the app runs at: `http://127.0.0.1:5000/`

- `http://127.0.0.1:5000/` – Landing page
- `http://127.0.0.1:5000/signup` – Create account
- `http://127.0.0.1:5000/login` – Login
- `http://127.0.0.1:5000/dashboard` – Main dashboard (requires login)

---

## Testing

### 1. Manual functional testing

- **Auth flow**
  - Sign up with a new email
  - Verify the email using the received link
  - Log in and log out
  - Test the password reset flow

- **Dashboard & scoring**
  - Log in and open `/dashboard`
  - Score different tickers via `/score?ticker=<SYMBOL>`
  - Confirm:
    - Score is shown and changes with ticker
    - Feature importance, price/MAs, and sentiment plots render
    - Explanation text is consistent with the plots

- **Events & peers**
  - `/events` – verify events list and sentiment counts
  - `/peer?symbol=AAPL` – check peer list, peer scores, and comparison chart

- **Glossary**
  - `/glossary` – verify financial terms are shown and linked from UI

### 2. API testing

Use Postman, Insomnia, or `curl`:

```bash
# Score API
curl http://127.0.0.1:5000/api/score/AAPL

# Events API
curl http://127.0.0.1:5000/api/events

# Score history
curl http://127.0.0.1:5000/api/score_history/AAPL

# Price forecast
curl "http://127.0.0.1:5000/api/price_forecast?ticker=AAPL&window=30&days=5"
```

Check that JSON responses have the expected structure and values.

### 3. Module-level testing (optional)

You can write unit tests (e.g. with `pytest`) around:

- `data_pipeline.ingest.fetch_yahoo_finance`
- `data_pipeline.preprocess.clean_financial_data` and `feature_engineering`
- `scoring_engine.model.CreditScoringModel`
- Forecast helpers and `utils.explain.make_plain_language_explanation`

---



