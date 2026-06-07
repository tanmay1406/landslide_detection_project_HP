# Landslide Prediction and Risk Mapping System

## Overview

Landslides are among the most destructive natural hazards in mountainous regions, causing significant loss of life, infrastructure damage, and economic disruption.

This project develops an end-to-end geospatial machine learning pipeline for landslide susceptibility prediction and risk mapping in **Himachal Pradesh, India**. The system integrates terrain, infrastructure, hydrological, vegetation, and climate-related features derived from multiple geospatial datasets and evaluates machine learning models for regional-scale landslide risk assessment.

The project includes:

- DEM-based terrain feature extraction
- OpenStreetMap infrastructure and land-use analysis
- NDVI vegetation analysis
- NASA POWER rainfall integration
- Machine learning model training and evaluation
- Landslide susceptibility classification and risk mapping

---

## Study Area

**Himachal Pradesh, India**

High-resolution geospatial datasets covering the state were processed to construct a large-scale landslide susceptibility dataset containing approximately **56,000 samples**.

---

## Project Structure

```text
landslidehp/
│
├── notebooks/
│   ├── 01_data_extraction.ipynb
│   ├── 02_preprocessing.ipynb
│   ├── 03_mlp_model.ipynb
│   ├── 05_ML_model.ipynb
│   └── 05_3_ML_model.ipynb
│
├── data/
│   ├── raw/
│   └── processed/
│
├── outputs/
│   ├── figures/
│   └── models/
│
└── README.md
```

---

## Notebook Descriptions

### 01_data_extraction.ipynb

Responsible for geospatial feature extraction and dataset construction.

Features extracted:

- DEM processing
- Elevation extraction
- Slope generation
- Aspect generation
- Terrain roughness generation
- OSM feature extraction
- Distance calculations
- NDVI integration
- Rainfall data collection

---

### 02_preprocessing.ipynb

Data preparation and preprocessing.

Tasks performed:

- Data cleaning
- Missing value handling
- Label generation
- Feature engineering
- Dataset balancing analysis
- Train/test split creation

Outputs:

- X_train.csv
- X_test.csv
- y_train.csv
- y_test.csv

---

### 03_mlp_model.ipynb

Deep learning baseline model.

Model:

- Multi-Layer Perceptron (MLP)

Tasks:

- Model training
- Evaluation
- Performance comparison

Results:

| Metric | Value |
|----------|--------:|
| Accuracy | 52.67% |

---

### 05_ML_model.ipynb

Four-class machine learning experiments.

Classes:

- Not Exposed
- At Risk / Concerned
- Vulnerable
- High Vulnerability

Models:

- Random Forest
- XGBoost

Results:

| Model | Accuracy | Macro F1 |
|---------|---------:|---------:|
| Random Forest | **57.32%** | **0.55** |
| XGBoost | 54.12% | 0.49 |

---

### 05_3_ML_model.ipynb

Three-class reformulation experiments.

Classes:

- Low Risk
- Medium Risk
- High Risk

Models:

- Random Forest
- XGBoost

Results:

| Model | Accuracy | Macro F1 |
|---------|---------:|---------:|
| Random Forest | **67.86%** | **0.67** |
| XGBoost | 65.68% | 0.64 |

The three-class formulation significantly improved model performance by reducing overlap between intermediate susceptibility categories.

---

## Dataset Construction Pipeline

### 1. Terrain Feature Engineering

A high-resolution DEM (~13,495 × 12,096 pixels, ~30m spatial resolution) was processed using Rasterio and NumPy to derive:

- Elevation
- Slope
- Aspect
- Terrain Roughness

Generated raster layers:

- `hp_slope.tif`
- `hp_aspect.tif`
- `hp_roughness.tif`

---

### 2. Infrastructure and Land Use Features

OpenStreetMap data was integrated and reprojected to **EPSG:32644**.

Extracted features:

- Distance to nearest road
- Distance to nearest building
- Distance to nearest river
- Building density
- Land-use category

Spatial indexing using **STRtree** was employed to efficiently process tens of thousands of geospatial points.

---

### 3. Vegetation Features

Satellite-derived vegetation information was integrated.

Feature:

- NDVI (Normalized Difference Vegetation Index)

This feature captures vegetation density and land-cover characteristics that influence slope stability.

---

### 4. Climate Features

Daily precipitation data was obtained from NASA POWER datasets.

Extracted features:

- Average 7-day accumulated rainfall
- Average 15-day accumulated rainfall

To improve efficiency, coordinates were grouped into rainfall grid cells before querying the API, reducing thousands of potential requests to fewer than one hundred unique spatial queries.

---

### 5. Final Feature Dataset

The final dataset contains approximately **56,000 geospatial samples**.

#### Terrain Features

- Elevation
- Slope
- Aspect
- Roughness

#### Infrastructure Features

- Distance to Road
- Distance to Building
- Building Density

#### Hydrological Features

- Distance to River

#### Vegetation Features

- NDVI

#### Climate Features

- Rainfall (7-Day)
- Rainfall (15-Day)

#### Metadata

- Latitude
- Longitude
- District
- Tehsil

#### Target Variables

- Landslide Label
- Exposure Category

---

## Model Performance Summary

### Four-Class Classification

| Model | Accuracy | Macro F1 |
|---------|---------:|---------:|
| MLP | 52.67% | 0.35 |
| XGBoost | 54.12% | 0.49 |
| Random Forest | **57.32%** | **0.55** |

---

### Three-Class Classification

| Model | Accuracy | Macro F1 |
|---------|---------:|---------:|
| XGBoost | 65.68% | 0.64 |
| Random Forest | **67.86%** | **0.67** |

The best-performing model was the **3-Class Random Forest**, achieving:

- **67.86% Accuracy**
- **0.67 Macro F1 Score**

---

## Feature Importance Insights

Feature importance analysis identified the following key predictors:

- Terrain Roughness
- Slope
- Elevation
- Distance to Road
- Rainfall (7-Day)
- Rainfall (15-Day)
- Distance to River
- Building Density

These findings align with established geotechnical and environmental factors known to influence landslide occurrence.

---

## Technology Stack

### Geospatial Processing

- Rasterio
- GeoPandas
- Shapely
- STRtree
- NumPy
- SciPy

### Machine Learning

- Scikit-Learn
- XGBoost

### Deep Learning

- TensorFlow / Keras

### Data Sources

- Digital Elevation Models (DEM)
- OpenStreetMap (OSM)
- NASA POWER Climate Data
- Satellite-derived NDVI Data

---

## Future Work

- CatBoost benchmarking
- SHAP explainability
- Susceptibility raster generation
- Interactive risk visualization dashboard
- MERN stack deployment
- Real-time rainfall integration
- State-wide risk map generation

---

## Key Results

Built a complete geospatial feature engineering pipeline

Processed DEM data covering Himachal Pradesh (~163 million pixels)

Generated terrain, rainfall, vegetation, infrastructure, and hydrological features

Constructed a dataset of ~56,000 geospatial samples

Evaluated MLP, Random Forest, and XGBoost models

Improved performance through risk-category reformulation

Achieved **67.86% accuracy and 0.67 Macro F1** using a 3-class Random Forest model
