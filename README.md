# 📊 Sales Performance Automation

**End-to-end automated sales reporting system** — combining an interactive Excel dashboard with an AI-powered n8n pipeline that pulls data, calculates KPIs, generates an AI summary, and delivers the report by email while logging it to Airtable — fully hands-off.

**Author:** Abdelrahman Bahaa Lotfi
**Stack:** Excel (Power Query, Power Pivot, DAX) · n8n · OpenRouter (AI) · Gmail API · Airtable API

---

## 🎥 Demo

A full walkthrough of the dashboard in action and the n8n pipeline running end-to-end — from trigger to the AI-written email landing in the inbox.

**[▶ Watch the demo video](https://youtu.be/HfSfSLSpKEg)**

---

## 🎯 Business Problem

Sales teams typically spend hours every week manually pulling data, building pivot tables, and writing summary emails for stakeholders. This project solves that by combining:

1. A **self-service Excel dashboard** analysts/managers can explore interactively (filter by Year/Territory).
2. A **fully automated n8n pipeline** that runs on a schedule, recalculates KPIs from the live dataset, uses AI to write a natural-language executive summary, and emails it out automatically — with every run logged for audit history.

---

## 🗂️ Dataset

Global B2B sales transactions dataset (2003–2005), 2,823 orders across 4 territories and 7 product lines.

**Key fields:** `ORDERNUMBER`, `SALES`, `ORDERDATE`, `STATUS`, `TERRITORY`, `PRODUCTLINE`, `DEALSIZE`, `CUSTOMERNAME`

---

## 📈 Part 1 — Excel Dashboard

Built with **Power Query** (data cleaning/loading) → **Power Pivot / DAX** (KPI measures) → interactive **Dashboard** with slicers.

### Key KPIs
| Metric | Value |
|---|---|
| Total Revenue | $9,291,501.08 |
| Total Orders | 2,617 |
| Avg Order Value | $3,550.44 |
| Best Territory | EMEA |
| Best Product | Classic Cars |
| Cancelled Orders | 60 |

### Dashboard Features
- **Revenue Trend** by Year/Quarter
- **Territory Performance** (EMEA, NA, APAC, Japan)
- **Product Mix** breakdown (Classic Cars, Vintage Cars, Motorcycles, Trucks & Buses, Planes, Ships, Trains)
- **Deal Size distribution** (Small / Medium / Large)
- Interactive **slicers** for Year and Territory — dashboard recalculates live

📷 See `/screenshots/dashboard.png` and `/screenshots/dashboard_filtered.png`

**Workbook structure (sheets):** `Raw_Data` → `KPIs` → `Pivots` → `Charts` → `Dashboard` → `Auto_Report` (the last sheet is the bridge that feeds the n8n automation below).

---

## 🤖 Part 2 — n8n Automation Pipeline

An end-to-end automated pipeline that turns raw sales data into a delivered, AI-written report — no manual work required.

**Workflow:** `Schedule Trigger → HTTP Request → Parse CSV → Calculate KPIs (Code) → Generate AI Summary (OpenRouter) → Format Message → Send Gmail → Log to Airtable`

📷 See `/screenshots/n8n_workflow.png` and `/screenshots/n8n_code_node.png`

### How it works
1. **Schedule Trigger** — runs the pipeline automatically on a set interval.
2. **HTTP Request** — pulls the latest sales CSV directly from Google Drive.
3. **Parse CSV** — parses 2,800+ rows into structured JSON.
4. **Calculate KPIs (JavaScript Code node)** — computes `totalRevenue`, `totalOrders`, `avgOrder`, `bestTerritory`, and `bestProduct` on the fly by aggregating the parsed rows.
5. **Generate AI Summary (OpenRouter Chat Model)** — feeds the calculated KPIs to an LLM to generate a natural-language executive summary of performance.
6. **Format Message** — structures the AI output + KPIs into a clean email body.
7. **Send Gmail** — delivers the report automatically to stakeholders.
8. **Log to Airtable** — archives every report run (date, KPIs, AI summary) for historical tracking.

### KPI Calculation Logic (excerpt)
```javascript
const totalOrders = shipped.length;
const avgOrder = totalRevenue / totalOrders;

const territoryMap = {};
shipped.forEach(r => {
  territoryMap[r.TERRITORY] =
    (territoryMap[r.TERRITORY] || 0) + parseFloat(r.SALES || 0);
});
const bestTerritory = Object.entries(territoryMap)
  .sort((a, b) => b[1] - a[1])[0][0];
```

---

## 🛠️ Tools & Skills Demonstrated

- **Data Analysis:** Power Query, Power Pivot, DAX, Data Modeling, Pivot Tables, Dashboard Design
- **Automation / AI Agents:** n8n, JavaScript (Code nodes), REST API integration, LLM prompting (OpenRouter)
- **Integrations:** Gmail API, Airtable API, Google Drive
- **Concepts:** ETL pipeline design, scheduled automation, AI-generated reporting, KPI engineering

---

## 📁 Repository Structure

```
Sales-Performance-Automation/
├── README.md
├── excel/
│   └── Sales_Analysis_Portfolio.xlsx
├── n8n/
│   └── Sales-Report-Automation.json      # exported workflow
└── screenshots/
    ├── dashboard.png
    ├── dashboard_filtered.png
    ├── n8n_workflow.png
    └── n8n_code_node.png
```

> 📌 **Note on the demo video:** it's hosted on YouTube (linked above) rather than committed to the repo — video files are too large for git and GitHub doesn't preview `.mp4` inline, so a hosted link gives better playback and reach anyway.

---

## 🚀 Key Takeaway

This project shows the full loop of a modern data role: **analyze → automate → deliver**. Instead of stopping at a static dashboard, the pipeline closes the loop by turning insights into a recurring, AI-summarized report delivered with zero manual effort — the same pattern used in real BI/automation ops at companies today.

---

## 📬 Contact

**Abdelrahman Bahaa Lotfi** — Data Analyst & AI Automation Specialist
🔗 [LinkedIn](#) · [GitHub](https://github.com/abdelrahmanbahaa-lotfy)
