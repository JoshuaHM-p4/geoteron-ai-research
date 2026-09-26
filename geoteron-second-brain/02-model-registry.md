# GEOTERON Model Registry
Updated: 2026-09-26 (rewritten to match the locked freeze, D-008) · Owner: ML/AI engineer (Joshua) · Scope: Q4 2026 POC  
Governing notes: `03-poc-decision-freeze.md` (LOCKED) · `02-model-registry-POC-OVERRIDE.md` · `07-decision-log.md`  
This version **incorporates the override** (priorities, D-005, D-006, D-010, D-011). The override file is kept; if the two ever differ, the override wins.  
The previous five-dimension registry (2026-09-19) is summarized under "Beyond POC / later" at the bottom.

## How to read this
| Field | Meaning |
|-------|---------|
| **ID** | Stable registry key (`DIM-##`) |
| **Status** | **Must-ship** = required for the POC · **Optional** = stretch · **Later** = beyond POC |
| **Layer** | D = Descriptive · Diag = Diagnostic · Pred = Predictive · Pres = Prescriptive |
| **Type** | GIS / EQ (equation) / SIM / ML / OPT / LLM |

POC: Central Luzon industrial district POC (internal AOI: Capas · Bamban · Mabalacat corridor). Scenarios **A (Baseline)** vs **B (High industrial growth)**. Levers **L01, L02, L03, L09**. Objectives **O1** (NPV/ROI) and **O4** (travel time / congestion). Scoring on **Behavioral + Economic only**, before-phase only (freeze §1–§6).

## Must-ship vs optional
| Status | IDs | Notes |
|--------|-----|-------|
| **Must-ship** | BASE-01, BEH-01, BEH-02, BEH-03, ECO-FIN-01, UNC-01, DIAG-01, LLM-01 | UNC-01 on ECO-FIN-01 only (D-010). LLM-01 required before external engineering review; optional for the first internal demo (freeze §9). |
| **Optional** | PRES-01 (O1 vs O4 only), DIAG-02 (becomes must-ship only if XGBoost is on the critical path), ENV-01 / ENV-02 / ENV-03 as an unscored appendix (freeze §9) | Static A vs B is enough for v1. |
| **Later** | Everything in "Beyond POC / later" | Not scored in the POC. |

**Confidence rule (D-005):** every must-ship model emits a **High / Medium / Low** label with a one-line reason (data quality, calibration, or assumption). P10/P50/P90 appear only on cost/ROI outputs from UNC-01, shown alongside the band.

---

# Cross-cutting engines

