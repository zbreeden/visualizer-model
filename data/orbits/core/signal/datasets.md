# The Signal • Open Data Brainstorm (for **/visualizer-model/data/orbits/core/signal/datasets.md**)

> **Intent:** Curate open/public datasets to practice **digital analytics + visualization**: event streams, attention/traffic signals, UX performance, and campaign metrics. Favor sources that pair well with GA4/GTM-style taxonomies and are great for building small star schemas and near-real-time dashboards.

---

## Guiding principles

* **Broadcast mindset:** Prioritize datasets that can be polled or streamed on a cadence (cron/Actions) to simulate “pulses.”
* **Event-first:** Clicks, searches, pageviews, and performance timings that map cleanly to an event table + typed params.
* **Explainable visuals:** Data that produces intuitive KPIs (reach, engagement, conversion, latency, core web vitals).
* **Portfolio-ready:** Interesting enough for GIFs/screens in `/artifacts` and BI models in `/powerbi`.

---

## Candidate datasets (digital analytics + attention)

| Theme           | Dataset                                                         | Access/Format           | Why it fits The Signal                                                   | Visual ideas                                            |
| --------------- | --------------------------------------------------------------- | ----------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------- |
| Web Analytics   | **Google Analytics 4 Sample Export (Google Merchandise Store)** | BigQuery public dataset | Real GA4 events (sessions, items, params) to model star schema + funnels | Session funnels, product performance, event taxonomies  |
| Web Performance | **HTTP Archive (Web Almanac)**                                  | BigQuery public dataset | Page-level perf + tech stack over time; aligns with Core Web Vitals      | Lighthouse score distributions, tech adoption timelines |
| UX Field Data   | **Chrome UX Report (CrUX)**                                     | BigQuery + API          | Real-user CWV per origin; great for RUM-style viz                        | Origin-level CWV timelines, P75 LCP/CLS maps            |
| Knowledge       | **Wikipedia Pageviews**                                         | API + BigQuery          | Clean “attention” time series; topic trends                              | Topic race charts, anomaly detection                    |
| Dev Activity    | **GitHub Archive**                                              | BigQuery streaming      | Event stream (push, issues, stars) for cadence dashboards                | Stars/issues over time, repo “heartbeat”                |
| Social/Tech     | **Hacker News (BigQuery)**                                      | BigQuery                | Posts + scores + comments; ideal for virality curves                     | Decay curves, time-to-top analysis                      |
| Video Trends    | **YouTube Trending (Kaggle mirror)**                            | CSV                     | Title/engagement signals; thumbnail/caption metadata                     | Thumbnail A/B, title sentiment vs views                 |
| Packages        | **npm Downloads (API)**                                         | API JSON                | Install/download counts by package + date                                | Adoption curves, weekday/seasonality heatmaps           |
| Server Logs     | **NASA HTTP Web Server Logs (classic)**                         | CSV/LOG                 | Raw logs for sessionization + bot filtering practice                     | Sessionization funnel, status-code trees                |
| Social Text     | **Pushshift (Reddit) samples**                                  | Parquet/JSON            | Topic + sentiment streams; moderation gaps to clean                      | Topic drift, campaign mentions over time                |

> Tip: When a dataset is extremely large, start with a sampler (date windows, top-N categories) while keeping the pipeline ready for backfills.

---

## Suggested repo scaffold

```
visualizer-model/
  data/
    orbits/
      core/
        signal/
          datasets.md              # ← this file
          sources/
            ga4_sample_bigquery.yml
            http_archive.yml
            crux.yml
            wikipedia_pageviews.yml
            github_archive.yml
            hackernews.yml
            youtube_trending.yml
            npm_downloads.yml
            nasa_http_logs.yml
          schema/
            events/*.schema.yml      # event contracts (name, params, types)
            dims/*.schema.yml        # campaign, content, channel, device
          pipelines/
            ga4_star_model.sql       # fact_events + dims
            crux_cwv_pull.py         # origin-level CWV to parquet
            wiki_pageviews_pull.py   # daily pull + anomaly marks
            gh_archive_pull.sql      # hourly windowed ingest
            nasa_logs_sessionize.ipynb
          powerbi/
            signal_model.pbit        # star schema template
          artifacts/
            README_viz_shots.png
```

Each `sources/*.yml` should include:

```yaml
key: ga4_sample
label: "GA4 Sample (Google Merchandise Store)"
category: web_analytics
cadence: daily
access:
  method: bigquery
  dataset: bigquery-public-data.google_analytics_sample
contracts:
  event_pk: [event_timestamp, event_name, user_pseudo_id]
  param_types:
    cta_id: string
    cta_text: string
    location: string
visual_hooks:
  good_for: [funnels, attribution, retention]
  portfolio_value: high
```

---

## Event taxonomy alignment (Signal-friendly)

Use a small, typed vocabulary that mirrors GA4/GTM best practices:

* `page_view(page_id, page_title)`
* `cta_click(cta_id, cta_text, location)`
* `nav_search(query, results_count)`
* `form_submit(form_id, form_name, fields_count)`

> Map external datasets to this vocabulary with a thin translation layer (e.g., GA4→Signal params, Wikipedia→page\_view, NASA logs→cta\_click where applicable).

---

## Mini‑backlogs (fast wins)

* **Story A — GA4 → Star Schema:** Materialize `fact_events` + `dim_content` + `dim_channel` in SQL; build a Power BI funnel + retention cohort.
* **Story B — CrUX + HTTP Archive:** Blend field (CrUX) and lab (HA) metrics; show CWV trend vs Lighthouse score for a set of origins.
* **Story C — Wikipedia Pulse:** Daily pageview poller with anomaly flags (Z-score) and auto “spike” screenshots for `/artifacts`.
* **Story D — GitHub Heartbeat:** Hourly GH Archive ingest; visualize pushes/issues/stars as a "nervous system" graph.
* **Story E — NASA Logs Sessionizer:** Build a sessionization notebook with bot filtering/UA parsing; export an events table matching the taxonomy.

---

## QA & Observability

* **Freshness:** expected row counts per pull; alert if 0 rows.
* **Validity:** param types + required fields; reject unknown event names.
* **Volume:** rolling 7‑day volume bands (notify on spikes/drops).
* **Lineage:** track dataset → translation → events table → BI model.

---

## Notes on licensing & ethics

* Use only public/aggregate datasets or ones explicitly licensed for analysis.
* Avoid joining across sources in ways that could re-identify individuals.
* Keep telemetry in demos anonymous; never paste real PII.
