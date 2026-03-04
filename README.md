# PricePulse — AI-Powered Real-Time Price Tracker

**Author: Shreyas**

PricePulse is a Jupyter Notebook that helps everyday shoppers avoid overpaying for products online. It fetches real-time prices from Google Shopping across dozens of retailers, stores price history in a local SQLite database, and uses a Groq-hosted LLM (Llama 3.3 70B) to generate a structured buy/wait recommendation.

---

## Features

- **Real-time price fetching** — Queries Google Shopping via the SerpAPI `google_shopping` engine, returning prices from Amazon, Walmart, Best Buy, eBay, Target, Newegg, B&H Photo, Costco, and more in a single API call.
- **Persistent price history** — Every price lookup is saved to a local SQLite database (`pricepulse_history.db`). History survives kernel restarts and accumulates across sessions.
- **AI buy/wait recommendations** — Sends price data and history to Groq's Llama 3.3 70B model, which factors in price spreads, seasonal sales (Black Friday, Prime Day, back-to-school), and historical trends to recommend `buy_now`, `wait`, or `set_alert`.
- **Target price alerts** — Pass an optional target price to `hunt_price()`; the report will flag when the current best deal meets or beats your target.
- **Interactive product tracker** — A built-in `input()` loop lets you search any product live and get an instant AI-powered report.
- **Price history viewer** — `show_price_history()` prints a timestamped log and min/avg/max summary for any tracked product.

---

## Tech Stack

| Layer | Tool |
|---|---|
| Price data | [SerpAPI](https://serpapi.com) `google_shopping` engine |
| AI/LLM | [Groq](https://console.groq.com) — Llama 3.3 70B Versatile (free tier) |
| Database | SQLite (Python built-in) |
| Runtime | Python 3.8+ Jupyter Notebook |
| External packages | `groq`, `google-search-results` |
| Standard library | `sqlite3`, `json`, `os`, `datetime`, `getpass` |

No Docker, no web server, no paid services required.

---

## Prerequisites

- Python 3.8 or higher
- A free [SerpAPI account](https://serpapi.com) — 100 searches/month, no credit card required
- A free [Groq account](https://console.groq.com) — generous rate limits, no credit card required

---

## Getting Started

### 1. Clone the repository

```bash
git clone <repository-url>
cd pricepulse-ai
```

### 2. Open the notebook

```bash
jupyter notebook pricepulse.ipynb
# or in JupyterLab:
jupyter lab pricepulse.ipynb
```

### 3. Run the cells in order

**Section 1 — Setup**: Cell 1 installs the two required packages. Cells 3 and 4 prompt you to enter your API keys securely via `getpass` — they are never stored in the notebook.

```bash
# Packages installed automatically by Cell 1:
pip install groq google-search-results
```

**Section 2 — Database**: Creates `pricepulse_history.db` in the notebook directory on first run.

**Section 3–5 — Core functions**: Defines all price-fetching, AI-recommendation, and pipeline functions. Run all definition cells before the demo.

**Section 6 — Demo**: Runs a live price search on three products and opens an interactive tracker.

---

## Project Structure

```
pricepulse-ai/
├── pricepulse.ipynb          # Main notebook (8 sections, 24 cells)
├── pricepulse_history.db     # SQLite price history (auto-created on first run)
└── README.md
```

---

## Notebook Sections

| # | Section | What It Does |
|---|---|---|
| 1 | Introduction & Setup | Install packages, import libraries, enter API keys via `getpass` |
| 2 | Price History Database | Define SQLite schema; `save_price_record()`, `get_price_history()` |
| 3 | Fetching Real Prices | `fetch_google_shopping_prices()` via SerpAPI; `display_prices_table()` |
| 4 | AI Recommendation Engine | `build_analysis_prompt()`; `get_ai_recommendation()` via Groq |
| 5 | Full Pipeline | `hunt_price()` orchestrator; `display_full_report()` |
| 6 | Interactive Demo | 3-product live demo + interactive `input()` tracker |
| 7 | Price History Analysis | `show_price_history()` — timestamped DB log with summary stats |
| 8 | Limitations & Next Steps | Cron scheduling, email alerts, watchlist files, charting |

---

## Key Functions

### `hunt_price(product_name, serpapi_key, groq_client, target_price=None)`
The main orchestrator. Fetches live prices → saves to SQLite → retrieves history → calls LLM → returns a structured result dict.

### `fetch_google_shopping_prices(product_name, serpapi_key)`
Makes a real API call to SerpAPI's `google_shopping` engine. Raises a `ValueError` with a descriptive message if no results are returned — no mock data, no silent fallbacks.

### `get_ai_recommendation(prompt, groq_client)`
Calls Groq (Llama 3.3 70B) with `response_format={"type": "json_object"}` enforced. Returns a parsed dict with: `recommendation`, `confidence`, `reasoning`, `predicted_best_price`, `predicted_best_time`, `savings_potential`.

### `show_price_history(product_name)`
Queries the local SQLite database and prints a timestamped price log with min/avg/max summary for any previously tracked product.

---

## API Key Setup

| API | Where to get it | Free tier |
|---|---|---|
| SerpAPI | [serpapi.com/manage-api-key](https://serpapi.com/manage-api-key) | 100 searches/month, no credit card |
| Groq | [console.groq.com/keys](https://console.groq.com/keys) | Generous rate limits, no credit card |

Both keys are entered using Python's `getpass` — they are never written to disk or shown in cell output.

---

## Free Tier Usage

- Each call to `hunt_price()` consumes **1 SerpAPI search credit**.
- The 3-product demo in Section 6 uses 3 credits.
- With 100 credits/month, the notebook is well-suited for tracking a watchlist of ~2–3 products checked daily.

---

## Limitations

- **SerpAPI results are region-dependent.** Results default to the US market. Add `gl` (country) and `hl` (language) parameters to `fetch_google_shopping_prices()` to localise.
- **Price history improves over time.** On the first run for a product, the AI has no historical baseline. Repeated runs over days and weeks produce richer recommendations.
- **No continuous monitoring.** The notebook runs on demand. For scheduled monitoring, see the cron/GitHub Actions approach described in Section 8.

---

## License

This project is licensed under the MIT License.