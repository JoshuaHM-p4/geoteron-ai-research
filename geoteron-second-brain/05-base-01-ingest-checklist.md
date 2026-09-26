# BASE-01 Ingest Checklist
AOI class: Central Luzon industrial district POC (Capas · Bamban · Mabalacat corridor)  
Purpose: Feed BEH-01→03 + ECO-FIN-01 only  
Status: Ready to execute · 2026-09-19

## Scope status (D-015, 2026-09-26)
A franchise scope has been reported but is not confirmed (draft in `08-new-scope-franchise-draft.md`). The locked freeze (`03`) is unchanged. Items below are infrastructure-only and are now **Beyond POC / later**. Nothing was deleted. Everything not listed stays as it is and could carry over to the franchise scope (not confirmed).

**Beyond POC / later (infrastructure-only):**
- Phase 1 study-area boundary as written (the area is open in `08`).
- Phase 2 road network and SUMO network stub (whole phase).
- 3.5 industrial zones, 3.6 blueprint.
- 4.6 jobs proxy, 4.7 scenario B multipliers.
- 5.4 road / building / utility unit costs, 5.6 budget cap, 5.7 L01 / L09 capex formulas.
- Phase 6 mobility calibration, including 6.4 freight. 6.3 calibration honesty file could carry over.
- Phase 7 blueprint / zoning request (whole phase).
- Phase 8 road-graph limit. The QA gate format could carry over, and the population limit carries over only if the area stays.
- Optional appendix (DEM, flood).

**Carry over only if the area stays:** 3.1–3.4 building footprints and intensity, 4.1–4.5 population.

---

## How to use
- Check boxes in order within each phase.
- Every layer needs a **provenance row** (source, URL, as-of date, license, CRS, resolution, coverage %).
- Confidence band per freeze: **High** = local/blueprint · **Medium** = corridor/PH benchmark · **Low** = national extrapolate.
- Skip ENV/ECOL rasters for decision path (optional appendix at end).

---

## Phase 0 — Workspace & conventions (day 0)

- [ ] Create PostGIS DB `geoteron_poc` + schema `base01`
- [ ] Lock CRS: **EPSG:32651** (UTM 51N) for analysis; keep WGS84 copies for exchange
- [ ] Create table `base01.layer_catalog` (name, path, crs, resolution, as_of, source_url, license, coverage_pct, quality_band, notes)
- [ ] Create table `base01.ingest_log` (layer, started_at, finished_at, row_count, checksum, operator, status)
- [ ] Define AOI placeholder polygon `base01.aoi` (municipal dissolve Capas+Bamban+Mabalacat until blueprint AOI arrives)
- [ ] Folder layout:
  ```
  data/
    raw/
    interim/
    processed/
    provenance/
  ```
- [ ] Write `DATA_LICENSE.md` listing redistribution constraints per source

**Exit:** empty DB + conventions committed; AOI draft exists.

---

## Phase 1 — Administrative & AOI (blocks everything)

| # | Task | Source | Format | Band | Done |
|---|------|--------|--------|------|------|
| 1.1 | Download PH Admin3 (city/municipality) boundaries | PSA / HDX / geoBoundaries | GeoJSON/SHP | High/Med | [ ] |
| 1.2 | Filter to Capas, Bamban, Mabalacat | — | — | High | [ ] |
| 1.3 | Dissolve → `base01.aoi` + 2–5 km buffer `base01.aoi_buffer` (for network edge effects; buffer width is a working assumption, Joshua; unsourced; co-researcher to test, D-009) | — | — | High | [ ] |
| 1.4 | Record area_ha, centroid, bbox in catalog | — | — | High | [ ] |
| 1.5 | When blueprint arrives: replace with exact site polygon; version as `aoi_v2` | Project / FOI | — | High | [ ] |

**Exit:** `aoi` + `aoi_buffer` in PostGIS; catalog rows written.

---

## Phase 2 — Road network (BEH critical path) · **Beyond POC / later (infrastructure-only, D-015)**

| # | Task | Source | Link / note | Band | Done |
|---|------|--------|-------------|------|------|
| 2.1 | Download Philippines OSM PBF | Geofabrik | https://download.geofabrik.de/asia/philippines.html | Med | [ ] |
| 2.2 | Extract AOI+buffer roads via OSMnx or osmium | — | lanes, maxspeed, highway class | Med | [ ] |
| 2.3 | Build directed graph → `base01.road_nodes`, `base01.road_edges` | OSMnx + NetworkX | length_m, freeflow_s | Med | [ ] |
| 2.4 | Tag SCTEX, MacArthur Highway, known BCDA access roads | Manual / OSM names | Critical corridors | Med | [ ] |
| 2.5 | Join HDX PH road surface (paved/unpaved) where coverage exists | HDX | https://data.humdata.org/dataset/philippines-road-surface-data | Med | [ ] |
| 2.6 | QA: connected component count; island edges; null speeds filled with class defaults | — | Document defaults | — | [ ] |
| 2.7 | Export SUMO network stub (`netconvert` from OSM or edge list) | SUMO | Needed for BEH-03 | Med | [ ] |

