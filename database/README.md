# Database Description

This directory documents the remote sensing datasets and data preparation workflow used in the **Sentinel-2 Unsupervised Land Cover Labeling Framework**.

Due to the large size of satellite imagery products, raw Sentinel-2 data files are not included in this repository. Instead, this folder provides information required to reproduce the experiments using publicly available Earth Observation data sources.

---

# Data Source

The framework is developed for:

**Sentinel-2 Multispectral Instrument (MSI) Level-2A Surface Reflectance Data**

Main characteristics:

* Satellite platform: Sentinel-2
* Product level: Level-2A (Surface Reflectance)
* Spatial resolution: 10 m / 20 m
* Data type: Multispectral optical imagery
* Processing environment:

  * Google Earth Engine
  * STAC-based Sentinel-2 Cloud Optimized GeoTIFF (COG)

---

# Required Spectral Bands

The proposed pipeline uses Sentinel-2 spectral bands compatible with the following configuration:

| Band | Description           | Resolution |
| ---- | --------------------- | ---------- |
| B2   | Blue                  | 10 m       |
| B3   | Green                 | 10 m       |
| B4   | Red                   | 10 m       |
| B5   | Red Edge 1            | 20 m       |
| B6   | Red Edge 2            | 20 m       |
| B7   | Red Edge 3            | 20 m       |
| B8   | Near Infrared         | 10 m       |
| B8A  | Narrow Near Infrared  | 20 m       |
| B11  | Short Wave Infrared 1 | 20 m       |
| B12  | Short Wave Infrared 2 | 20 m       |

The 20 m bands are resampled when required to create a consistent feature space.

---

# Data Processing Workflow

The raw Sentinel-2 imagery is processed using the following pipeline:

```text
Sentinel-2 Level-2A Imagery

          ↓

AOI Selection

          ↓

Cloud and Shadow Masking

          ↓

Reflectance Normalization

          ↓

Temporal Composite Generation

          ↓

Spectral Feature Extraction

          ↓

Unsupervised Clustering

          ↓

Semantic Pseudo-label Generation
```

---

# Feature Generation

The input imagery is transformed into a multi-dimensional feature space using spectral indices and texture descriptors.

Generated features include:

## Vegetation Features

* NDVI
* GNDVI
* EVI
* SAVI
* MSAVI2
* NDRE
* NDRE2

## Moisture and Water Features

* NDWI
* MNDWI
* NDMI

## Soil and Built-up Features

* NDBI
* BSI
* NBR

## Additional Features

* Tasseled Cap transformations
* GLCM texture descriptors

These features are used for clustering, semantic interpretation, and pseudo-label generation.

---

# Dataset Usage in This Repository

Two complementary data pipelines are implemented:

## Pipeline I — Google Earth Engine Raster Labeling

Input:

* Sentinel-2 Level-2A image collections
* User-defined Area of Interest (AOI)

Processing:

```text
Sentinel-2 Collection

        ↓

Feature Stack Generation

        ↓

KMeans / Gaussian Mixture Model

        ↓

Explainable Semantic Mapping

        ↓

Raster Land-cover Labels
```

Outputs:

* GeoTIFF land-cover maps
* Earth Engine Assets
* Class area statistics

---

## Pipeline II — Local Ensemble Polygon Labeling

Input:

* Sentinel-2 Cloud Optimized GeoTIFF (COG)

Processing:

```text
Sentinel-2 COG

        ↓

Spectral Index Extraction

        ↓

KMeans + GMM + Random Forest

        ↓

Ensemble Voting

        ↓

Vector Pseudo-label Generation
```

Outputs:

* GeoJSON
* GeoPackage (GPKG)
* CSV statistics
* Interactive visualization maps

---

# Reproducibility

To reproduce the experiments:

1. Obtain Sentinel-2 Level-2A imagery from a compatible public data source.
2. Provide the required spectral bands.
3. Configure the Area of Interest (AOI) and acquisition period.
4. Run the notebooks sequentially:

```text
01_sentinel2_unsupervised_raster_labeling.ipynb

                ↓

02_sentinel2_ensemble_polygon_labeling.ipynb
```

---

# Data Availability Note

The original satellite imagery is intentionally excluded because:

* Sentinel-2 scenes are large-scale remote sensing products.
* Public repositories such as GitHub are not designed for storing raw Earth Observation archives.
* The experiments can be reproduced using freely available Sentinel-2 data sources.

This repository contains the complete processing methodology and implementation required to regenerate the derived pseudo-label datasets.
