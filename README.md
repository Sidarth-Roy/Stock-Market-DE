# 📊 Stock Market Data Engineering Project

## 🚀 Project Overview
This project builds a **Stock Market Data Pipeline** using **Snowflake, dbt, Dagster, Power BI, and Python**, following the **Medallion Architecture (Bronze, Silver, Gold)**. It integrates multiple **free financial data sources** for a realistic **financial data engineering pipeline**.

## 📌 Key Features
✅ **Multiple free data sources** (Yahoo Finance, Tiingo, FRED, Polygon.io, FinancialModelingPrep)  
✅ **Medallion Architecture** (Raw → Cleaned → Aggregated)  
✅ **Dagster for Orchestration**  
✅ **dbt for Data Transformations**  
✅ **Snowflake as Data Warehouse**  
✅ **Power BI for Data Visualization**  
✅ **CI/CD for automation** (Optional)  

---

## 📂 Project Architecture

### **🟤 Bronze Layer (Raw Data) – Landing Zone**
| Table Name | Source |
|------------|--------|
| `raw_stock_prices_yfinance` | Yahoo Finance |
| `raw_stock_prices_tiingo` | Tiingo |
| `raw_technical_indicators` | Alpha Vantage |
| `raw_macro_data_fred` | FRED (Macroeconomic data) |
| `raw_stock_news_iex` | IEX Cloud |
| `raw_financials_fmp` | FinancialModelingPrep |
| `raw_market_data_polygon` | Polygon.io |

### **🟡 Silver Layer (Cleaned & Processed Data)**
| Table Name | Description |
|------------|-------------|
| `clean_stock_data` | Cleaned & validated stock prices |
| `market_sentiment` | News sentiment analysis |
| `macro_trends` | GDP, inflation, interest rate trends |

### **🟢 Gold Layer (Aggregated & BI)**
| Table Name | Description |
|------------|-------------|
| `stock_performance` | Growth, volatility, returns |
| `market_health_index` | Macro trends + stock performance |
| `investment_recommendations` | AI-based investment insights |

---

## 🔄 Data Extraction with Python

```python
import requests
import pandas as pd
import os
import yfinance as yf

# API Keys
TIINGO_API_KEY = "YOUR_TIINGO_API_KEY"
FRED_API_KEY = "YOUR_FRED_API_KEY"
POLYGON_API_KEY = "YOUR_POLYGON_API_KEY"
FMP_API_KEY = "YOUR_FMP_API_KEY"

def fetch_tiingo_data(ticker="AAPL"):
    url = f"https://api.tiingo.com/tiingo/daily/{ticker}/prices?token={TIINGO_API_KEY}"
    response = requests.get(url).json()
    df = pd.DataFrame(response)
    df.to_csv(f"data/{ticker}_tiingo.csv")

def fetch_fred_data():
    url = f"https://api.stlouisfed.org/fred/series/observations?series_id=GDP&api_key={FRED_API_KEY}&file_type=json"
    response = requests.get(url).json()
    df = pd.DataFrame(response["observations"])
    df.to_csv("data/macro_fred_gdp.csv")

def extract_all_data(ticker="AAPL"):
    fetch_tiingo_data(ticker)
    fetch_fred_data()

if __name__ == "__main__":
    extract_all_data("AAPL")
```

---

## 📊 Dagster Pipeline for Orchestration

```python
from dagster import job, op

@op
def extract_tiingo():
    from extract import fetch_tiingo_data
    return fetch_tiingo_data("AAPL")

@op
def extract_fred():
    from extract import fetch_fred_data
    return fetch_fred_data()

@job
def stock_data_pipeline():
    extract_tiingo()
    extract_fred()
```

Run Dagster:
```sh
dagster dev
```

---

## 🛠 Data Transformation with dbt
Create `models/clean_stock_data.sql`:
```sql
WITH source AS (
    SELECT * FROM {{ ref('raw_stock_prices_yfinance') }}
)
SELECT
    ticker,
    date,
    open,
    close,
    high,
    low,
    volume
FROM source
WHERE volume IS NOT NULL;
```
Run dbt:
```sh
dbt run
```

---

## 📊 Power BI Dashboards
✅ **Stock Performance Analysis** 📈  
✅ **Macroeconomic Trends from FRED** 🌍  
✅ **Stock Sentiment Analysis** 📰  
✅ **Investment Recommendations** 💡  

---

## 🏗️ Project Roadmap

### ✅ Phase 1: Data Ingestion & Storage
- [x] Extract data from Yahoo Finance, Tiingo, FRED, Polygon.io, FinancialModelingPrep
- [x] Store raw data in Snowflake **(Bronze Layer)**

### ✅ Phase 2: Data Transformation
- [x] Clean and process data using **dbt** (Silver Layer)
- [x] Join stock data with macroeconomic trends

### ✅ Phase 3: Orchestration & Automation
- [x] Schedule data pipeline with **Dagster**

### ✅ Phase 4: BI & Analytics
- [x] Create Power BI dashboards
- [x] Implement AI-based **investment recommendations**

---

## 🔧 Setup & Installation
1️⃣ Clone the repository:
```sh
git clone https://github.com/your-username/stock-data-engineering.git
cd stock-data-engineering
```
2️⃣ Install dependencies:
```sh
pip install -r requirements.txt
```
3️⃣ Set up API keys in `.env`:
```
TIINGO_API_KEY=your_tiingo_key
FRED_API_KEY=your_fred_key
POLYGON_API_KEY=your_polygon_key
FMP_API_KEY=your_fmp_key
```
4️⃣ Run the data extraction script:
```sh
python extract.py
```
5️⃣ Run the Dagster pipeline:
```sh
dagster dev
```
6️⃣ Transform data with dbt:
```sh
dbt run
```
7️⃣ Open Power BI and connect to Snowflake for visualization.

---

## 📚 Tech Stack
- **Data Extraction**: Python, APIs (Yahoo Finance, Tiingo, FRED, Polygon.io, FMP)
- **Data Storage**: Snowflake (Bronze, Silver, Gold)
- **Data Transformation**: dbt (SQL-based)
- **Orchestration**: Dagster
- **Visualization**: Power BI
- **Automation**: CI/CD with GitHub Actions

---

## 🎯 Why This Project is Resume-Worthy
✅ **Real-world financial data pipeline**  
✅ **End-to-end ETL using modern stack**  
✅ **Medallion architecture implementation**  
✅ **Data pipeline automation with Dagster**  
✅ **Dashboards for financial analysis**  

---

## 🤝 Contributing
Feel free to fork this repository and submit **pull requests** to enhance the project!

---

## 📧 Contact
🔗 **LinkedIn**: [Your Profile](https://linkedin.com/in/yourprofile)  
🐦 **Twitter**: [@yourhandle](https://twitter.com/yourhandle)  
📩 **Email**: your@email.com  

