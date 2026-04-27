# 🍫 SectionB\_G-17 — Chocolate Sales Dataset

> **NST DVA Capstone 2** | Newton School of Technology · Data Visualization & Analytics
> A 2-week industry simulation converting raw chocolate retail data into actionable business intelligence using Python, GitHub, and Tableau.

---

## ⚡ Before You Start

| Step | Action |
|------|--------|
| 1 | Ensure all five raw CSVs are downloaded from the links in `data/raw/raw.md` and placed inside `data/raw/` |
| 2 | Complete the notebooks in order: `01` → `05` |
| 3 | Publish the final Tableau dashboard and add the public link in `tableau/dashboard_links.md` |
| 4 | Export the final report and presentation as PDFs into `reports/` |

---

## 🚀 Quick Start

**Working locally:**

```bash
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook
```

**Working in Google Colab:**

- Upload or sync notebooks from `notebooks/`
- Keep final `.ipynb` files committed to GitHub
- Export cleaned datasets into `data/processed/`

---

## 📋 Project Overview

| Field | Details |
|-------|---------|
| **Project Title** | Chocolate Sales Performance Analysis & Business Intelligence |
| **Sector** | Retail / FMCG (Fast-Moving Consumer Goods) |
| **Team ID** | G-17 |
| **Section** | Section B |
| **Faculty Mentor** | To be filled by team |
| **Institute** | Newton School of Technology |
| **Submission Date** | To be filled by team |

---

## 👥 Team Members

| Role | Name | GitHub Username |
|------|------|-----------------|
| Project Lead | Name | github-handle |
| Data Lead | Name | github-handle |
| ETL Lead | Name | github-handle |
| Analysis Lead | Name | github-handle |
| Visualization Lead | Name | github-handle |
| Strategy Lead | Name | github-handle |
| PPT & Quality Lead | Name | github-handle |

---

## 💼 Business Problem

The global chocolate confectionery market is highly competitive, with sales performance varying significantly across geographies, product lines, store formats, and customer segments. Retail decision-makers need a clear, data-driven picture of where revenue is being generated, which products are underperforming, and which customer segments drive the most value — without wading through raw spreadsheets.

This project transforms five interconnected raw datasets (sales transactions, product catalogue, store master, customer demographics, and calendar) into a unified analytical pipeline that surfaces the KPIs a regional sales director would act upon.

### 🎯 Core Business Question

> **Which combination of product category, store type, and customer segment drives the highest revenue per transaction — and where are the biggest untapped growth opportunities across the portfolio?**

### ✅ Decision Supported

The analysis enables the regional sales director to reallocate promotional budgets toward the highest-ROI product-store-customer combinations, discontinue or reposition low-performing SKUs, and prioritise expansion in high-potential store clusters.

---

## 📦 Dataset

| Attribute | Details |
|-----------|---------|
| **Source Name** | Internal Chocolate Retail Simulation Dataset |
| **Raw Files** | `calender.csv`, `customers.csv`, `products.csv`, `sales.csv`, `stores.csv` |
| **Direct Access** | See `data/raw/raw.md` for individual Google Drive links |
| **Row Count** | > 5,000 sales transactions (combined across files) |
| **Column Count** | > 8 meaningful columns across the star schema |
| **Time Period Covered** | As per `calender.csv` date range |
| **Format** | CSV |

### 🔑 Key Columns Used

| Column Name | Source File | Description | Role in Analysis |
|-------------|-------------|-------------|-----------------|
| `Date` | `calender.csv` | Transaction / calendar date | Time-series KPI, trend analysis |
| `CustomerID` | `customers.csv` | Unique customer identifier | Segmentation, repeat purchase rate |
| `Gender` / `Age` | `customers.csv` | Customer demographics | Segment-level revenue breakdowns |
| `ProductID` | `products.csv` | Unique product identifier | SKU-level profitability |
| `Category` | `products.csv` | Product category (e.g., Dark, Milk, White) | Filter / dashboard dimension |
| `CostPerBox` / `PricePerBox` | `products.csv` | Unit cost and price | Margin computation |
| `SaleID` | `sales.csv` | Unique transaction ID | Transaction count, deduplication |
| `Boxes` | `sales.csv` | Units sold per transaction | Revenue and volume KPIs |
| `Amount` | `sales.csv` | Revenue per transaction | Core revenue KPI |
| `StoreID` | `stores.csv` | Unique store identifier | Store-level benchmarking |
| `Region` / `Country` | `stores.csv` | Geographic attributes | Regional performance filter |

> For full column definitions, data types, and cleaning notes, see [`docs/data_dictionary.md`](docs/data_dictionary.md).

---

## 📊 KPI Framework

