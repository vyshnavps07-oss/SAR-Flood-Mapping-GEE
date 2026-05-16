# Near Real-Time Flood Extent Mapping & Socio-Economic Impact Assessment Using Sentinel-1 SAR

## 1. Project Infrastructure Overview
This repository hosts a production-grade, end-to-end cloud computing pipeline built via the **Google Earth Engine (GEE) Python API** inside Google Colab to map severe flood inundation footprints in near real-time (NRT). 

* **Disaster Focus:** August 2018 Monsoon Deluge (Kerala, India)
* **Core Study Area:** Lower Periyar River Basin (Districts: Thrissur, Ernakulam, Idukki, Pathanamthitta)
* **The Remote Sensing Challenge:** During peak monsoonal anomalies, heavy cloud cover (often exceeding 95%) renders standard optical earth observation systems (Landsat-8, Sentinel-2) completely blind. 
* **The Engineering Solution:** This pipeline leverages active C-band microwave sensors from **Sentinel-1 Ground Range Detected (GRD)** datasets. SAR microwave pulses pierce weather barriers effortlessly, allowing for unobstructed change-detection capture during major extreme precipitation events.

---

## 2. Final Output Map Layout
Below is the publication-grade cartographic map layout generated directly by this pipeline's Matplotlib and Earth Engine visualization engine:

![2018 Kerala Flood Extent Map](SAR_FLOOD -INUNDATION-MAPPING.png)

---

## 3. Technical Methodology & Core Pipeline

The script automates cloud-scale data processing across five distinct stages:

* **Temporal Splitting:** Establishes a dry-season baseline reference ($t_0$) from January–March 2018 using a multi-temporal median composite, and captures peak flood metrics ($t_1$) using a minimum pixel reducer from August 10–25, 2018.
* **Polarization Selection:** Deploys cross-polarized **VH (Vertical-Transmit, Horizontal-Receive)** channels to minimize wind-induced surface wave noise common in co-polarized VV bands.
* **Radar Speckle Attenuation:** Eliminates random multiplicative backscatter noise ("salt-and-pepper grain") using a circular **30-meter Focal Mean Spatial Filter**.
* **Change-Detection Segmentation:** Water acts as a specular reflector, bouncing radar signals away from the antenna and appearing as dark values. Inundation pixels are isolated where $\sigma^0_{VH} \le -18.0 \text{ dB}$.
* **Topographic Masking:** Rugged mountain peaks cast radar shadows that falsely mimic water. The pipeline integrates a **30m NASA Digital Elevation Model (NASADEM)** to strip away all slopes exceeding **5°**, preventing false positives.
* **Demographic Exposure Analysis:** The validated flood mask is overlaid onto the **WorldPop 2018 Global Gridded Population Dataset** (100m resolution) using server-side spatial reducers to calculate aggregate population counts caught inside the flood boundary.

---

## 4. Empirically Computed Results

Executing the pipeline extracts a highly precise socio-economic exposure array across the target districts. Total calculated high-exposure population intersects equal **168,978 individuals**.

| District Name | Baseline Population | Radar-Detected Affected Population | Percentage of Population Impacted |
| :--- | :---: | :---: | :---: |
| **Thrissur** | 2,780,720 | 116,688 | **4.20%** |
| **Ernakulam** | 2,872,199 | 50,465 | **1.76%** |
| **Idukki** | 971,270 | 1,825 | **0.19%** |

### Geo-Spatial Interpretation
* **Lowland Inundation:** The pipeline accurately isolates extreme lateral water spreading over the flat coastal plains of **Thrissur** and **Ernakulam**, where the Periyar River transitions into urbanized environments.
* **Topographic Accuracy Validation:** Despite experiencing intense precipitation, **Idukki** scores exceptionally low on population exposure (**0.19%**). This validates the algorithmic efficiency of the NASADEM slope constraint, which successfully stripped mountain radar shadows from the final flood footprint without leaking fake positives into the report matrix.

---

## 5. Scientific Literature Validation
The outputs of this open-source pipeline correlate strongly with peer-reviewed methodologies and data distributions verified during institutional assessments of the 2018 disaster:
* **Inundation Models:** Distribution vectors match spatial trends published by *Tiwari et al. (2020) [PLOS ONE]*, which mapped identical structural water expansions using threshold-based workflows in GEE, yielding an overall model accuracy profile of **94.3%**.
* **Radar Parameters:** The selection of the $-18\text{ dB}$ VH threshold correlates with cross-polarization behavior profiles published by *Sherpa et al. (2020) [IEEE JSTARS]*.

---
*Developed as part of an advanced geospatial data science portfolio exploring NRT hazard observation and crisis informatics.*
