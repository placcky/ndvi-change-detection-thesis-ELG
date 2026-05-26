# 🌿 NDVI Vegetation Change Detection Workflow

## 📌 Project Overview

This repository contains Jupyter Notebook workflows developed for seasonal vegetation change detection using Sentinel-2 satellite imagery and NDVI analysis.

The project focuses on identifying vegetation decrease between spring and late summer periods using ArcPy-based raster processing workflows.

The workflow combines:

* Sentinel-2 raster preprocessing
* NDVI calculation
* Seasonal raster mosaicking
* Vegetation threshold masking
* Seasonal NDVI difference analysis

---

# 📂 Repository Structure

```text
.
├── README.md
├── 01_generate_ndvi.ipynb
├── 02_create_seasonal_mosaics.ipynb
├── 03_apply_vegetation_mask.ipynb
├── 04_calculate_ndvi_delta.ipynb
```

---

# 📘 Notebook Descriptions

## 1️⃣ 01_generate_ndvi.ipynb

**Purpose:**
Calculates NDVI rasters from Sentinel-2 B04 (Red) and B08 (NIR) bands.

**Main steps:**

* Search Sentinel-2 raster folders
* Match B04 and B08 raster pairs
* Calculate NDVI
* Export NDVI rasters
* Generate clean date-based filenames

**NDVI formula:**

```text
NDVI = (NIR - Red) / (NIR + Red)
```

**Outputs:**

```text
YYYY-MM-DD_NDVI.tif
```

---

## 2️⃣ 02_create_seasonal_mosaics.ipynb

**Purpose:**
Creates seasonal NDVI mosaics from individual NDVI rasters.

**Main steps:**

* Parse raster dates
* Group rasters by year
* Separate spring and summer periods
* Create seasonal mosaics
* Export annual seasonal composites

**Season definitions:**

```text
Spring = May–June
Summer = August–September
```

**ArcPy tools used:**

```text
MosaicToNewRaster
```

**Outputs:**

```text
NDVI_spring_2020.tif
NDVI_summer_2020.tif
```

---

## 3️⃣ 03_apply_vegetation_mask.ipynb

**Purpose:**
Applies NDVI threshold masking to isolate vegetation-covered areas.

**Main steps:**

* Load seasonal NDVI mosaics
* Apply NDVI threshold
* Remove low-NDVI pixels
* Preserve vegetation-only areas
* Export filtered rasters

**Threshold used:**

```text
NDVI >= 0.5
```

**ArcPy expression:**

```python
vegetation = SetNull(ndvi < 0.5, ndvi)
```

**Outputs:**

```text
NDVI_spring_2020_veg.tif
NDVI_summer_2020_veg.tif
```

---

## 4️⃣ 04_calculate_ndvi_delta.ipynb

**Purpose:**
Calculates seasonal NDVI difference between spring and summer vegetation rasters.

The notebook identifies areas where vegetation significantly decreased by late summer.

**Main steps:**

* Pair spring and summer rasters by year
* Replace NoData values with 0
* Calculate NDVI delta
* Export annual vegetation change rasters

**Delta formula:**

```text
NDVI delta = summer NDVI - spring NDVI
```

**ArcPy expression:**

```python
delta = summer_clean - spring_clean
```

**Interpretation:**

| Delta values | Meaning             |
| ------------ | ------------------- |
| Negative     | vegetation decrease |
| Around 0     | stable vegetation   |
| Positive     | vegetation increase |

**Important note:**

NoData values are converted to 0 before subtraction to correctly detect vegetation disappearance between spring and summer periods.

**Outputs:**

```text
NDVI_delta_2020.tif
NDVI_delta_2021.tif
```

---

# ⚙️ Requirements

## Core requirements

```text
ArcGIS Pro
ArcPy
Spatial Analyst Extension
Jupyter Notebook
Python 3.x
```

---

## ArcGIS Pro / ArcPy

This workflow requires:

```text
ArcGIS Pro Python environment (arcpy)
```

Run notebooks using:

* ArcGIS Pro Notebook
* or ArcGIS Pro Conda environment (`arcgispro-py3`)

---

# 📊 Outputs

## Raster outputs

* NDVI rasters
* Seasonal mosaics
* Vegetation masks
* NDVI delta rasters

## Main output folders

```text
NDVI_clean
NDVI_seasonal_mosaic
NDVI_filtered
NDVI_delta
```

---

# ⚠️ Notes

* File paths are currently hardcoded
* Input filenames must contain valid dates (`YYYY-MM-DD`)
* Raster outputs are excluded from GitHub
* Seasonal mosaics use maximum-value compositing
* NDVI delta is calculated as:

```text
summer - spring
```

* Strong negative values indicate vegetation loss

---

# 📚 Use Cases

* Seasonal vegetation monitoring
* Vegetation loss detection
* Environmental GIS workflows
* Remote sensing analysis
* Raster change detection
* Thesis and academic GIS projects

---

# 👤 Author

Emese Lana Gojdár

GIS thesis project – NDVI vegetation change detection and seasonal raster analysis
