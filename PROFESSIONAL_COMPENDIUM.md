# Fernando Núnez Sánchez — Compendio Profesional de Proyectos

> Documento generado para uso en procesos de selección y entrevistas técnicas.
> Cubre todos los repositorios propios (públicos y privados) a fecha de abril 2026.

---

## Perfil Técnico Resumido

Ingeniero con especialización en IA aplicada: sistemas conversacionales de voz, pipelines de datos, modelos generativos, NLP y desarrollo móvil. Trabajo tanto en proyectos de producción propios como en proyectos de investigación académica (Máster Universitario en Inteligencia Artificial, USC/UDC/UVigo). Stack principal: Python (LangGraph, FastAPI, uv), Flutter/Dart, TensorFlow/Keras, PyTorch, Spark.

---

## Proyectos de Producción

### 1. Loaira — Asistente de Voz para Atención Primaria (SERGAS) & gl-speech-streaming

**Repositorios:** `nandonunez/sergas-agent` (privado) y `nandonunez/galego-streaming-voz` (privado) | **Lenguaje:** Python | **Estado:** Entregado y evaluado (TFM Máster en IA, USC)

**Qué es:**
Agente telefónico conversacional de voz para atención primaria en el Servizo Galego de Saúde (SERGAS), acompañado de una infraestructura propia de modelos de voz nativamente streaming en gallego (`gl-speech-streaming`). Gestiona citas médicas en tiempo real con voz bidireccional en gallego y castellano.

**Problema que resuelve:**
Automatizar la gestión integral de citas médicas con baja latencia (<700 ms de latencia percibida de turno), alta fiabilidad clínica y cero coste de infraestructura en nube mediante modelos desplegados en CPU.

**Stack técnico y componentes:**
- **Orquestación conversacional:** LangGraph (grafo de estados con reparación sintética de mensajes ante interrupciones / *safe barge-in*)
- **Voz en tiempo real y turn-taking:** FastRTC / Pipecat, Silero VAD + Smart-Turn v3 para confirmación de pausas, solapamiento de generación de tokens (`pysbd`) y síntesis por oraciones
- **STT nativamente streaming (`gl-speech-streaming`):** FastConformer-Transducer cuantizado a int8 con sherpa-onnx, evaluado a chunks de 560 ms sobre CPU ARM (supera a los modelos offline de Proxecto Nós en habla espontánea)
- **TTS nativamente streaming (`gl-speech-streaming`):** Matcha-TTS (flow-matching no autorregresivo) + Vocos, con front-end fonémico Cotovia G2P (SAMPA compilado para aarch64, eliminando derivas fonéticas al portugués o castellano). Time-To-First-Audio de 527 ms en CPU
- **Supervisor de seguridad multicapa:** 9 reglas deterministas (invariantes R1-R7, anti-bucles R-LOOP, anti-duplicados R-DUP) + crítico LLM para operaciones de escritura con registro de auditoría
- **Resolución de fechas habladas:** Pipeline determinista (`regex → dateparser → SLM fine-tuneado Qwen3-0.6B LoRA` entrenado con ruido acústico de STT)
- **Modelo de datos:** Esquema FHIR-lite (`Patient`, `Practitioner`, `Slot`, `Appointment`) persistido con SQLModel (SQLite/PostgreSQL)
- **9 herramientas clínicas implementadas:** Verificación de paciente, búsqueda de huecos con filtrado NLP, reserva con confirmación explícita, listado y cancelación de citas, búsqueda y reserva de atención a domicilio, solicitud de llamada humana de triaje y generación/envío de certificados de asistencia en PDF
- **Cascada resiliente de proveedores LLM:** Groq (`gpt-oss-120b`) por defecto con conmutación en caliente ante errores 429 hacia Cerebras, NVIDIA NIM y backends vLLM locales

**Logros destacables para entrevistas:**
- Latencia de turno percibida p50 de 691 ms medida extremo a extremo en hardware de producción (Oracle Ampere A1, solo CPU) a coste cero.
- Superación en régimen streaming de los modelos de referencia de Proxecto Nós (whisper-turbo-gl y xls-r-300m-gl).
- Arquitectura robusta ante interrupciones del llamante (barge-in sin pérdida de contexto en el grafo).
- Implementación de supervisor determinista y crítico de seguridad para operaciones destructivas en base de datos.

