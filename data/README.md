# Data

The data files are **not stored on GitHub** because they are too large. The supervisor provides a download link.

Copy the **contents** of the downloaded folder into `data/raw/` (do not create an extra subfolder). The notebooks in `code/` expect exactly this structure:

```
data/
├── raw/
│   ├── twitter_tract_tract_edgelist.csv
│   ├── acs_data_census_tracts_2012_2020.csv.gz
│   ├── places_US_newyork_boston_detroit.parquet
│   ├── censustract_geoms_top50.geojson
│   ├── nyc_puma_all.geojson
│   └── metro_areas/                               # shapefile (.shp, .shx, .dbf, .prj, .cpg)
└── processed/                                     # your cleaned / derived datasets
```

---

## Raw data

**`twitter_tract_tract_edgelist.csv`**: Twitter social ties between census tracts (separator: `;`)
- Mutual-follower ties between Twitter users, aggregated to pairs of census tracts by the inferred home location of the users
- 152,898 tract pairs in 3 metros: Boston (22,066), Detroit (39,501), New York (91,331)
- `cbsacode`, `cbsaname`: metro area code and name
- `tract_home1`, `tract_home2`: the two connected census tracts (e.g. `14000US34003001000`). Each pair appears once, with `tract_home1 <= tract_home2`
- `nr_ties`: number of mutual-follower ties between the two tracts (median 1, max 162)
- Rows with `tract_home1 == tract_home2` are ties *within* a tract (2,657 rows)

**`acs_data_census_tracts_2012_2020.csv.gz`**: American Community Survey (ACS) 5-year estimates by census tract
- 38,788 census tracts (50 largest metros), wide format: one column per variable and year (2012–2020), e.g. `income2019`
- `tract_id`: census tract ID, matches `tract_home1` / `tract_home2` in the Twitter file
- `population<year>`: total population
- `income<year>`: median household income (USD)
- `education_bachelor<year>`, `education_total<year>`: population with a bachelor's degree, and the total population used to compute the education share
- `race_total<year>`, `white<year>`, `black<year>`, `native<year>`, `asian<year>`: population by race
- **Note:** use **2019**. In the 2020 columns ~15% of tracts are missing because census tract boundaries were redrawn in 2020. Earlier years are complete.

**`places_US_newyork_boston_detroit.parquet`**: Foursquare Open Source Places for the 3 metros
- ~2.2 million POIs; ~13% are closed (`date_closed` not empty), ~8% have no category
- Key columns: `fsq_place_id`, `name`, `latitude`, `longitude`, `date_closed`, `fsq_category_labels`
- `fsq_category_labels` holds the category hierarchy, e.g. `["Dining and Drinking > Restaurant > Italian Restaurant"]`.
- The file is large, so read only the columns you need (e.g. `pl.read_parquet(..., columns=[...])`)

**`censustract_geoms_top50.geojson`**: census tract boundaries of the 50 largest metro areas
- 38,788 tracts, coordinates in WGS84 (EPSG:4326)
- `full_geoid`: census tract ID, matches `tract_id` in the ACS file
- Also: `display_name`, `aland` / `awater` (land / water area in m²), `population`
- **Note:** the file has one GeoJSON feature per line, so `gpd.read_file()` reads only the first tract. Read it line by line instead (see `code/01_data_overview.ipynb`).
- The file has no metro code. Assign tracts to metros with a spatial join to `metro_areas/`.

**`metro_areas/`**: boundaries of the 50 largest metropolitan areas (shapefile)
- `cbsacode`: metro area code (Boston `14460`, Detroit `19820`, New York `35620`)
- Coordinates in NAD83 (EPSG:4269). Convert with `.to_crs("EPSG:4326")` before joining with the tracts or POIs.

**`nyc_puma_all.geojson`**: Public Use Microdata Areas (PUMAs) in the New York metro area
- 336 PUMAs (~100,000 people each), coordinates in NAD83 (EPSG:4269)
- Useful for aggregating tracts into larger neighborhoods

