# 💳 Transaction Dataset

**Spec:** `spec.transactions.json`  
**Output:** `transactions.csv`  

### Fields
- `transaction_id` — UUID  
- `customer_id` — FK → `customer.customers.csv`  
- `transaction_dt` — datetime  
- `amount` — float, Gamma distribution  
- `payment_method` — Credit Card / Debit / ACH / Cash  
- `status` — Settled, Pending, Declined  
- `notes` — Lorem snippet  

### Use Cases
- Fact table for revenue dashboards.  
- Payment method distribution, churn indicators, and monthly revenue trends.

### Example KPIs
- Monthly revenue trend  
- Share of payment methods  
- Transaction success vs. failure rate  
- Average transaction value
