# GEOTERON Model Registry
Updated: 2026-09-19 · Owner: ML/AI engineer · Scope: 2026 Q4 POC

## How to read this

| Field | Meaning |
|-------|---------|
| **ID** | Stable registry key (`DIM-##`) |
| **Priority** | P0 = must ship in POC · P1 = strong if data ready · P2 = post-POC |
| **Layer** | D = Descriptive · Diag = Diagnostic · Pred = Predictive · Pres = Prescriptive |
| **Type** | GIS / EQ (equation) / SIM / ML / OPT / LLM |

POC geography: Capas · Bamban · Mabalacat corridor · New Clark City / Pax Silica context.

---

## Priority summary (build order)

| Priority | IDs | Rationale |
|----------|-----|-----------|
| **P0** | BASE-01, BEH-01, BEH-02, BEH-03, ENV-01, ENV-02, ENV-03, ECO-FIN-01, UNC-01, DIAG-01, PRES-01, LLM-01 | One decision, several scenarios, defensible comparison |
| **P1** | SOC-01, SOC-02, ENV-04, ECON-02, DIAG-02, PRES-02 | Deepens credibility without new exotic models |
| **P2** | BEH-04, ECOL-*, DURING-*, AFTER-*, CAUSAL-* | Needs more data, sensors, or post-construction reality |

---

# Cross-cutting engines

### BASE-01 — Spatial Baseline Engine
| | |
|--|--|
| **Target** | Baseline feature layers for the study area (no single scalar) |
| **Inputs** | OSM/Geofabrik roads; HDX road surface; Google Open Buildings / MS footprints; WorldPop 100m; HDX/PSA Admin3 pop; Copernicus DEM; GeoRiskPH / HazardHunter; UP NOAH flood rasters; BIR zonal values (RDO 17, 21B) |
| **Algorithms** | PostGIS spatial joins/overlays; GeoPandas; Rasterio zonal stats; OSMnx graph build; NetworkX metrics |
| **Type / Layer** | GIS · D |
| **Validation** | Area coverage QA; CRS consistency; completeness % vs AOI; sample visual QA vs satellite |
| **Uncertainty** | Data coverage flags per tile/layer |
| **Explainability** | Layer provenance table (source, date, resolution, license) |
| **Output schema** | `{aoi_id, layers[{name, path, crs, resolution, as_of, coverage_pct, source_url}], graph_stats{nodes, edges, total_km}}` |
| **Priority** | **P0** |

### UNC-01 — Uncertainty / Monte Carlo Engine
| | |
|--|--|
| **Target** | Distribution over any scenario KPI (not a point estimate) |
| **Inputs** | Parameter priors (population growth, occupancy, per-capita water/energy, mode shares, unit costs); model outputs from engines |
| **Algorithms** | Monte Carlo sampling; optional PyMC for Bayesian params; conformal intervals later |
| **Type / Layer** | SIM · Pred / Diag |
| **Validation** | Coverage of held-out historical baselines where available; prior elicitation review with domain experts |
| **Uncertainty** | *This is the uncertainty method* → report P10 / P50 / P90, 95% PI |
| **Explainability** | Tornado / Sobol sensitivity on input params |
| **Output schema** | `{kpi_id, scenario_id, p10, p50, p90, mean, std, n_draws, dominant_uncertainty[{param, contribution}]}` |
| **Priority** | **P0** — replace vague “confidence %” |

### DIAG-01 — Scenario Ablation + Sensitivity
| | |
|--|--|
| **Target** | Attribution of ΔKPI between scenarios (or vs baseline) |
| **Inputs** | Paired scenario runs; feature toggles; UNC-01 draws |
| **Algorithms** | One-at-a-time / Morris sensitivity; full ablation of scenario levers |
| **Type / Layer** | SIM · Diag |
| **Validation** | Expert review of top drivers; consistency with SHAP where ML used |
| **Uncertainty** | Driver ranks under Monte Carlo |
| **Explainability** | Ranked contribution table (feeds LLM Why?) |
| **Output schema** | `{kpi_id, base_scenario, alt_scenario, delta, drivers[{name, delta_contrib, method}]}` |
| **Priority** | **P0** |

