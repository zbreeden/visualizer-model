# FourTwenty Analytics • Open Data Brainstorm (for **/visualizer-model/data/orbits/elemental/fourtwentyanalytics/datasets.md**)

> **Intent:** Curate open datasets that showcase *data engineering credibility* (ingest → model → serve → visualize) for The Visualizer. Favor **large scale**, **high‑velocity**, **semi‑structured**, or **messy** sources that let us prove pipelines, orchestration, governance, and cost‑aware architecture. Treat this file as a living seed; add PRs as modules spin up.

---

## Guiding principles

* **Barycenter first:** Prioritize sources that benefit *multiple* modules (Bank, Protector, Gambler, Coach, Story, Archive, Signal).
* **Demonstrate the stack:** Batch + streaming, partitioning, SCD types, schema drift handling, quality tests, and lineage.
* **Public + permissive:** Prefer sources we can redistribute (or at least reference reproducibly).
* **Observable:** Sources with update schedules to exercise orchestration (cron/Actions/Airflow/Prefect) and monitoring.

---

## Portfolio‑class dataset short list (mix of size, velocity, messiness)

| Domain            | Dataset                              |         Scale | Freshness        | Format / Access            | Why it’s great for engineering                                                       | Visualizer angles                             |
| ----------------- | ------------------------------------ | ------------: | ---------------- | -------------------------- | ------------------------------------------------------------------------------------ | --------------------------------------------- |
| Web / NLP         | **Common Crawl**                     |       100s TB | Monthly          | WARC on S3; index APIs     | Massive raw web text; needs partitioned ingest, de‑dup, robots/filters, cost control | Topic maps, language coverage, crawl heatmaps |
| Images / ML       | **LAION‑5B**                         |  \~240M+ rows | Static           | Parquet/TSV + S3 links     | Petabyte‑adjacent; external object refs; data hygiene; copyright flags               | CLIP embeddings explorer, similarity search   |
| Scholarly / Graph | **OpenAlex**                         |   250M+ works | Daily            | JSONL dumps + API          | Incremental upserts; graph joins; SCD on entities                                    | Knowledge graph, field growth trends          |
| News / Events     | **GDELT**                            |       100s GB | 15‑min           | CSV via HTTP; BigQuery     | Streaming append; late events; internationalization                                  | Event timelines, sentiment world map          |
| Maps / Geo        | **OpenStreetMap (planet + diffs)**   |        1–2 TB | Minute           | PBF; minutely diffs        | Complex schema; spatial indexing; changefeeds                                        | Change animations, POI density                |
| Finance / Filings | **SEC EDGAR**                        | 10s TB (docs) | Daily            | S3 bulk + API              | Text + XBRL; entity resolution; parse + enrich                                       | Filing volumes, KPI extraction                |
| Mobility          | **NYC TLC Trips**                    |    200M+ rows | Monthly          | CSV/Parquet; S3            | Partition strategy; cost/perf tuning; anonymization                                  | Taxis over time, geospatial flows             |
| Weather           | **NOAA/NWS (GHCN/ISD + alerts)**     |       100s GB | Hourly–Daily     | FTP/HTTPS; APIs            | Multi‑granularity time series; late corrections                                      | Climate normals vs current                    |
| Health (public)   | **CMS Medicare/Medicaid (PUF)**      |        10s GB | Annual/Quarterly | CSV; downloadable          | Large fact tables; HCC risk adj; HIPAA‑safe                                          | Procedure mix, cost maps                      |
| Safety            | **NFIRS (Fire Incidents)**           |       100s GB | Annual           | Flat files (needs reg)     | Wide schema; code tables; data quality rules                                         | Incident cause, response time                 |
| Economics         | **Bureau of Labor Statistics (BLS)** |        10s GB | Monthly          | API; flat files            | Hierarchical series; seasonal adj                                                    | CPI/Unemployment trackers                     |
| Social            | **Pushshift (Reddit archive)**       |        10s TB | Historical       | Zstd JSON; parquet mirrors | Text+graph; deletions; moderation gaps                                               | Topic drift, community maps                   |

> **Note:** We can stage truly massive sources (Common Crawl, OSM planet) as *samplers* to demonstrate architecture patterns, while still showing we can scale the approach.

---

## Tactically excellent, resume‑friendly starters (balanced scope)

1. **OpenAlex → parquet lake → DuckDB/BigQuery → vector index**

   * *Goals:* Incremental sync, SCD merges on `works`, `authors`, `institutions`; build embeddings of titles/abstracts; serve similarity search.
   * *What it proves:* Idempotent ELT, columnar storage, partitioning by year, vector search integration, semantic dashboards.

