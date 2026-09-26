# 06 — Co-Researcher Brief (GEOTERON POC)

**From:** Joshua (Lead ML/AI Engineer & Lead Researcher)
**Date:** 2026-09-26
**Status:** Active handoff (name-safe, revised 2026-09-26; reading list points to name-safe copies in `handoff/`)

## Context in one paragraph
GEOTERON is an infrastructure decision-intelligence system: it simulates development scenarios and compares trade-offs, with every number traceable to its data, assumptions, and model. The proof of concept (POC) is locked to a narrow scope: a Central Luzon industrial district (the Capas–Bamban–Mabalacat corridor), two scenarios (A = Baseline, B = High industrial growth), and only two scored dimensions: **Behavioral** (traffic/mobility) and **Economic** (cost/ROI). The LLM never generates numbers; models do. Your research feeds the data and evidence those models stand on.

**Read first (in this order):**
1. `handoff/03-poc-decision-freeze-HANDOFF.md` — what's in and out of scope (locked).
2. `handoff/05-base-01-ingest-checklist-HANDOFF.md` — the datasets we plan to load for the baseline.
3. `handoff/02-model-registry-POC-OVERRIDE-HANDOFF.md` — which models are P0 for the POC.
4. `handoff/04-coworker-poc-review-overlap-HANDOFF.md` — the critical review we're responding to.

**Ground rules**
- Call the project the "Central Luzon industrial district POC" in everything you write or share. Do not use any agency or project-brand names.
- Every figure you report needs a source (link, document, page/table), a year, and a geography. If a value is borrowed from another country or city, say so.
- "Not found" is a valid, useful answer. Don't fill gaps with guesses.
- **Tools:** Work in the spreadsheet only (the source register). Do not touch the project database. Where a dataset has loading quirks (format, projection, size, cleaning needed), write them as "loading notes" in the register for Joshua.
- Joshua owns pipeline architecture (with AI Ops) and model selection. Flag implications for those; don't redesign them.

---

## Track A (main): Data accessibility & data engineering

**Goal:** For every dataset in `handoff/05-base-01-ingest-checklist-HANDOFF.md`, confirm we can actually get it, use it, and load it for our AOI.

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
- Any official land-use or development blueprint for the area that is already public (or confirmation it would need an FOI request). Do not file any FOI request; we are using a labeled stand-in blueprint until after a quiet validation review.

**Deliverable:** `data-source-register.xlsx` (or `.csv`) with one row per dataset and the columns above, plus a 1–2 page summary of blockers and recommendations.

## Track B: Validation standards

**Goal:** Know what a transport engineer or cost engineer would accept as a credible result before we build.

- What calibration/validation standards exist for traffic simulation (e.g., acceptance thresholds for matching observed counts or travel times)? Which are used or accepted in the Philippines?
- How have others calibrated SUMO (or similar microsimulation) with sparse local data?
- What accuracy ranges are considered acceptable for early-stage cost estimates and ROI/NPV at feasibility stage?
- Which Philippine agencies' guidelines apply (e.g., for traffic impact assessment or feasibility studies)?
- **Test Joshua's four working assumptions** in the ingest checklist (`handoff/05-base-01-ingest-checklist-HANDOFF.md`): the 2–5 km AOI buffer (row 1.3), and in the Phase 8 QA gate the ≥ 95% coverage limit, the ≤ 1 major disconnected road component, and population within 20% of the Admin3 projection sum. For each, find a published standard or precedent (source, year, place) or write "not found", and say whether the limit looks too loose, too strict, or reasonable.

**Deliverable:** 2–3 page memo: standards found, recommended thresholds for our POC, and sources.

## Track C: Legal side of the data

- Redistribution/commercial terms for each Track A source (can share with Track A register).
- How the FOI process works for the agencies we would request from (portal, typical turnaround, what to ask for). Research only: do not file anything.
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
**Work order (a suggestion; adjust with Joshua):**
1. Week 1: Track A for Phases 1–4 of the ingest checklist (AOI, roads, buildings, population).
2. Week 2: Track A remainder + Track C.
3. Week 3: Track B + Track D.
4. Background tracks as time allows.

**Check-ins:**
- **One early read-out at the end of week 1** with Joshua: what you have found so far and anything that looks blocked.
- **After that, no fixed check-ins.** It is on you to flag a blocker to Joshua as soon as you hit one (a dataset you cannot access, a license you cannot confirm, a question only Joshua can answer). Don't wait for a meeting.
- When you flag a blocker, say what is blocked, what you tried, and a proposed workaround.

### Week-1 read-out template (D-012)
Bring this to the end-of-week-1 read-out. It only uses items this brief already asks for. "Not found" is a valid answer anywhere.
- **Datasets covered:** AOI boundary, roads, buildings, population (Phases 1–4 of the ingest checklist).
- **For each dataset:** coverage of Capas, Bamban and Mabalacat; date of the data; how to get it; license terms; file format and projection (CRS); known quality issues; loading notes.
- **Blocked items:** what is blocked, what you tried, and a proposed workaround.
- **Known gaps so far:** jobs data, local traffic counts, unit construction costs, and any public land-use or development blueprint.
- **Questions only Joshua can answer.**
- **Plan for week 2:** Track A remainder + Track C (legal memo).
- Every number carries its source, year, and geography.

## Definition of done
- Every dataset in the ingest checklist has a filled row in the register, or an explicit "blocked, because ___".
- Every number delivered has a source, year, and geography.
- Blockers are listed with a proposed workaround.
