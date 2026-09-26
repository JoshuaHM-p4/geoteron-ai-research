# GEOTERON — ML / LLM Analytics Decision Map
Updated: 2026-09-26 (rewritten to match the locked freeze, D-008)  
Previous version (2026-09-19) described the full five-dimension vision; that content now lives in "Beyond POC / later" at the bottom.  
Governing notes: `03-poc-decision-freeze.md` (LOCKED) · `02-model-registry.md` · `07-decision-log.md`

## Product
**GEOTERON**: infrastructure decision intelligence for land / infrastructure developers.  
Principle: **the LLM wraps the analytical system; it does not replace it.**

## POC in one line
Central Luzon industrial district POC (internal AOI: Capas / Bamban / Mabalacat corridor): compare **Scenario A (Baseline)** vs **Scenario B (High industrial growth)** on **Behavioral (traffic/mobility)** and **Economic (cost/ROI)** only, **before-phase only**, with every number traceable (`03-poc-decision-freeze.md` §1–§4).

- Levers in play: **L01** industrial_floor_ha · **L02** employment_density · **L03** freight_trip_share · **L09** capex_intensity (§5).
- Decision objectives: **O1** maximize NPV/ROI (ECO-FIN-01) · **O4** minimize travel time / congestion (BEH-03) (§6).
- Confidence: **High / Medium / Low** label on every must-ship model output (§8, D-005).
- A human decides from the A vs B comparison. No single opaque score.

## North-star rule (applies to the POC)
| Use | When |
|-----|------|
| GIS / equations / simulation | Defensible, traceable, engineer-trusted numbers |
| Classic ML (e.g. Poisson/NB, XGBoost) | Only where it beats a rate table and SHAP can explain it |
| LLM | Parse the question, call tools, explain with provenance, retrieve documents |
| Deep learning / GNN / RL | Not in the POC (see Beyond POC / later) |

---

## 1. Descriptive — what exists in the AOI?
| Need | Approach | ML? | LLM? | Tools |
|------|----------|-----|------|-------|
| Roads, network, population, buildings, cost baselines | Spatial SQL + network build (BASE-01) | No | Query / explain only | PostGIS, GeoPandas, OSMnx, NetworkX |
| Development intensity from footprints | Aggregate polygons | No | No | GeoPandas |

Decision: descriptive work uses essentially no ML. Ingest steps are in `05-base-01-ingest-checklist.md`.

## 2. Diagnostic — why does A differ from B?
| Need | Approach | ML? | LLM? |
|------|----------|-----|------|
| Traffic-side drivers | **One-lever-at-a-time sweeps** over L01, L02, L03, L09 (DIAG-01, D-006) | No | Narrate the ranked drivers |
| Cost-side drivers | **Monte Carlo draws** from UNC-01 on ECO-FIN-01 (DIAG-01, D-006) | No | Narrate the ranked drivers |
| Contributions inside any ML model | SHAP (DIAG-02), only if XGBoost is on the critical path | Yes | Narrate SHAP |

## 3. Predictive — what happens under A and B?
| Dimension | Pipeline | Models |
|-----------|----------|--------|
| Behavioral | Trip generation, then destination + mode choice, then SUMO assignment | BEH-01 → BEH-02 → BEH-03 |
| Economic | Deterministic CAPEX / OPEX / NPV / ROI | ECO-FIN-01 (+ UNC-01 Monte Carlo ranges, cost only, D-005) |

Freight trips for B enter as a Low-confidence placeholder until research fills them (`05` row 6.4, D-007).

## 4. Prescriptive — which configuration to advance?
The POC answer is a **static A vs B comparison**. Optional: PRES-01 searches L01–L03–L09 within bounds for a small Pareto on **O1 vs O4 only** (freeze §6). No hidden weighting.

## 5. LLM layer (LLM-01)
Pattern: ask, retrieve, call tools, explain with provenance. It never invents traffic or cost numbers.  
POC tool surface: `get_project_baseline`, `query_spatial_dataset`, `run_traffic_simulation`, `run_economic_model`, `compare_scenarios`, `run_sensitivity_analysis`, `explain_prediction`, `get_model_provenance`. Optional: `optimize_scenarios` (PRES-01, O1 vs O4 only).  
Required before external engineering review; optional for the first internal demo (freeze §9).

## POC stack
| Layer | Choice |
|-------|--------|
| Spatial DB | PostgreSQL + PostGIS |
| Vector | GeoPandas, Shapely |
| Road graph | OSMnx + NetworkX |
| Traffic | SUMO |
| Tabular ML (only if needed) | scikit-learn / XGBoost + SHAP |
| Uncertainty | Monte Carlo on ECO-FIN-01 inputs only |
| Optional search | pymoo (O1 vs O4) |
| Tracking / provenance | MLflow + Postgres metadata |
| Data validation | Great Expectations or SQL checks |
| LLM | Retrieval + embeddings + tool calling |

## Open gaps (POC)
1. Exact AOI polygon and a labeled stand-in blueprint (FOI held until after quiet validation, D-002).
2. Unit costs and `budget_cap_php` (freeze §12).
3. Trip-rate, mode-share, and freight priors for the corridor (co-researcher Track D).

---

## Beyond POC / later
Everything below is **not** part of the Q4 2026 POC. It is kept as the long-term product vision.

- **Five impact dimensions:** Behavioral · Societal · Environmental · Ecological · Economic. Environmental, Ecological and Societal are placeholders in the POC and are not scored.
- **Extra scenarios C (water), D (low-carbon), E (transit)** and levers **L04–L08** (water, carbon, transit, mode).
- **Six-engine vision:** spatial baseline; mobility; human/population (synthetic population + Mesa ABM); resource/impact (water, energy, CO₂, flood overlay); economic (incl. land-value ML); decision/uncertainty (Monte Carlo on all KPIs, PyMC, conformal intervals).
- **Descriptive later:** land-cover segmentation (U-Net / SegFormer), hotspot clustering (DBSCAN/HDBSCAN), sensor anomaly detection.
- **Diagnostic later:** GLM / GAM / Bayesian relationship models; causal ML (DoWhy / EconML) once observational data exists.
- **Environmental later:** water/energy/CO₂ factor models, SUMO emissions, NOAH / GeoRisk hazard overlays.
- **Societal later:** accessibility, isochrones, inequality metrics.
- **Ecological later:** blocked on data (land cover, habitat, protected areas; ECOL-00).
- **Prescriptive later:** multi-objective over ROI, carbon, habitat; OR-Tools hard-constraint planning (budget, water, level of service); Bayesian optimization for expensive simulation search.
- **LLM later:** multimodal blueprint / map / PDF understanding; small LLM for extraction; tools such as `run_water_model`, `run_energy_model`, `run_environmental_model`, `run_population_simulation`, `calculate_accessibility`.
- **Lifecycle later:** DURING (construction progress CV, delay risk, sensor anomalies) and AFTER (actual vs predicted, drift, recalibration).
- **Not planned even later without new data:** GNN traffic, LSTM/TFT everywhere, RL, LLM fine-tuning, multi-agent LLM swarms, fully neural digital twin, autonomous final decisions.

## Source uploads
- Product brief (five dimensions, buyer and validation path)
- Capas–Bamban–Mabalacat data catalog (OSM, Open Buildings, WorldPop, GeoRisk, NOAH, BIR, etc.)
- Hybrid architecture draft (four analytics layers + LLM orchestration)
- Coworker independent POC review (`04-coworker-poc-review-overlap.md`)
