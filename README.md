# Applied AI & Scientific Computing Portfolio

A curated collection of system architectures, predictive pipelines, and applied modeling projects across renewable energy, environmental risk, and voice systems.

All documentation emphasizes system design, mathematical methodology, and data pipelines. Proprietary code and client-specific data are excluded.

---

## Project Index

### Voice & Conversational Systems

#### [Loaira — Primary-Care Healthcare Voice Agent](./healthcare-agent)
Production-grade real-time conversational voice assistant designed for public healthcare appointment management.
- **Architecture:** Full-duplex streaming audio pipeline with overlapped token-to-sentence generation and per-sentence TTS streaming.
- **Tooling & Workflows:** FHIR-lite data model with 9 production tools (patient verification, NLP slot search, booking confirmation, listing/cancellation, home visits, callback requests, and PDF attendance certificates).
- **Turn-Taking & Resilience:** Silero VAD + Smart-Turn v3 pause verification; conversational spoken fillers masking database/critic latencies; safe barge-in with synthetic state repair in LangGraph.
- **Safety & Robustness:** Layered safety supervisor with 9 deterministic rules + LLM write critic with audit log; date resolution via a fine-tuned SLM (Qwen3-0.6B LoRA) trained with acoustic noise.
- **Multi-Provider Cascade:** Resilient failover across Groq (gpt-oss-120b), Cerebras, NVIDIA NIM, and self-hosted models.

#### [Galician Natively Streaming Speech Stack](./healthcare-agent#speech-infrastructure)
Natively streaming, bidirectional speech microservices (STT + TTS) engineered for Galician, deployed entirely on CPU (ARM aarch64) at zero cloud cost.
- **Streaming STT:** Frame-synchronous FastConformer-Transducer (ONNX int8 via sherpa-onnx) evaluated at 560 ms chunk size; outperforms offline reference models on spontaneous speech.
- **Streaming TTS:** Non-autoregressive flow-matching Matcha-TTS with Vocos vocoder chunked by clause; delivers 527 ms Time-to-First-Audio (TTFA) on CPU.
- **Phonemic Front-End:** Native aarch64 Cotovia G2P (SAMPA) preventing phonetic drift into Portuguese or Spanish phonology.
- **End-to-End Latency:** 691 ms p50 perceived turn latency (STT final + TTS first audio) on a free-tier Oracle Ampere A1 VM.

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

#### [quepraia — Multimodal Coastal Recommendation Agent](#quepraia)
Context-aware multimodal conversational agent that delivers beach and coastal recommendations across Galicia.
- **Multi-Source Fusion:** Integrates high-resolution marine and meteorological forecasts, tidal tables, coastal warnings, and bathing water quality indices.
- **Edge Visual Validation:** Analyzes real-time conditions from public coastal webcams using lightweight computer vision deployed at the edge.
- **Spatial Reasoning:** Employs deterministic spatial tools to ensure reliable distance, routing, and geographical grounding without hallucinations.

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
