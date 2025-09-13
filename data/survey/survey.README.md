# 📝 Survey Dataset

**Spec:** `spec.survey.json`  
**Output:** `survey.csv`  

### Fields
- `respondent_id` — UUID  
- `submitted_at` — datetime  
- `city`, `age`, `gender`, `channel`  
- `nps` — 0–10  
- `sat_overall` — Likert-5  
- `ease_of_use` — Likert-5  
- `cs_response_time` — Likert-7  
- `feature_importance` — multi-choice (Price, Quality, Support, etc.)  
- `open_feedback` — lorem text  

### Use Cases
- Voice-of-customer dashboards.  
- NPS and satisfaction tracking, feature prioritization.

### Example KPIs
- NPS score over time  
- Top 3 features valued by respondents  
- Average satisfaction by channel  
- Survey completion demographics