### BASE-01 — Spatial Baseline Engine · Must-ship
| | |
|--|--|
| **Target** | Baseline feature layers for the AOI (no single scalar) |
| **Inputs (Beh + Econ only)** | OSM/Geofabrik roads; HDX road surface; Google Open Buildings / MS footprints; WorldPop 100m (2020 population estimate, unconstrained; produced 2018-11-01; DOI 10.5258/SOTON/WP00645); HDX/PSA Admin3 population; BIR zonal values (RDO 17, 21B); unit-cost sheet. DEM and hazard layers are appendix only. Ingest steps: `05-base-01-ingest-checklist.md`. |
| **Algorithms** | PostGIS joins/overlays; GeoPandas; Rasterio zonal stats; OSMnx graph build; NetworkX metrics |
| **Type / Layer** | GIS · D |
| **Validation** | Phase 8 QA gate in `05` (numeric limits are Joshua's working assumptions, co-researcher to test, D-009) |
| **Confidence output** | High/Medium/Low per layer + coverage flags |
| **Output schema** | `{aoi_id, layers[{name, path, crs, resolution, as_of, coverage_pct, source_url}], graph_stats{nodes, edges, total_km}, confidence}` |

### UNC-01 — Monte Carlo cost ranges · Must-ship (ECO-FIN-01 only, D-010)
| | |
|--|--|
| **Target** | P10 / P50 / P90 for CAPEX, OPEX, NPV, ROI under A and B |
| **Inputs** | Unit-cost ranges and schedule ranges (from the cost sheet; Low band where unsourced). Discount rate stays a fixed ECO-FIN-01 input, not sampled (D-005 scope). |
| **Algorithms** | Monte Carlo sampling |
| **Type / Layer** | SIM · Pred / Diag |
| **Scope limit** | Cost/ROI only. Traffic models get High/Medium/Low labels only, no draws (D-005). |
| **Feeds** | DIAG-01 cost-side sensitivity (D-006) |
| **Confidence output** | Band + P10/P50/P90 |
| **Output schema** | `{kpi_id, scenario_id, p10, p50, p90, n_draws, dominant_uncertainty[{param, contribution}], confidence}` |

### DIAG-01 — Scenario Ablation + Sensitivity · Must-ship
| | |
|--|--|
| **Target** | Why A and B differ on O1 and O4 |
| **Inputs (D-006)** | Traffic side: **one-lever-at-a-time sweeps** over L01, L02, L03, L09 through BEH-01→03. Cost side: **UNC-01 Monte Carlo draws** on ECO-FIN-01. |
| **Type / Layer** | SIM · Diag |
| **Validation** | Expert review of top drivers |
| **Confidence output** | High/Medium/Low on the driver ranking |
| **Output schema** | `{kpi_id, base_scenario, alt_scenario, delta, drivers[{lever, delta_contrib, method}], confidence}` |

### DIAG-02 — SHAP explainability · Optional
Only if an XGBoost model sits on the critical path (e.g. a BEH-01 alternative). Then it becomes must-ship for that model. Output: `{model_id, prediction, base_value, shap[{feature, value, shap}]}`.

### PRES-01 — Pareto search on O1 vs O4 · Optional
| | |
|--|--|
| **Target** | Small Pareto set of configurations |
| **Decision variables** | L01, L02, L03, L09 within bounds only |
| **Objectives** | O1 (max NPV/ROI) and O4 (min travel time / congestion) only (freeze §6) |
| **Constraints** | C1 budget cap, C4 zoning envelope |
| **Algorithms** | pymoo NSGA-II |
| **Rule** | No hidden weighting; no single collapsed score |

### LLM-01 — Orchestration + retrieval + provenance narration · Must-ship before external engineering review
| | |
|--|--|
| **Target** | Parse the question, call tools, explain A vs B with citations. Never produces traffic or cost numbers itself. |
| **Inputs** | Question; retrieval corpus (stand-in blueprint, assumptions, model cards, run metadata); tool results |
| **Validation** | Citation required; narrated numbers must match tool results |
| **Confidence output** | Passes through each model's band (and UNC-01 ranges) verbatim |
| **POC tools** | `get_project_baseline`, `query_spatial_dataset`, `run_traffic_simulation`, `run_economic_model`, `compare_scenarios`, `run_sensitivity_analysis`, `explain_prediction`, `get_model_provenance`. Optional: `optimize_scenarios` (PRES-01). |
| **Output schema** | `{answer_md, citations[], tool_calls[], kpi_refs[], warnings[]}` |

---

# Behavioral (O4)

### BEH-01 — Trip Generation · Must-ship
| | |
|--|--|
| **Target** | Zone productions/attractions (home-based work, other, freight) |
| **Inputs** | WorldPop / Admin3 population; industrial floor area (L01); employment density (L02); freight share (L03; Low-confidence placeholder until Track D, `05` row 6.4, D-007) |
| **Algorithms** | Rate factors + Poisson or Negative Binomial · Alt: XGBoost (then DIAG-02 applies) |
| **Type / Layer** | EQ/ML · Pred |
| **Validation** | Trip-rate reasonableness vs published priors (co-researcher Track D) |
| **Confidence output** | High/Medium/Low label |
| **Output schema** | `{zone_id, purpose, produced, attracted, scenario_id, confidence}` |

### BEH-02 — Destination + Mode Choice · Must-ship
| | |
|--|--|
| **Target** | OD matrix + mode shares |
| **Inputs** | BEH-01; network skims; costs |
| **Algorithms** | Gravity + multinomial logit |
| **Validation** | Mode-share bounds vs corridor priors |
| **Confidence output** | High/Medium/Low label |
| **Output schema** | `{origin, dest, mode, trips, generalized_cost, confidence}` |

### BEH-03 — Network Assignment + Traffic Simulation (SUMO) · Must-ship
| | |
|--|--|
| **Target** | Mean travel time, speed, VKT, congestion index (O4) |
| **Inputs** | Road graph (BASE-01); BEH-02 demand |
| **Algorithms** | SUMO (micro, or meso if scale forces it); seed ensemble |
| **Validation** | Corridor speeds/volumes vs observed counts where usable (acceptance thresholds from co-researcher Track B) |
| **Confidence output** | High/Medium/Low label (no Monte Carlo, D-005) |
| **Output schema** | `{scenario_id, kpi{avg_tt, avg_speed, vkt, cong_index}, by_edge[{edge_id, volume, speed}], confidence}` |

---

# Economic (O1)

### ECO-FIN-01 — CAPEX / OPEX / NPV / ROI · Must-ship
| | |
|--|--|
| **Target** | Development + infrastructure cost, OPEX, NPV, IRR/ROI |
| **Inputs** | Stand-in blueprint quantities (D-002); unit costs; capex intensity (L09); phasing; discount rate; budget cap C1 |
| **Algorithms** | Deterministic spreadsheet-grade equations in code |
| **Validation** | Dual-control with a cost engineer; unit tests on formulas |
| **Confidence output** | High/Medium/Low label + UNC-01 P10/P50/P90 |
| **Output schema** | `{scenario_id, capex, opex_annual, npv, irr, p10_npv, p50_npv, p90_npv, confidence}` |

---

# POC scorecard (what the dashboard shows for A and B)
| Dimension | Source | Scored? |
|-----------|--------|---------|
| Behavioral | BEH-03 travel time / congestion / VKT (O4) | **Yes** |
| Economic | ECO-FIN-01 NPV / ROI (O1) with UNC-01 ranges | **Yes** |
| Environmental · Ecological · Societal | Placeholder panel ("not scored in POC") | No |

For every scored KPI: High/Medium/Low band, DIAG-01 "why", and LLM-01 narration with citations. No single collapsed score.

# Model card template
```yaml
id: BEH-01
name: Trip Generation
owner: ml-ai
status: must-ship        # must-ship | optional | later
dimension: behavioral    # behavioral | economic | cross-cutting
layer: predictive
type: equation+ml
target: zone_trips_by_purpose
inputs: [...]
algorithms: [rate_factors, poisson_nb, xgboost?]
validation: [...]
confidence: high|medium|low + reason
uncertainty: none        # monte_carlo only for ECO-FIN-01 via UNC-01
explainability: [diag01_sweeps, shap?]
provenance_tables: [assumptions, factors, code_version, data_as_of]
status_detail: proposed | in_progress | calibrated | retired
```

# Engineering backlog (POC)
1. BASE-01: PostGIS AOI + roads, buildings, WorldPop, Admin3, BIR, cost sheet (`05`).
2. BEH-01 then BEH-02 then BEH-03, ending in SUMO KPIs for A and B.
3. ECO-FIN-01 + UNC-01 cost ranges.
4. DIAG-01 (lever sweeps + cost draws).
5. LLM-01 tools + retrieval over assumptions and model cards.
6. Optional: PRES-01 on O1 vs O4.

---

## Beyond POC / later
Not part of the Q4 2026 POC. Kept from the 2026-09-19 registry as the long-term plan.

| ID | Dimension | What it was | Why later |
|----|-----------|-------------|-----------|
| Scored environmental dimension | Environmental | ENV-01 / ENV-02 / ENV-03 as scored KPIs (water; energy + CO₂; flood/hazard). In the POC they are optional, unscored appendix only (see above). | Environmental is a placeholder in the POC |
| ENV-04 / ENV-05 | Environmental | Waste/heat/noise; air-quality dispersion | Same |
| SOC-01 / SOC-02 / SOC-03 | Societal | Accessibility; inequality/service pressure; displacement | Societal not scored |
| ECOL-00 / ECOL-01 / ECOL-02 | Ecological | Data intake; land-cover + habitat; habitat suitability ML | Blocked on data |
| ECON-02 | Economic | Land-value uplift (hedonic / XGBoost) | Not in O1 definition for POC |
| ECON-03 | Economic | Employment / activity model | L02 covers employment density as a lever |
| PRES-02 | Cross-cutting | OR-Tools hard-constraint planning | Static A vs B is enough |
| BEH-04 | Behavioral | Spatiotemporal traffic ML (GNN / LSTM) | Needs sensor history |
| UNC-01 on all KPIs | Cross-cutting | Monte Carlo / PyMC / conformal on every KPI | POC limits UNC-01 to cost/ROI (D-005, D-010) |
| DURING-01/02/03 | Lifecycle | Construction progress CV; delay risk; sensor anomaly | Before-phase only |
| AFTER-01 | Lifecycle | Actual vs predicted, drift | Before-phase only |
| CAUSAL-01 | Research | Transit to mode-shift effect (DoWhy / EconML) | Needs observational data |

Also later: scenarios C–E, levers L04–L08, the five-dimension scorecard, and the LLM tools `calculate_accessibility`, `run_population_simulation`, `run_water_model`, `run_energy_model`, `run_environmental_model`.
