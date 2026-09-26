# Coworker POC review vs our freeze
Source: independent review of GEOTERON POC (same product, not another topic)
Our docs: 01-map · 02-registry · 03-poc-decision-freeze
Updated: 2026-09-19

## Scope status (D-015, 2026-09-26)
A franchise scope has been reported but is not confirmed (draft in `08-new-scope-franchise-draft.md`). The locked freeze (`03`) is unchanged. Items below are infrastructure-only and are now **Beyond POC / later**. Nothing was deleted. Everything not listed stays as it is and could carry over to the franchise scope (not confirmed).

**Beyond POC / later (infrastructure-only):**
- This whole note, as a historical review of the infrastructure POC.

**Could carry over:** its principles (provenance on every number, no bare-percentage confidence, keep the POC thin).

---

## What it is
Critical scope review of GEOTERON Q4 2026 POC — not a separate product plan.

## Commons (already aligned)

| Review point | Our freeze / registry |
|--------------|----------------------|
| Five dimensions + Env ≠ Ecol as end-state vision | Kept as product framing |
| Click-Why provenance is the trust moat | DIAG-01 + LLM-01 + model cards |
| Buyer (developer) ≠ validation partner (gov) | Already in product doc |
| Behavioral + Economic most tractable | BEH-* + ECO-FIN-01 are P0 |
| Ecological hardest / fabrication risk without ground truth | ECOL-* blocked on ECOL-00; not scored in POC |
| During/After cannot be real in Q4 POC | Explicit non-goals; P2 stubs only |
| Numeric “68% confidence” is reputational risk | UNC-01 → P10/P50/P90; freeze rejects vague confidence % |
| Don’t staple five shallow toys | Six engines; cut list in registry |

## Tensions (review is stricter than our freeze)

> **Historical table.** The "Our freeze" column shows the **old, replaced freeze** (before 2026-09-19). The current freeze (`03-poc-decision-freeze.md`) already adopts the review: Behavioral + Economic only, A vs B only, anonymized external name, qualitative bands.

| Topic | Our freeze (old, replaced) | Coworker review | Recommendation |
|-------|------------|-----------------|----------------|
| Dimensions in POC | Behavioral + Environmental + Economic (+ Societal P1) | **Only Behavioral + Economic**; Env/Ecol → placeholder slide | Consider demoting ENV to P1 *or* keep only ENV-01 water as thin third |
| Scenarios | Min A,B,C · catalog A–E | **Only A vs B** | Tighten min run to A vs B; keep C as stretch |
| Site branding | NCC / Pax Silica corridor named | **Generic/anonymized site first**; no Pax Silica in external materials until BCDA contact | Split: internal AOI can stay Capas–Bamban–Mabalacat; **external pitch name = anonymized** until partner conversation |
| Uncertainty UX | Monte Carlo intervals | Qualitative high/med/low tied to assumption source | Compatible: show P10/P50/P90 *and* a data-quality band (blueprint-direct vs national-extrapolated) |
| Verdict on current plan | Executable if cut to P0 stack | “Not fundable as written” if full 5-dim + 5-scenario + confidence UI | Agree: full original vision ≠ POC |

## Net takeaway
Same product, same risks. Review validates our cuts (ecology deferred, During/After out, no fake confidence) and pushes further: **even thinner POC** (2 dims, 2 scenarios, quiet site branding).

## Proposed alignment patch (if accepted)
1. POC scorecard = Behavioral (BEH-03) + Economic (ECO-FIN-01) only for “decision.”
2. ENV-01/02/03 → “appendix / stretch” not decision objectives.
3. Minimum scenarios = A vs B only; C/D/E = post-demo.
4. External materials: “Central Luzon industrial district POC” until BCDA engagement.
5. Uncertainty UI: intervals + qualitative assumption band.