| KPI | Definition | Formula / Computation |
|-----|-----------|----------------------|
| **Total Revenue** | Gross sales value across all transactions | `SUM(Amount)` from `sales.csv` |
| **Revenue per Box** | Average selling price per unit | `SUM(Amount) / SUM(Boxes)` |
| **Gross Margin %** | Profitability at product level | `(PricePerBox − CostPerBox) / PricePerBox × 100` |
| **Monthly Revenue Growth %** | MoM change in total revenue | `(Revenue_M − Revenue_M-1) / Revenue_M-1 × 100` |
| **Repeat Purchase Rate** | Share of customers with > 1 transaction | `Customers with ≥ 2 SaleIDs / Total Customers × 100` |
| **Revenue by Category** | Revenue split across chocolate types | `SUM(Amount) GROUP BY Category` |
| **Top 10 Products by Revenue** | Highest-grossing SKUs | `SUM(Amount) GROUP BY ProductID ORDER BY DESC LIMIT 10` |
| **Store Performance Index** | Revenue per store normalised by region | `Store Revenue / Regional Avg Revenue` |
| **Average Order Value (AOV)** | Average transaction value | `SUM(Amount) / COUNT(DISTINCT SaleID)` |

> KPI logic is fully documented in [`notebooks/04_statistical_analysis.ipynb`](notebooks/04_statistical_analysis.ipynb) and [`notebooks/05_final_load_prep.ipynb`](notebooks/05_final_load_prep.ipynb).

---

## 📈 Tableau Dashboard

| Item | Details |
|------|---------|
| **Dashboard URL** | *(Paste Tableau Public link here)* |
| **Executive View** | High-level KPI scorecard — Total Revenue, Gross Margin %, MoM Growth, Top Category |
| **Operational View** | Drill-down by Product, Store, Region, and Customer Segment with trend lines |
| **Main Filters** | Date Range · Product Category · Store Region · Customer Gender · Age Band |

> Store dashboard screenshots in `tableau/screenshots/` and document the public link in [`tableau/dashboard_links.md`](tableau/dashboard_links.md).

---

## 💡 Key Insights

> *(To be completed after analysis. Each insight should tell the reader **what to act on**, not just describe a chart.)*

1. **Insight 1** — *(e.g., Dark Chocolate drives 42 % of total revenue despite being 28 % of SKUs — prioritise shelf space.)*
2. **Insight 2** — *(e.g., Repeat customers account for 61 % of revenue but represent only 38 % of the customer base.)*
3. **Insight 3** — *(e.g., The North-East region records 23 % higher AOV, suggesting premium pricing headroom.)*
4. **Insight 4** — *(e.g., Q4 accounts for 38 % of annual revenue — inventory planning must front-load Q3.)*
5. **Insight 5** — *(e.g., Female customers in the 25–34 age band show the highest average spend per visit.)*
6. **Insight 6** — *(e.g., Three SKUs with negative gross margin are still active — immediate discontinuation saves cost.)*
7. **Insight 7** — *(e.g., Stores with in-store promotions record 2.1× higher basket size.)*
8. **Insight 8** — *(e.g., White Chocolate category is declining MoM over the last 6 months — reformulation or phaseout warranted.)*

---

## 🎯 Recommendations

| # | Insight Addressed | Recommendation | Expected Impact |
|---|-------------------|---------------|----------------|
| 1 | Dark Chocolate revenue concentration | Increase Dark Chocolate SKU count and shelf prominence in top 20 % stores | +8–12 % category revenue in 2 quarters |
| 2 | Repeat customer value | Launch targeted loyalty programme for high-frequency buyers | +15 % retention, +10 % revenue per customer |
| 3 | North-East premium AOV | Pilot premium gifting bundles exclusively in North-East stores | +5 % AOV region-wide |
| 4 | Q4 revenue spike | Build 6-week pre-Q4 inventory buffer; negotiate supplier capacity by August | Eliminate Q4 stockouts (~3 % lost revenue) |
| 5 | Negative-margin SKUs | Immediately discontinue or reprice the 3 identified loss-making SKUs | Direct COGS saving of estimated ₹X per quarter |

---

## 🗂️ Repository Structure

```
SectionB_G-17_ChocolateSalesDataset/
│
├── README.md                            # ← You are here
│
├── data/
│   ├── raw/                             # Original CSVs (never edited)
│   │   └── raw.md                       # Google Drive download links
│   └── processed/                       # Cleaned output from ETL pipeline
│
├── notebooks/
│   ├── 01_extraction.ipynb              # Data sourcing & loading
│   ├── 02_Cleaning.ipynb                # Cleaning & transformation
│   ├── 03_eda.ipynb                     # Exploratory data analysis
│   ├── 04_statistical_analysis.ipynb    # Statistical tests & KPI computation
│   └── 05_final_load_prep.ipynb         # Final dataset for Tableau
│
├── scripts/
│   └── etl_pipeline.py                  # Reusable ETL pipeline
│
├── tableau/
│   ├── screenshots/                     # Dashboard screenshots
│   └── dashboard_links.md               # Tableau Public URLs
│
├── reports/
│   ├── README.md
│   ├── project_report_template.md
│   └── presentation_outline.md
│
├── docs/
│   └── data_dictionary.md               # Full column definitions & cleaning notes
│
├── DVA-oriented-Resume/
└── DVA-focused-Portfolio/
```

