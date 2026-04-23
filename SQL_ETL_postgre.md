# 📊 ETL Pipeline: CSV → PostgreSQL (Staging → Clean)

## 🎯 Objective

Convert raw CSV data into a **clean, queryable PostgreSQL table** using a staging (raw) layer to avoid data type issues.

---

# 🧠 Architecture Overview

```plaintext
CSV File
   ↓
loan_raw (Staging - TEXT only)
   ↓
Transformation SQL
   ↓
loan_cleaned (Final - Typed, Clean)
```

---

# ⚠️ Why Staging Layer?

| Problem                             | Solution                      |
| ----------------------------------- | ----------------------------- |
| CSV has inconsistent types          | Load as TEXT first            |
| Import fails due to schema mismatch | Avoid strict typing initially |
| Dirty / unexpected values           | Clean during transformation   |

---

# ✅ Step 1 — Create Database

```sql
CREATE DATABASE loan_db;
```

---

# ✅ Step 2 — Create Staging Table (`loan_raw`)

> All columns as TEXT to avoid import errors

```sql
CREATE TABLE loan_raw (
    id TEXT,
    year TEXT,
    issue_date TEXT,
    final_date TEXT,
    term_months TEXT,
    emp_length_int TEXT,
    home_ownership TEXT,
    annual_inc TEXT,
    income_cat TEXT,
    loan_amount TEXT,
    income_to_loan_ratio TEXT,
    purpose TEXT,
    interest_rate TEXT,
    interest_payments TEXT,
    grade TEXT,
    dti TEXT,
    total_pymnt TEXT,
    total_rec_prncp TEXT,
    recoveries TEXT,
    installment TEXT,
    region TEXT,
    profitability TEXT,
    loan_condition TEXT,
    risk_flag TEXT,
    default_rate_indicator TEXT
);
```

---

# ✅ Step 3 — Load CSV into Staging

### Using pgAdmin UI:

* Right click `loan_raw`
* Import/Export Data
* Format: CSV
* Header: ON
* Delimiter: `,`

---

### Verify Load

```sql
SELECT COUNT(*) FROM loan_raw;
```

---

# ✅ Step 4 — Create Clean Table (`loan_cleaned`)

```sql
CREATE TABLE loan_cleaned AS
SELECT
    id::BIGINT AS id,
    year::INT AS year,

    TO_DATE(issue_date, 'DD/MM/YYYY') AS issue_date,
    TO_DATE(final_date, 'DD/MM/YYYY') AS final_date,

    term_months::INT AS term_months,
    emp_length_int::NUMERIC AS emp_length_int,

    home_ownership,
    annual_inc::NUMERIC AS annual_inc,
    income_cat,

    loan_amount::NUMERIC AS loan_amount,
    income_to_loan_ratio::NUMERIC AS income_to_loan_ratio,

    purpose,
    interest_rate::NUMERIC AS interest_rate,
    interest_payments,

    grade,
    dti::NUMERIC AS dti,

    total_pymnt::NUMERIC AS total_pymnt,
    total_rec_prncp::NUMERIC AS total_rec_prncp,
    recoveries::NUMERIC AS recoveries,
    installment::NUMERIC AS installment,

    region,
    profitability::NUMERIC AS profitability,

    loan_condition,
    risk_flag::INT AS risk_flag,
    default_rate_indicator::NUMERIC AS default_rate_indicator

FROM loan_raw;
```

---

# ✅ Step 5 — Validate Clean Data

```sql
SELECT COUNT(*) FROM loan_cleaned;
```

---

# ✅ Step 6 — Add Index (Performance)

```sql
CREATE INDEX idx_loan_id ON loan_cleaned(id);
```

---

# 🚀 Final Usage

```sql
SELECT *
FROM loan_cleaned
WHERE id = 6297794;
```

---

# 🧠 Key Learnings

* Always separate **raw ingestion** and **clean transformation**
* Never trust CSV schema
* Use staging tables for robustness
* Use indexing for performance
* Transformation layer = business logic layer
---
