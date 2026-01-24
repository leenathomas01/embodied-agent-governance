# Layer 4: Irreversibility & Value Gate

## Purpose

Establish boundaries where agent autonomy must halt pending human oversight, regardless of agent confidence or capability level.

---

## The Problem It Solves

Layers 0–3 produce a capable, stable, and verification-aware agent. This agent can:
- Operate in imperfect environments (Layer 0)
- Recognize and escalate assumption failures (Layer 1)
- Reference failure patterns without belief destabilization (Layer 2)
- Verify outcome integrity beyond completion metrics (Layer 3)

However, capability without boundaries creates risk. A competent agent applying autonomous action in high-stakes domains may cause harm that correct execution cannot prevent.

****Layer 4 defines where autonomy ends and human authority begins.****

---

## Architectural Rationale for the Gate

Agent competence should not exceed agent authority.

| Property | Without Layer 4 | With Layer 4 |
|----------|-----------------|--------------|
| Decision scope | Unbounded | Bounded by oversight gates |
| High-stakes actions | Autonomous | Require human authorization |
| Value trade-offs | Agent-resolved | Human-resolved |
| Irreversible actions | Executed if confident | Halted pending approval |

The gate does not evaluate whether the agent's decision is correct in principle. It evaluates whether the decision falls within the agent's authorized scope.

---

## Gate Categories

### Category 1: Irreversible Physical Actions

Actions that cannot be undone after execution.

| Examples | Gate Behavior |
|----------|---------------|
| Cutting, drilling, permanent modification | Halt; request authorization |
| Chemical application altering substrate | Halt; request authorization |
| Consumption or destruction of samples | Halt; request authorization |
| Discarding objects | Halt; request authorization |

**Gate rule:** If the action is irreversible and the object value exceeds threshold, halt and request oversight.

### Category 2: High-Value Objects

Objects with value exceeding replacement cost.

| Examples | Gate Behavior |
|----------|---------------|
| Antiques, heirlooms, historical items | Any modification requires oversight |
| Legal documents, contracts, records | Any modification requires oversight |
| Artwork, creative works | Any modification requires oversight |
| Rare or irreplaceable materials | Any modification requires oversight |

**Gate rule:** Any modification to flagged high-value objects requires oversight, regardless of action reversibility.

### Category 3: Strategic Consequences

Decisions affecting goals beyond the immediate task scope.

| Examples | Gate Behavior |
|----------|---------------|
| Time trade-offs impacting scheduled commitments | Present trade-off; await decision |
| Resource consumption affecting future availability | Present trade-off; await decision |
| Actions constraining future options | Present trade-off; await decision |

**Gate rule:** If resolution impacts priority-1 external commitments or long-term resource availability, halt and present alternatives.

### Category 4: Destructive Verification

Situations where confirming a hypothesis requires actions that may cause damage.

| Examples | Gate Behavior |
|----------|---------------|
| Sampling that consumes limited material | Request authorization before probe |
| Probes that risk damage to fragile objects | Request authorization before probe |
| Tests that are themselves irreversible | Request authorization before probe |

**Gate rule:** If diagnostic probe from Layer 1/Layer 2 carries non-trivial damage risk, request oversight before execution.

### Category 5: Value Conflicts

Situations requiring trade-offs between competing valid outcomes.

| Examples | Gate Behavior |
|----------|---------------|
| Functional fix vs. aesthetic preservation | Present options; do not select autonomously |
| Speed vs. thoroughness | Present options; do not select autonomously |
| Task completion vs. resource conservation | Present options; do not select autonomously |

**Gate rule:** If multiple valid resolutions exist with different value implications, present options to human rather than selecting autonomously.

---

## Oversight Database Schema

The gate relies on an external classification database describing protected zones and conditions.

The gate references an external database of protected zones:

| Field | Type | Description |
|-------|------|-------------|
| `zone_id` | String | Unique identifier for protected zone or object class |
| `category` | Enum | Irreversibility / HighValue / Strategic / Verification / Conflict |
| `trigger_condition` | String | Specific actions or contexts that activate the gate |
| `required_oversight` | Enum | User / Expert / Administrator |
| `escalation_path` | String | Behavior if oversight is unavailable |
| `context_tags` | Array | Tags for retrieval filtering |

The Oversight Database is subject to the same maintenance and versioning protocol described in [Reference Library Maintenance and Versioning](reference-library-maintenance.md).

---

## Example Entries

### OVR-001: Antique Objects

| Field | Value |
|-------|-------|
| `zone_id` | OVR-001 |
| `category` | HighValue |
| `trigger_condition` | Any physical modification, cleaning, or repair action |
| `required_oversight` | User (owner) |
| `escalation_path` | Do not proceed; document condition; await instruction |
| `context_tags` | ["antique", "heirloom", "historical", "fragile"] |

### OVR-007: Time-Critical Trade-off

| Field | Value |
|-------|-------|
| `zone_id` | OVR-007 |
| `category` | Strategic |
| `trigger_condition` | Resolution requires >30 minutes AND conflicts with Priority-1 calendar event |
| `required_oversight` | User |
| `escalation_path` | Present trade-off with time estimates; do not resolve autonomously |
| `context_tags` | ["scheduling", "time-sensitive", "commitment"] |

### OVR-012: Chemical Application

| Field | Value |
|-------|-------|
| `zone_id` | OVR-012 |
| `category` | Irreversibility |
| `trigger_condition` | Applying chemical agent to unclassified or high-value surface |
| `required_oversight` | User or Expert |
| `escalation_path` | Use mechanical methods only; flag for material classification |
| `context_tags` | ["chemical", "cleaning", "surface-treatment"] |

---

## Gate Activation Protocol

When any gate condition is detected:

1. **Halt autonomous action** — Do not proceed with planned execution
2. **Construct oversight request** — Include context, proposed action, and identified risk
3. **Present options** — If multiple valid paths exist, enumerate with trade-offs
4. **Await explicit authorization** — Silence or ambiguity must not be interpreted as approval
5. **Log decision** — Record human decision and rationale for future reference

```
{
  gate_triggered: "OVR-001",
  proposed_action: "apply cleaning solution to wooden surface",
  identified_risk: "object flagged as antique; chemical may damage patina",
  options: [
    { action: "proceed with chemical cleaning", risk: "high" },
    { action: "use dry mechanical cleaning only", risk: "low" },
    { action: "defer to professional conservator", risk: "none" }
  ],
  awaiting: "user_authorization"
}
```

---

## Cross-Layer Interaction

| Condition | Action |
|-----------|--------|
| Layer 1 identifies high-value object during doubt escalation | Gate activates; halt before attempting resolution |
| Layer 2 returns entry with `risk_level: Requires-Oversight` | Gate activates; request authorization before diagnostic probe |
| Layer 3 cannot verify integrity on high-value object | Gate activates; request expert verification |
| Gate authorization received | Proceed with authorized action; log approval |
| Gate authorization denied | Abort action; log denial; report to user |

---

## Key Properties

1. **Authority boundary** — Defines scope of autonomous action, not capability
2. **Category-based activation** — Gates trigger on context classification, not confidence level
3. **Human-in-loop requirement** — Certain decisions are structurally reserved for human judgment
4. **Explicit authorization** — Silence does not constitute approval
5. **Option presentation** — Agent informs; human decides
6. **Override authority** — Layer 4 can halt actions approved by Layers 0–3
7. **Architecture-agnostic** — Applicable to any agent system with external query capability
