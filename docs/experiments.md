# Experiments

## Experimental Design

Two complementary experiments were conducted to evaluate unsupervised semantic labeling from Sentinel-2 imagery.

Pipeline I focuses on raster-based labeling within Google Earth Engine, while Pipeline II investigates polygon-level labeling using an ensemble learning strategy.

---

# Experiment I — Raster-Based Semantic Labeling

## Study Area

Approximately **938 km²**

---

## Input Data

- Sentinel-2 Level-2A imagery
- Cloud-filtered multispectral composites

---

## Feature Space

Each image is represented by a 17-dimensional feature vector composed of spectral indices, tasseled-cap transformations, and GLCM texture descriptors.

---

## Clustering Evaluation

Multiple KMeans and Gaussian Mixture configurations were evaluated.

Candidate models were ranked according to internal clustering quality before semantic interpretation.

---

## Semantic Mapping

Clusters were converted into

- Non-Vegetation / Water
- Sparse Vegetation
- Dense Vegetation

using explainable spectral decision rules.

---

## Spatial Refinement

Spatial consistency was improved using

- focal mode filtering
- removal of isolated patches

before exporting the final raster products.

---

# Experiment II — Polygon-Level Ensemble Labeling

## Input Data

Sentinel-2 Cloud Optimized GeoTIFFs

Five predefined AOIs were processed independently.

---

## Ensemble Models

Three lightweight models participated in the final voting scheme.

| Model | Function |
|--------|----------|
| KMeans | Spectral partitioning |
| Gaussian Mixture Model | Probabilistic clustering |
| Random Forest | Ensemble refinement |

---

## Cluster Validation

| Metric | Value |
|---------|------:|
| Silhouette Score | 0.585 |
| Davies–Bouldin Index | 0.542 |

The obtained values indicate satisfactory intra-cluster compactness and inter-cluster separation.

---

## PCA Analysis

Principal Component Analysis (PCA) was employed for visual assessment of feature separability.

The projected feature space demonstrates clear discrimination among

- Non-Vegetation
- Non-Tree Vegetation
- Tree Vegetation

supporting the effectiveness of the engineered feature representation.

---

## Exported Products

Each experiment produces

- GeoTIFF
- Google Earth Engine Assets
- GeoJSON
- GeoPackage (GPKG)
- Interactive HTML maps

ready for GIS visualization and downstream machine learning applications.
