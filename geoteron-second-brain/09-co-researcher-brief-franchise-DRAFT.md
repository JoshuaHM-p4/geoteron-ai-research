# 09 — Co-Researcher Brief, franchise scope (DRAFT)

**From:** Joshua (Lead ML/AI Engineer & Lead Researcher)
**Date:** 2026-09-26
**Status:** Draft, ready for Joshua's review. **Hand over once the open items below are finished** (D-017, 2026-09-26). This replaces the D-013 hold. The cover message is drafted at the end of this note (D-019). It is a draft only. Joshua edits it and sends it himself.
**Purpose:** The franchise-scope rewrite of the co-researcher brief, ready to send once the scope is confirmed.
**Basis:** The 12 answers in `08-new-scope-franchise-draft.md`. The CEO confirmed summary points 1–4 and 8 and sent no corrections (2026-09-26). Scenarios, levers and scoring (points 5–7) are Joshua's decisions.
**Relationship to other notes:** Replaces `06-co-researcher-brief.md` now that the CEO has confirmed the scope. `06`, the infrastructure `handoff/` pack and the old cover message stay unchanged and do not go out. The locked freeze (`03`) still describes the infrastructure scope and is unchanged.

---

## Context in one paragraph
We are building a proof of concept (POC) that helps a franchising company (stores, restaurants and similar businesses) decide whether to open at a specific location in our Central Luzon study area. It tests each location under two scenarios, A (Baseline) and B (High industrial growth), and scores two things: **Behavioral** (traffic and mobility around the site) and **Economic** (cost and return). Environmental, ecological and societal effects are placeholders, not scored. There are two checks: a **before** check (pre-opening location check) and an **after** check (post-opening, comparing what the models predicted with what happened). The LLM never generates numbers; models do. Your research finds the data and evidence those models stand on.

**Who uses the results:** franchising companies are the buyers. The companies, their stakeholders, and industrial economists or analysts validate the results.

**Read first (franchise reading list, D-018, 2026-09-26):** read only the parts listed here. Everything in them is name-safe.
1. `08-new-scope-franchise-draft.md`, the whole note. The franchise scope, the CEO confirmation, and Joshua's 12 answers.
2. This brief (`09`).
3. `handoff/05-base-01-ingest-checklist-HANDOFF.md`, carry-over data layers only (`08` answer 10):
   - Phase 3, items 3.1–3.4: building footprints and intensity.
   - Phase 4, items 4.1–4.5: population.
   - Phase 5, items 5.1–5.3 (land values) and 5.5 (discount rate).
   - Phase 6, item 6.1: foot-traffic priors, also the post-opening stand-in (`08` answer 9).
4. `02-model-registry.md`, carry-over model sections only (`08` answer 11):
   - BASE-01 Spatial Baseline Engine.
   - ECO-FIN-01, the model form only. Its infrastructure inputs do not carry over.
   - UNC-01 Monte Carlo cost ranges.
   - DIAG-01, the cost side only.
   - LLM-01, without the traffic simulation tool.
   - BEH-01 Trip Generation, BEH-02 Destination + Mode Choice, BEH-03 Traffic Simulation.
   - AFTER-01 (actual vs predicted), the row in the "Beyond POC / later" table.
5. `handoff/02-model-registry-POC-OVERRIDE-HANDOFF.md`, the "Uncertainty outputs" section only: how the models above show High / Medium / Low confidence.

**Scope tags in these files:** the source notes still tag BEH-01 to BEH-03 and item 6.1 as "Beyond POC / later (infrastructure-only)". For this brief they are in franchise scope (D-017). AFTER-01 is still listed as Beyond POC / later in `02`, but the franchise scope uses it as the post-opening model (`08` answer 11).

**Do not read for this brief:** `01`, `03`, `04`, `06`, the other parts of `02` and `05`, and the `handoff/` copies of `03` and `04`. They describe the infrastructure scope.

**Ground rules**
- Call the project the "Central Luzon industrial district POC" in everything you write or share. Do not use any agency or project-brand names.
- Every figure you report needs a source (link, document, page/table), a year, and a geography. If a value is borrowed from another country or city, say so.
- "Not found" is a valid, useful answer. Don't fill gaps with guesses.
- **Do not contact franchise companies, their stakeholders, or data vendors.** Research what exists and how access works. Joshua handles any outreach.
- **Tools:** Work in the spreadsheet only (the source register). Do not touch the project database. Write loading quirks as "loading notes" in the register for Joshua.
- Joshua owns pipeline architecture (with AI Ops) and model selection. Flag implications for those; don't redesign them.

---

## Track A (main): Data access

**Goal:** Confirm we can get, use and load the data the franchise POC needs, for Capas, Bamban (Tarlac) and Mabalacat (Pampanga).

**1. Carry-over layers** (from the baseline ingest checklist, `05`): population, building footprints, land values, discount rate, and foot-traffic priors (in franchise scope, D-017). For each, answer:
- **Coverage:** Does it cover the three municipalities? At what resolution?
- **Freshness:** Date of the data and update cadence.
- **Access:** Direct download, API, request, or paid? Steps and lead time.
- **License:** Commercial use? Redistribution of derived outputs? Attribution?
- **Format & CRS:** File format, projection, size, known quirks.
- **Quality:** Known accuracy issues or validation studies for the Philippines.
- **Fit for a single-site check:** Is the resolution fine enough to judge one store location, not just a district?
- **Alternatives:** Next-best source if this one fails.

