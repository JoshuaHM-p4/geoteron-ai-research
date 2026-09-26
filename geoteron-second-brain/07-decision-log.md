# 07 — Decision Log (GEOTERON POC)

**Date started:** 2026-09-26
**Status:** Active
**Purpose:** One place to record each decision the lead researcher makes, with the options considered and what changed in the notes.

---

## D-001 — Co-researcher handoff naming (2026-09-26)
- **Decision:** The co-researcher counts as **internal**, but the handoff brief and its reading list are made **name-safe** before handoff.
- **Options considered:** Name-safe pack (chosen, with internal status) / Scrub the brief only / Internal, send as is.
- **Why:** Briefs and reading lists tend to get forwarded; the freeze requires "Central Luzon industrial district POC" and no agency or project-brand names in anything that could leave the team (`03-poc-decision-freeze.md`, external materials rule).
- **Changes made:**
  - `06-co-researcher-brief.md` scrubbed of agency, brand, and landmark-city names; its reading list now points to the name-safe copies.
  - Name-safe copies created in `handoff/` for `02-model-registry-POC-OVERRIDE`, `03-poc-decision-freeze`, `04-coworker-poc-review-overlap`, and `05-base-01-ingest-checklist`. The freeze (03) was added because it is on the reading list and names both.
  - At the time of D-001 the originals 02–05 were unchanged. Later decisions (D-002 onward) edited `02-model-registry-POC-OVERRIDE.md` and `05-base-01-ingest-checklist.md` together with their handoff copies. If a handoff copy and its original differ, the original wins.

---

## D-002 — FOI request timing (2026-09-26)
- **Decision:** Build a **labeled stand-in (synthetic) blueprint now** and **hold** the FOI request for the master plan until after the quiet validation review.
- **Options considered:** Stand-in now, request later (chosen) / Both now / Request now and wait.
- **Why:** The freeze's partner path is build, then quiet validation with one engineering firm or LGU, then approach the development authority (`03-poc-decision-freeze.md`, partner path). A day-one request contradicted that.
- **Changes made:**
  - `05-base-01-ingest-checklist.md` step 7.1 (and its handoff copy) now says: build the stand-in blueprint now, hold the FOI request.
  - `06-co-researcher-brief.md`: the co-researcher researches how the FOI process works and what is already public, but files nothing.

---

## D-003 — Co-researcher tool access (2026-09-26)
- **Decision:** **Spreadsheet only for now.** He reports loading notes in the source register but does not touch the database.
- **Options considered:** Spreadsheet only (chosen) / Spreadsheet plus read access later / Full database access.
- **Why:** The database is not set up yet (step 0 of `05-base-01-ingest-checklist.md` is still open), and the brief's main deliverable is a spreadsheet.
- **Changes made:** `06-co-researcher-brief.md` ground rules now include a Tools line.

---

## D-004 — Co-researcher check-in schedule (2026-09-26)
- **Decision:** One **early read-out after week 1**, then check in **only when he flags a blocker**. He is responsible for flagging blockers himself.
- **Options considered:** Weekly / Twice a week / Week-1 read-out, then as needed (chosen).
- **Changes made:** `06-co-researcher-brief.md` check-ins section rewritten to match.

---

## D-005 — How must-ship models show uncertainty (2026-09-26)
- **Decision:** Every must-ship model carries a **High / Medium / Low confidence label**. **Monte Carlo ranges (UNC-01) on the cost/ROI model (ECO-FIN-01) only.**
- **Options considered:** Labels plus simple sweeps / Labels plus Monte Carlo on cost only (chosen) / Make Monte Carlo must-ship again.
- **Changes made:** `02-model-registry-POC-OVERRIDE.md` and its handoff copy gained an "Uncertainty outputs" section.
- **Knock-on:** DIAG-01 sensitivity needs a traffic-side input other than Monte Carlo draws (open below).

---