---

### 2. A Eito — App Móvil para Cantigas Tradicionales Gallegas

**Repositorio:** `nandonunez/a_eito` (privado) | **Lenguaje:** Flutter/Dart | **Estado:** Release publicada (abril 2026)

**Qué es:**
Aplicación móvil local-first para gestionar letras y audio de canciones y coplas del folklore gallego. Publicada en Google Play Store.

**Problema que resuelve:**
Preservar y dar acceso offline al repertorio musical tradicional gallego sin depender de servicios en la nube ni cuentas de usuario.

**Stack técnico:**
- **Framework:** Flutter (cross-platform: Android, iOS, web, desktop)
- **Arquitectura:** Local-first, sin backend propio, sin autenticación
- **Almacenamiento:** Local (dispositivo), con sistema de backup propio
- **Formato de intercambio:** `.aeito` (formato de exportación/importación propio)

**Funcionalidades implementadas:**
- Crear, editar y eliminar canciones con letras y audio adjunto
- Filtrado por categorías y marcado de favoritos
- Modo autoscroll con niveles de velocidad ajustables
- Importación/exportación de archivos `.aeito`
- Sistema de backup local con deduplicación de archivos de audio
- Selector de categorías tipo "pill tabs"
- Detalle con rail de navegación y links
- Listados en Play Store en 4 idiomas: gallego, español, inglés, portugués

**Logros destacables para entrevistas:**
- Ciclo completo de producto: diseño → desarrollo → publicación en tienda
- Arquitectura local-first (privacidad por diseño, sin dependencias externas)
- Formato de serialización propio para importación/exportación
- Soporte multilingüe real (4 idiomas) incluyendo idiomas minorizados

---

### 3. quepraia — Axente Multimodal para Recomendación de Praias

**Repositorio:** `nandonunez/quepraia` (privado) | **Lenguaje:** Python | **Estado:** En desarrollo / validación (TFM Máster en IA)

**Qué es:**
Axente conversacional multimodal (*galego-first*) que recomenda a que praia ir en Galicia cruzando predición meteo/mariña por praia, mareas, avisos costeiros, calidade da auga, tempo de viaxe e o estado real visto polas webcams públicas de MeteoGalicia mediante visión artificial no edge.

**Problema que resuelve:**
A variabilidade meteorolóxica e oceanográfica na costa galega fai complexo elixir praia. O sistema unifica fontes heteroxéneas con razoamento espacial determinista (evitando alucinacións xeográficas dos LLM) e validación visual en tempo real a custo cero.

**Stack técnico:**
- **Razoamento xeoespacial:** Ferramentas espaciais deterministas con métrica *Spatial Grounding Accuracy (SGA)*
- **Visión no edge:** Destilación VLM→CNN (ONNX INT8 na VM ARM de Oracle a custo cero)
- **Modelos de linguaxe:** Destilación LLM→SLM (LoRA local)
- **Fontes de datos:** MITECO, AEMET OpenData, MeteoSIX v4, Open-Meteo Marine, NÁYADE, webcams públicas MeteoGalicia
- **Seguridade e robustez:** Supervisor de seguridade *fail-closed*, *privacy-by-design*, avaliación con xuíz validado por Cohen's kappa

---

## Proyectos de Portfolio Público (Applied AI Portfolio)

**Repositorio:** `nandonunez/applied-ai-portfolio` (público)

Documentación técnica de proyectos de IA aplicada en sectores estratégicos. Ver README principal para detalles completos. Proyectos incluidos:

