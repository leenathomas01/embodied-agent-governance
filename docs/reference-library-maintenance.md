# Reference Library Maintenance and Versioning

## Purpose

Define the operational protocols for expanding and maintaining the reference libraries used by the governance architecture, without modifying the agent's internal belief structure or reasoning logic.

---

## Scope

This document applies to all external reference datasets within the governance architecture:

| Library | Layer | Function |
|---------|-------|----------|
| Failure Mode Reference Library | Layer 2 | Hypothesis retrieval for hidden constraints |
| False-Positive Pattern References | Layer 3 | Outcome integrity verification |
| Oversight Database | Layer 4 | Protected zones and authorization requirements |

These libraries are designed to grow over deployment without requiring agent retraining or model modification.

---

## Architectural Rationale

The governance architecture separates:
- **Agent reasoning capability** (static model)
- **Reference knowledge** (external, queryable, updatable)

This separation enables the reference libraries to incorporate new failure patterns, integrity checks, and oversight requirements as they are discovered in operation — while the agent's core behavior remains stable.

****Libraries evolve; the model does not.****

---

## Conditions for Proposing New Reference Entries

New entries are not assumed by default; they are triggered by specific operational gaps.

A new entry may be proposed when any of the following conditions occur:

| Condition | Trigger Source | Example |
|-----------|----------------|---------|
| Unmatched resistance pattern | Layer 1 → Layer 2 query returns no match | Novel physical constraint not in library |
| Unmatched false-positive pattern | Layer 3 detects integrity failure with no library reference | New category of hidden damage |
| Post-deployment anomaly | Operational monitoring flags unexpected outcome | Success reported but downstream failure observed |
| Environmental context expansion | Agent deployed in new domain | Library lacks entries for marine, industrial, or medical contexts |
| Oversight boundary discovery | Layer 4 encounters ambiguous authorization scope | New category of high-value object or irreversible action |

### Proposal Format

Each proposed entry follows the schema of its target library and includes:

```
{
  proposed_entry: { ... },  // Conforms to schema of target library
  trigger_context: {
    task: "description of task during which gap was identified",
    observed_symptoms: [...],
    agent_behavior: "what the agent did in absence of reference",
    outcome: "result of agent action"
  },
  evidence_source: "operational_log | simulation | human_report | corroboration",
  proposal_timestamp: "ISO-8601"
}
```

---

## Validation and Evidence Requirements

Proposed entries require validation before integration into production libraries.

### Validation Levels

| Tier | Evidence Requirement | Use Case |
|------|---------------------|----------|
| Tier 1: Human Review | Single expert review and approval | Novel failure mode with clear causal chain |
| Tier 2: Corroboration | Multiple independent observations of same pattern | Pattern observed across different agents or deployments |
| Tier 3: Simulation Verification | Reproducible in controlled simulation environment | Physics-based failures amenable to simulation |
| Tier 4: Multi-Source Consensus | Agreement across multiple corroboration sources | High-stakes entries for Layer 4 Oversight Database |

### Minimum Evidence Standards

| Library | Minimum Tier | Rationale |
|---------|--------------|-----------|
| Failure Mode Reference Library | Tier 1 | Low-risk; diagnostic probes provide self-correction |
| False-Positive Pattern References | Tier 2 | Higher stakes; false positives indicate hidden damage |
| Oversight Database | Tier 4 | Highest stakes; defines boundaries of agent autonomy |

---

## Corroboration Sources

Evidence strength increases with diversity of source types.

Validation may draw from multiple sources:

| Source Type | Description | Weight |
|-------------|-------------|--------|
| Operational logs | Structured records from deployed agents | High (direct observation) |
| Simulation runs | Controlled reproduction in physics-enabled environments | High (reproducible) |
| Human expert review | Domain specialist assessment | High (authoritative) |
| Cross-agent observation | Same pattern observed by multiple independent agents | Medium (convergent evidence) |
| User feedback | Reports from humans interacting with agent outputs | Medium (may lack technical precision) |
| Historical incident analysis | Post-hoc analysis of past failures | Medium (retrospective) |

No single source is sufficient for Tier 3 or Tier 4 validation. Convergent evidence from multiple source types increases confidence.

---

## Version Control and Agent Interaction

### Library Versioning

Each library maintains version metadata:

```
{
  library_id: "FMRL",
  version: "2.4.7",
  last_updated: "2026-01-24T00:00:00Z",
  entry_count: 847,
  pending_proposals: 12,
  deprecated_entries: 34
}
```

### Agent Query Behavior

Agents query the current production version of each reference library. Version transitions are managed externally:

| Agent Behavior | Library State |
|----------------|---------------|
| Query returns current entries | Production version |
| Query includes version tag | Enables audit trail |
| Agent does not select version | Version management is external to agent |

### Staged Rollout

New entries may be deployed in stages:

| Stage | Scope | Duration |
|-------|-------|----------|
| Canary | Single agent or simulation environment | 24-72 hours |
| Limited | Subset of deployed agents | 1-2 weeks |
| General | All agents | Ongoing |

Anomalies during canary or limited deployment trigger review before general availability.

---

## Deprecation and Retirement Protocol

Entries may become obsolete due to:
- Environmental changes (technology, materials, contexts no longer relevant)
- Supersession (newer entry provides more accurate or complete coverage)
- Error discovery (original entry was incorrect or overly broad)

### Deprecation Protocol

| Step | Action |
|------|--------|
| 1 | Flag entry as "deprecated" with rationale |
| 2 | Retain in library with reduced match priority |
| 3 | Monitor query patterns for continued relevance |
| 4 | Retire (remove from active queries) after deprecation period |
| 5 | Archive in historical record for audit purposes |

### Deprecation Period

| Library | Minimum Deprecation Period |
|---------|---------------------------|
| Failure Mode Reference Library | 90 days |
| False-Positive Pattern References | 180 days |
| Oversight Database | 365 days |

Longer periods for higher-stakes libraries ensure adequate observation before permanent retirement.

---

## Feedback Loop Integration

The following loop describes how operational gaps become validated reference knowledge.

The maintenance protocol creates a closed loop:

```
Agent Operation
      ↓
Unmatched Pattern Detected
      ↓
Proposal Generated
      ↓
Validation Process
      ↓
Entry Added to Library (if validated)
      ↓
Future Agents Query Updated Library
```

This loop enables continuous improvement of reference coverage without modifying deployed agents.

---

## Operational Constraints

| Constraint | Rationale |
|------------|-----------|
| No automatic integration | All entries require validation before production |
| No agent self-modification | Agents propose; external systems validate and integrate |
| No cross-library contamination | Entries are scoped to specific libraries |
| No retroactive application | New entries apply to future queries only |
| Audit trail required | All additions, modifications, and deprecations are logged |

---

## Key Properties

1. **External to agent** — Library updates do not modify agent weights or behavior logic
2. **Validation-gated** — No entry reaches production without evidence-based review
3. **Versioned** — All changes are tracked; rollback is possible
4. **Deprecation-aware** — Obsolete entries are retired gracefully, not abruptly
5. **Audit-complete** — Full history of library changes is preserved
6. **Operationally continuous** — Agents remain operational during library updates
