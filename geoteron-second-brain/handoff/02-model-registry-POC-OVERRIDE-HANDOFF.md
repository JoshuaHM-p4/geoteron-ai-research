> **Name-safe handoff copy** (2026-09-26). Agency and project-brand names removed per the lead researcher's decision. The internal original in the parent folder remains the source and may be updated; if they differ, the original wins.

# Model registry — POC priority override
Date: 2026-09-19  
Parent: 02-model-registry.md  
Reason: Freeze tightened to coworker review (Beh + Econ, A vs B)  
Updated: 2026-09-26 (D-010: UNC-01 must-ship, cost/ROI only)

## Scope status (D-015, 2026-09-26)
A franchise scope has been reported but is not confirmed (draft in `08-new-scope-franchise-draft.md`). The locked freeze (`03`) is unchanged. Items below are infrastructure-only and are now **Beyond POC / later**. Nothing was deleted. Everything not listed stays as it is and could carry over to the franchise scope (not confirmed).

**Beyond POC / later (infrastructure-only):**
- BEH-01, BEH-02, BEH-03 wherever they appear in the lists below (P0 list, must-ship list, confidence-label list).
- DIAG-01 traffic-side one-lever-at-a-time sweeps over L01, L02, L03, L09.
- ECO-FIN-01 infrastructure inputs (blueprint quantities, L09, infrastructure unit costs). UNC-01 cost ranges, the confidence rule and the cost side of DIAG-01 could carry over.

---

## Still P0 for POC demo
BASE-01, BEH-01, BEH-02, BEH-03, ECO-FIN-01, DIAG-01, **UNC-01 (cost/ROI only, D-010)**  
LLM-01 before external eng review

## Demoted from decision-critical (was P0)
ENV-01, ENV-02, ENV-03 → **stretch / appendix only**  
PRES-01 → optional (static A vs B is enough for v1)

## Promoted to must-ship (2026-09-26)
UNC-01 → **must-ship (D-010, freeze §8–§9 updated by D-011)**, limited to ECO-FIN-01 cost/ROI ranges; DIAG-01 cost-side sensitivity depends on its draws (D-006). High/Medium/Low bands remain the locked UX.

## Uncertainty outputs (D-005, 2026-09-26)
- **Every must-ship model** (BASE-01, BEH-01, BEH-02, BEH-03, ECO-FIN-01, UNC-01, DIAG-01, LLM-01) adds a **confidence label output: High / Medium / Low**, with a one-line reason (data quality, calibration, or assumption).
- **UNC-01 (Monte Carlo) runs on ECO-FIN-01 only**: P10/P50/P90 ranges on unit costs and schedule, shown alongside the band. Traffic models (BEH-*) get labels only, no Monte Carlo draws.
- **DIAG-01 sensitivity inputs (D-006):** traffic side (BEH-*) uses **one-lever-at-a-time sweeps** over the active levers L01, L02, L03, L09; cost side (ECO-FIN-01) uses the **UNC-01 Monte Carlo draws**.
- Supersedes the parent registry's "UNC-01 intervals on every KPI" line for the POC.

## Unchanged
ECOL-* blocked · SOC-* not decision · DURING/AFTER/CAUSAL P2 · no GNN
