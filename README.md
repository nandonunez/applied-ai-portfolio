# Applied AI & Scientific Computing Portfolio

A curated collection of system architectures, predictive pipelines, and applied modeling projects across renewable energy, environmental risk, and voice systems.

All documentation emphasizes system design, mathematical methodology, and data pipelines. Proprietary code and client-specific data are excluded.

---

## Project Index

### Voice & Conversational Systems

#### [Public Healthcare Voice Agent](./healthcare-agent)
Real-time conversational voice assistant designed for public healthcare appointment management.
- **Architecture:** Full-duplex streaming audio with sub-second latency using FastRTC and LangGraph graph orchestration.
- **Pipeline:** Dual-layer STT/TTS integration (Groq Whisper, Azure Speech, local models) with relational database persistence (SQLModel).
- **Scope:** Identity verification, calendar slot resolution, and native bilingual execution (Galician and Spanish).

---

### Renewable Energy & Physical Systems

#### [Wind Energy Production Forecasting](./wind-forecasting)
End-to-end predictive pipeline forecasting generation for wind farm assets.
- **Data Integration:** Numerical weather prediction forecasts (GFS, ECMWF) coupled with high-frequency turbine SCADA telemetry.
- **Modeling:** Feature engineering for wake effects and complex terrain; gradient-boosted ensembles (XGBoost, LightGBM) with temporal cross-validation.
- **Delivery:** Automated scheduled inference with direct API integration into operational client databases.

#### [Standalone PV Sizing via Loss of Load Probability (LLP)](./pv-llp-sizing)
Engineering framework for sizing off-grid photovoltaic and storage systems using analytical isoreliability curves.
- **Methodology:** Multi-year global solar irradiation (MeteoGalicia) and empirical load profiles evaluated across multiple temporal resolutions.
- **Optimization:** Cost-reliability Pareto frontier identification in the low-storage / high-generation quadrant.
- **Impact:** Up to ~46% potential cost reduction compared to conventional rule-of-thumb oversizing.

---

### Environmental & Geospatial Analytics

#### [Wildfire Risk Mapping (SOM-based)](./wildfire-risk)
Operational wildfire risk forecasting pipeline based on atmospheric pattern clustering.
- **Pipeline:** Automated ingestion and preprocessing of NOAA GFS gridded atmospheric forecasts.
- **Methodology:** Unsupervised synoptic pattern classification via Self-Organizing Maps (SOM) calibrated against historical fire severity datasets.
- **Output:** Programmatic risk indices delivered to civil protection and emergency planning workflows.

#### [Meteorological Report Generation Agent](./weather-agent)
Automated pipeline translating raw multi-variable meteorological data into structured operational briefs.
- **Pipeline:** API ingestion of numerical atmospheric variables, data validation, and contextual brief generation.
- **Application:** Rapid situational awareness reports for civil protection and emergency dispatchers.

#### [Urban Waste & Energy Recovery Dashboard](./urban-waste-energy-dashboard)
Analytics dashboard monitoring material recovery and biogas/energy generation across treatment facilities.
- **Integration:** Multi-source public datasets covering waste fraction flows, sorting efficiency, and thermal/biogas outputs.
- **Analytics:** Geospatial facility mapping and interactive drill-down views in Power BI.

---

## Engineering Tenets

- **Domain-Grounded:** Models are bounded by physical constraints, energy conservation principles, and empirical sensor data.
- **Production-Oriented:** Designed for real-world constraints: audio latency budgets, concept drift, sampling frequency sensitivity, and automated delivery.
- **Modern Tooling:** Built on standardized Python workflows (`uv`, `pyproject.toml`, Docker) and typed data interfaces.

---

## Contact

- **Profile:** [Fernando Núnez Sánchez](https://github.com/nandonunez)
- **LinkedIn:** [linkedin.com/in/nandonunez](https://www.linkedin.com/in/nandonunez)