---

## 🔄 Analytical Pipeline

```
1. DEFINE     →  Sector scoped (Chocolate Retail), business question defined, mentor approved
2. EXTRACT    →  5 raw CSVs sourced from Drive, committed to data/raw/; data dictionary drafted
3. CLEAN      →  Null handling, type casting, joins, deduplication in 02_Cleaning.ipynb
4. ANALYZE    →  EDA (03), statistical analysis & KPI computation (04)
5. VISUALIZE  →  Interactive Tableau dashboard published on Tableau Public
6. RECOMMEND  →  5 data-backed business recommendations delivered
7. REPORT     →  Final report & deck exported as PDFs into reports/
```

---

## 🛠️ Tech Stack

| Tool | Status | Purpose |
|------|--------|---------|
| Python + Jupyter Notebooks | ✅ Mandatory | ETL, cleaning, EDA, KPI computation |
| Google Colab | ✔️ Supported | Cloud notebook execution |
| Tableau Public | ✅ Mandatory | Dashboard design, publishing & sharing |
| GitHub | ✅ Mandatory | Version control, collaboration, contribution audit |
| SQL | ⚙️ Optional | Initial data extraction only, if documented |

**Recommended Python libraries:** `pandas` · `numpy` · `matplotlib` · `seaborn` · `scipy` · `statsmodels`

---

## 🏆 Evaluation Rubric

| Area | Marks | Focus |
|------|-------|-------|
| Problem Framing | 10 | Is the business question clear and well-scoped? |
| Data Quality & ETL | 15 | Is the cleaning pipeline thorough and documented? |
| Analysis Depth | 25 | Are statistical methods applied correctly with insight? |
| Dashboard & Visualization | 20 | Is the Tableau dashboard interactive and decision-relevant? |
| Business Recommendations | 20 | Are insights actionable and well-reasoned? |
| Storytelling & Clarity | 10 | Is the presentation professional and coherent? |
| **Total** | **100** | |

> Marks are awarded for **analytical thinking and decision relevance**, not chart quantity, visual decoration, or code length.

---

## ✅ Submission Checklist

### GitHub Repository
- [ ] Public repository with correct naming convention (`SectionB_G-17_ChocolateSalesDataset`)
- [ ] All notebooks committed in `.ipynb` format
- [ ] `data/raw/` contains download links to the original, unedited dataset
- [ ] `data/processed/` contains cleaned pipeline output
- [ ] `tableau/screenshots/` contains dashboard screenshots
- [ ] `tableau/dashboard_links.md` contains the Tableau Public URL
- [ ] `docs/data_dictionary.md` is complete
- [ ] `README.md` explains the project, dataset, and team
- [ ] All members have visible commits and pull requests

### Tableau Dashboard
- [ ] Published on Tableau Public and accessible via public URL
- [ ] At least one interactive filter included
- [ ] Dashboard directly addresses the business problem

### Project Report
- [ ] Final report exported as PDF into `reports/`
- [ ] Cover page, executive summary, sector context, problem statement
- [ ] Data description, cleaning methodology, KPI framework
- [ ] EDA with written insights, statistical analysis results
- [ ] Dashboard screenshots and explanation
- [ ] 8–12 key insights in decision language
- [ ] 3–5 actionable recommendations with impact estimates
- [ ] Contribution matrix matches GitHub history

### Presentation Deck
- [ ] Final presentation exported as PDF into `reports/`
- [ ] Title slide through recommendations, impact, limitations, and next steps

### Individual Assets
- [ ] DVA-oriented resume updated to include this capstone
- [ ] Portfolio link or project case study added

---

## 🧑‍💻 Contribution Matrix

> This table must match evidence in GitHub Insights, PR history, and committed files.

| Team Member | Dataset & Sourcing | ETL & Cleaning | EDA & Analysis | Statistical Analysis | Tableau Dashboard | Report Writing | PPT & Viva |
|------------|-------------------|---------------|---------------|---------------------|------------------|---------------|-----------|
| Member 1 | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support |
| Member 2 | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support |
| Member 3 | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support |
| Member 4 | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support |
| Member 5 | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support |
| Member 6 | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support | Owner / Support |

> **Declaration:** We confirm that the above contribution details are accurate and verifiable through GitHub Insights, PR history, and submitted artifacts.
>
> **Team Lead Name:** _____________________________
>
> **Date:** _______________

---

## 🎓 Academic Integrity

All analysis, code, and recommendations in this repository must be the original work of the team listed above. Free-riding is tracked via GitHub Insights and pull request history. Any mismatch between the contribution matrix and actual commit history may result in individual grade adjustments.

---

<div align="center">

**Newton School of Technology · Data Visualization & Analytics · Capstone 2**<br>
*Section B — Group 17 — Chocolate Sales Dataset*

</div>
