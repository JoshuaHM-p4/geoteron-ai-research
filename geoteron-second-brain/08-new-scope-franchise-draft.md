# 08 — New scope: franchises (DRAFT)

- **Date:** 2026-09-26
- **Status:** Draft, pending written confirmation (questions 1–11 answered per Joshua, unconfirmed; 12 open)
- **Purpose:** Hold what we know about the reported scope change, separate from the locked freeze, until it is confirmed in writing.
- **Source:** Word-of-mouth change reported by Joshua in the team chat, 2026-09-26. Not yet confirmed in writing.
- **Relationship to other notes:** `03-poc-decision-freeze.md` stays LOCKED and unchanged. Nothing in this note overrides it until the change is confirmed and Joshua decides how to apply it (see D-013, D-014 in `07-decision-log.md`).

## What we know so far
1. **Subject:** Franchises only (stores, restaurants and similar businesses), with the aim of helping them. This replaces infrastructure as the subject.
2. **Phases:** Before and after construction. (The locked freeze currently covers the before phase only, so this would add the after phase.)

Nothing else has been decided.

## Answers per Joshua, unconfirmed (2026-09-26, D-016; Q6–11 added same day)
Joshua gave these in the team chat. They are **per Joshua, unconfirmed** until the confirmation request comes back.
1. **Who confirms, and when:** The CEO, last week (Sep 19). Whether that was in writing is not stated.
2. **Area:** The Central Luzon study area still applies.
3. **Project name:** Same project (no new name).
4. **Buyer and validators:** Franchising companies are the buyers. Validators are the companies themselves, their stakeholders, and industrial economists or analysts.
5. **Decision:** Validating whether a franchise should start at a specific location.
6. **Scenarios:** Keep the two existing scenarios, A (Baseline) and B (High industrial growth), and test the franchise location under each.
7. **Levers:** Keep all four levers as they are: L01 industrial floor area, L02 employment density, L03 freight trip share, L09 capex intensity. These four now carry over to the franchise scope, even though the Sep 26 relabel (D-015) marked them infrastructure-only. That relabel is left unchanged for now.
8. **Scoring:** Keep the same two scored dimensions: Behavioral (traffic and mobility) and Economic (cost and return). Environmental, ecological and societal stay placeholders. **Still to redefine:** cost and return currently mean the developer's construction cost and return (CAPEX, NPV/ROI in `03`, objective O1), so they need a franchise definition.
9. **Phases:** Build both phases now. "Before" is the pre-opening location check, and "after" is a post-opening check. **Data gap:** the after check needs sales or traffic data from after a store opens. The locked freeze (`03`) says real post-occupancy outcomes "don't exist yet" (§ uncertainty, D-010/D-011) and lists "No During/After real modules" and AFTER-* as not required. No post-opening data source is identified in the notes. `03` is unchanged.
10. **Data:** Reuse the data layers that carry over (population, building footprints, land values, discount rate, foot-traffic priors; see `05`), and add franchise-specific data such as store sales and locations as it arrives. **Note:** the foot-traffic priors (`05` item 6.1) sit in Phase 6 mobility calibration, which the Sep 26 relabel (D-015) tagged infrastructure-only. That relabel is unchanged. No franchise data source is identified yet.
11. **Models:** Use the models that carry over, bring the three traffic models back for the franchise site, and add the post-opening model.
    - *Carry-over (per the D-015 scope status in `02` and the override):* BASE-01 baseline, the ECO-FIN-01 model form (its infrastructure inputs do not carry over), UNC-01 cost ranges, the cost side of DIAG-01, and LLM-01 without the `run_traffic_simulation` tool.
    - *Traffic models brought back for the franchise site:* BEH-01 Trip Generation, BEH-02 Destination + Mode Choice, BEH-03 Traffic Simulation.
    - *Post-opening model:* AFTER-01 (actual vs predicted, drift), currently listed in `02` under Beyond POC / later.
    - **Tags unchanged:** BEH-01 to BEH-03 are still tagged "Beyond POC / later (infrastructure-only, D-015)" in `02` and the override, and AFTER-01 is still Beyond POC / later in `02`. Those tags stay as they are everywhere else until Joshua says otherwise. AFTER-01 needs the post-opening data flagged as a gap in item 9.

## Open questions (none filled in)
- **Existing work:** What happens to the infrastructure notes (`01`–`06`) and the held co-researcher handoff?
