# CABA Traffic Safety & Emergency Response Geospatial Analysis

This repository contains a data science project dedicated to the **geospatial analysis of traffic accidents and emergency healthcare response** in the Autonomous City of Buenos Aires (CABA), Argentina.

Historically, after mastering Exploratory Data Analysis (EDA) and Feature Engineering, this capstone project consolidates multi-source urban data to evaluate the relationship between traffic fatalities and emergency medical infrastructure coverage, accounting for potential statistical confounding factors like the **ecological fallacy**.

---

## 📌 Objectives & Research Questions
1. **Spatial Coverage:** Do severe and fatal traffic accidents occur disproportionately inside urban areas lacking emergency medical coverage?
2. **Statistical Correlation:** Do communes with lower healthcare infrastructure density experience higher traffic mortality rates when adjusting for estimated traffic flow?

---

## 🛠️ Data Sources & Integration
- **Communes (Polygons):** Official GeoJSON boundary map of CABA's 15 communes.
- **Hospitals & CeSACs (Points):** Georeferenced public healthcare system locations. Specialized centers were filtered to isolate only **Trauma-ready Emergency Facilities** (General Acute Care & Pediatric Hospitals).
- **Bases del SAME (Points):** Manual coordinates of out-of-hospital ambulance dispatch units representing the emergency response system.
- **Traffic Accidents (CSV/XLSX):** Comprehensive historical logs of traffic incidents (Victims and Events) cataloged by severity (*Light, Severe, Fatal*).
- **Traffic Flow (Proxy):** Comunal-level traffic volume index utilized to normalize accident rates.

---

## 📐 Technical Pipeline

### 1. Advanced Feature Engineering & GIS Operations
* **Trauma Care Filtering:** Filtered out 19 specialized clinics (odontology, mental health, etc.) from the 36 total hospitals to prevent "false-positive" coverage zones. Only 17 general trauma hospitals were analyzed.
* **Metric Projection (CRS):** Reprojected coordinates from WGS84 (EPSG:4326) to **POSGAR 2007 Argentina Zone 5 (EPSG:5347)** to accurately compute metric distance buffers.
* **Service Area Buffers:** Generated **1.5 km** service-area rings for trauma hospitals and **1.0 km** coverage ranges for SAME ambulance bases.
* **Rate Standardization:** Constructed standardized metrics per commune including `infra_100k` (clinics per 100,000 residents) and `siniestros_por_100k_veh` (accidents adjusted for traffic volume).

### 2. Analytical & Statistical Modeling
* **Spatial Containment Test:** Used GIS intersection methods (`shapely.geometry.within`) to verify whether fatal incidents cluster within medical coverage "gaps."
* **Correlation Heatmaps:** Computed both **Pearson (linear)** and **Spearman (rank-order)** correlation matrices to check relationships between healthcare availability, traffic density, and lethality rates.
* **The Ecological Fallacy:** Identified a counterintuitive positive correlation ($r = +0.51$) between healthcare density and traffic lethality, explaining it mathematically as a spatial confounding issue (central communes attract massive commuter traffic and house more hospitals, inflating resident-normalized rates).

### 3. Advanced Visualizations
* **Static Panels:** Double-panel maps contrasting hospital-only coverage gaps vs. integrated hospital + SAME dispatch coverage.
* **Bivariate Choropleths:** Side-by-side comparative choropleth maps aligning medical capacity (greens) with traffic lethality % (reds).
* **Interactive GIS Dashboard (Folium):** 
  * Dynamic heatmap layer of accident density.
  * Interactive markers for fatal victims containing metadata tooltips (date, vehicle types).
  * Transparent red overlays representing identified spatial coverage gaps.

---

## 🔑 Key Findings
* **No Disproportionate Risk:** The percentage of fatal victims falling inside coverage gaps (~38.6%) matches the physical area of the gap (~41.1%). This indicates that **severity is not caused by distance to treatment**.
* **Prevention is Key:** Once a robust emergency coverage base is established (which CABA currently possesses), mortality is determined by the speed, impact energy, and vulnerability of the commuter (pedestrians & motorcyclists make up the vast majority of fatalities).

---

## 🚀 Technologies Used
* **Language:** Python
* **Spatial Stack:** `geopandas`, `shapely`, `folium` (HeatMap, MarkerCluster)
* **Data Science:** `pandas`, `numpy`, `scipy` (Spearman rank test), `scikit-learn`
* **Visualization:** `matplotlib`, `seaborn`
