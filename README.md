# Cooling Effects of Large Urban Mountains: A Case Study of Chengdu Longquan Mountains

This repository contains all scripts, data structure descriptions, and reproducible workflows for the study **"Cooling Effects in Large Urban Mountains: A Case Study of Chengdu Longquan Mountains Urban Forest Park"**.  
The project evaluates the spatiotemporal cooling effects of Longquan Mountain Urban Forest Park (LMFP) using **Landsat-based LST**, **buffer-based cooling metrics**, and **XGBoost–SHAP modeling**.

<img width="1119" height="1474" alt="graphical abstract forests-3996816" src="https://github.com/user-attachments/assets/342d6afa-f33d-41a9-aa97-d9bbeb616986" />

---

##  Key Objectives

1. **Retrieve and analyze Land Surface Temperature (LST)** for 2001, 2011, and 2023.  
2. **Quantify cooling effects** using Park Cooling Distance (PCD), Intensity (PCI), Area (PCA), and Efficiency (PCE).  
3. **Identify drivers of cooling intensity (MCI)** using XGBoost and SHAP.  
4. **Analyze long-term thermal evolution** of the LMFP and surrounding urban areas.

---

## Repository Structure

---

## 🛰 Data Description

### **1. LST Data**
Derived from Landsat 5 TM & Landsat 8 OLI/TIRS using the **Radiative Transfer Equation (RTE)**:

- Atmospheric correction  
- Emissivity correction using NDVI  
- Brightness temperature conversion  
- Single-date images selected (cloud <10%)

### **2. Cooling Metrics**

| Metric | Description |
|--------|-------------|
| **PCD** | Park Cooling Distance (m) |
| **PCI** | Park Cooling Intensity (°C) |
| **PCA** | Park Cooling Area (km²) |
| **PCE** | Park Cooling Efficiency (%) |

Computed using **concentric buffer rings** at 30 m intervals and **inflection-point method**.

### **3. Driving Factors**
Categories include:

- Vegetation structure (LAI, FVC, CH)  
- Evapotranspiration (Es, Ec)  
- Topography (slope, aspect, elevation)  
- Human activity (population, road density)  
- Landscape pattern (LPI, SHDI, PD, CA, CONTAG, DIVISION)

---

## 🔧 Software Requirements

### **R environment**
Required packages:

install.packages(c("raster", "rgdal", "sp", "sf", "ggplot2", "dplyr"))
install.packages(c("randomForest", "xgboost", "iml", "pdp"))


### **Python Requirements**
pip install numpy pandas shap xgboost matplotlib scikit-learn geopandas rasterio

## **How to Reproduce the Results**
### Step 1 — Preprocess Data

Run:Rscript driverFactor/data_preprocessing.R

This generates:

Standardized variables

Landscape metrics

Overlay of all factors with LST grids

### Step 2 — Run XGBoost + SHAP

Execute the Python script:

python driverFactor/shap-XGboost.py

This produces:

Trained XGBoost models

SHAP value plots

Feature importance

Interaction effects

Outputs are stored in:

driverFactor/XGB2001/
driverFactor/XGB2011/
driverFactor/XGB2023/

### Step 3 — Compute Cooling Metrics

(MCI, PCD, PCI, PCA, PCE)

Rscript driverFactor/PDP.R

### Step 4 — Generate Visualizations

Spatial maps, PDP curves, and cooling-distance figures.

SHAP shows clear nonlinear patterns and strong vegetation–terrain interactions.

## **Contents**

Cooling_Effects_Large_Urban_Mountains/
│
├── driverFactor/ # Driving factors & model results
│ ├── 2001/ # Processed data (standardized CSV, SHAP results)
│ ├── 2011/
│ ├── 2023/
│ ├── XGB2001/ # XGBoost results (models, plots, importance)
│ ├── XGB2011/
│ ├── XGB2023/
│ ├── venv/ # Python virtual environment (optional)
│ ├── data_preprocessing.R # R script for data cleaning and variable generation
│ ├── PDP.R # Partial Dependence Plot (R)
│ ├── biased_ranking.R # Feature ranking validation
│ └── shap-XGboost.py # Python script for XGBoost + SHAP
│
├── LST/ # Raw Landsat LST files (input)
│ ├── 20010614_lst.tif
│ ├── 20110630_lst.tif
│ └── 20230705_lst.tif
│
├── MCI/ # Mountain Cooling Intensity (output)
│
├── scope/
│ └── lqs.shp # Study area boundary (Longquan Mountain)
│
└── README.md

## **Contact**

For data requests or collaboration:

Email (Corresponding Author):
zhanglin@cib.ac.cn

This repository is released under the MIT License.
You may freely use, modify, and distribute the code with attribution.

## **Acknowledgments**

This project was supported by:

-National Natural Science Foundation of China (32360298)

-West Light Foundation of CAS

-Longquan Mountain Native Flora and Fauna Conservation Project