### DIAG-02 — SHAP Explainability (tabular ML)
| | |
|--|--|
| **Target** | Feature contributions for any P0/P1 XGBoost prediction |
| **Inputs** | Trained model + instance or cohort |
| **Algorithms** | SHAP (TreeExplainer) |
| **Type / Layer** | ML · Diag |
| **Validation** | Sanity vs known engineering relationships |
| **Uncertainty** | Optional SHAP variance across bootstrap models |
| **Explainability** | *This is the method* |
| **Output schema** | `{model_id, prediction, base_value, shap[{feature, value, shap}]}` |
| **Priority** | **P1** (P0 wherever XGBoost is in the critical path) |

### PRES-01 — Multi-Objective Scenario Optimization
| | |
|--|--|
| **Target** | Pareto set of scenario configurations over user objectives |
| **Inputs** | Decision variables (intensity, mix, transit, recycling rate, energy mix…); constraint bounds; KPI evaluators from engines |
| **Algorithms** | pymoo NSGA-II / NSGA-III |
| **Type / Layer** | OPT · Pres |
| **Validation** | Constraint satisfaction rate; expert review of frontier extremes |
| **Uncertainty** | Optimize on P50; optionally robustify on P90 of bad outcomes |
| **Explainability** | Objective vectors + constraint slacks per candidate |
| **Output schema** | `{candidates[{id, x, objectives{}, constraints_ok, dominated}], objective_names[], weights_optional{}}` |
| **Priority** | **P0** for “one decision” POC framing |

### PRES-02 — Hard Constraint Planning
| | |
|--|--|
| **Target** | Feasible discrete plans (facility/road allocation, budget split) |
| **Inputs** | Cost tables, capacity, zoning polygons, budget |
| **Algorithms** | Google OR-Tools (CP-SAT / MIP) |
| **Type / Layer** | OPT · Pres |
| **Validation** | Infeasibility explanation; expert check |
| **Uncertainty** | Scenario-parameterized costs |
| **Explainability** | Binding constraints list |
| **Output schema** | `{status, solution{}, binding_constraints[], objective_value}` |
| **Priority** | **P1** |

### LLM-01 — Orchestration + RAG + Provenance Narration
| | |
|--|--|
| **Target** | Structured scenario params + evidence-backed explanations (never raw KPIs from weights alone) |
| **Inputs** | User NL; RAG corpus (blueprints, assumptions, model cards, run metadata); tool results |
| **Algorithms** | Embeddings + hybrid retrieval; tool-calling reasoning LLM; optional small LLM for extraction; multimodal later for blueprints |
| **Type / Layer** | LLM · all layers |
| **Validation** | Citation required; groundedness checks; tool-result must match narrated numbers |
| **Uncertainty** | Pass through UNC-01 intervals verbatim |
| **Explainability** | Always attach `get_model_provenance` + DIAG outputs |
| **Output schema** | `{answer_md, citations[], tool_calls[], kpi_refs[], warnings[]}` |
| **Priority** | **P0** |
| **Tools to expose** | `get_project_baseline`, `query_spatial_dataset`, `calculate_accessibility`, `run_traffic_simulation`, `run_population_simulation`, `run_water_model`, `run_energy_model`, `run_environmental_model`, `run_economic_model`, `compare_scenarios`, `run_sensitivity_analysis`, `optimize_scenarios`, `explain_prediction`, `get_model_provenance` |

---

# Behavioral

### BEH-01 — Trip Generation
| | |
|--|--|
| **Target** | `trips_ij_purpose` or zone productions/attractions (home-based work, other, freight) |
| **Inputs** | Synthetic / WorldPop population; employment floorspace; land use; income proxies; scenario intensity levers |
| **Algorithms** | **POC:** Gravity / rate factors + Poisson or Negative Binomial regression · **Alt:** XGBoost |
| **Type / Layer** | EQ/ML · Pred |
| **Validation** | MAE / MAPE vs available mobility OD aggregates (Techsalerator / published counts); trip rate reasonableness |
| **Uncertainty** | UNC-01 on rates and population |
| **Explainability** | Coeff tables or DIAG-02 if XGBoost |
| **Output schema** | `{zone_id, purpose, produced, attracted, scenario_id}` |
| **Priority** | **P0** |

