# 08 — New scope: franchises (DRAFT)

- **Date:** 2026-09-26
- **Status:** Draft. Scope summary confirmed by the CEO on 2026-09-26 (per Joshua), except scenarios, levers and scoring, which are Joshua's call. Both data questions decided by Joshua (see "CEO confirmation" below). The CEO sent no corrections. The three traffic models and the foot-traffic data are in franchise scope in this note and `09` (D-017).
- **Purpose:** Hold what we know about the reported scope change, separate from the locked freeze, until it is confirmed in writing.
- **Source:** Word-of-mouth change reported by Joshua in the team chat, 2026-09-26. CEO reply reported by Joshua in the team chat, 2026-09-26. Whether the reply was in writing is not stated.
- **Relationship to other notes:** `03-poc-decision-freeze.md` stays LOCKED and unchanged. Nothing in this note overrides it until the change is confirmed and Joshua decides how to apply it (see D-013, D-014 in `07-decision-log.md`).

## What we know so far
1. **Subject:** Franchises only (stores, restaurants and similar businesses), with the aim of helping them. This replaces infrastructure as the subject.
2. **Phases:** Before and after construction. (The locked freeze currently covers the before phase only, so this would add the after phase.)

Nothing else has been decided.

## CEO confirmation (2026-09-26, per Joshua)
Joshua sent the CEO a confirmation request with an 8-point scope summary and 4 questions. Joshua reports the CEO got back to him on Sep 26.
- **Confirmed by the CEO:** summary points 1–4 and 8: franchises as the subject (item 1 above); the Central Luzon study area and same project name (answers 2, 3); franchising companies as buyers, validated by the companies, their stakeholders and industrial economists or analysts (answer 4); the location decision (answer 5); and two checks, pre-opening and post-opening (answer 9).
- **Left to Joshua:** summary points 5–7, which the CEO said he can't answer himself. These are scenarios (answer 6), levers (answer 7) and scoring (answer 8). Joshua's answers above stand as his decisions.
- **Not answered by the CEO (per Joshua):** question 3 (where post-opening store data comes from) and question 4 (whether franchise companies will share store sales and location data). Joshua is choosing these himself. Question 3 is decided (foot-traffic stand-in, see answer 9). Question 4 is decided (store locations first, sales data later, see answer 10).
- **Not recorded:** whether the CEO answered question 2 (what cost and return should measure for a franchise). Answer 8 still lists it as needing a definition.
- **No corrections:** the CEO sent no corrections to the summary (per the team chat, 2026-09-26).
- Answers 10–12 (data, models, handoff) were internal and were not in the request.

## Answers per Joshua, unconfirmed (2026-09-26, D-016; Q6–12 added same day)
Joshua gave these in the team chat. See "CEO confirmation" above for which ones the CEO has since confirmed and which are Joshua's call.
1. **Who confirms, and when:** The CEO, last week (Sep 19). Whether that was in writing is not stated.
2. **Area:** The Central Luzon study area still applies.
3. **Project name:** Same project (no new name).
4. **Buyer and validators:** Franchising companies are the buyers. Validators are the companies themselves, their stakeholders, and industrial economists or analysts.
5. **Decision:** Validating whether a franchise should start at a specific location.
6. **Scenarios:** Keep the two existing scenarios, A (Baseline) and B (High industrial growth), and test the franchise location under each.
7. **Levers:** Keep all four levers as they are: L01 industrial floor area, L02 employment density, L03 freight trip share, L09 capex intensity. These four now carry over to the franchise scope, even though the Sep 26 relabel (D-015) marked them infrastructure-only. That relabel is left unchanged for now.
8. **Scoring:** Keep the same two scored dimensions: Behavioral (traffic and mobility) and Economic (cost and return). Environmental, ecological and societal stay placeholders. **Still to redefine:** cost and return currently mean the developer's construction cost and return (CAPEX, NPV/ROI in `03`, objective O1), so they need a franchise definition.
9. **Phases:** Build both phases now. "Before" is the pre-opening location check, and "after" is a post-opening check. **Data gap:** the after check needs sales or traffic data from after a store opens. The locked freeze (`03`) says real post-occupancy outcomes "don't exist yet" (§ uncertainty, D-010/D-011) and lists "No During/After real modules" and AFTER-* as not required. No post-opening data source is identified in the notes. `03` is unchanged.
    - **Decided (Joshua, 2026-09-26):** the post-opening check uses foot-traffic data (`05` item 6.1) as a stand-in until real store data is available. That source is rated Low/Med confidence in `05` and is still tagged infrastructure-only under D-015, so post-opening results from it carry a **Low** confidence label.
    - **In franchise scope (D-017, 2026-09-26):** after the CEO confirmed the scope, `05` item 6.1 is in franchise scope for this note and `09`. Its D-015 tag in `05` and the `handoff/` copy stays unchanged.
10. **Data:** Reuse the data layers that carry over (population, building footprints, land values, discount rate, foot-traffic priors; see `05`), and add franchise-specific data such as store sales and locations as it arrives. **Note:** the foot-traffic priors (`05` item 6.1) sit in Phase 6 mobility calibration, which the Sep 26 relabel (D-015) tagged infrastructure-only. That relabel is unchanged. No franchise data source is identified yet.
    - **Decided (Joshua, 2026-09-26):** ask the franchise companies for store locations first, and ask for sales data later. Until sales data arrives, the cost and return check rests on weaker inputs.
    - **Foot-traffic priors (`05` item 6.1):** in franchise scope for this note and `09` (D-017). The D-015 tag in `05` is unchanged.
11. **Models:** Use the models that carry over, bring the three traffic models back for the franchise site, and add the post-opening model.
    - *Carry-over (per the D-015 scope status in `02` and the override):* BASE-01 baseline, the ECO-FIN-01 model form (its infrastructure inputs do not carry over), UNC-01 cost ranges, the cost side of DIAG-01, and LLM-01 without the `run_traffic_simulation` tool.
    - *Traffic models brought back for the franchise site:* BEH-01 Trip Generation, BEH-02 Destination + Mode Choice, BEH-03 Traffic Simulation.
    - *Post-opening model:* AFTER-01 (actual vs predicted, drift), currently listed in `02` under Beyond POC / later.
    - **Tags unchanged:** BEH-01 to BEH-03 are still tagged "Beyond POC / later (infrastructure-only, D-015)" in `02` and the override, and AFTER-01 is still Beyond POC / later in `02`. Those tags stay as they are everywhere else until Joshua says otherwise. AFTER-01 needs the post-opening data flagged as a gap in item 9.
    - **In franchise scope (D-017, 2026-09-26):** BEH-01 to BEH-03 are in franchise scope for this note and `09`. Their tags in `02`, the override and `03` are unchanged.
12. **Existing work and the co-researcher handoff:** Rewrite the co-researcher brief for the franchise scope now, and hand it over only once the CEO confirms. The rewrite is a separate draft, `09-co-researcher-brief-franchise-DRAFT.md`. `06`, the `handoff/` pack and the cover message stay unchanged and on hold under D-013. The infrastructure notes (`01`–`05`) keep their D-015 labels. Joshua has not said what happens to them after confirmation.
    - **Handover (D-017, 2026-09-26):** finish the open items in `09`, then hand it over. This replaces the D-013 hold for the franchise brief. `09` now has its own franchise reading list (D-018) and a draft cover message that Joshua edits and sends himself (D-019).

## Open questions
- **Cost and return definition:** did the CEO answer question 2, or is it Joshua's call?
- **Infrastructure notes after confirmation:** keep as Beyond POC / later, archive, or rewrite? Not decided.