## D-006 — Sensitivity analysis inputs (2026-09-26)
- **Decision:** DIAG-01 runs **one-lever-at-a-time sweeps** on the traffic side and uses the **Monte Carlo draws** on the cost side.
- **Options considered:** One-lever-at-a-time sweeps (chosen for traffic) / Swap A and B inputs / Both.
- **Changes made:** `02-model-registry-POC-OVERRIDE.md` and its handoff copy updated; resolves the D-005 knock-on.

---

## D-007 — Freight trip data for scenario B (2026-09-26)
- **Decision:** Add a freight item to the ingest checklist **now as a Low-confidence placeholder**; the co-researcher's modeling-priors research (brief Track D) fills it in later.
- **Options considered:** Placeholder now, research fills it (chosen) / Move freight research to week 1 / Wait for the research.
- **Changes made:** Row 6.4 added to `05-base-01-ingest-checklist.md` and its handoff copy.

---

## D-008 — Old analytics map and main registry (2026-09-26)
- **Decision:** **Rewrite 01 and 02 now** to match the locked freeze; keep the long-term vision in a section labeled "Beyond POC / later".
- **Options considered:** Banner / Banner now, rewrite after the POC / Rewrite now (chosen).
- **Changes made:** `01-ml-llm-analytics-map.md` and `02-model-registry.md` rewritten (Behavioral + Economic, A vs B, L01/L02/L03/L09, O1/O4, High/Medium/Low bands; 02 now incorporates the override). Old versions backed up outside the notes folder.
- **Audit fixes applied with this decision (already settled by earlier decisions):** "day 0" FOI line in `05` + handoff now holds the request per D-002; D-002 reference restored in handoff row 7.1; handoff/04 Tensions column relabeled "Our freeze (old, replaced)"; `coverage_pct: 99` labeled as an example; D-001 note corrected; WorldPop year (2020 estimate, DOI 10.5258/SOTON/WP00645) and Copernicus DEM link/year (OpenTopography, 2021, DOI 10.5069/G9028PQB) added.

## D-009 — Unsourced checklist pass/fail limits (2026-09-26)
- **Decision:** The four limits (2–5 km AOI buffer, row 1.3; coverage ≥ 95%, ≤ 1 major disconnected component, population within 20% of Admin3 sum, Phase 8) are **Joshua's working assumptions**, labeled as unsourced; the **co-researcher tests them**.
- **Options considered:** Label as my assumptions (chosen, plus testing) / Have research source them first / Label now, source later.
- **Changes made:** Labels added in `05` and its handoff copy; testing task added to `06-co-researcher-brief.md` Track B.

## D-010 — Monte Carlo cost ranges (UNC-01) status (2026-09-26)
- **Decision:** **UNC-01 is must-ship, limited to the cost/ROI model (ECO-FIN-01).** Resolves the conflict with D-006, where cost-side sensitivity depends on its draws.
- **Options considered:** Make it must-ship (chosen, cost/ROI only) / Keep optional, add a fallback.
- **Changes made:** `02-model-registry-POC-OVERRIDE.md` + handoff copy and `02-model-registry.md` updated.

## D-011 — Aligning the locked freeze with D-010 (2026-09-26)
- **Decision:** **Unlock the freeze for one change** and edit §8 and §9 so the Monte Carlo cost ranges (UNC-01) are must-ship, cost/ROI model only; then re-lock.
- **Options considered:** Add a freeze amendment file / Edit the freeze directly (chosen) / Reverse the last decision.
- **Changes made:** `03-poc-decision-freeze.md` and its handoff copy: §8 wording, UNC-01 added to the §9 required table and removed from stretch, dated change log added, status shows re-locked. Same pass cleared the remaining audit minors: override + handoff copy now list UNC-01 under "Promoted to must-ship"; original `04` Tensions table labeled "old, replaced"; `02-model-registry.md` gets the WorldPop year/DOI, a single status for ENV-01 to ENV-03 (optional unscored appendix), and UNC-01 inputs limited to unit costs + schedule (discount rate fixed).
- **Wording follow-up (2026-09-26, lead researcher via Chair):** In `03` and its handoff copy, §7 Economic row changed from "P50 + band if Monte Carlo ready" to "P50 + band, with Monte Carlo P10/P50/P90 ranges", and the §8 missing word fixed. In `04` and its handoff copy, the unsourced "thirty models" phrase removed ("Six engines; cut list in registry"). No scope change; freeze stays locked.