### BEH-02 — Destination + Mode Choice
| | |
|--|--|
| **Target** | OD matrix + mode shares (car, transit, walk/other) |
| **Inputs** | BEH-01; skim times/distances from network; costs; transit availability flag (scenario) |
| **Algorithms** | Gravity + Multinomial Logit (discrete choice) |
| **Type / Layer** | EQ · Pred |
| **Validation** | Mode share bounds vs PH / corridor priors; OD entropy sanity |
| **Uncertainty** | Utility param priors via UNC-01 |
| **Explainability** | Utility terms breakdown |
| **Output schema** | `{origin, dest, mode, trips, generalized_cost}` |
| **Priority** | **P0** |

### BEH-03 — Network Assignment + Traffic Simulation (SUMO)
| | |
|--|--|
| **Target** | Mean travel time, speed, queue, VKT, congestion index, emissions proxies |
| **Inputs** | Road graph (BASE-01); BEH-02 demand; signal/default speeds; scenario network edits |
| **Algorithms** | SUMO microscopic (or meso if scale forces) |
| **Type / Layer** | SIM · Pred |
| **Validation** | Compare corridor speeds/volumes to Smart Mobility / foot-traffic calibrators where usable; conservation of demand |
| **Uncertainty** | Demand scaling + seed ensemble |
| **Explainability** | DIAG-01 link/corridor contribution to delay |
| **Output schema** | `{scenario_id, kpi{avg_tt, avg_speed, vkt, cong_index, emissions_co2}, by_edge[{edge_id, volume, speed}]}` |
| **Priority** | **P0** — core behavioral differentiator |

### BEH-04 — Spatiotemporal Traffic ML (later)
| | |
|--|--|
| **Target** | Short-horizon link speed / congestion |
| **Inputs** | Dense sensor / probe time series |
| **Algorithms** | Graph WaveNet / DCRNN / Temporal Transformer |
| **Priority** | **P2** — do not start without dense telemetry |

---

# Societal

### SOC-01 — Accessibility & Service Catchment
| | |
|--|--|
| **Target** | Jobs / healthcare / school accessibility (e.g. jobs within 30/45/60 min); isochrone areas |
| **Inputs** | Network; facility points (to collect); population grid; employment |
| **Algorithms** | OSMnx isochrones; gravity accessibility indices |
| **Type / Layer** | GIS · D/Pred |
| **Validation** | Expert planner review; monotonicity (more roads/transit → not worse access, ceteris paribus) |
| **Uncertainty** | Travel-time uncertainty from BEH-03 |
| **Explainability** | Map + top blocked links |
| **Output schema** | `{zone_id, metric, value, threshold_min}` |
| **Priority** | **P1** (P0 if societal is in the “one decision” scorecard) |

### SOC-02 — Inequality & Service Pressure
| | |
|--|--|
| **Target** | Accessibility Gini / gap; population per facility load |
| **Inputs** | SOC-01; population; facility capacity assumptions |
| **Algorithms** | Gini / ratio metrics; simple load = pop / capacity |
| **Type / Layer** | EQ · Pred |
| **Validation** | Bounds checks; expert review |
| **Uncertainty** | Capacity assumption ranges |
| **Explainability** | Grouped tables (barangay / income proxy) |
| **Output schema** | `{metric, value, by_group[]}` |
| **Priority** | **P1** |

### SOC-03 — Displacement / Population Redistribution
| | |
|--|--|
| **Target** | Estimated displaced households / redistributed population under footprint |
| **Inputs** | Building footprints; zoning; scenario footprint; household size priors |
| **Algorithms** | GIS intersection + demographic rates (not a black-box ML) |
| **Type / Layer** | GIS/EQ · Pred |
| **Validation** | Order-of-magnitude vs known resettlement cases |
| **Uncertainty** | Household size, occupancy |
| **Explainability** | Hectares × density assumptions shown |
| **Output schema** | `{scenario_id, hectares_affected, hh_estimate, p10, p50, p90}` |
| **Priority** | **P1** |

---

# Environmental

