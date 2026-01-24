# Layer 2: Failure Mode Reference Library

## Purpose

Provide an external, queryable reference of common failure patterns encountered in physical environments.

---

## The Problem It Solves

When Layer 1 (Doubt Escalation Loop) reaches Stage 3 (Assumption Audit), the agent requires hypotheses about hidden constraints or environmental conditions.

Without external reference, hypothesis generation depends on:
1. **Inference from first principles** — computationally expensive and unreliable for edge cases
2. **Learned patterns from prior failures** — creates belief volatility risk

The Failure Mode Reference Library provides structured hypothesis retrieval without model modification.

---

## Architectural Rationale

Agents can maintain separation between:
- **Knowledge used** (referenced at query time)
- **Beliefs held** (encoded in model weights)

This separation is architecturally feasible because agents have constant-time access to external indexed storage. Humans lack this capability and must internalize failure knowledge.

**Externalizing failure patterns prevents destabilizing belief updates within the core model.**

**The agent references caution; it does not encode caution.**

---

## Library Schema

Each failure entry captures both observable symptoms and hidden realities in a structured, retrievable format.

Each entry follows a standardized structure:

| Field | Type | Description |
|-------|------|-------------|
| `failure_id` | String | Unique identifier (e.g., `PHYS-042`) |
| `category` | Enum | Classification: Physical / Material / Constraint / Environmental / State |
| `surface_symptom` | String | Observable condition that triggers query |
| `hidden_reality` | String | Underlying cause not directly observable |
| `diagnostic_probe` | String | Low-risk action to confirm or reject hypothesis |
| `resolution_path` | String | Recommended action if hypothesis confirmed |
| `risk_level` | Enum | Low / Medium / High / Requires-Oversight |
| `context_tags` | Array | Environmental or object-type tags for retrieval filtering |

---

## Query Interface

### Query Input

Query construction from Layer 1 Stage 3:

```
{
  observed_symptom: "0mm displacement on pull action",
  object_class: "drawer",
  force_applied: "within Class B parameters",
  environmental_context: ["indoor", "furniture", "wood-frame"]
}
```

### Ranked Retrieval

The library returns ranked hypotheses:

```
[
  { failure_id: "PHYS-042", match_score: 0.87, hypothesis: "internal latch" },
  { failure_id: "PHYS-051", match_score: 0.72, hypothesis: "internal obstruction" },
  { failure_id: "MAT-023", match_score: 0.34, hypothesis: "humidity-swollen wood" }
]
```

### Output Protocol

The agent processes returned hypotheses sequentially based on match score.

For each returned hypothesis (highest match score first):

1. Execute `diagnostic_probe`
2. Evaluate probe result against confirmation criteria
3. If confirmed → apply `resolution_path`
4. If rejected → proceed to next hypothesis
5. If `risk_level` = Requires-Oversight → trigger Layer 4 before probe execution

---

## Example Entries

The following entries illustrate how common physical and material failures are encoded.

### PHYS-042: Internal Latch

| Field | Value |
|-------|-------|
| `failure_id` | PHYS-042 |
| `category` | Constraint |
| `surface_symptom` | 0mm displacement despite adequate force; no visible obstruction |
| `hidden_reality` | Object secured by internal mechanism not externally visible |
| `diagnostic_probe` | Inspect for release mechanism; assess resistance pattern (rigid stop vs. gradual) |
| `resolution_path` | Locate and disengage mechanism; if not found, escalate to Layer 4 |
| `risk_level` | Low (standard objects); High (antique or fragile) |
| `context_tags` | ["drawer", "door", "cabinet", "furniture"] |

### MAT-017: Material Degradation

| Field | Value |
|-------|-------|
| `failure_id` | MAT-017 |
| `category` | Material |
| `surface_symptom` | Tool slippage; fastener rotation without advancement |
| `hidden_reality` | Fastener head stripped; tool worn; corrosion fusion |
| `diagnostic_probe` | Visual inspection of contact surfaces; attempt with alternative tool |
| `resolution_path` | Replace tool; apply appropriate lubricant; escalate if high-value object |
| `risk_level` | Medium (continued force risks further degradation) |
| `context_tags` | ["fastener", "screw", "bolt", "tool-interaction"] |

### STATE-008: Pre-Existing Material Degradation

| Field | Value |
|-------|-------|
| `failure_id` | STATE-008 |
| `category` | State |
| `surface_symptom` | Process completed successfully; output exhibits unexpected properties |
| `hidden_reality` | Input material was degraded prior to processing |
| `diagnostic_probe` | Pre-process material inspection (visual, olfactory if applicable, metadata review) |
| `resolution_path` | Discard output; validate input source; restart with verified materials |
| `risk_level` | Context-dependent (Low for consumables; High for chemicals or pharmaceuticals) |
| `context_tags` | ["liquid", "food", "chemical", "biological", "state-change"] |

---

## Why External Storage Is Required

| Architecture | Failure Pattern Handling | Belief Stability |
|--------------|-------------------------|------------------|
| Learned (weights) | Pattern encoded within the model | Volatile — overcorrection and oscillation risk |
| External (reference) | Pattern retrieved on demand | Stable — model unchanged by failure exposure |

The library is a tool the agent queries, not knowledge the agent internalizes.

Post-task, no residual caution persists. The agent returns to Class B baseline without category-level suspicion.

---

## Population Methods

The library can be populated from multiple sources:

| Source | Method | Coverage |
|--------|--------|----------|
| Expert curation | Engineers document known failure modes | High precision, limited breadth |
| Incident analysis | Real-world agent failures are post-processed | Empirically grounded |
| Crowdsourcing | Humans contribute failure scenarios from experience | High breadth, requires validation |
| Simulation extraction | Failures in physics-enabled environments are cataloged | Scalable, transferable to physical deployment |

Crowdsourced contributions follow the standard schema and undergo validation before inclusion.

---

## Library Maintenance

The Failure Mode Reference Library is not static.

As agents encounter novel failure patterns not represented in the library, entries may be proposed, validated, and versioned according to the Reference Library Maintenance protocol.

See: [Reference Library Maintenance and Versioning](reference-library-maintenance.md)

---

## Interaction with Other Layers

| Condition | Action |
|-----------|--------|
| Layer 1 Stage 3 triggers query | Construct query from observed state; retrieve ranked hypotheses |
| Retrieved entry has `risk_level` = Requires-Oversight | Trigger Layer 4 before executing diagnostic probe |
| No matching entries returned | Log as novel failure; attempt conservative resolution; flag for library addition |
| Resolution succeeds | Log outcome; optionally enrich existing entry with context |
| Resolution fails after all hypotheses exhausted | Escalate to Layer 4; flag for investigation |

---

## Key Properties

1. **Stateless access** — Query does not modify library or model state
2. **Ranked retrieval** — Multiple hypotheses returned in probability order
3. **Disposable context** — Retrieved information does not persist beyond task scope
4. **Extensible schema** — New failure categories can be added without architectural change
5. **Layer integration** — Designed for direct invocation from Layer 1 and handoff to Layer 4
6. **Architecture-agnostic** — Applicable to any embodied agent capable of external query