2. **SEC EDGAR 10‑K/10‑Q XBRL → structured facts → quality gates**

   * *Goals:* Parse and normalize key facts (Revenue, OCF, FCF) across filers; entity resolution via CIK; SCD Type 2 for company metadata.
   * *What it proves:* Complex parsing, governance, reproducibility, and comparability across issuers.

3. **GDELT events → streaming append → late‑data watermarking**

   * *Goals:* Ingest every 15 minutes; watermark windows; duplicate suppression; alerting on pipeline lag.
   * *What it proves:* Streaming orchestration, SLA/SLOs, ops metrics.

4. **NYC TLC trips → partition design bake‑off**

   * *Goals:* Benchmark monthly vs daily vs `pickup_date` partitions across DuckDB, BigQuery, and Spark on S3.
   * *What it proves:* Cost/performance tuning, Z‑ordering, file size heuristics.

5. **OpenStreetMap diffs → slowly changing geo**

   * *Goals:* Apply minutely diffs, materialize POI snapshots, index with H3; geofencing joins with external datasets (e.g., permits).
   * *What it proves:* Change data capture, spatial indexing, incremental materialization.

---

## Pipeline patterns to showcase

* **Bronze/Silver/Gold** layers with explicit contracts (schema registry + JSONSchema/YAML schemas in `/schema/…`).
* **Validation:** Great Expectations/DBT tests; freshness & volume checks; referential integrity.
* **Orchestration:** GitHub Actions for small jobs; Prefect/Temporal/Airflow for scheduled & backfills; parametrized runs via env.
* **Storage formats:** Parquet + partitioning; Delta/Iceberg for ACID tables where needed; DuckDB for prototyping.
* **CDC & SCD:** Watermarks; Type 1/2 strategies; late‑arriving handling; idempotent upserts (MERGE).
* **Observability:** Metrics (rows, bytes, partitions), lineage graphs, data quality dashboards in *The Archive* and *The Signal*.

---

## Suggested repo scaffold (drop‑in for this module)

```
visualizer-model/
  data/
    orbits/
      elemental/
        fourtwentyanalytics/
          datasets.md              # ← this file
          sources/
            openalex.yml           # src config (endpoint, cadence, sample size, legal)
            sec_edgar.yml
            gdelt.yml
            nyctlc.yml
            osm.yml
          schema/
            openalex/*.schema.yml
            sec_edgar/*.schema.yml
          pipelines/
            openalex_ingest.py
            openalex_model_dbt/
            sec_xbrl_parse.py
            gdelt_stream.py
            nyctlc_partition_bakeoff.ipynb
          notebooks/
            exploration_openalex.ipynb
            edgar_fact_extractor.ipynb
```

Each `sources/*.yml` should include:

```yaml
key: openalex
label: "OpenAlex (Scholarly Graph)"
owner: "Our Research"
legal:
  license: "CDLA-Sharing"
  redistribution_ok: true
cadence: daily
access:
  method: "bulk + api"
  auth: none
  urls:
    - "https://…"
volume_estimate:
  rows: 250000000
  bytes_gb: 500
contracts:
  primary_keys: [work_id]
  natural_keys: [doi]
  partition: [year]
  slas:
    freshness_minutes: 1440
quality:
  expectations: [not_null(work_id), valid_year(year), unique(work_id)]
```

---

## Mini‑backlogs (stories you can implement quickly)

* **Story A:** *OpenAlex incremental sync* — bootstrap year partitions, MERGE on `work_id`, materialize `gold.top_fields` (domain, year, citations, embeddings).
* **Story B:** *SEC XBRL fact normalizer* — extract `us-gaap` facts to tall table; unit normalization; build `gold.financial_kpis`.
* **Story C:** *GDELT stream* — 15‑min fetcher with watermark; dedupe on `(GLOBALEVENTID)`; push lag metrics to *The Signal*.
* **Story D:** *NYC TLC bake‑off* — notebook comparing scan costs and latencies for three partition strategies; publish results as markdown.
* **Story E:** *OSM POI snapshots* — daily diff apply; H3 index; density tiles for Visualizer.

---

## Licensing & ethics quick notes

* Confirm redistribution terms (e.g., LAION links vs media files). Keep only metadata or signed URLs where required.
* Respect robots/ToS for crawls; throttle; add `User-Agent` & contact.
* For health/finance, only use public, de‑identified datasets (no re‑id attempts).

---

## Next seed PRs to open in this module

* [ ] Add `sources/openalex.yml` with realistic fields
* [ ] Add `pipelines/openalex_ingest.py` (requests → parquet → DuckDB) with `--since` support
* [ ] Add `pipelines/gdelt_stream.py` with watermark + retries
* [ ] Add `notebooks/nyctlc_partition_bakeoff.ipynb`
* [ ] Add `schema/openalex/work.schema.yml` + dbt tests
* [ ] Create `README.md` section linking datasets → demo dashboards in **The Visualizer**
