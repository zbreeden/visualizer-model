# 🏭 Manufacturing Datasets

**Specs:**
- `spec.work_orders.json` → `work_orders.csv`
- `spec.maintenance_logs.json` → `maintenance_logs.csv`

### Work Orders Fields
- `order_id`, `product_id`, `plant`  
- `start_dt`, `end_dt`  
- `quantity`, `scrap_pct`, `status`

### Maintenance Logs Fields
- `maintenance_id`, `machine_id`, `maintenance_dt`  
- `technician`, `issue_code`, `duration_hrs`, `cost_usd`, `status`

### Use Cases
- Production and operations dashboards.  
- Scrap % monitoring, downtime analysis, maintenance cost trends.

### Example KPIs
- On-time vs. delayed work orders  
- Scrap % by product  
- Maintenance cost per machine  
- Average downtime hours