| Proyecto | Dominio | Stack |
|---|---|---|
| Loaira (SERGAS Agent) | Sanidad pública | LangGraph, FHIR-lite, 9 tools, FastRTC, Qwen3 LoRA |
| Galician Streaming Speech | Modelos de voz (STT/TTS) | FastConformer-Transducer, Matcha-TTS, Cotovia G2P, sherpa-onnx |
| quepraia | Recomendación costera / Visión | VLM/CNN edge, MeteoSIX, SGA, herramientas espaciales |
| Wind Forecasting | Energía eólica | Series temporales, ML, datos MeteoGalicia + SCADA |
| Standalone PV LLP Sizing | Solar fotovoltaica | Optimización, simulación LLP, HPC CESGA |
| Wildfire Risk | Medioambiente | Clasificación geoespacial, Self-Organizing Maps (SOM) |
| Weather Agent | Clima | Agentes, APIs meteorológicas |
| Urban Waste Energy Dashboard | Economía circular | Dashboards, análisis de datos, Power BI |

---

## Proyectos Académicos (Máster en Inteligencia Artificial)

### 3. Language Modelling Lab — Word Embeddings desde Cero

**Repositorio:** `nandonunez/lm-word-embeddings-cbow-skipgram` (privado) | **Materia:** Language Modelling (LM) | **Con:** Santiago Suárez Carrera

**Qué es:** Implementación from scratch de dos arquitecturas de word embeddings entrenadas sobre el corpus de Juego de Tronos.

**Técnicas implementadas:**
- **CBOW (Continuous Bag of Words):** Predicción de palabra objetivo dado contexto circundante
- **Skip-gram:** Predicción del contexto dado la palabra objetivo
- Visualización de embeddings con t-SNE
- Cálculo de similitudes semánticas entre palabras

**Stack:** TensorFlow/Keras, NumPy, Pandas, scikit-learn, Matplotlib

---

### 4. Deep Learning Lab 2 — Regularización sobre IMDB

**Repositorio:** `nandonunez/dl-regularization-imdb` (privado) | **Materia:** Deep Learning (DL) | **Con:** Santiago Suárez Carrera

**Qué es:** Exploración sistemática de técnicas de regularización y optimización aplicadas a clasificación de sentimiento sobre el dataset IMDB. El objetivo es construir una red densa bien generalizada experimentando con combinaciones de métodos.

**Técnicas comparadas:**
- Dropout
- Batch Normalization
- Regularización de pesos (L1 / L2)
- Early stopping
- Estrategias de inicialización de pesos
- Ajuste del optimizador de descenso por gradiente

**Stack:** TensorFlow/Keras, NumPy, Matplotlib, Jupyter

---

### 5. Deep Learning P2 — RNNs: Regresión y Clasificación

**Repositorio:** `nandonunez/dl-rnn-regression-classification` (privado) | **Materia:** Deep Learning (DL) | **Con:** Santiago Suárez Carrera

**Qué es:** Dos casos de uso con Redes Neuronales Recurrentes (RNNs):
- **P2.1 — Regresión:** Predicción de ventas semanales de 45 tiendas Walmart (series temporales con metadatos de tienda, markdowns y festivos)
- **P2.2 — Clasificación de texto:** Clasificación de categoría de productos Amazon a partir de reviews (NLP)

**Stack:** Python, TensorFlow/Keras, Pandas, Jupyter

---

### 6. Deep Learning P3 — Modelos Generativos (GANs y VAEs)

**Repositorio:** `nandonunez/dl-generative-models-gans-vae` (privado) | **Materia:** Deep Learning (DL) | **Con:** Santiago Suárez Carrera

**Qué es:** Implementación y comparación de tres arquitecturas generativas de imagen, todas aplicadas a datasets de rostros (CelebA).

**Modelos implementados:**
- **DCGAN** (Deep Convolutional GAN): generación de rostros en TensorFlow/Keras
- **WGAN-GP** (Wasserstein GAN con Gradient Penalty): entrenamiento sobre CelebA reducido, varias resoluciones (32px, grises, color)
- **VAE** (Variational Autoencoder): generación y reconstrucción de rostros
- **Evaluación:** FID score e Inception Score

**Stack:** TensorFlow/Keras, PyTorch, NumPy

---

### 7. Trustworthy & Explainable AI — Proyecto de Investigación

**Repositorio:** `nandonunez/txai-explainability-fairness` (privado) | **Materia:** Trustworthy & Explainable AI (TXAI)

**Qué es:** Proyecto de investigación que combina técnicas de explicabilidad (SHAP, modelos interpretables) con una auditoría de equidad sobre clasificadores ML, con análisis de disparidad por género y nivel socioeconómico.