### ENV-01 — Water Demand
| | |
|--|--|
| **Target** | `water_demand_mld` (ML/day) by sector (residential, industrial, other) |
| **Inputs** | Population; industrial floor area; recycling rate (scenario C lever); climate/temp proxy; engineering unit rates |
| **Algorithms** | **POC:** engineering coefficients + linear/GLM · **Alt:** XGBoost if enough tabular history |
| **Type / Layer** | EQ/ML · Pred |
| **Validation** | Compare to published utility / PEZA-type benchmarks; residual plots |
| **Uncertainty** | UNC-01 on rates + occupancy |
| **Explainability** | DIAG-01 / DIAG-02 |
| **Output schema** | `{scenario_id, sector, mld, p10, p50, p90}` |
| **Priority** | **P0** |

### ENV-02 — Energy Demand + CO₂
| | |
|--|--|
| **Target** | `energy_mwh` and `co2_t` (scope defined in model card) |
| **Inputs** | Building mix; industrial intensity; energy mix lever (scenario D); VKT from BEH-03; emission factors |
| **Algorithms** | Factor models + SUMO emissions for traffic portion |
| **Type / Layer** | EQ/SIM · Pred |
| **Validation** | Factor sources documented (IPCC / PH grid factors); unit tests on mix lever |
| **Uncertainty** | Factor ranges + demand uncertainty |
| **Explainability** | Stacked contribution (buildings vs traffic vs industry) |
| **Output schema** | `{scenario_id, energy_mwh, co2_t, by_source[]}` |
| **Priority** | **P0** |

### ENV-03 — Flood / Hazard Exposure Overlay
| | |
|--|--|
| **Target** | Hectares / asset value / population in flood or liquefaction / fault buffers |
| **Inputs** | UP NOAH rasters; GeoRiskPH polygons; DEM; scenario footprint & roads |
| **Algorithms** | GIS overlay (not ML hydrology) |
| **Type / Layer** | GIS · D/Pred |
| **Validation** | Visual QA; area conservation |
| **Uncertainty** | Return-period choice as scenario parameter |
| **Explainability** | Map layers + intersection table |
| **Output schema** | `{hazard, return_period, pop_exposed, ha_exposed, assets_exposed}` |
| **Priority** | **P0** |

### ENV-04 — Waste / Heat / Noise (lightweight)
| | |
|--|--|
| **Target** | Waste t/day; optional UHI proxy; noise buffers later |
| **Inputs** | Per-capita / industry coefficients; land cover; traffic |
| **Algorithms** | Coefficient models; spatial regression later for heat |
| **Priority** | **P1** waste · **P2** heat/noise detailed |

### ENV-05 — Air Quality Dispersion (later)
| | |
|--|--|
| **Algorithms** | Regression now → dispersion model later |
| **Priority** | **P2** |

---

# Ecological

> **Blocked on data.** Foundational catalog is weak here. Do not claim habitat KPIs until BASE-ECOL data lands.

### ECOL-00 — Ecological Data Intake (blocker)
| | |
|--|--|
| **Target** | Land cover, protected areas, habitat/vegetation, watershed boundaries for AOI |
| **Candidate sources** | ESA WorldCover / Dynamic World; NAMRIA land cover; DENR protected areas; GBIF (limited); watershed shapefiles |
| **Priority** | **P0 data task** before any ECOL model is “production” |

### ECOL-01 — Land Cover Change & Habitat Intersection
| | |
|--|--|
| **Target** | Ha land-cover transition; ha habitat / protected overlap with footprint |
| **Inputs** | ECOL-00; scenario polygon |
| **Algorithms** | GIS overlay; landscape fragmentation metrics (e.g. patch density, effective mesh size) |
| **Type / Layer** | GIS · Pred |
| **Validation** | Ecologist review |
| **Uncertainty** | Classification accuracy metadata |
| **Explainability** | Before/after maps |
| **Output schema** | `{scenario_id, loss_ha_by_class{}, protected_overlap_ha, fragmentation{}}` |
| **Priority** | **P1** after ECOL-00 |

### ECOL-02 — Habitat Suitability (ML, later)
| | |
|--|--|
| **Target** | Suitability score / presence probability |
| **Inputs** | Environmental covariates + species/habitat labels |
| **Algorithms** | Random Forest / XGBoost; optional U-Net land-cover from imagery |
| **Priority** | **P2** |

---

# Economic

