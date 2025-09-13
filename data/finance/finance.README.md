# 💰 Finance Dataset

**Spec:** `spec.invoices.json`  
**Output:** `invoices.csv`  

### Fields
- `invoice_id`, `customer_id` (FK → customers)  
- `invoice_dt`, `due_dt`  
- `amount_usd`  
- `status` — Open, Paid, Late, Disputed  

### Use Cases
- AR dashboards.  
- Invoice aging, DSO (Days Sales Outstanding).  
- Demonstrates *The Bank* module.

### Example KPIs
- AR aging buckets (0–30, 31–60, 61–90, 90+)  
- % invoices paid on time  
- Average invoice size  
- DSO (Days Sales Outstanding)