**Técnicas:**
- SHAP values (global y local), visualización de árbol de decisión, análisis interpretabilidad vs. accuracy
- Equidad: paridad demográfica, ratios de disparidad FPR/FNR, Pareto front (accuracy vs. fairness)
- Modelos: Decision Tree (profundidad 2 y 6), Random Forest

**Entregables:** Notebook con análisis, presentación (PowerPoint), informe (PDF).

**Stack:** Python, scikit-learn, SHAP, Matplotlib, Jupyter

---

### 8. NLU P1 — POS Tagger Multilingüe

**Repositorio:** `nandonunez/nlu-pos-tagger-multilingual` (privado) | **Materia:** Natural Language Understanding (NLU)

**Qué es:** Etiquetador morfosintáctico (Part-of-Speech tagging) con red neuronal propia, entrenado y evaluado en tres idiomas: inglés, español, portugués.

**Logros:**
- Modelos entrenados y serializados (`.h5`) para los tres idiomas
- Pipeline reutilizable con `DataProcessor` por idioma
- Implementación propia del modelo con TensorFlow/Keras

---

### 9. NLU P2 — Dependency Parser

**Repositorio:** `nandonunez/nlu-dependency-parser` (privado) | **Materia:** Natural Language Understanding (NLU)

**Qué es:** Parser de dependencias sintácticas implementado con el algoritmo arc-eager (transición de estados).

**Técnicas:**
- Algoritmo arc-eager para parsing transicional
- Modelo neuronal para la clasificación de transiciones (SHIFT, LEFT-ARC, RIGHT-ARC, REDUCE)
- Evaluación con métrica estándar CoNLL (UAS/LAS)
- Datos en formato CoNLL-U

**Stack:** Python, TensorFlow/Keras, formato CoNLL-U

---

### 10. Big Data Engineering Lab

**Repositorio:** `nandonunez/bde-spark-mllib-labs` (privado) | **Materia:** Big Data Engineering (BDE) | **Con:** Ortega, Santiago Suárez Carrera

**Prácticas:**
- **A2:** Tratamiento de valores ausentes (estrategias de imputación, análisis de impacto)
- **A4:** Clasificación distribuida con Apache Spark MLlib
- **A5:** Visualización de datos a escala

**Stack:** Python, Apache Spark (MLlib), PySpark, Jupyter

---

## Proyectos Académicos — Mirrors (co-autoría plena, repo propio)

Los siguientes proyectos fueron desarrollados en igual autoría con compañeros. El repo original está bajo el nombre de otro miembro del equipo; se mantiene un mirror privado en la cuenta propia.

### 11. Data Engineering — Disaster ETL Dashboard

**Repositorio:** `nandonunez/de-natural-disasters-etl-tableau` (privado) | **Materia:** Data Engineering (DE)
**Repo original:** `MIQario/DE_disaster_rep` | **Equipo:** MIQario, ToniG14 y otros

**Qué es:** Proyecto grupal de Data Engineering para análisis de desastres naturales. Pipeline ETL completo con Pentaho y visualización en Tableau.

**Técnicas:**
- Diseño de esquema OLAP (star schema) en SQL
- ETL con Pentaho Data Integration (transformaciones de dimensiones y hechos)
- Dashboard de análisis en Tableau
- Fuente de datos: EM-DAT (noviembre 2023)

**Stack:** Pentaho, Tableau, SQL (OLAP/star schema)

---

### 12. Machine Learning — Mobile Phone Price Prediction (Julia)

**Repositorio:** `nandonunez/ml-mobile-price-prediction-julia` (privado) | **Materia:** Machine Learning (ML)
**Repo original:** `MIQario/ml_project` | **Equipo:** MIQario

**Qué es:** Predicción del precio de móviles comparando múltiples algoritmos de ML. Destaca por usar Julia como lenguaje de implementación.

**Técnicas comparadas:**
- ANN (mejor resultado, sin necesidad de reducción dimensional)
- kNN, SVM, Decision Trees
- Stacking Ensemble
- Tres enfoques de preprocesado: sin reducción (NDR), PCA, Feature Selection
- Validación cruzada + HoldOut final