### ECO-FIN-01 — CAPEX / OPEX / NPV / ROI
| | |
|--|--|
| **Target** | Development + infra cost, OPEX, NPV, IRR/ROI, optional tax/revenue |
| **Inputs** | Blueprint quantities; unit costs; phasing; discount rate; employment assumptions |
| **Algorithms** | Deterministic spreadsheet-grade equations in code |
| **Type / Layer** | EQ · Pred |
| **Validation** | Dual-control with cost engineer; unit tests on formulas |
| **Uncertainty** | UNC-01 on unit costs & schedule |
| **Explainability** | Full formula + line items in provenance |
| **Output schema** | `{scenario_id, capex, opex_annual, npv, irr, revenue, p10_npv, p50_npv, p90_npv}` |
| **Priority** | **P0** |

### ECON-02 — Land Value Uplift
| | |
|--|--|
| **Target** | `predicted_zonal_or_market_value` ₱/sqm or uplift % |
| **Inputs** | BIR zonal baseline; distance to highway/station; density; flood risk; accessibility (SOC-01); land use |
| **Algorithms** | Hedonic / GLM · **Alt:** XGBoost + DIAG-02 |
| **Type / Layer** | ML · Pred |
| **Validation** | RMSE / MAPE vs BIR or transaction samples; spatial CV |
| **Uncertainty** | Prediction intervals |
| **Explainability** | SHAP |
| **Output schema** | `{parcel_or_zone_id, value, uplift_pct, interval}` |
| **Priority** | **P1** |

### ECON-03 — Employment / Activity
| | |
|--|--|
| **Target** | Jobs created / spatial employment distribution |
| **Inputs** | Industrial/commercial intensity; employment densities |
| **Algorithms** | Density factors + optional spatial regression |
| **Priority** | **P1** |

---

# Lifecycle (post-POC stubs)

| ID | Phase | Target | Algorithms | Priority |
|----|-------|--------|------------|----------|
| DURING-01 | During | Construction progress % | CV (later) | P2 |
| DURING-02 | During | Schedule delay risk | Survival / XGBoost | P2 |
| DURING-03 | During | Sensor anomaly | Isolation Forest | P2 |
| AFTER-01 | After | Actual vs predicted residual | Calibration / drift | P2 |
| CAUSAL-01 | Research | Transit → mode shift effect | DoWhy / EconML | P2 |

---

# POC scorecard (what the dashboard must show)

For each scenario A…E (or the subset you freeze):

| Dimension | KPI source (P0) |
|-----------|-----------------|
| Behavioral | BEH-03 travel time / congestion (+ emissions from traffic) |
| Societal | SOC-01 accessibility (if in decision) else defer |
| Environmental | ENV-01 water · ENV-02 CO₂/energy · ENV-03 flood exposure |
| Ecological | Show “data gap” until ECOL-00 · or exclude from POC decision |
| Economic | ECO-FIN-01 NPV/ROI · optional ECON-02 |

Plus for every KPI: UNC-01 intervals · DIAG-01 Why · LLM-01 narration with citations.

**Do not** ship a single 72/100 unless weights are user-set and shown (PRES-01 can still return the Pareto set without collapsing).

---

# Model card template (use per ID)

```yaml
id: ENV-01
name: Water Demand
owner: ml-ai
priority: P0
dimension: environmental
layer: predictive
type: equation+ml
target: water_demand_mld
inputs: [...]
algorithms: [engineering_coeffs, glm, xgboost?]
training_data: [...]
validation: [mape, expert_benchmark]
uncertainty: monte_carlo
explainability: [ablation, shap?]
output_schema: {...}
provenance_tables: [assumptions, factors, code_version, data_as_of]
status: proposed | in_progress | calibrated | retired
```

---

# Immediate engineering backlog (from this registry)

1. Stand up BASE-01 PostGIS AOI + ingest roads, buildings, WorldPop, DEM, NOAH, GeoRisk, BIR  
2. Implement BEH-01→03 pipeline ending in SUMO KPIs  
3. Implement ENV-01, ENV-02, ENV-03 + ECO-FIN-01  
4. Wrap UNC-01 + DIAG-01  
5. LLM-01 tools + RAG over assumptions/model cards  
6. PRES-01 on the frozen decision variables  
7. Parallel track: ECOL-00 data acquisition  

