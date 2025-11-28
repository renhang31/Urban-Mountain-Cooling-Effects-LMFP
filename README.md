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
  Emissivity correction using NDVI  
  Brightness temperature conversion  
  Single-date images selected (cloud <10%)

- Data Download
  The data is from the United States Geological Survey (USGS), specifically Landsat 5 and Landsat 8 satellite imagery.
  - Landsat 5 and Landsat 8 Download Link:https://earthexplorer.usgs.gov/
  - United States Geological Survey (USGS) - Earth Explorer
  Steps:
  - Visit the above link, log in or register your USGS account.
  - Select Landsat in the Data Sets section, then choose Landsat 5 or Landsat 8.
  - Set the region and time range in the search box.
  - Submit the search, filter the relevant data, and select the imagery files for download.
- Data Processing:
  Software Requirement: ENVI software for radiative transfer processing and surface temperature calculation.
  Steps:
  - After downloading, import the data into ENVI software.
  - Apply the radiative transfer method for data processing and surface temperature calculation.
  - Follow the steps for radiometric calibration and temperature calculation as outlined in the ENVI manual.

The final result will be the surface temperature image.
- 

### **2. Cooling Metrics**

| Metric | Description |
|--------|-------------|
| **PCD** | Park Cooling Distance (m) |
| **PCI** | Park Cooling Intensity (°C) |
| **PCA** | Park Cooling Area (km²) |
| **PCE** | Park Cooling Efficiency (%) |

Computed using **concentric buffer rings** at 30 m intervals and **inflection-point method**.

### **3. Driving Factors**
**Categories include**

- Vegetation structure (LAI, FVC, CH)  
- Evapotranspiration (Es, Ec)  
- Topography (slope, aspect, elevation)  
- Human activity (population, road density)
- dscape pattern (LPI, SHDI, PD, CA, CONTAG, DIVISION)

**Data Download**   
- FVC: https://code.earthengine.google.com/6ecd51a29d50964c40bb76eee897ff2e

- CLCD: https://code.earthengine.google.com/8d9df37585e1d2485aaffbe2ab8410d5

- LAI：https://code.earthengine.google.com/db7adb98b85919c3ae4760e1ce793c78

- Ec and Es: https://code.earthengine.google.com/bbd0e2a417522b284c8e02e8621114ca

- CH：https://code.earthengine.google.com/9582e1161b1b2e2a97d68a8a11d0ed7b

- Slope、Elevation、Aspect:
  https://code.earthengine.google.com/6a4ed947402e7f8dec5197bd42877301

- Population: https://code.earthengine.google.com/89bc4512e53c030e8a51b9315f6e4ba0

- Road：https://www.openstreetmap.org/

- Landscape Metrics: CA, PD, LPI, SHDI, DIVISION, AI, CONTAG

  Input: green patches extracted from CLCD within the study area.

  Tool: Fragstats 4.2 (or equivalent).
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