**Exit:** routable graph + SUMO net for AOI buffer; provenance for OSM as-of date.

---

## Phase 3 — Buildings & development intensity (BEH + ECON)

| # | Task | Source | Link / note | Band | Done |
|---|------|--------|-------------|------|------|
| 3.1 | Download Google Open Buildings tiles covering AOI | Google Open Buildings v3 | https://sites.research.google/open-buildings/ | Med | [ ] |
| 3.2 | Optional: MS Planetary Computer building footprints (Geoparquet) | MS Buildings | https://planetarycomputer.microsoft.com/dataset/ms-buildings | Med | [ ] |
| 3.3 | Clip to AOI → `base01.buildings` (area_m2, confidence if present) | — | — | Med | [ ] |
| 3.4 | Aggregate to zones: built_m2_ha, building_count | Grid or barangay | Feeds trip gen / cost | Med | [ ] |
| 3.5 | Placeholder land-use zones (industrial / mixed / other) until zoning FOI | Manual sketch OK | Mark band **Low** if guessed | Low | [ ] |
| 3.6 | Blueprint overlay when available (buildings/parcels from plan) | Project file | Band **High** | High | [ ] |

**Exit:** building layer + zonal intensity table; clear Low vs High bands.

---

## Phase 4 — Population & employment proxies (BEH-01)

| # | Task | Source | Link / note | Band | Done |
|---|------|--------|-------------|------|------|
| 4.1 | WorldPop PH 100m pop raster for AOI (2020 population estimate, unconstrained; produced 2018-11-01; DOI 10.5258/SOTON/WP00645) | WorldPop | https://hub.worldpop.org/geodata/summary?id=6316 | Med | [ ] |
| 4.2 | Zonal sum → `base01.pop_grid` / zone population | Rasterio | — | Med | [ ] |
| 4.3 | HDX Admin3 pop projections 2020–2025 for three municipalities | HDX | https://data.humdata.org/dataset/philippines-population-projection-2020-2025-admin3 | Med | [ ] |
| 4.4 | Cross-check WorldPop totals vs Admin3 projections; record ratio | — | Calibration note | — | [ ] |
| 4.5 | Optional: PSA OpenSTAT tables for Tarlac/Pampanga | PSA OpenSTAT | https://openstat.psa.gov.ph | High/Med | [ ] |
| 4.6 | Employment proxy v0: jobs ∝ industrial/commercial built area × density assumption | Assumption register | Band **Low** until better data | Low | [ ] |
| 4.7 | Freeze scenario B multipliers for L01/L02 in `assumptions.yaml` | Freeze doc | — | — | [ ] |

**Exit:** population by zone; documented employment proxy; A vs B population/jobs inputs defined.

---

## Phase 5 — Cost & economic baselines (ECO-FIN-01)

| # | Task | Source | Link / note | Band | Done |
|---|------|--------|-------------|------|------|
| 5.1 | Download BIR zonal values RDO 17 (Tarlac City/Capas/Bamban) | BIR | https://www.bir.gov.ph/index.php/zonal-values.html | High | [ ] |
| 5.2 | Download BIR zonal values RDO 21B (South Pampanga) | BIR | same portal | High | [ ] |
| 5.3 | Digitize/tabularize into `base01.zonal_values` (₱/sqm by barangay/class) | — | Manual OK | High | [ ] |
| 5.4 | Unit cost sheet v0: road ₱/km, building ₱/m², utilities allowance | Eng. benchmarks / project | Band Med/Low — cite | Med | [ ] |
| 5.5 | Lock discount rate + analysis horizon in `assumptions.yaml` | Finance assumption | Document band | — | [ ] |
| 5.6 | Define `budget_cap_php` placeholder (C1) — even if TBD | Product | — | — | [ ] |
| 5.7 | Scenario B: map L01/L09 → CAPEX delta formulas | ECO-FIN-01 | — | — | [ ] |

**Exit:** land value table + unit cost sheet + NPV inputs for A and B.

---

## Phase 6 — Mobility calibration aids (BEH validation)

