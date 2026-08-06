# KPI, Anomaly Detection & Customer Segmentation Pipeline

An end-to-end analytics pipeline built on a real motor vehicle insurance policy portfolio — from raw data to automated, periodic reporting.

[![Python](https://img.shields.io/badge/Python-3.10-blue)]()
[![pandas](https://img.shields.io/badge/pandas-data%20processing-150458)]()
[![scikit-learn](https://img.shields.io/badge/scikit--learn-clustering-f7931e)]()
[![License](https://img.shields.io/badge/license-MIT-green)]()

---

## Overview

This project transforms a raw, 105,555-row motor vehicle insurance policy portfolio into a reproducible analytics system covering:

- **KPI tracking** — channel-level loss ratio, claim frequency, and lapse rate
- **Anomaly detection** — a two-layer system (policy-level and channel/trend-level) that flags statistically unusual behavior
- **Customer segmentation** — separate risk/tenure and demographic/vehicle segmentations
- **Automated pipeline** — a periodic (quarterly) process that recomputes all of the above and persists results

The project emphasizes **evidence-driven method selection**: every technique used was tested against the data's actual properties (distribution shape, zero-inflation, trend behavior) rather than applied by default, and every abandoned approach is documented along with the reason it was replaced.

📄 **Full write-up (Executive Summary, Workflow, Results, Data Card):** [Notion page link]

---

## Dataset

- **Source:** [Motor Vehicle Insurance Data — Mendeley Data](https://data.mendeley.com/datasets/5cxyb5fp4f/2)
- **Size:** 105,555 rows, 30 columns
- **Structure:** Panel data — each policyholder (`ID`) has multiple annual renewal records spanning 2015–2018
- **Supplementary file:** a claim-type breakdown table (9 claim categories per policy)

Full variable definitions are documented in [`docs/data_card.md`](docs/data_card.md).

---

## Project Structure

```
├── notebooks/
│   └── motor_insurance_pipeline.ipynb   # full analysis, stage by stage
├── data/
│   ├── raw/                             # original source files
│   └── cleaned/                         # output of Stage 1
├── reports/
│   ├── kpi/                             # per-period KPI tables
│   ├── anomalies/                       # cumulative anomaly log
│   ├── contact_lists/                   # per-period priority contact lists
│   └── segments/                        # customer segment assignments
├── docs/
│   └── data_card.md                     # full variable dictionary
└── README.md
```

---

## Pipeline Stages

| Stage | Description |
|---|---|
| **1. Data Cleaning & Exploration** | Missing value handling, feature engineering, distribution testing, outlier flagging |
| **2. KPI Framework** | A reusable `calculate_kpis()` function computing loss ratio, claim frequency, claim severity, and lapse rate for any breakdown |
| **3. Anomaly Detection** | Policy-level (log-zscore) and channel-level (forecast-based trend deviation) anomaly detection |
| **4. Customer Segmentation** | Two independent K-means segmentations — risk/tenure and demographic/vehicle |
| **5. Automated Pipeline** | `run_periodic_pipeline()` — runs all of the above per period and persists results to `reports/` |

Detailed methodology, including alternative approaches tried and why they were abandoned, is available in the full write-up linked above.

---

## Key Findings

- The **Broker** distribution channel shows a consistently higher loss ratio, claim frequency, and lapse rate than the **Agent** channel.
- Forecast-based anomaly detection identified a portfolio-wide deviation in **2016Q3**, affecting both channels simultaneously.
- Risk segmentation revealed a small (**8.7%**) but distinct customer group with a claim rate 8–20x higher than the rest of the portfolio, despite short tenure.
- Demographic segmentation separated the customer base into luxury-vehicle, high-performance-vehicle, and standard-profile groups — directly usable for product and campaign targeting.

---

## Tech Stack

`Python` · `pandas` · `numpy` · `scipy` · `scikit-learn` · `matplotlib` · `seaborn`

---

## How to Run

```bash
git clone <repo-url>
cd <repo-name>
pip install -r requirements.txt
jupyter notebook notebooks/motor_insurance_pipeline.ipynb
```

The notebook is organized into five clearly labeled stages and can be run top to bottom. Pipeline outputs are written to `reports/`.

---

## License

This project is released under the MIT License. The dataset is publicly available on Mendeley Data under its own license terms — see the [dataset page](https://data.mendeley.com/datasets/5cxyb5fp4f/2) for details.

---