---

## D-012 — Week-1 read-out template (2026-09-26)
- **Decision:** Add a short "Week-1 read-out template" section to `06-co-researcher-brief.md`, under the check-ins.
- **Options considered:** Add it to the brief (chosen) / Separate one-pager / Leave it out.
- **Changes made:** New subsection in 06 built from the Synthesizer's draft, using only items the brief already asks for (Track A fields for Phases 1–4, blockers, known gaps, questions for Joshua, week-2 plan). No new scope; name-safe.

---

## D-013 — Co-researcher handoff on hold (2026-09-26)
- **Decision:** **Hold the co-researcher handoff** (the brief `06`, the `handoff/` pack, and the cover message) until the reported scope change is confirmed.
- **Context:** A word-of-mouth scope change was reported on 2026-09-26: franchises only (stores, restaurants, etc.) instead of infrastructure, and a before-and-after construction data scope. The freeze currently limits the POC to the before phase only, so "before and after" would add the after phase. Not yet confirmed in writing.
- **Options considered:** Hold the handoff (chosen) / Send with a caution note / Send as is.
- **Changes made:** None to the freeze or any other note. Freeze stays locked; cover message stays unsent.

---

## D-014 — Recording the reported scope change (2026-09-26)
- **Decision:** **Start a separate new-scope note now** and leave the freeze untouched.
- **Options considered:** Log it as pending and wait for written confirmation / Start a new scope note now (chosen) / Unlock and amend the freeze now.
- **Changes made:** New note `08-new-scope-franchise-draft.md`, marked "Draft, pending written confirmation". It holds only the two known points (franchises only; before and after construction) and lists everything else as open questions. Freeze and all other notes unchanged.

---

## D-015 — Relabel infrastructure-only work (2026-09-26)
- **Decision:** **Relabel infrastructure-only work as "Beyond POC / later" now**, while the freeze stays untouched.
- **Options considered:** Keep it as is and run a read-only impact check / Keep it as is and check nothing yet / Relabel infrastructure-only work now (chosen).
- **Basis:** Scope Guard's read-only impact check against `08` (`/workspace/geoteron-scope/franchise-impact-2026-09-26.md`). The four active levers are industrial floor area, employment density, freight trip share and capex intensity, all infrastructure-only.
- **Changes made:** Added a "Scope status (D-015)" block to `01`, `02`, `02` override, `04`, `05`, `06` and the `handoff/` copies of `02` override, `04` and `05`, listing only the items marked infrastructure-only. Tagged the wholly infrastructure-only sections (`01` §4; `02` Behavioral section, BEH-01 to BEH-03 and PRES-01; `05` Phases 2 and 7 and the optional appendix; `06` context and later tracks). "Could carry over" items unchanged. Nothing deleted. Freeze (`03`, `handoff/03`) and `08` untouched.
- **Side effect:** the locked freeze still describes the infrastructure scope until the new scope is confirmed.

---

## D-016 — How to confirm the franchise scope (2026-09-26)
- **Decision:** **Joshua answers the open questions he already knows first**; a confirmation request then covers the rest.
- **Options considered:** Draft a confirmation request now / Answer what I know first (chosen) / Wait for them to send it.
- **Changes made:** `08` gained an "Answers per Joshua, unconfirmed" section with his answers to questions 1–5 (CEO set the change on Sep 19; Central Luzon area still applies; same project; franchising companies as buyers, validated by the companies, stakeholders and industrial economists or analysts; the POC validates whether a franchise should start at a specific location). Questions 6–12 stay open. Freeze and all other notes unchanged.

---

## Open (pending decision)
- Reported scope change (franchises only; before and after construction): awaiting written confirmation. Draft in `08`.
- Franchise scope questions 6–12 (scenarios, levers, scoring, before/after, data, models, existing work): Joshua answering one at a time; the rest go into a confirmation request.
