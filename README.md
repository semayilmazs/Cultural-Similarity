# Cultural Similarity

data_collection.ipynb: Handles the initial data acquisition (e.g., via pytrends) to gather global interest data across multiple categories.

index_generation.ipynb: Processes raw search data into normalized indices (e.g., Luxury, Crime, Financial Speculation).

feature_labeling.ipynb: Cleans and prepares final feature sets for modeling, ensuring consistent country-code mapping.

EDA.ipynb: Exploratory Data Analysis including correlation heatmaps, distribution plots, and outlier detection.

clustering.ipynb: Implementation of K-Means clustering and Z-score profiling to define country segments.

cluster_umap_hdbscan.ipynb: Advanced experimentation using UMAP for dimensionality reduction and HDBSCAN for density-based clustering.


## Notebooks (Execution Order)

### 1) `data_collection.ipynb`
**Goal:** Collect Google Trends signals at scale (weekly time series) for many countries & topics.

**Key output:**
- `data/raw/final_proje_dataset_CLEAN.csv`

> Notes:
> - Collection is rate-limit sensitive. The notebook uses `pytrends`, retry/wait logic, and checkpointing patterns.
> - This stage may take a long time depending on request limits.

---

### 2) `EDA.ipynb`
**Goal:** Basic exploration and sanity checks, plus producing a “cleaned features” CSV used downstream.

**Key input:**
- `data/raw/final_proje_dataset_CLEAN.csv`

**Key output:**
- `data/raw/final_proje_dataset_CLEANED_features.csv`

---

### 3) `feature_labeling.ipynb`
**Goal:** Replace raw topic IDs with normalized, readable feature names and produce a clean time-series table.

**Key inputs:**
- `data/raw/final_proje_dataset_CLEANED.csv`
- `data/raw/keywords_FINAL_2025.json`

**Key output:**
- `data/processed/labeled_time_series.csv`

### 4) `index_generation.ipynb`
**Goal:** Reduce feature dimensionality by aggregating labeled features into theme-based indices (e.g., “crime”, “relationships”, “consumption”…).

**Key input:**
- `data/processed/labeled_time_series.csv`

**Key outputs:**
- `data/processed/country_index_matrix_mean.csv`
- `data/processed/country_index_matrix_median.csv`
- `data/processed/country_index_matrix_index_report.csv` (coverage/missingness report)
- `data/processed/country_index_matrix_HIGH_VARIANCE.csv` (subset used for clustering)

---

### 5) `clustering.ipynb`
**Goal:** Cluster countries using index matrices (optionally filtered to high-variance indices), evaluate `k` via silhouette and other metrics, then interpret clusters via z-score profiles.

**Key inputs:**
- `data/processed/country_index_matrix_HIGH_VARIANCE.csv` *(preferred if exists)*
- `data/processed/country_index_matrix_median.csv` *(fallback)*

**Key outputs:**
- `data/processed/clustering/k_report.csv`
- `data/processed/clustering/country_clusters_kmeans.csv`
- `data/processed/clustering/country_clusters_kmeans_plus_agglo.csv`
- `data/processed/clustering/cluster_profiles_zscores.csv`
- `data/processed/clustering/cluster_summary_top_features.csv`

---

### 6) `cluster_umap_hdbscan.ipynb` & `visualize.ipynb` (Alternative)
**Goal:** Cluster countries after non-linear dimensionality reduction (UMAP) and density clustering (HDBSCAN). Runs multiple time windows:
- 3 years (3y)
- 2 years (2y)
- 6 months (6m)

**Key input:**
- `data/processed/labeled_time_series.csv`

**Key output directory:**
- `data/processed/clustering_umap_hdbscan/`
  - Per-window labels + `run_report.csv` summarizing parameters and results


---

## Installation

### Option A — pip (recommended)
pip install -U pip
pip install pandas numpy scikit-learn matplotlib seaborn tqdm pytrends umap-learn hdbscan
