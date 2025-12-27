GÜNCELLENECEK

Project Summary (Clustering Pipeline)

Data Preprocessing: Temiz veri seti oluşturuldu, düşük kapsamalı feature’lar elendi

Feature Selection: 178 → ~140+ feature, gürültü azaltıldı

Scaling Comparison: No / Robust / Standard / MinMax / Log+Std denendi

Best Scaling: RobustScaler (en yüksek silhouette)

Dimensionality Reduction: UMAP (40D) + PCA (%90 varyans)

Algorithms Compared: K-Means, DBSCAN, HDBSCAN, Hierarchical, Spectral, GMM

Main Algorithm: K-Means (grid search ile optimize edildi)

Density-Based Methods: DBSCAN (grid search), HDBSCAN (auto cluster + outlier)

Hyperparameter Tuning: Silhouette score ile manuel grid search

Evaluation Metrics: Silhouette Score, Davies–Bouldin Index

Stability Testing: Random subsample (10 run) + K-Fold style (Mean ± Std)

Pipeline: Scaling → Reduction → Clustering (reproducible)

Visualization: t-SNE ile 2D cluster haritaları

Timeframe Analysis: 4 yıl / 2 yıl / 6 ay ayrı clustering

Final Outcome: HDBSCAN + UMAP en tutarlı ve anlamlı cluster yapısını verdi
