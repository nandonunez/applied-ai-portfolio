# Wildfire Risk Prediction System (SOM-based)

## System Architecture

```mermaid
graph LR
      A[User or Client Request] --> B[Automated GFS Data Download]
      B --> C[Data Preprocessing and Variable Extraction]
      C --> D[SOM Model: Climate Pattern Classification]
      D --> E[Risk Index Calculation using Historical Fire Data]
      E --> F[Risk Category Mapping: Very Low, Low, Medium, High]
      F --> G[API Delivery to Client Database]
```

<p align="center" style="font-size: 0.9em; color: #888;">
   <em>Illustration: Training process of a Self-Organizing Map (SOM). Source: <a href="https://commons.wikimedia.org/wiki/File:Somtraining.svg">Wikimedia Commons</a></em>
</p>

---

## Overview

This system leverages global numerical weather forecasts (NOAA GFS) and historical wildfire occurrence records to deliver operational wildfire risk indices. Using an unsupervised Self-Organizing Map (SOM), the pipeline classifies synoptic meteorological patterns and assigns risk metrics based on historical fire severity. Results are mapped to standardized risk bands and delivered via API.
 
### Key Features
- **Automated Ingestion Pipeline:** Automated retrieval and preprocessing of GFS atmospheric grids for configured target regions.
- **Configurable Domain Parameters:** Parameterized specification of atmospheric levels, meteorological variables, and bounding coordinates.
- **Unsupervised Pattern Discovery:** 2D SOM topology identifies synoptic configurations statistically correlated with extreme wildfire episodes.
- **Empirical Risk Quantification:** Neurons are weighted by the empirical ratio of severe fire events relative to total recorded fire incidents mapped to that weather pattern.
- **Actionable Outputs:** Categorized indices (Very Low, Low, Medium, High) formatted for direct downstream API consumption.

### Pipeline Workflow
1. **Data Acquisition:** Scheduled retrieval of GFS forecast runs (wind vectors, geopotential height, temperature, relative humidity).
2. **Preprocessing:** Coordinate alignment, vertical level extraction, and multidimensional array normalization.
3. **Pattern Classification:** Best Matching Unit (BMU) selection via the pre-trained SOM network.
4. **Risk Quantification:** Statistical mapping of the identified BMU to historical fire incidence distributions.
5. **Operational Delivery:** Serialized output pushed to client endpoints and monitoring databases.
 
---

## Operational Impact & Context

- **Context:** High climate variability and multi-variable atmospheric data make wildfire anticipation computationally demanding for emergency response units.
- **Solution:** Dimensionality reduction via SOM translates complex 3D atmospheric states into interpretable, calibrated risk states.
- **Application:** Operational decision support for civil protection teams, resource pre-positioning, and alert scheduling.

---

## Technical Stack

- **Machine Learning:** scikit-learn, MiniSom
- **Scientific Computing & Data:** pandas, numpy, xarray
- **Atmospheric Data:** NOAA GFS (Global Forecast System)
- **Deployment & Formats:** Python, Docker, GRIB2, Parquet

---

**Note:** This documentation represents system architecture and design principles; proprietary model weights and confidential client data are excluded.
