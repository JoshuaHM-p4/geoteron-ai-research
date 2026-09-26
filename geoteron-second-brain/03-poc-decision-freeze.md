# GEOTERON POC Decision Freeze
Status: **LOCKED** (tightened to coworker independent review) · 2026-09-19  
Prior version: broader 5-KPI / A–E catalog — superseded for Q4 POC execution  
Scope: Q4 2026 proof — one site · two scenarios · one dimension pair · Before only

---

## 1. The one decision (frozen)

**Decision owner:** Land / infrastructure developer.

**Frozen decision question:**

> For a realistic industrial–mixed-use district in our study area, **which configuration should we advance to detailed engineering** — comparing **Baseline (A)** vs **High industrial growth (B)** on **traffic/mobility** and **cost/ROI**, with every number traceable to data, assumptions, and models?

**Human decides** from a clear A vs B comparison (and optional Pareto over lever tweaks within B). No autonomous “AI picks the winner.” No single opaque 72/100.

---

## 2. Scope cuts (locked from review)

| Keep in POC | Cut / defer |
|-------------|-------------|
| Behavioral (traffic/mobility) | Societal as decision KPI |
| Economic (cost/ROI) | Environmental as decision KPI (placeholder slide only) |
| Before only | Ecological entirely (fabrication risk without ground truth) |
| Two scenarios: A vs B | Scenarios C/D/E (stretch only, not demo-critical) |
| Provenance + Why | During / After (mock screens only if ever shown) |
| Qualitative confidence bands (+ optional intervals) | Aesthetic “68% / 74%” confidence |

---

## 3. Site & branding (locked)

| Context | Rule |
|---------|------|
| **Internal engineering** | AOI may use Capas / Bamban / Mabalacat corridor data (Central Luzon). |
| **External materials / pitch** | Name it a **“Central Luzon industrial district POC”** (or fully anonymized). **Do not use “Pax Silica” or BCDA branding** until there has been a real conversation with BCDA. |
| **Partner path** | Build → quiet validate with one engineering firm or LGU → then approach BCDA with a working tool, not a named grand challenge. |

Exact AOI polygon + blueprint still TBD; municipalities/corridor class are enough to start BASE-01 ingest.

---

## 4. Scenarios (locked)

| ID | Name | Intent | Levers |
|----|------|--------|--------|
| **A** | Baseline | Stated / current plan assumptions | Defaults |
| **B** | High industrial growth | More factories, workers, logistics | ↑ industrial_floor_ha, ↑ employment_density, ↑ freight_trip_share |

**POC run set = A vs B only.**  
C (water), D (low-carbon), E (transit) remain documented as post-POC stretch — not required to call the POC done.

---

## 5. Levers (POC-active subset)

| Lever | Name | Role in A vs B |
|-------|------|----------------|
| `L01` | `industrial_floor_ha` | Primary B ↑ |
| `L02` | `employment_density` | Primary B ↑ |
| `L03` | `freight_trip_share` | Primary B ↑ |
| `L09` | `capex_intensity` | Cost sensitivity on both |

L04–L08 (water, carbon, transit, mode) = **frozen out of POC decision** until a later phase. May exist in code as stubs; must not block demo.

---

## 6. Objectives & constraints (locked)

### Decision objectives (only these)

| ID | Sense | KPI | Engine |
|----|-------|-----|--------|
| `O1` | Maximize | NPV / ROI (show CAPEX + NPV) | ECO-FIN-01 |
| `O4` | Minimize | Avg travel time and/or congestion index (+ VKT) | BEH-03 (via BEH-01→02) |

### Hard constraints (minimal)

| ID | Constraint |
|----|------------|
| `C1` | `capex <= budget_cap_php` (set when cost sheet exists) |
| `C4` | Floor area within zoning envelope when zoning lands |

Water / flood / CO₂ constraints = **out of POC decision** (appendix only if built early).

### Optional lever search
PRES-01 may explore L01–L03–L09 within bounds to show a small Pareto on **O1 vs O4** only. Not required for v1 demo if A vs B static comparison is clearer.

---

## 7. KPI pack on the decision dashboard

| Dimension | KPI | POC status |
|-----------|-----|------------|
| **Behavioral** | Avg travel time / congestion, VKT; optional emissions-from-traffic as secondary | **Decision** |
| **Economic** | CAPEX, NPV/ROI (P50 + band if Monte Carlo ready) | **Decision** |
| Environmental | Placeholder / future work | Not scored |
| Ecological | Placeholder / future work — do not invent habitat ha | Not scored |
| Societal | Future work | Not scored |

Every decision KPI: value · **assumption-quality band** · top Why drivers · provenance link.

---

## 8. Uncertainty UX (locked — replaces fake confidence %)

**Do not print** unexplained “68% confidence.”

Use a **three-tier qualitative band** tied to an inspectable assumption source:

| Band | Meaning |
|------|---------|
| **High** | Direct from blueprint / measured local data |
| **Medium** | Calibrated from corridor or PH published benchmarks |
| **Low** | Extrapolated from national average or expert prior |

Optional add-on (if UNC-01 is ready): P10/P50/P90 **parameter uncertainty** — never labeled as calibrated predictive confidence against real post-occupancy outcomes (those don’t exist yet).

---

## 9. Engines required to call POC done

| ID | Role |
|----|------|
| BASE-01 | Spatial + network baseline for AOI |
| BEH-01 → BEH-02 → BEH-03 | Trip gen → OD/mode → SUMO KPIs |
| ECO-FIN-01 | CAPEX / OPEX / NPV / ROI |
| DIAG-01 | Why A vs B (ablation / sensitivity) |
| LLM-01 | Provenance narration + RAG (optional for first internal demo; required before external eng review) |

Stretch (not blocking): UNC-01 intervals, PRES-01 Pareto on O1 vs O4, ENV-* appendix.

**Explicitly not required:** ECOL-*, SOC-* as scored KPIs, DURING-*, AFTER-*, causal ML, traffic GNNs.

---

## 10. What “done” means (success test)

1. Real or realistic AOI + blueprint intensity band ingested.  
2. Scenario A and B both run end-to-end.  
3. Dashboard shows Behavioral + Economic KPIs with Why + provenance.  
4. Confidence shown only as qualitative bands (and optional intervals).  
5. External naming complies with §3.  
6. A civil/transport or cost engineer can say: *“I understand where this number came from and trust it as one input.”*

---

## 11. Non-goals checklist

- [ ] No Pax Silica / BCDA name on external decks until real contact  
- [ ] No Environmental/Ecological decision scores  
- [ ] No C/D/E required for demo  
- [ ] No During/After real modules  
- [ ] No aesthetic confidence percentages  
- [ ] No LLM-invented traffic or cost numbers  

---

## 12. Open items (roles frozen; numbers later)

| Item | Needed for |
|------|------------|
| Exact AOI polygon + blueprint | BASE-01 quantities |
| `budget_cap_php` + unit costs | ECO-FIN-01 / C1 |
| Trip-rate / mode priors for corridor | BEH-01/02 calibration |
| Quiet validation partner (eng firm or LGU) | Credibility before any BCDA ask |

---

## Lock statement

**LOCKED:** decision §1 · Beh+Econ only · A vs B only · levers L01/L02/L03/L09 · objectives O1+O4 · branding §3 · uncertainty bands §8 · done criteria §10.

Supersedes the earlier proposed freeze that included Env KPIs, A/B/C minimum, and named Pax Silica in the decision framing.
