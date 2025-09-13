# 👥 Customer Dataset

**Spec:** `spec.customers.json`  
**Output:** `customers.csv`  

### Fields
- `customer_id` — UUID primary key  
- `first_name`, `last_name`, `full_name`  
- `email`, `city`, `state`, `zip`  
- `signup_date` — YYYY-MM-DD  
- `is_active` — Boolean flag (0/1)  
- `age`  

### Use Cases
- Dimension table for transactions, invoices, or CRM simulations.  
- Useful for demographic dashboards and customer lifecycle KPIs.

### Example KPIs
- Active customers over time  
- City/region distribution  
- Average customer age  
- Signup cohorts
