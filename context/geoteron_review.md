# GEOTERON POC plan — independent review

Fresh, context-free subagent review of the GEOTERON "infrastructure decision
intelligence" plan (Q4 2026 POC + BCDA/Pax Silica partner strategy).

## What's strong

- The five-dimension framing (Behavioral, Societal, Environmental,
  Ecological, Economic) and the explicit Environmental ≠ Ecological
  distinction is a coherent end-state vision.
- The "click Why" provenance drill-down (dataset, assumption, model
  equation, calibration, confidence interval) is the right instinct for
  earning engineer trust.
- Separating the paying customer (land/infrastructure developer) from the
  validation partner (government agencies) is correct and avoids a common
  early-stage mistake of conflating who pays with who lends credibility.

## Where it overreaches

### 1. Scope realism
A two-person team building spatial analysis, agent-based traffic
simulation, economic modeling, environmental modeling (water, air, carbon,
energy, noise, waste, heat, flood), ecological modeling (habitat,
biodiversity, watershed), scenario generation, an AI orchestration layer, a
confidence-interval engine, and a full provenance UI — across 5 scenarios —
by Q4 2026, is not one POC. It's five separate research projects stapled
together. Two failure points:
- The modeling layer: each "dimension" is its own technical discipline
  with its own data pipeline; stapling all five together for a demo
  produces five shallow, uncalibrated toys instead of one deep, defensible
  one.
- The provenance chain: real versioned datasets and cited model equations
  behind every number is a data-engineering project in itself, usually
  invisible in scope until you try to actually build it.

### 2. The confidence-number claim (biggest reputational risk)
Printing "68%," "74%," "61%" implies calibrated uncertainty — backtested
against real outcomes. With zero historical validation data (the
development doesn't exist yet at the scale being simulated), those numbers
are aesthetic, not statistical. A real engineer who asks "confidence in
what sense — bootstrapped, expert-elicited, model ensemble spread?" and
gets no answer will conclude the whole system is dressed-up guessing.

**Fix:** replace numeric confidence with qualitative bands tied to an
explicit, inspectable assumption ("high confidence: direct from blueprint,"
"low confidence: extrapolated from national average"), or don't claim
confidence at all yet.

### 3. Tractability by dimension
- **Behavioral / Economic** — most tractable. OSM road networks, WorldPop,
  published cost/m² benchmarks, standard trip-generation models all exist
  off the shelf.
- **Environmental** (water, energy demand) — moderately tractable with
  public rasters and utility benchmarks.
- **Ecological** (biodiversity, habitat, ecosystem services) — hardest.
  Needs field survey data or specialist remote-sensing products this team
  almost certainly can't access or license in this timeframe. A "habitat
  affected" number without ground-truth is a fabrication risk, not a
  modeling gap.

### 4. BCDA / Pax Silica framing
Naming a live, presidentially-backed national project (Pax Silica, tied to
New Clark City and the Luzon Economic Corridor) as your "grand challenge"
before any contact with the agency is presumptuous, and risks looking like
freeriding on BCDA's brand for a pitch deck if the numbers are wrong or
unwelcome.

**Credible path (reverse of what's proposed):** build the POC on a generic
or anonymized site first, get one engineering firm or LGU to quietly
validate the methodology, and only then approach BCDA with a working tool
and an invitation to co-validate — not with "Pax Silica" already named in
external materials.

### 5. Before/During/After lifecycle conflation
"During" (construction intelligence) needs live construction telemetry
(IoT, contractor reporting) that doesn't exist yet for any accessible site.
"After" (operational intelligence) needs years of post-occupancy data.
Neither can be more than a mocked-up screen in a Q4 2026 POC. Including
them in POC scope dilutes engineering effort that should go entirely into
"Before."

## Recommended POC scope

- **One dimension pair**: Behavioral (traffic/mobility) + Economic
  (cost/ROI).
- **Two scenarios**: A (Baseline) vs. B (High Industrial Growth).
- **One real or realistic-but-not-politically-loaded site.**
- **"Before" only** — no During/After.
- Cut Environmental and Ecological to a placeholder/future-work slide.
- Replace numeric confidence with a three-tier qualitative label tied to a
  visible assumption.
- Drop the "Pax Silica" name from external materials until there's been a
  real conversation with BCDA.

## Verdict

**Not fundable or executable as written.** The end-state vision and the
customer/partner-separation strategy are sound, but the POC scope is
roughly 5x what two engineers can credibly deliver by Q4 2026, and the
confidence-interval claim is exactly the kind of unbacked precision that
destroys trust with the technical audience (engineers, government
reviewers) this product most needs to win over.

**Single most important change:** cut the POC to one dimension pair
(Behavioral + Economic) for one site and two scenarios, with qualitative
rather than numeric confidence — and drop "Pax Silica" from the pitch until
BCDA has actually been in the room.
