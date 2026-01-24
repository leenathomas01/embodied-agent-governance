# Layer 3: Outcome Integrity Check

## Purpose

Verify that task completion corresponds to genuine outcome success, detecting cases where surface metrics indicate success while underlying damage may have occurred.

---

## The Problem It Solves

Layers 0–2 address failures where the environment resists the agent's actions. Layer 3 addresses a distinct failure class: **false positives**.

A false positive occurs when:
- The task completes according to defined criteria
- The outcome satisfies surface metrics
- Hidden damage or degradation has nonetheless occurred

Without explicit outcome verification, agents conflate task completion with task success.

---

## Architectural Rationale

Completion criteria and integrity criteria are distinct:

| Criterion Type | Definition | Example |
|----------------|------------|---------|
| **Completion** | Did the specified action execute? | Temperature reached 100°C |
| **Integrity** | Is the outcome suitable for its intended purpose? | Output is consumable/usable |

An agent optimizing for completion alone will report success in scenarios where the outcome is unusable, damaged, or degraded.

****Outcome integrity requires explicit verification independent of task completion.****

---

## Trigger Conditions

The check is not universal; it is conditionally activated based on risk signals.

The Outcome Integrity Check activates when task completion occurs AND any of the following conditions apply:

| Condition | Rationale |
|-----------|-----------|
| Action was irreversible | No opportunity for correction post-completion |
| Material is flagged as high-value or fragile | Damage carries elevated cost |
| Method applied was aggressive or non-standard | Higher probability of unintended effects |
| Output quality cannot be immediately verified | Delayed manifestation of damage |
| Time between action and visible consequence is long | Latent failure risk |

For routine, reversible, low-value tasks, the check may be lightweight or bypassed.

---

## Verification Protocol

### Step 1: Define Integrity Criteria (Pre-Execution)

Before execution, establish:

```
{
  task: "heat liquid to boiling",
  completion_criteria: {
    temperature_target: "100°C",
    duration: "sustained for 30 seconds"
  },
  integrity_criteria: {
    texture: "uniform, no separation",
    usability: "suitable for intended consumption/application",
    material_state: "no degradation indicators"
  }
}
```

### Step 2: Execute Task

Proceed with standard execution through Layers 0–2 as needed.

### Step 3: Verify Integrity (Post-Task)

After completion criteria are satisfied:

1. Evaluate each integrity criterion independently
2. Flag any criterion that fails validation
3. Determine overall integrity status

```
{
  completion_status: "SUCCESS",
  integrity_status: "FAILED",
  failed_criteria: ["texture: separation detected"],
  recommendation: "discard output; verify input material quality"
}
```

### Step 4: Report Accurately

The agent reports both completion status and integrity status. A task may be:

| Completion | Integrity | Status |
|------------|-----------|--------|
| Success | Success | Full success |
| Success | Failed | **False positive** — task completed but outcome unusable |
| Failed | N/A | Task failure (handled by Layers 0–2) |

---

## False Positive Categories

The following categories illustrate common integrity failures masked by apparent task success.

Common patterns where completion masks failure:

| Category | Surface Success | Hidden Failure |
|----------|-----------------|----------------|
| Overcleaning | Object appears cleaner | Protective coating or patina removed |
| Overtightening | Fastener is secure | Threads stripped; will fail under load |
| Aggressive repair | Function restored | Historical, aesthetic, or structural integrity compromised |
| Thermal success | Target temperature reached | Material state degraded (curdling, crystallization, denaturation) |
| Chemical success | Reaction completed | Substrate damaged by reagent |

These patterns should be encoded in the Failure Mode Reference Library (Layer 2) with `category: FalsePositive` for cross-reference.

---

## Integration with Layer 2

Layer 2 should include false-positive entries that are queryable during post-task verification:

| Field | Example Value |
|-------|---------------|
| `failure_id` | FP-003 |
| `category` | FalsePositive |
| `surface_symptom` | Task completed successfully; output appears normal |
| `hidden_reality` | Irreversible damage occurred; not immediately visible |
| `diagnostic_probe` | Secondary inspection; material property test; expert consultation |
| `resolution_path` | If damage confirmed, do not deliver output; report failure despite completion |
| `risk_level` | High (damage already incurred) |

False-positive patterns identified during outcome verification may result in new entries being proposed to the Failure Mode Reference Library under the maintenance protocol.

See: [Reference Library Maintenance and Versioning](reference-library-maintenance.md)

---

## Cross-Layer Interaction

| Condition | Action |
|-----------|--------|
| Task completes with irreversible action on high-value object | Mandatory integrity check before reporting success |
| Integrity check fails | Report false positive; do not deliver output; log for analysis |
| Integrity cannot be verified (insufficient sensors/data) | Report completion with "integrity unverified" flag; escalate to Layer 4 if high-stakes |
| Integrity check passes | Report full success; log outcome |

---

## Key Properties

1. **Post-completion activation** — Operates after task execution, not during
2. **Criterion separation** — Distinguishes completion metrics from integrity metrics
3. **Explicit verification** — Does not assume success from completion
4. **False-positive detection** — Primary function is catching hidden damage
5. **Asymmetric application** — Verification intensity scales with stakes, not task complexity
6. **Layer 2 integration** — References known false-positive patterns from Failure Mode Reference Library
7. **Epistemically cautious** — Acknowledges that success signals may be misleading
