# Social issues in cities through urban social networks

**Supervisor:** Sándor Juhász

---

## Overview

This thesis investigates geolocated social networks in U.S. metropolitan areas to uncover how urban social structures reflect and reinforce social issues such as inequality, segregation, and unequal access to opportunities. Using spatially embedded network data, the project aims to provide quantitative insights that can inform urban planning, mobility policies, and neighborhood well-being.

---

## Research questions

- Do network-based communities correspond to spatially segregated neighborhoods defined by infrastructure or historical boundaries?
- How do racial and income inequalities manifest in urban social networks?
- How does the diversity of individuals' social connections relate to income or social mobility at the neighborhood level?
- Does the structure of urban social networks differ systematically across cities with different levels of inequality or segregation?
- Are opportunity-rich areas more interconnected within cities?
- Can network measures predict resilience or vulnerability of neighborhoods to social or economic shocks?
- **Graph neural networks:** Can GNNs predict social ties between neighborhoods by combining socioeconomic, POI, and infrastructure layers (e.g. transit, highways)? Do they do better than a simple gravity model of population and distance? See [City2Graph](https://city2graph.net) and `code/02_city2graph_gnn.ipynb`.

---

## Data

- **Context:** Boston, Detroit, and New York metropolitan areas, census-tract level
- **Social connections:** Twitter mutual-follower ties aggregated to census tract pairs
- **Socioeconomic indicators:** American Community Survey (ACS) 5-year estimates, 2012–2020
- **Points of interest:** Foursquare Open Source Places
- **Geographies:** census tract boundaries, metro area boundaries, NYC PUMAs

The datasets, and how to download them, are described in [`data/README.md`](data/README.md).

---

## Structure of the repository

```
├── code/              # Data overview and a toy GNN example with City2Graph
├── data/              # Data description (README) — files are downloaded separately
│   ├── raw/           # Twitter ties, ACS, POI, census tract and metro area files
│   └── processed/     # Your cleaned / derived datasets
├── figures/           # Generated figures
├── literature/        # Key papers for the conceptual framework
├── notes/             # Basic idea and step-by-step TODO list
└── README.md
```

---

## Literature

Aiello, L. M. et al. (2025) Urban highways are barriers to social ties. _Proceedings of the National Academy of Sciences_, 122 (10), e2408937122. <br>
Kovács, Á. J. et al. (2023) Income-related spatial concentration of individual social capital in cities. _Environment and Planning B: Urban Analytics and City Science_, 50 (4), 1072–1086. <br>
Bokányi, E. et al. (2021) Universal patterns of long-distance commuting and social assortativity in cities. _Scientific Reports_, 11 (1), 20829. <br>
Sato, Y. et al. (2026) City2Graph: A Python library for heterogeneous graph neural networks and spatial analysis in urban systems. _Computers, Environment and Urban Systems_, 130, 102492.
