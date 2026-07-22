# Results

This document summarizes the experimental outcomes of the two complementary unsupervised land-cover labeling pipelines developed in this repository.

The reported results demonstrate that meaningful semantic land-cover labels can be generated from Sentinel-2 imagery without manually annotated training data by combining spectral feature engineering, explainable semantic mapping, and lightweight ensemble learning.

---

# Pipeline I — Google Earth Engine Raster Labeling

## Study Area

The Google Earth Engine (GEE) workflow was evaluated over an Area of Interest (AOI) covering approximately **938.30 km²**.

The preprocessing stage automatically generated:

- AOI geometry
- Bounding box (BBox)
- Centroid coordinates
- Area statistics

before constructing a cloud-free Sentinel-2 composite for feature extraction.

---

## Feature Engineering

A multi-dimensional feature stack composed of **17 spectral and texture descriptors** was generated from Sentinel-2 imagery.

The extracted features include vegetation, moisture, soil, brightness, and texture information such as:

- NDVI
- NDWI
- NDMI
- NDBI
- REIP
- Tasseled Cap proxies
- GLCM texture measures

This feature representation served as the input space for unsupervised clustering.

---

## Clustering Performance

Multiple executions of KMeans and Gaussian Mixture Models (GMMs) were performed using different initialization strategies.

Candidate models were ranked according to internal clustering quality metrics before semantic interpretation.

The resulting clusters were converted into meaningful land-cover categories using explainable decision rules derived from NDVI, NDWI, and vegetation-score statistics.

The final semantic classes were

- Non-Vegetation / Water
- Sparse Vegetation
- Dense Vegetation

---

# KMeans Results

| Class | Area (ha) | Percentage |
|------------------------|-----------:|-----------:|
| Non-Vegetation / Water | 59,661.65 | 67.2% |
| Sparse Vegetation | 23,451.85 | 26.4% |
| Dense Vegetation | 5,703.01 | 6.4% |

The KMeans solution indicates that approximately two-thirds of the study area corresponds to non-vegetated surfaces, while dense vegetation occupies roughly six percent of the mapped region.

Spatial post-processing using a **3×3 focal mode filter** successfully removed isolated noisy regions and improved boundary continuity without altering the global land-cover distribution.

---

# Gaussian Mixture Model Results

| Class | Area (ha) | Percentage |
|------------------------|-----------:|-----------:|
| Non-Vegetation / Water | 53,264.46 | 59.8% |
| Sparse Vegetation | 30,231.68 | 33.9% |
| Dense Vegetation | 5,558.65 | 6.2% |

Compared with KMeans, the Gaussian Mixture Model assigned a larger proportion of pixels to sparse vegetation while maintaining nearly identical estimates for dense vegetation.

The consistency between the two clustering strategies indicates that the generated semantic classes are robust with respect to the underlying clustering model.

Minor discrepancies between the total mapped area and the AOI originate primarily from cloud masks, shadow masks, and NoData regions.

---

# Raster Products

The raster workflow generated:

- Semantic land-cover maps
- Smoothed raster maps
- Google Earth Engine Assets
- GeoTIFF files

Each exported product contains metadata describing

- acquisition period
- feature configuration
- percentile thresholds
- semantic class definitions

to facilitate reproducibility.

---

# Pipeline II — Ensemble Polygon Labeling

The second workflow performs polygon-level pseudo-label generation from Sentinel-2 Cloud Optimized GeoTIFFs (COGs).

Five predefined Areas of Interest (AOIs) were processed using identical preprocessing parameters.

Following feature extraction, polygon-level semantic labels were generated through an ensemble composed of

- KMeans
- Gaussian Mixture Model
- Lightweight Random Forest

The final semantic label for each polygon was determined using majority voting.

---

## Internal Cluster Validation

The quality of the generated clusters was evaluated using standard internal validation metrics.

| Metric | Value |
|-----------------------|------:|
| Silhouette Score | **0.585** |
| Davies–Bouldin Index | **0.542** |

The obtained values indicate satisfactory intra-cluster compactness and inter-cluster separation for unsupervised land-cover labeling.

Principal Component Analysis (PCA) further confirmed clear separation among the generated semantic vegetation classes in the embedded feature space.

---

# Polygon Statistics

| Class | Polygon Count | Area (ha) | Percentage |
|-------------------------|---------:|----------:|-----------:|
| Non-tree Vegetation | 412 | 2,097.80 | 83.1% |
| Non-Vegetation | 84 | 232.07 | 9.2% |
| Tree Vegetation | 68 | 167.46 | 6.6% |
| Unknown | 12 | 26.31 | 1.0% |

Most generated polygons correspond to non-tree vegetation.

Unknown regions are mainly associated with image boundaries, cloud-contaminated pixels, and NoData areas.

The fixed semantic class identifiers

- 0 → Unknown
- 1 → Non-tree Vegetation
- 2 → Non-Vegetation
- 3 → Tree Vegetation

remain consistent throughout the entire processing pipeline, enabling reproducible comparison across AOIs and future acquisitions.

---

# Visual Assessment

Visual inspection of the generated products indicates that:

- semantic labels follow expected spectral characteristics;
- spatial smoothing substantially reduces isolated classification noise;
- polygon boundaries remain geographically coherent;
- PCA visualization confirms meaningful separation among semantic classes;
- ensemble voting improves label stability compared with individual clustering methods.

---

# Output Products

The complete workflow exports multiple geospatial products suitable for GIS software and downstream machine learning applications.

Generated outputs include:

- Google Earth Engine Assets
- GeoTIFF raster labels
- GeoJSON polygons
- GeoPackage (GPKG)
- Interactive HTML visualization maps

These products can be directly integrated into:

- Semantic Segmentation
- Land-Cover Classification
- Dataset Construction
- Active Learning
- Self-Supervised Learning
- Earth Observation Foundation Models

---

# Key Findings

The experiments demonstrate several important observations:

- A fully unsupervised workflow can generate geographically meaningful land-cover labels from Sentinel-2 imagery without manually annotated training data.
- Explainable semantic mapping provides an interpretable transformation from anonymous cluster IDs to physically meaningful land-cover categories.
- Both KMeans and Gaussian Mixture Models produce highly consistent land-cover distributions despite relying on different clustering assumptions.
- Spatial post-processing significantly improves cartographic quality while preserving global class statistics.
- Lightweight ensemble voting increases polygon-level label stability and produces GIS-ready semantic products suitable for downstream machine learning pipelines.

---

# Conclusion

The proposed framework establishes a reproducible pipeline for large-scale pseudo-label generation in Earth Observation.

By integrating spectral feature engineering, unsupervised clustering, explainable semantic mapping, and ensemble learning, the workflow produces high-quality raster and polygon labels that can support future research in semantic segmentation, land-cover mapping, self-supervised learning, and foundation models for remote sensing.
