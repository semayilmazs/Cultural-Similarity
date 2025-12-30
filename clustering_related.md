# Clustering-Related Methodology

This document describes the clustering approaches implemented in the project, focusing on the logic and execution of `clustering.ipynb` and `cluster_umap_hdbscan.ipynb`.

---

## 1. Index-Based Clustering (`clustering.ipynb`)

This approach clusters countries using **pre-aggregated thematic indices** derived from Google Trends features.

### Input
- Country × index matrix  
  (high-variance indices are preferred when available)

### Procedure
1. **Preprocessing**
   - Missing values are imputed using median values.
   - Feature scaling is applied (StandardScaler by default; RobustScaler optional).
   - Optional PCA can be used to stabilize variance and reduce noise.

2. **Cluster Number Selection**
   - KMeans is evaluated over a range of cluster counts.
   - Silhouette score is used as the primary criterion for selecting the optimal number of clusters.

3. **Clustering**
   - Final KMeans clustering is performed using the selected number of clusters.
   - Agglomerative clustering may be applied as a structural comparison.

4. **Interpretation**
   - Cluster-wise z-scores are computed for each index.
   - Dominant positive and negative indices are extracted to characterize each cluster.

### Output
- Country-level cluster assignments
- Cluster z-score profiles
- Summary of defining indices per cluster

---

## 2. UMAP + Density-Based Clustering (`cluster_umap_hdbscan.ipynb`)

This approach identifies natural country groupings using **non-linear embedding and density-based clustering**, without assuming a fixed number of clusters.

### Input
- Labeled country-level time-series features

### Procedure
1. **Temporal Segmentation**
   - Clustering is performed over multiple time windows (3 years, 2 years, 6 months).
   - Countries with insufficient temporal coverage are excluded.

2. **Aggregation & Scaling**
   - Time-series values are aggregated per country using the median.
   - Robust scaling is applied to reduce the influence of outliers.

3. **Dimensionality Reduction**
   - UMAP projects high-dimensional features into a low-dimensional space while preserving local structure.

4. **Clustering**
   - HDBSCAN detects clusters based on data density and labels low-density observations as noise.
   - For comparison, KMeans is also applied on the UMAP embedding.

5. **Evaluation**
   - Cluster counts, noise ratios, and model parameters are recorded for each time window.

### Output
- Country-level cluster labels (HDBSCAN and KMeans-on-UMAP)
- Noise and outlier identification
- Run-level clustering summary across time windows

---

## Conceptual Distinction

- **Index-Based Clustering**  
  Emphasizes interpretability and controlled feature abstraction with a predefined number of clusters.

- **UMAP + HDBSCAN Clustering**  
  Emphasizes data-driven structure discovery, flexible cluster shapes, and explicit outlier detection.

Together, these methods provide complementary perspectives on cross-country behavioral similarity.