| # | Task | Source | Link / note | Band | Done |
|---|------|--------|-------------|------|------|
| 6.1 | Review Techsalerator PH foot traffic / mobility (Kaggle) for OD shape priors | Kaggle | Use for temporal/OD priors only | Low/Med | [ ] |
| 6.2 | Review Smart Mobility traffic dataset for feature sanity — **do not** treat as Capas ground truth | Kaggle | Mark geographic mismatch | Low | [ ] |
| 6.3 | Write `calibration_notes.md`: what is local vs borrowed | — | Required for Why UI | — | [ ] |
| 6.4 | **Freight trip rates (scenario B), placeholder.** Add a labeled freight-trip input to the baseline with **no value set yet**; mark it *placeholder, not calibrated*. Fill later from the co-researcher's modeling-priors research (brief Track D: freight trip rates per hectare or per employee for industrial land) and record source, year, place | Co-researcher Track D (pending) | Borrowed values must be labeled borrowed (D-007) | Low | [ ] |

**Exit:** explicit calibration honesty file (reviewer trust).

---

## Phase 7 — Zoning / blueprint (unblock High band) · **Beyond POC / later (infrastructure-only, D-015)**

| # | Task | Source | Note | Band | Done |
|---|------|--------|------|------|------|
| 7.1 | **Now:** build a labeled stand-in (synthetic) blueprint. **Hold** the BCDA master plan / zoning FOI request until after the quiet validation review (D-002, 07-decision-log.md) | FOI: https://www.foi.gov.ph/agencies/bcda/ | External pitch stays anonymized | High if real | [ ] |
| 7.2 | Ingest zoning / parcel / proposed industrial polygon | — | Versioned | High | [ ] |
| 7.3 | Diff blueprint vs Open Buildings (existing vs proposed) | — | Story for A vs B | — | [ ] |

**Exit:** either real blueprint (internal) or labeled synthetic blueprint (external).

---

## Phase 8 — QA gate before BEH/ECON modeling

> The numeric pass/fail limits here and the 2–5 km buffer in row 1.3 are **Joshua's working assumptions**, not sourced standards. The co-researcher tests them (brief Track B) and they get replaced or confirmed with a cited source (D-009).

- [ ] All P0 layers listed in `layer_catalog` with non-null provenance
- [ ] CRS unified to EPSG:32651 for analysis tables
- [ ] AOI coverage_pct ≥ 95% for roads, buildings, population *(working assumption, Joshua; unsourced; co-researcher to test, D-009)*
- [ ] Road graph: ≤1 major disconnected component inside AOI (or documented) *(working assumption, Joshua; unsourced; co-researcher to test, D-009)*
- [ ] Population AOI total within 20% of Admin3 projection sum (or explained) *(working assumption, Joshua; unsourced; co-researcher to test, D-009)*
- [ ] Cost sheet has units and citations
- [ ] `assumptions.yaml` lists every Low-band default used in A vs B
- [ ] Great Expectations (or SQL checks) for null rates, bbox, duplicate IDs
- [ ] Snapshot tag: `base01_v0.1` + checksums in `ingest_log`

**Exit criteria = BASE-01 “ready for BEH-01.”**

---

## Suggested ownership & sequencing (small team)

| Day | Focus |
|-----|--------|
| 0 | Phase 0 + 1 |
| 1–2 | Phase 2 roads + SUMO stub |
| 2–3 | Phase 3 buildings |
| 3 | Phase 4 population |
| 4 | Phase 5 costs / BIR |
| 5 | Phase 6–8 QA + assumptions freeze |

Parallel: build the labeled stand-in blueprint (Phase 7, row 7.1) early. **Hold** the FOI / master-plan request until after the quiet validation review (D-002).

---

## Optional appendix (not decision path) · **Beyond POC / later (infrastructure-only, D-015)**

Only if spare time — do **not** block Beh+Econ:

- [ ] Copernicus DEM GLO-30 (OpenTopography collection published 2021; https://opentopography.org/meta/OT.032021.4326.1 ; DOI 10.5069/G9028PQB)
- [ ] UP NOAH flood rasters
- [ ] GeoRiskPH / HazardHunter layers

---

## Provenance row template

```yaml
layer: road_edges
source_name: Geofabrik OSM Philippines
source_url: https://download.geofabrik.de/asia/philippines.html
as_of: YYYY-MM-DD
license: ODbL
crs: EPSG:4326 → 32651
resolution: vector
coverage_pct: 99            # example value only, not a measured figure
quality_band: medium
transform: osmium extract + OSMnx graph
checksum: sha256:...
```

---

## Handoff outputs (what BEH/ECON expect)

| Consumer | Needs from BASE-01 |
|----------|-------------------|
| BEH-01 | Zone pop, jobs proxy, land-use intensity |
| BEH-02 | Skim times/distances from road graph |
| BEH-03 | SUMO `.net.xml` + demand hooks |
| ECO-FIN-01 | Zonal values, unit costs, blueprint quantities / L01 ha |
| DIAG-01 | Catalog + assumptions.yaml for Why |
| LLM-01 | Same provenance corpus for RAG |