**2. Franchise data (new).** Find what exists, who holds it, and how it is usually shared:
- Store locations of franchises in or near the study area.
- Store sales, or proxies for sales, by location.
- Foot traffic or visit counts at store level.

**3. Post-opening data (known gap, `08` item 9).** The after check needs sales or traffic data from stores after they open. No source is identified in the notes. Find candidate sources and what access would take, or write "not found".

**Deliverable:** `data-source-register.xlsx` (or `.csv`), one row per dataset with the columns above, plus a 1–2 page summary of blockers and recommendations.

## Track B: Validation standards

**Goal:** Know what the validators (franchise companies, stakeholders, industrial economists or analysts) would accept as a credible result before we build.
- How are franchise location decisions usually validated? What evidence do franchise companies ask for before opening a site?
- What accuracy is considered acceptable for a pre-opening sales or return estimate?
- How are traffic and mobility around a single retail or restaurant site usually assessed and checked against observed counts? Which Philippine guidelines apply (for example, traffic impact assessment)?
- How do others compare predicted with actual results after a store opens?

**Deliverable:** 2–3 page memo: standards found, recommended acceptance levels for our POC, and sources.

## Track C: Legal side of the data
- Redistribution and commercial terms for each Track A source (share with the register).
- Whether the Data Privacy Act affects store sales, foot-traffic or mobility data, especially anything derived from phones or individuals.
- Typical terms when franchise companies share sales or location data with an outside analyst (confidentiality, allowed uses). Research only; no contact.

**Deliverable:** 1–2 page memo with a clear "safe to use / needs permission / avoid" list.

## Track D (question list, not model design): Model input numbers

Joshua picks the models; you find the numbers they need.
- **Cost and return for a franchise.** How do franchise companies and analysts measure the cost and return of opening a location? List the common measures with sources. Joshua decides which one the POC uses (`08` item 8).
- **Trip generation for retail and restaurant sites:** trips per store or per floor area for Philippine or comparable Southeast Asian locations.
- **Franchise cost and revenue benchmarks:** published set-up costs, operating costs, or sales ranges by franchise type.
- **Levers and franchise demand:** the POC keeps four levers (industrial floor area, employment density, freight trip share, capital cost intensity). Find evidence on how nearby industrial floor area and employment density relate to franchise visits or sales.

**Deliverable:** A table of candidate values with source, year, location, and how comparable it is to our study area (High / Medium / Low).

---

## Suggested order & check-ins
**Work order (a suggestion; adjust with Joshua):**
1. Week 1: Track A parts 1 and 2 (carry-over layers, franchise data).
2. Week 2: Track A part 3 (post-opening data) + Track C.
3. Week 3: Track B + Track D.

**Check-ins:**
- **One early read-out at the end of week 1** with Joshua.
- After that, no fixed check-ins. Flag a blocker to Joshua as soon as you hit one: what is blocked, what you tried, and a proposed workaround.

### Week-1 read-out template
- **Datasets covered:** carry-over layers and franchise data found so far.
- **For each dataset:** coverage of Capas, Bamban and Mabalacat; date; how to get it; license terms; format and projection; quality issues; loading notes.
- **Blocked items:** what is blocked, what you tried, and a proposed workaround.
- **Known gaps so far:** post-opening data, store sales, and the franchise definition of cost and return.
- **Questions only Joshua can answer.**
- Every number carries its source, year, and geography.

## Definition of done
- Every dataset in the register has a filled row, or an explicit "blocked, because ___".
- Every number delivered has a source, year, and geography.
- Blockers are listed with a proposed workaround.

---

## Cover message (DRAFT ONLY, D-019, 2026-09-26)
**Not sent.** Joshua edits this and sends it himself, together with this brief.

> Hi [name],
>
> Here is your research brief for the Central Luzon industrial district POC. The project now helps franchising companies (stores, restaurants and similar businesses) decide whether to open at a specific location in our study area of Capas, Bamban and Mabalacat. It runs two checks: one before a store opens and one after.
>
> Please start with the "Read first" list near the top of the brief. It points you to the parts of the notes that apply to the franchise work and lists what you can skip.
>
> Three research items are still open, and they matter most:
> - **What cost and return should measure for a franchise location.** Find the common measures and their sources. I'll choose the one we use.
> - **Franchise data sources.** Store locations, store sales or proxies for sales, and store-level foot traffic. We plan to ask the franchise companies for store locations first and sales later, so start with location data.
> - **Post-opening data.** Sales or traffic after a store opens. For now we use foot-traffic data as a stand-in, and results from it carry a Low confidence label.
>
> Please don't contact franchise companies, their stakeholders or data vendors. I'll handle any outreach. Every figure needs a source, a year and a place, and "not found" is a fine answer.
>
> Let's do a short read-out at the end of week 1. If you hit a blocker before then, tell me what's blocked, what you tried and a workaround you'd suggest.
>
> Thanks,
> Joshua

---

## Open questions (for Joshua, before handoff)
- **Review before handover:** read this brief and edit the draft cover message above, then send both yourself (D-017, D-019).

**Closed on 2026-09-26:** CEO corrections (none sent); reading list (added above, D-018); carry-over caveats and the foot-traffic relabel (BEH-01 to BEH-03 and `05` item 6.1 are in franchise scope in `08` and `09`, D-017; their tags elsewhere are unchanged); cover message (drafted above, D-019).
