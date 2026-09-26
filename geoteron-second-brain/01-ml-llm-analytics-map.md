# GEOTERON — ML / LLM Analytics Decision Map
Updated: 2026-09-19

## Product (locked)
**GEOTERON**: Infrastructure Decision Intelligence for developers.
POC target: one real project (New Clark City / Pax Silica context), one blueprint, one geography (Capas/Bamban/Mabalacat corridor), several scenarios, one decision.
Five impact dimensions: Behavioral · Societal · Environmental · Ecological · Economic.
Principle: **LLM wraps the analytical system; it does not replace it.**

## North-star rule
| Use | When |
|-----|------|
| GIS / equations / simulation | Defensible, traceable, engineer-trusted numbers |
| Classic ML (XGBoost etc.) | Tabular nonlinear prediction with SHAP |
| LLM | Parse scenarios, call tools, explain with provenance, RAG over docs |
| Deep learning / GNN / RL | Later — only when data volume and need justify |

---

## 1. DESCRIPTIVE — What exists?

| Need | Approach | ML? | LLM? | POC tools |
|------|----------|-----|------|-----------|
| Population, roads, accessibility, terrain, hazard overlay | Spatial SQL + network analytics | No | Query/explain only | PostGIS, GeoPandas, Rasterio, OSMnx, NetworkX |
| Building density / land use from existing footprints | Aggregate polygons | No (use Google Open Buildings / MS footprints) | No | GeoPandas |
| Land-cover from satellite (later) | Segmentation | Optional: U-Net / SegFormer | No | Skip for POC |
| Hotspots / clusters | Clustering | Optional: DBSCAN/HDBSCAN | No | Optional |
| Anomalous sensors (During phase) | Anomaly detection | Later: Isolation Forest | No | Skip for POC |

**Decide:** Descriptive = almost no ML. Don't force models where PostGIS is exact.

---

## 2. DIAGNOSTIC — Why did this happen?

| Need | Approach | ML? | LLM? | POC tools |
|------|----------|-----|------|-----------|
| Factor contribution on ML predictions | Explainability | Yes: SHAP on XGBoost | Narrate SHAP + provenance | SHAP |
| Simulation "why" | Sensitivity + scenario ablation | No | Narrate deltas | Custom sensitivity |
| Variable relationships | Stats | GLM / GAM / Bayesian | No | statsmodels / PyMC |
| Causal ("does transit reduce car use?") | Causal ML | Later: DoWhy / EconML | No | **Not POC** |

**Decide:** POC diagnostic = SHAP + sensitivity + ablation. Causal ML waits for better observational data.

---

## 3. PREDICTIVE — What if we build Scenario X?

### Six POC engines (not dozens of models)

| Engine | Core method | ML models | Simulation | LLM role |
|--------|-------------|-----------|------------|----------|
| 1. Spatial baseline | GIS stack | Minimal | — | Query baseline |
| 2. Mobility | Gravity/OD + route | Trip gen: Poisson/NB or XGBoost; later GNN | **SUMO** | Run/compare scenarios |
| 3. Human/population | Synthetic pop + limited ABM | Clustering optional | **Mesa** | Explain agent outcomes |
| 4. Resource/impact | Engineering coeffs + regression | XGBoost where data supports (water/energy) | Flood = hazard overlay not ML | Explain drivers |
| 5. Economic | CAPEX/OPEX/NPV/ROI | Land-value: XGBoost or hedonic | — | Explain tradeoffs |
| 6. Decision/uncertainty | Monte Carlo + intervals | PyMC / conformal later | Sensitivity | Explain P10/P50/P90 |

### By dimension

| Dimension | POC approach | Primary tech | Avoid for POC |
|-----------|--------------|--------------|---------------|
| Behavioral | Trip gen → OD → mode → SUMO | Gravity, MNL, SUMO, XGBoost travel time | Spatiotemporal GNN |
| Societal | Accessibility, isochrones, inequality metrics | OSMnx, GeoPandas, regression | Giant "societal NN" |
| Environmental | Water/energy/CO₂ factor models + SUMO emissions + NOAH/GeoRisk overlays | Regression + equations | Replacing hydrology with DL |
| Ecological | **DATA GAP** — need land cover/habitat sources first | GIS overlay once data exists | Habitat ML until data |
| Economic | Deterministic finance + land-value ML | NPV/IRR + XGBoost | End-to-end economic LLM |

### Uncertainty (mandatory)
Prefer P10/P50/P90 + data coverage + model validity range over a vague "Confidence: 68%".

---

## 4. PRESCRIPTIVE — What configurations should we examine?

| Need | Approach | Tool | LLM role |
|------|----------|------|----------|
| Competing objectives (ROI vs carbon vs habitat) | Multi-objective → Pareto | **pymoo / NSGA-II** | Explain frontier, not pick secretly |
| Hard constraints (budget, water, LOS) | CP / MIP | **OR-Tools** | Restate feasible set |
| Expensive sim search (later) | Bayesian optimization | BoTorch / similar | Later |

**Decide:** No single "72/100" score unless user sets explicit weights that are shown in the UI.

---

## 5. LLM layer (four model types)

| Component | Purpose | POC |
|-----------|---------|-----|
| Reasoning LLM | Scenario understanding, tool planning, explanation | Tool-calling model (no fine-tune) |
| Small/fast LLM | Classification, extraction | Optional |
| Multimodal | Blueprint / map / PDF understanding | When you ingest blueprints |
| Embeddings | RAG over docs, assumptions, calibration reports | Required |

**Pattern:** Ask → Retrieve → Call tools → Explain with provenance. Never invent traffic/water numbers from model weights alone.

**Tool surface (examples):** get_project_baseline, query_spatial_dataset, run_traffic_simulation, run_water_model, compare_scenarios, run_sensitivity_analysis, optimize_scenarios, explain_prediction, get_model_provenance.

---

## POC stack (commit list)

| Layer | Choice |
|-------|--------|
| Spatial DB | PostgreSQL + PostGIS |
| Vector / raster | GeoPandas, Shapely, Rasterio |
| Road graph | OSMnx + NetworkX |
| Traffic | SUMO |
| ABM | Mesa (limited) |
| Tabular ML | XGBoost + scikit-learn |
| Explain | SHAP |
| Uncertainty | Monte Carlo + PyMC |
| Multi-objective | pymoo / NSGA-II |
| Constraints | OR-Tools |
| Tracking | MLflow |
| Data validation | Great Expectations |
| LLM | RAG + embeddings + tool calling |
| Provenance | Postgres + MLflow metadata |

## Explicitly NOT for POC
GNN traffic, LSTM/TFT everywhere, RL, LLM fine-tuning, multi-agent LLM swarms, fully neural digital twin, autonomous final decisions, causal ML (DoWhy) until data ready, heavy CV unless blueprint ingestion forces it.

## Lifecycle note
- **BEFORE (POC focus):** scenario prediction + optimization + explainability
- **DURING (later):** CV progress, delay risk, sensor anomalies
- **AFTER (later):** actual-vs-predicted, drift, recalibration loop

## Open gaps from your research
1. Ecological datasets (land cover, habitat, biodiversity) — not in foundational data list yet
2. BCDA zoning shapefiles via FOI
3. Exact POC site + blueprint + single decision to optimize against

## Source uploads
- Product / POC / five dimensions / Pax Silica
- Capas–Bamban–Mabalacat data catalog (OSM, Open Buildings, WorldPop, GeoRisk, NOAH, BIR, etc.)
- Hybrid architecture draft (4 analytics + LLM orchestration)