**Stack:** Julia, Flux.jl (ANN), scikit-learn (via PyCall), Jupyter, HDF5

---

### 13. Knowledge Representation — Light-Up Puzzle Solver (ASP)

**Repositorio:** `nandonunez/rp-lightup-puzzle-solver` (privado) | **Materia:** Knowledge Representation & Planning (RP)
**Repo original:** `SantiagoSuarezC/RP_A2` | **Con:** Santiago Suárez Carrera

**Qué es:** Solucionador declarativo del puzzle Light-Up usando Answer Set Programming (ASP) con Clingo. Las soluciones se visualizan con clingraph.

**Técnicas:**
- Lógica declarativa con Clingo (solver ASP)
- Visualización de soluciones con clingraph
- Encodings propios en `.lp`
- Scripts Python para lectura/transformación de puzzles en ASCII

**Stack:** Clingo (ASP), Python, clingraph

*Relevante para: razonamiento lógico, constraint programming, representación del conocimiento.*

---

### 14. Machine Learning 2 — Air Pollution Online Prediction

**Repositorio:** `nandonunez/ml2-pollution-online-prediction` (privado) | **Materia:** Machine Learning 2 (ML2)
**Repo original:** `marcinjedrzejowski/Pollution-Predictor` | **Equipo:** Brian García Machado, Marcin Jedrzejowski, Santiago Suárez Carrera

**Qué es:** Proyecto de online/stream learning para predecir niveles de PM2.5 con 24h de antelación usando datos meteorológicos. El modelo actualiza incrementalmente sin reentrenamiento completo.

**Técnicas:**
- Online learning con la librería River (KNN Regressor, progressive validation)
- Predictor batch como baseline comparativo
- Análisis de concept drift (variaciones estacionales, cambios urbanos/políticos)
- Métricas: R², RMSE, MAE

**Stack:** Python, River, scikit-learn, Pandas, Jupyter

---

## Resumen de Tecnologías por Categoría

| Categoría | Tecnologías |
|---|---|
| **Agentes & LLMs** | LangGraph, OpenAI API, Groq, Anthropic Claude |
| **Voz (STT/TTS)** | FastRTC, Whisper (Groq), Azure Speech, Kokoro, Orpheus/RunPod |
| **Deep Learning** | TensorFlow/Keras, PyTorch, Flux.jl |
| **ML Clásico** | scikit-learn, Apache Spark MLlib, kNN, SVM, DT, Stacking |
| **Online Learning** | River (stream ML), progressive validation, concept drift |
| **NLP** | POS tagging, Dependency parsing, Word embeddings, LMs, RNNs |
| **Modelos Generativos** | DCGAN, WGAN-GP, VAE |
| **Lógica & Razonamiento** | Answer Set Programming (Clingo), constraint solving |
| **Data Engineering** | Pentaho ETL, Tableau, OLAP/star schema |
| **Bases de Datos** | SQLite, PostgreSQL, SQLModel |
| **Mobile** | Flutter, Dart (Android, iOS, web, desktop) |
| **Lenguajes** | Python, Julia, Dart, SQL |
| **Infraestructura** | Docker, Docker Compose, uv, GitHub Actions |
| **Observabilidad** | Opik |
| **Idiomas cubiertos** | Gallego, Español, Inglés, Portugués |

---

## Patrones de Trabajo Observables

- **Full-cycle ownership:** En proyectos propios cubre todo el ciclo — arquitectura, implementación, pruebas, despliegue y publicación.
- **Local-first & privacidad:** Enfoque consistente en no depender de backends externos cuando no es necesario (A Eito, SERGAS con modelos locales opcionales).
- **Idiomas minorizados:** Trabajo activo con gallego tanto en NLP como en productos de consumo.
- **Stack moderno Python:** uv, pyproject.toml, Docker, typing — no legacy.
- **Amplitud técnica:** Python, Julia, Dart, ASP, SQL — adapta el lenguaje al problema.
- **Co-autoría real:** Los repos bajo nombre de compañeros tienen mirror propio en `nandonunez/` — el trabajo es de igual autoría, solo difiere quién creó el repo original.

---

*Actualizado el 2026-04-14.*
