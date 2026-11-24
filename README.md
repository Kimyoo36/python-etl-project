# Python ETL Pipelines — Largest Banks & World GDP Data

This repository contains two end-to-end **Extract, Transform, Load (ETL)** data engineering projects built in Python. Each pipeline scrapes financial/economic data from archived Wikipedia tables, transforms and normalizes the metrics, loads the results into both **CSV** and **SQLite** databases, runs analytical SQL queries, and records timestamped execution logs.

---

## Projects Included

### 1. World's Largest Banks ETL (`banks_project.py`)
Compiles the **top 10 largest banks in the world** ranked by market capitalization (USD billion), converts the valuations into **GBP**, **EUR**, and **INR** using an external exchange-rate CSV, and stores the processed dataset locally and in SQLite.

- **Data Source:** Archived Wikipedia — *List of largest banks* (`By market capitalization`)
- **Exchange Rates:** `exchange_rate.csv` (`EUR: 0.93`, `GBP: 0.80`, `INR: 82.95`)
- **Outputs:**
  - `Largest_banks_data.csv` (`Name`, `MC_USD_Billion`, `MC_GBP_Billion`, `MC_EUR_Billion`, `MC_INR_Billion`)
  - `Banks.db` → table `Largest_banks`
  - `code_log.txt` — timestamped stage-by-stage execution log
- **SQL Queries Run:**
  - `SELECT * FROM Largest_banks`
  - `SELECT AVG(MC_GBP_Billion) FROM Largest_banks` *(Result: `151.987` billion GBP)*
  - `SELECT Name from Largest_banks LIMIT 5`

### 2. Countries by Nominal GDP ETL (`etl_project_gdp.py`)
Extracts IMF nominal GDP estimates for all countries, filters out aggregate (`World`) and missing (`—`) entries, converts values from **Million USD** to **Billion USD** (rounded to 2 decimal places), and queries all economies exceeding **100 billion USD**.

- **Data Source:** Archived Wikipedia — *List of countries by GDP (nominal)*
- **Outputs:**
  - `Countries_by_GDP.csv` (`Country`, `GDP_USD_billions` — 191 countries)
  - `World_Economies.db` → table `Countries_by_GDP`
  - `etl_project_log.txt` — timestamped execution log
- **SQL Query Run:**
  - `SELECT * from Countries_by_GDP WHERE GDP_USD_billions >= 100` *(Returns 69 countries)*

---

## Repository Structure

```text
.
├── banks_project.py         # Largest Banks ETL pipeline
├── exchange_rate.csv        # Currency exchange rates (EUR, GBP, INR)
├── Largest_banks_data.csv   # Transformed top-10 banks dataset
├── Banks.db                 # SQLite database (table: Largest_banks)
├── code_log.txt             # Execution log for banks_project.py
├── etl_project_gdp.py       # World GDP ETL pipeline
├── Countries_by_GDP.csv     # Transformed countries GDP dataset
├── World_Economies.db       # SQLite database (table: Countries_by_GDP)
├── etl_project_log.txt      # Execution log for etl_project_gdp.py
├── requirements.txt         # Python dependencies
└── README.md                # Project documentation
```

---

## Technologies Used

- **Python 3**
- **Requests** — HTTP requests to fetch web pages
- **BeautifulSoup4 & lxml** — HTML parsing and web scraping
- **Pandas & NumPy** — Data manipulation, currency conversion, and rounding
- **SQLite3** — Lightweight relational database storage and SQL querying
- **datetime** — Timestamped pipeline logging

---

## How to Run

### 1. Install Dependencies
```bash
pip install -r requirements.txt
```

### 2. Run the Largest Banks ETL Pipeline
```bash
python banks_project.py
```

### 3. Run the World GDP ETL Pipeline
```bash
python etl_project_gdp.py
```

---

## Sample Output (`Largest_banks_data.csv`)

| Name | MC_USD_Billion | MC_GBP_Billion | MC_EUR_Billion | MC_INR_Billion |
| --- | ---: | ---: | ---: | ---: |
| JPMorgan Chase | 432.92 | 346.34 | 402.62 | 35910.71 |
| Bank of America | 231.52 | 185.22 | 215.31 | 19204.58 |
| Industrial and Commercial Bank of China | 194.56 | 155.65 | 180.94 | 16138.75 |
| Agricultural Bank of China | 160.68 | 128.54 | 149.43 | 13328.41 |
| HDFC Bank | 157.91 | 126.33 | 146.86 | 13098.63 |
