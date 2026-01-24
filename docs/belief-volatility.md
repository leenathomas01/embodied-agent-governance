# Architectural Rationale: Belief Volatility

## The Problem

When agents learn directly from failure, they risk destabilizing their core world model.

This manifests in two failure modes:

### Failure Mode 1: Overcorrection

One failure generalizes too broadly.

**Example:**  
Agent encounters a jammed drawer. It learns: "Drawers may be jammed."

Next interaction: Agent approaches all drawers with excessive caution, performs unnecessary diagnostics, operates slower than needed.

The lesson from one instance propagates incorrectly to the entire category.

### Failure Mode 2: Oscillation

Agent swings between competing beliefs based on recent experience.

**Example:**  
- Day 1: Drawer jams → Agent becomes cautious about drawers
- Day 2: Ten drawers open normally → Agent becomes confident
- Day 3: Another jam → Agent becomes cautious again

The agent never stabilizes. Its behavior depends on recency, not reality.

---

## Why This Happens

Traditional coupling of experience and belief:

```
Experience → Update model → Behavior changes
```

This works well when:
- Experiences are representative
- Categories are well-defined
- Feedback is immediate and accurate

It fails when:
- Experiences are edge cases
- Category boundaries are fuzzy
- Feedback is noisy or delayed

In physical environments, edge cases are common, categories are fuzzy, and feedback is often ambiguous.

The result: **belief volatility** — a model that shifts unpredictably based on recent experience.

---

## The Alternative: Externalized Skepticism

Decoupled experience and belief in this architecture:

```
Experience → Reference library updated → Model unchanged → Behavior guided by lookup
```

The agent's core model remains stable.

**Caution is not a belief state; it is a query result.**

### Key Properties

1. **Contextual activation** — Skepticism activates only when queried, not persistently
2. **Scoped application** — Lessons apply to specific contexts, not categories
3. **Disposable caution** — After task completion, the agent "closes the book"
4. **No generalization risk** — One jammed drawer doesn't teach "drawers are unreliable"

---

## Comparison

| Aspect | Learned Beliefs | External Reference |
|--------|-----------------|-------------------|
| Model stability | Volatile | Stable |
| Lesson scope | Category-wide | Instance-specific |
| Persistence | Permanent until unlearned | Active only during query |
| Correction cost | Requires retraining | Just update database |
| Generalization risk | High | None |

---

## Why This Is Architecturally Feasible for Agents

Humans must internalize lessons because we lack indexed external memory at query time. When we need caution, it must already be "in us."

Agents have no such constraint. They can:
- Query external databases in milliseconds
- Reference arbitrarily large knowledge bases
- Maintain perfect separation between "knowledge I use" and "beliefs I hold"

This is an architectural advantage that should be exploited, not ignored.

---

## Implications for Design

1. **Failure Mode Reference Library** (Layer 2) should be external and queryable, not trained into the model

2. **Outcome Integrity Check** (Layer 3) should reference known false-positive patterns, not learn to be suspicious

3. **Oversight Database** (Layer 4) should be a lookup table, not a learned boundary

4. **Updates flow to libraries**, not to the model. When a new failure mode is discovered, add it to the database — don't retrain the agent.

---

## Knowledge Accretion Over Time

The use of versioned, external reference libraries allows the system to accumulate operational knowledge over time without inducing belief volatility in the agent. Lessons are added to the reference system rather than internalized by the model.

See: [Reference Library Maintenance and Versioning](reference-library-maintenance.md)

---

## The Stability Guarantee

With externalized skepticism:

- The agent's baseline behavior remains optimistic (Class B, but not paranoid)
- Caution is available on demand but does not persist
- Lessons are captured without destabilizing the model
- The agent can be corrected by editing a database, not retraining weights

This is not a workaround. It is a fundamentally more stable architecture for agents operating in unpredictable physical environments.
