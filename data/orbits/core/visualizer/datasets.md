# The Visualizer • Creative Dataset Brainstorm (for **/visualizer-model/data/orbit/core/visualizer/datasets.md**)

> **Intent:** Curate datasets chosen not for scale or governance, but for *creative visualization practice*. This catalog fuels dashboards, infographics, and interactive prototypes in **The Visualizer**. The emphasis is on *engaging storytelling*, variety of formats, and breadth of domains.

---

## Guiding principles

* **Show craft:** Each dataset should invite a unique visualization challenge (e.g., Sankey, radial charts, geospatial heatmaps, narrative timelines).
* **Portfolio‑ready:** End results should make compelling screenshots, interactive embeds, or Tableau/Power BI stories.
* **Mix mediums:** Prioritize data with temporal, geospatial, categorical, or network dimensions.
* **Creative license:** Favor fun, quirky, or cultural datasets alongside serious themes.

---

## Candidate datasets (creative + engaging)

| Theme           | Dataset                                         | Why it’s visually rich                          | Viz ideas                                                                   |
| --------------- | ----------------------------------------------- | ----------------------------------------------- | --------------------------------------------------------------------------- |
| Music & Culture | **Spotify Charts / Last.fm API**                | Time‑series, genre hierarchies, artist networks | Animated genre evolution, chord progression sunburst, streaming race charts |
| Cinema          | **IMDB / TMDB open data**                       | Film metadata, genres, networks of actors       | Co‑star network graph, movie revenue vs rating bubble chart                 |
| Sports          | **FiveThirtyEight Sports (Elo, NFL, NBA)**      | Game outcomes, rankings                         | Win probability ribbons, Elo race over seasons                              |
| Gaming          | **Steam reviews / game metadata (Kaggle)**      | Text sentiment, tags, playtime                  | Word clouds + network of tags, review sentiment vs hours played             |
| Travel          | **Airbnb Inside Airbnb (public data)**          | Hosts, prices, geospatial points                | Mapbox/PowerBI map layers, host activity density                            |
| Food & Drink    | **Open Brewery DB / Wine reviews (Kaggle)**     | Categorical + text; location                    | Flavor wheel, geo‑map of breweries, ratings vs ABV                          |
| Literature      | **Project Gutenberg word frequencies**          | Text corpora across eras                        | Heatmap of word usage over centuries, streamgraph of themes                 |
| Environment     | **Global Plastic Waste (Our World in Data)**    | Temporal + geospatial                           | Choropleth of waste production, Sankey of flows                             |
| Social Media    | **Reddit Pushshift samples**                    | Temporal text sentiment                         | Topic clusters, meme evolution timelines                                    |
| Astronomy       | **Exoplanet Archive (NASA)**                    | Numeric + categorical                           | Scatter of exoplanet size vs orbit, star map overlays                       |
| Art             | **Museum APIs (MET, Rijksmuseum, Smithsonian)** | Metadata, categories, images                    | Artwork timelines, network of themes/materials                              |
| History         | **Historical Battles / Conflict Data (UCDP)**   | Time + geography                                | Timeline maps, casualty heatmaps, rise/fall visual narratives               |

---

## Quick‑hit starter projects

1. **Spotify genre race chart** → Tableau animation of genres overtaking each other over time.
2. **IMDB network graph** → Python/NetworkX + Gephi visual → export to Power BI/interactive D3.
3. **Airbnb density heatmap** → Power BI geospatial viz with filters by neighborhood.
4. **Wine flavor wheel** → Excel/Power BI radial chart to show descriptors frequency.
5. **Exoplanet scatter** → Tableau/Power BI interactive; filters for discovery method, orbit period.

---

## Skill coverage matrix

| Tool                                                   | Dataset fit                                                   |
| ------------------------------------------------------ | ------------------------------------------------------------- |
| **Power BI**                                           | Airbnb (map layers), Wine reviews (radial), BLS (trend lines) |
| **Tableau**                                            | Spotify charts (animations), Exoplanet Archive (scatter)      |
| **Excel**                                              | Wine flavor wheels, small‑scale trend charts                  |
| **Python (pandas, matplotlib, seaborn/altair/plotly)** | IMDB networks, Reddit topic modeling                          |
| **SQL (DuckDB/BigQuery/Postgres)**                     | Airbnb, FiveThirtyEight sports, Steam reviews                 |

---

## Suggested repo scaffold

```
visualizer-model/
  data/
    orbit/
      core/
        visualizer/
          datasets.md              # ← this file
          creative/
            spotify.yml
            imdb.yml
            airbnb.yml
            wine.yml
            exoplanets.yml
          notebooks/
            spotify_genre_viz.ipynb
            imdb_network.ipynb
            wine_flavor_wheel.xlsx
            airbnb_heatmap.pbix
          tableau/
            exoplanet_scatter.twb
```

Each `creative/*.yml` should include:

```yaml
key: spotify
label: "Spotify Charts"
category: music
legal:
  license: "varies, API terms"
access:
  method: api
  urls:
    - "https://developer.spotify.com/documentation/web-api/"
visual_hooks:
  good_for: [race_chart, timeseries, genre_treemap]
  portfolio_value: high
```

---

## Mini‑backlogs (creative quests)

* **Quest 1:** Build a Tableau race chart from Spotify Top 200 by country.
* **Quest 2:** Create a Power BI Airbnb dashboard with slicers for host vs guest behavior.
* **Quest 3:** Make a flavor wheel from wine tasting notes in Excel, export as polished graphic.
* **Quest 4:** Construct a Python/Plotly network graph of actors who frequently co‑star (IMDB).
* **Quest 5:** Animate exoplanet discoveries in Tableau — scatter by size/orbit with timeline slider.

---

## Next steps

* [ ] Select 2–3 datasets to implement as *launch demos* for The Visualizer.
* [ ] Seed `creative/*.yml` source descriptors.
* [ ] Add first Power BI `.pbix` and Tableau `.twb` files to repo.
* [ ] Capture screenshots/gifs → embed in README.md showcase section.
