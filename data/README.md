# 📊 The Visualizer — Data Chamber

Welcome to the **Data Chamber**, the central library of fictionalized datasets for demos, simulations, and KPI visualizations.  
This chamber houses the **`synth_data.py`** generator and a growing scroll of dataset specifications and CSVs across multiple internal system domains.

---

## 🔨 Synthesizer
- **File:** `synth_data.py`  
- **Purpose:** A universal synthetic data generator.  
- **Usage:**
  ```bash
  python data/synth_data.py --spec data/<domain>/spec.<name>.json
- **Spec Format:** JSON (YAML supported if PyYAML is installed)
- **Supported column types**: uuid, int, float, bool, choice, multichoice, date, datetime, first_name, last_name, city, state, zip, email, lorem, poisson, likert5, likert7, fk, derive.

---

## 📂 Scrolls (Domains)
- **👥 Customer**
  - Spec: customer/spec.customers.json
  - Output: customer/customers.csv
  - Fields: IDs, names, signup dates, demographics, activity flag.
  - Use Case: Dimension table for transactions, invoices, or CRM demos.
- **💳 Transaction**
  - Spec: transaction/spec.transactions.json
  - Output: transaction/transactions.csv
  - Fields: Linked to customers, includes amounts, payment methods, statuses, notes.
  - Use Case: Fact table for dashboards, KPI analysis (e.g., revenue trends, churn).
- **📝 Survey**
  - Spec: survey/spec.survey.json
  - Output: survey/survey.csv
  - Fields: Respondent details, NPS, Likert responses, multi-choice feature importance, open-text feedback.
  - Use Case: Voice-of-customer, experience analytics, sentiment dashboards.
- **🏭 Manufacturing**
  - Specs:
    - manufacturing/spec.work_orders.json → work_orders.csv
    - manufacturing/spec.maintenance_logs.json → maintenance_logs.csv
  - Fields: Work orders (product, plant, quantities, status), machine logs (issues, duration, cost).
  - Use Case: Operations dashboards, downtime tracking, OEE (Overall Equipment Effectiveness).
- **🛡️ Safety**
  - Spec: safety/spec.incidents.json → incidents.csv
  - Fields: Site incidents with severity, reporter, resolution.
  - Use Case: Compliance dashboards, incident trends, safety KPIs.
  - Tie-In: Demonstrates The Protector module.
- **👩‍💼 HR**
  - Spec: hr/spec.employees.json → employees.csv
  - Fields: Employee IDs, roles, hire dates, salaries, employment status.
  - Use Case: Workforce analytics, headcount dashboards, attrition modeling.
  - Tie-In: Demonstrates The Benefit module.
- **💰 Finance**
  - Spec: finance/spec.invoices.json → invoices.csv
  - Fields: Invoice amounts, due dates, status (open, paid, late, disputed).
  - Use Case: AR aging, revenue dashboards.
  - Tie-In: Demonstrates The Bank module.
- **🚚 Logistics**
  - Spec: logistics/spec.shipments.json → shipments.csv
  - Fields: Shipments with origin, destination, carrier, timing, status.
  - Use Case: Supply chain dashboards, on-time delivery KPIs.

---

## 🌱 Extending the Library
- **To add new scrolls**:
  - Create a subfolder under data/ (e.g., data/erp/).
  - Write a spec.<table>.json describing the schema.
- **Run**:
    ```bash
    python3 data/synth_data.py --spec data/<domain>/spec.<name>.json
- **Use the resulting datafile to run any module**.

---

## 🚀 Example KPIs
- **Customer**: Active customers over time, avg. age, city distribution.
- **Transaction**: Revenue per month, payment method share, failure rates.
- **Survey**: NPS, satisfaction breakdown, top features.
- **Manufacturing**: Scrap %, downtime hours, maintenance cost trends.
- **Safety**: Incident frequency, severity trends, closure rate.
- **HR**: Headcount by department, salary distribution, attrition rate.
- **Finance**: Invoice aging buckets, DSO (Days Sales Outstanding).
- **Logistics**: On-time delivery %, carrier performance, route volumes.

---

## 📌 This chamber is the living data seedbed for The Visualizer
- a ready-made library of internal datasets that can power dashboards, storytelling, and simulations across the constellation.
- a repository for other modules to create their own datasets.
- a repository used for improving the suitekeeper's data visualization skills using systems such as Tableau and Power BI.


