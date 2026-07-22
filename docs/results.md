# Results

## Pipeline I — Google Earth Engine Raster Labeling

The proposed raster labeling workflow successfully generated semantic land-cover maps over an Area of Interest covering approximately **938 km²**.

Three semantic land-cover categories were identified:

- Non-Vegetation / Water
- Sparse Vegetation
- Dense Vegetation

---

# KMeans Results

| Class | Area (ha) | Percentage |
|---------|----------:|-----------:|
| Non-Vegetation / Water | 59,661.65 | 67.2% |
| Sparse Vegetation | 23,451.85 | 26.4% |
| Dense Vegetation | 5,703.01 | 6.4% |

The classified raster indicates that approximately two-thirds of the study area correspond to non-vegetated surfaces, whereas dense vegetation occupies nearly six percent of the mapped region.

---

# Gaussian Mixture Model Results

| Class | Area (ha) | Percentage |
|---------|----------:|-----------:|
| Non-Vegetation / Water | 53,264.46 | 59.8% |
| Sparse Vegetation | 30,231.68 | 33.9% |
| Dense Vegetation | 5,558.65 | 6.2% |

The probabilistic GMM produced spatial distributions consistent with KMeans while allocating a slightly larger proportion of pixels to sparse vegetation.

---

# Comparative Analysis

Both clustering algorithms generated highly consistent semantic land-cover maps.

Dense vegetation remained remarkably stable across both methods, while observed differences primarily originated from probabilistic boundary assignments and cloud/NoData masking.

---

# Pipeline II — Ensemble Polygon Labeling

The polygon-based workflow generated four semantic categories through majority voting.

| Class | Polygons | Area (ha) | Percentage |
|---------|---------:|----------:|-----------:|
| Non-Tree Vegetation | 412 | 2,097.80 | 83.1% |
| Non-Vegetation | 84 | 232.07 | 9.2% |
| Tree Vegetation | 68 | 167.46 | 6.6% |
| Unknown | 12 | 26.31 | 1.0% |

Unknown polygons are primarily associated with image boundaries and NoData regions.

---

# Internal Validation

| Metric | Value |
|---------|------:|
| Silhouette Score | 0.585 |
| Davies–Bouldin Index | 0.542 |

These metrics indicate satisfactory cluster compactness and separation, supporting the stability of the generated pseudo-labels.

---

# Visual Assessment

Qualitative inspection of raster maps, polygon overlays, and PCA projections demonstrates strong agreement between spectral characteristics and semantic classes.

The spatial smoothing stage effectively removed isolated artifacts while preserving meaningful landscape structures.

---

# Deliverables

The repository produces

- Google Earth Engine Assets
- GeoTIFF raster labels
- GeoJSON polygons
- GeoPackage (GPKG)
- Interactive HTML visualization

These outputs can be directly integrated into

- semantic segmentation datasets,
- supervised land-cover classification,
- pseudo-label generation,
- active learning,
- self-supervised learning,
- Earth observation foundation models.

---

# Key Findings

The proposed framework demonstrates that high-quality semantic land-cover pseudo-labels can be generated from Sentinel-2 imagery without manual annotation by combining spectral feature engineering, unsupervised clustering, explainable semantic mapping, and ensemble learning. The modular design, reproducible processing pipeline, and GIS-compatible outputs make the framework suitable for scalable Earth observation research and downstream machine learning applications.
