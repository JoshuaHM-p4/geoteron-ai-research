# 06 — Co-Researcher Brief (GEOTERON POC)

**From:** Joshua (Lead ML/AI Engineer & Lead Researcher)
**Date:** 2026-09-26
**Status:** Active handoff

## Context in one paragraph
GEOTERON is an infrastructure decision-intelligence system: it simulates development scenarios and compares trade-offs, with every number traceable to its data, assumptions, and model. The proof of concept (POC) is locked to a narrow scope: a Central Luzon industrial district (the Capas–Bamban–Mabalacat corridor around New Clark City), two scenarios (A = Baseline, B = High industrial growth), and only two scored dimensions: **Behavioral** (traffic/mobility) and **Economic** (cost/ROI). The LLM never generates numbers; models do. Your research feeds the data and evidence those models stand on.

**Read first (in this order):**
1. `03-poc-decision-freeze.md` — what's in and out of scope (locked).
2. `05-base-01-ingest-checklist.md` — the datasets we plan to load for the baseline.
3. `02-model-registry-POC-OVERRIDE.md` — which models are P0 for the POC.
4. `04-coworker-poc-review-overlap.md` — the critical review we're responding to.

**Ground rules**
- In external materials, call it the "Central Luzon industrial district POC". Do not name BCDA or Pax Silica.
- Every figure you report needs a source (link, document, page/table), a year, and a geography. If a value is borrowed from another country or city, say so.
- "Not found" is a valid, useful answer. Don't fill gaps with guesses.
- Joshua owns pipeline architecture (with AI Ops) and model selection. Flag implications for those; don't redesign them.

---

## Track A (main): Data accessibility & data engineering

**Goal:** For every dataset in `05-base-01-ingest-checklist.md`, confirm we can actually get it, use it, and load it for our AOI.

For each dataset, answer:
- **Coverage:** Does it cover Capas, Bamban (Tarlac) and Mabalacat (Pampanga)? At what resolution?
- **Freshness:** Date of the data and update cadence.
- **Access:** Direct download, API, request/FOI, or paid? Steps and lead time.
- **License:** Can we use it commercially? Redistribute derived outputs? Attribution requirements?
- **Format & CRS:** File format, projection, size, known quirks (gaps, duplicates, encoding).
- **Quality:** Known accuracy issues or validation studies for the Philippines.
- **Ingest notes:** Anything that affects loading into PostGIS (EPSG:32651) or building the SUMO road network.
- **Alternatives:** Next-best source if this one fails.

Also find sources for the known gaps:
- Employment / jobs by barangay or municipality (current proxy is low confidence).
- Local traffic counts, travel surveys, or origin–destination data for the corridor or nearby.
- Unit construction costs (roads, industrial buildings, utilities) usable in the Philippines.
- Any official land-use or development blueprint for the area (or confirmation it needs an FOI request).

**Deliverable:** `data-source-register.xlsx` (or `.csv`) with one row per dataset and the columns above, plus a 1–2 page summary of blockers and recommendations.

## Track B: Validation standards

**Goal:** Know what a transport engineer or cost engineer would accept as a credible result before we build.

- What calibration/validation standards exist for traffic simulation (e.g., acceptance thresholds for matching observed counts or travel times)? Which are used or accepted in the Philippines?
- How have others calibrated SUMO (or similar microsimulation) with sparse local data?
- What accuracy ranges are considered acceptable for early-stage cost estimates and ROI/NPV at feasibility stage?
- Which Philippine agencies' guidelines apply (e.g., for traffic impact assessment or feasibility studies)?

**Deliverable:** 2–3 page memo: standards found, recommended thresholds for our POC, and sources.

## Track C: Legal side of the data

- Redistribution/commercial terms for each Track A source (can share with Track A register).
- How the FOI process works for the agencies we'd request from (portal, typical turnaround, what to ask for).
- Whether the Data Privacy Act affects any mobility or population data we'd use (especially anything derived from phones or individuals).

**Deliverable:** 1–2 page memo with a clear "safe to use / needs permission / avoid" list.

## Track D (question list, not model design): Modeling priors

Joshua picks the models; you find the numbers they need.
- Trip generation rates and mode shares for Philippine or comparable Southeast Asian cities, especially industrial zones and provincial corridors.
- Freight trip rates per hectare or per employee for industrial land.
- Cost benchmarks the cost/ROI model can cite (government unit costs, published project costs).

**Deliverable:** A table of candidate values with source, year, location, and how comparable it is to our corridor (High / Medium / Low).

## Later / background (only after A–C)
- **Prior art & competitors:** who sells scenario/planning tools for infrastructure, what they do, and what users criticise.
- **Validation partner map:** engineering firms, universities, or LGUs in Central Luzon who might quietly review our method.
- **Ecological data sources (ECOL-00):** habitat, protected areas, land cover for the AOI. Not scored in the POC, but needed later.

---

## Suggested order & check-ins
1. Week 1: Track A for Phases 1–4 of the ingest checklist (AOI, roads, buildings, population). Early read-out to Joshua.
2. Week 2: Track A remainder + Track C.
3. Week 3: Track B + Track D.
4. Background tracks as time allows.

Timelines are a suggestion; adjust with Joshua.

## Definition of done
- Every dataset in the ingest checklist has a filled row in the register, or an explicit "blocked, because ___".
- Every number delivered has a source, year, and geography.
- Blockers are listed with a proposed workaround.
