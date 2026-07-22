# Methodology

## Overview

This repository presents a reproducible framework for **unsupervised semantic land-cover labeling** from Sentinel-2 multispectral imagery. Rather than performing supervised land-cover classification, the proposed workflow automatically generates semantic pseudo-labels using spectral feature engineering, unsupervised clustering, explainable rule-based semantic interpretation, and lightweight ensemble learning.

The methodology consists of two complementary processing pipelines:

1. **Pipeline I — Google Earth Engine Raster Labeling**
2. **Pipeline II — Local Ensemble Polygon Labeling**

Together, these pipelines provide a scalable solution for generating raster and polygon pseudo-labels that can be used for downstream applications such as semantic segmentation, land-cover classification, active learning, and self-supervised representation learning.

---

# Pipeline I — Google Earth Engine Raster Labeling

## 1. Area of Interest (AOI) Definition

The workflow begins by defining an Area of Interest (AOI) within Google Earth Engine.

For each AOI, the following geometric descriptors are computed and exported:

- Geometry
- Bounding Box (BBox)
- Centroid
- Coordinate Reference System (CRS)
- Area (km²)

These metadata ensure complete spatial reproducibility.

---

## 2. Sentinel-2 Composite Generation

Sentinel-2 Level-2A imagery is filtered according to

- acquisition period
- cloud coverage threshold
- spatial extent

Image preprocessing includes

- cloud masking
- shadow masking
- invalid pixel masking
- temporal compositing

to generate a cloud-free multispectral composite suitable for subsequent analysis.

---

## 3. Spectral Feature Engineering

Each Sentinel-2 composite is transformed into a high-dimensional feature space composed of **17 spectral and texture descriptors**.

### Vegetation Indices

- NDVI
- SAVI
- EVI (when available)
- REIP

### Moisture and Water Indices

- NDWI
- NDMI

### Built-up and Burn Indices

- NDBI
- NBR

### Spectral Transformations

- Tasseled Cap Brightness
- Tasseled Cap Greenness
- Tasseled Cap Wetness

### Texture Descriptors

Gray-Level Co-occurrence Matrix (GLCM) features including

- Contrast
- Correlation
- Homogeneity
- Entropy
- Dissimilarity
- Angular Second Moment (ASM)

The resulting feature stack provides complementary spectral and spatial information for unsupervised clustering.

---

## 4. Unsupervised Clustering

Two independent clustering strategies are evaluated.

### KMeans

A centroid-based partitioning algorithm used to identify compact spectral clusters.

Multiple random initializations are evaluated to improve clustering stability.

### Gaussian Mixture Model (GMM)

A probabilistic clustering model that estimates soft class membership and better captures overlapping spectral distributions.

---

## 5. Cluster Selection

Multiple candidate clustering configurations are compared.

Each solution is ranked according to internal quality criteria including

- cluster compactness
- cluster separation
- spectral consistency

The highest-quality configuration is selected for semantic interpretation.

---

## 6. Explainable Semantic Mapping

Since cluster identifiers have no intrinsic semantic meaning, each cluster is translated into an interpretable land-cover category using explainable spectral decision rules based on

- NDVI
- NDWI
- Vegetation Score

Final semantic classes are

- Non-Vegetation / Water
- Sparse Vegetation
- Dense Vegetation

Unlike purely data-driven clustering, this step introduces physical interpretability while preserving reproducibility.

---

## 7. Spatial Post-processing

To improve cartographic quality, the classified raster undergoes

- 3×3 focal mode filtering
- isolated patch removal
- spatial smoothing

These operations reduce salt-and-pepper noise while preserving dominant spatial structures.

---

## 8. Output Generation

Final raster products are exported as

- Google Earth Engine Assets
- GeoTIFF

Metadata accompanying each product include

- acquisition period
- feature list
- percentile thresholds
- semantic class definitions

ensuring full experiment reproducibility.

---

# Pipeline II — Local Ensemble Polygon Labeling

The second workflow performs polygon-level pseudo-label generation from Sentinel-2 Cloud Optimized GeoTIFFs (COGs).

---

## AOI Registry

Five predefined Areas of Interest are maintained through an AOI registry containing

- coordinates
- bounding boxes
- centroids
- area
- descriptive metadata

---

## Feature Extraction

For every AOI, spectral descriptors are computed including

- NDVI
- NDWI
- NDMI
- NBR
- NDBI
- NDRE (when available)

Median and interquartile statistics are also computed for quality assessment.

---

## Ensemble Label Generation

Three lightweight machine learning models contribute to the final pseudo-label

- KMeans
- Gaussian Mixture Model
- Random Forest

Majority voting is used to obtain robust semantic labels while reducing model-specific biases.

---

## Polygon Export

Raster labels are vectorized into polygons and exported as

- GeoJSON
- GeoPackage (GPKG)
- Interactive HTML maps

compatible with QGIS, ArcGIS, and downstream geospatial workflows.

---

# Reproducibility

The entire workflow is deterministic given

- identical AOI definitions
- identical acquisition periods
- identical preprocessing parameters
- identical random seeds

All processing stages are documented within the accompanying notebooks and experiment logs.
