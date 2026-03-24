# Jmail Network Explorer

**End-to-end network analysis of the Jeffrey Epstein email archive** — from automated data collection to interactive visualization. Built with Python, NetworkX, and Streamlit.

[![Live Dashboard](https://img.shields.io/badge/Streamlit-Live_Dashboard-FF4B4B?logo=streamlit&logoColor=white)](https://jmail-network-msba.streamlit.app/)
[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://python.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## Overview

This project models a publicly available email archive as a directed communication network and applies graph-theoretic techniques to surface structure, identify influential participants, and visualize relationships at scale.

The dataset contains **~1,533 nodes** and **~2,132 edges** scraped from the [jmail.world](https://jmail.world) public archive, with edge weights representing message volume and timestamps capturing activity windows.

**[→ Launch the live dashboard](https://jmail-network-msba.streamlit.app/)**

<!-- Uncomment and update these once you add screenshots to the repo root -->
<!-- ![Dashboard Overview](dashboard_overview.png) -->
<!-- ![Network Graph](dashboard_network.png) -->

---

## Key Findings

- Jeffrey Epstein dominates the network core with **7,252 outbound** and **6,614 inbound messages** across 451 unique recipients — the highest-degree node by a wide margin
- The giant connected component contains **1,446 of 1,533 participants** (94%), indicating a tightly interconnected core with few isolated clusters
- Louvain community detection identifies **102 communities**, though most participants belong to a single dominant cluster — suggesting a hub-and-spoke topology rather than distinct subgroups
- Graph density is extremely low (**0.00091**), consistent with a network where most communication flows through a small number of high-centrality intermediaries

---

## Key Techniques

- **Graph construction** — directed weighted graph from sender/recipient edge lists using NetworkX
- **Centrality analysis** — degree, weighted degree, betweenness, and eigenvector centrality to rank participant influence
- **Community detection** — Louvain algorithm on the undirected projection to identify cluster structure
- **Ego networks** — configurable radius-1 and radius-2 subgraph extraction for individual-level exploration
- **Force-directed visualization** — PyVis Barnes-Hut simulation with dynamic node sizing, community coloring, and interactive filtering
- **Sankey flow diagrams** — Plotly-based visualization of the strongest sender → recipient message flows

---

## Dashboard

The Streamlit app provides six interactive views:

| Tab | What It Shows |
|---|---|
| **Overview** | KPI cards (participants, edges, density, components, communities), top senders/recipients, community size distribution |
| **Network Graph** | Full force-directed graph with filters for min edge weight, node cap, giant component isolation, and node highlighting. CSV export of filtered edges. |
| **Top Nodes** | Rank participants by any of 8 centrality metrics with bar charts and a sortable, downloadable metrics table |
| **Communities** | Louvain community breakdown with per-member metrics and standalone interactive subgraphs (capped at 150 nodes) |
| **Ego Network** | Select any participant to explore their radius-1 or radius-2 neighborhood — KPIs, strongest ties table, and focused PyVis graph |
| **Relationships** | Sankey diagram of top-N message flows, node spotlight mode, and a global top-25 strongest connections table |

---

## Notebooks

| Notebook | Purpose |
|---|---|
| `notebooks/01_cleaning.ipynb` | Raw data cleaning, deduplication, and schema normalization |
| `notebooks/02_eda.ipynb` | Exploratory network analysis — interactive graph, degree distribution, community detection, ego networks, activity timeline, heatmaps |

---

## Scraper

A two-phase Selenium pipeline (Jupyter notebook) that collects email metadata (sender, recipient, date, subject) from the jmail.world public archive. Phase 1 crawls thread IDs; Phase 2 extracts metadata from each thread. The scraper is resumable — re-running continues from where it left off.

---

## Run Locally

```bash
git clone https://github.com/andrewwhitedata/jmail-network-explorer.git
cd jmail-network-explorer
pip install -r requirements.txt
streamlit run app/app.py
```

Dashboard launches at `http://localhost:8501` and reads from the CSVs in `data/`.

---

## Repo Structure

```
jmail-network-explorer/
├── app/
│   ├── app.py                    # Streamlit dashboard (main entry point)
│   └── utils/
│       ├── charts.py             # PyVis + Plotly visualization
│       ├── data_loader.py        # CSV loading and schema normalization
│       ├── graph_builder.py      # NetworkX graph construction + metrics
│       └── network_views.py      # Graph filtering and layout helpers
├── data/
│   ├── cleaned_edges.csv         # Edge list (sender, recipient, weight)
│   ├── cleaned_nodes.csv         # Node metadata
│   └── network_edge_list.csv     # Aggregated weighted edge list
├── notebooks/
│   ├── 01_cleaning.ipynb         # Data cleaning and preprocessing
│   └── 02_eda.ipynb              # Exploratory network analysis
├── scraper/
│   └── scraper.ipynb             # Two-phase Selenium scraper notebook
├── .streamlit/
│   └── config.toml               # Streamlit theme configuration
├── .gitignore
├── requirements.txt
└── README.md
```

---

## Tech Stack

| Tool | Role |
|---|---|
| **Python** | Core language |
| **NetworkX** | Graph construction, centrality metrics, community detection |
| **Streamlit** | Interactive dashboard framework |
| **PyVis** | Physics-based network visualization |
| **Plotly** | Charts, Sankey diagrams, bar charts |
| **Pandas / NumPy** | Data wrangling |
| **python-louvain** | Louvain community detection |
| **Selenium** | Automated browser scraping |

---

## License

MIT
