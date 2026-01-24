# Architectural Design Principles

## Core Principles

### 1. Separate Intelligence from Governance

The agent's reasoning capability should be independent of the mechanisms that regulate caution, skepticism, and operational boundaries.

**Rationale:** Coupling these creates fragility. An agent that "becomes cautious" through learning is an agent whose caution can be unlearned, overgeneralized, or corrupted by edge cases.

**Implementation:** Governance layers operate alongside the core model, not inside it. They guide behavior without modifying beliefs.

---

### 2. Assume Imperfection by Default

The operating environment should be assumed imperfect (Class B) unless evidence indicates otherwise.

**Rationale:** The cost of assuming perfection in an imperfect world (damage, failure, incorrect learning) exceeds the cost of assuming imperfection in a perfect world (slightly slower operation).

**Implementation:** Layer 0 establishes Class B as the prior. Upgrade to Class A requires explicit evidence.

---

### 3. Doubt is a Feature, Not a Bug

Repeated failure should trigger escalating doubt about assumptions, not escalating force.

**Rationale:** When reality resists, the correct response is rarely increased effort. It is usually improved understanding.

**Implementation:** Layer 1 provides structured doubt escalation from execution → method → assumptions.

---

### 4. Externalize What Can Be Externalized

**Failure patterns, false-positive risks, and oversight boundaries should be stored externally, not learned internally.**

**Rationale:** External storage prevents belief volatility, enables easy updates, and maintains model stability.

**Implementation:** Layers 2, 3, and 4 all reference external databases rather than trained knowledge.

---

### 5. Success Requires Verification

Task completion is not equivalent to task success. Outcomes must be verified against integrity criteria, not just completion criteria.

**Rationale:** False positives — tasks that appear successful while causing hidden damage — are a distinct and dangerous failure mode.

**Implementation:** Layer 3 provides explicit outcome integrity checking, especially for irreversible actions.

---

### 6. Competence Must Be Bounded by Permission

Agent capability should not exceed agent authority. Certain domains require human oversight regardless of agent confidence.

**Rationale:** A capable agent that ignores boundaries is more dangerous than an incapable one. Safety requires knowing where not to be clever.

**Implementation:** Layer 4 establishes hard gates where autonomy pauses for human decision.

---

### 7. Asymmetric Caution

Invest verification effort proportional to potential harm, not proportional to task complexity.

**Rationale:** A simple task (cleaning a painting) can cause irreversible damage. A complex task (assembling furniture) may be fully reversible. Caution should follow consequence, not difficulty.

**Implementation:** All layers modulate their intensity based on irreversibility and value, not task complexity.

---

### 8. Externalized Knowledge Accretion

Operational knowledge should accumulate in versioned reference systems rather than through modification of the agent's internal belief structure.

**Rationale:** Direct internalization of lessons risks belief volatility. External reference systems can grow indefinitely without destabilizing the agent.

**Implementation:** Reference libraries are maintained and versioned externally. See: [Reference Library Maintenance and Versioning](reference-library-maintenance.md)

---

## Common Anti-Patterns

### Don't: Train the Agent to Be Cautious

This creates belief volatility. Caution should be queried, not held.

### Don't: Assume Success from Completion

Completion metrics are incomplete. Verify integrity explicitly.

### Don't: Apply Uniform Governance

Governance intensity should scale with stakes. Routine tasks need lightweight checks; irreversible actions on valuable objects need full protocol.

### Don't: Let the Agent Resolve Value Conflicts

When multiple valid paths exist with different value implications, present options to humans. The agent should inform, not decide.

### Don't: Treat Oversight as Failure

Triggering the oversight gate is correct behavior, not a limitation. The agent successfully recognized the boundary of its authority.

---

## The Goal

An agent governed by these principles will be:

- **Capable** — optimistic by default, able to complete tasks efficiently
- **Sane** — able to recognize when assumptions require revision
- **Stable** — resistant to belief volatility from edge cases
- **Bounded** — respectful of domains requiring human judgment
- **Verifiable** — explicit about what it checked and what remains uncertain

This is not AGI. This is not alignment. This is **operational governance** — the minimum viable sanity required for agents interacting with imperfect physical environments.
