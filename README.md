# Embodied Agent Governance

A governance pattern for embodied agents operating in imperfect environments.

---

## The Problem

Agents fail in the real world not because of lack of intelligence, but because of:

- Incorrect assumptions about environment quality
- Inability to detect when assumptions are wrong
- Belief volatility from overlearning failures
- Successful outcomes that hide underlying damage
- Applying autonomy in irreversible or high-value zones

These are not intelligence failures. They are **governance failures**.

---

## The Core Insight

Separate the agent's reasoning capability from the mechanisms that regulate caution, skepticism, and operational boundaries.

External reference of failure patterns prevents destabilizing belief updates to the core model. The agent remains capable and optimistic by default; governance layers provide contextual caution on demand.

---

## The Five-Layer Governance Architecture

| Layer | Component | Function |
|-------|-----------|----------|
| 0 | Imperfect Environment Prior | Assume environment is imperfect by default |
| 1 | Doubt Escalation Loop | Recognize and escalate assumption failures |
| 2 | Failure Mode Reference Library | External lookup for common environmental traps |
| 3 | Outcome Integrity Check | Detect successes that mask hidden damage |
| 4 | Irreversibility & Value Gate | Hard stop for high-stakes decisions |

---

## Maintenance of Reference Libraries

The Failure Mode Reference Library, Outcome Integrity references, and Value Gate database are external, versioned datasets.

Their expansion and maintenance are governed by a separate operational protocol that allows reference knowledge to grow over time without modifying the agent's internal belief structure.

See: [Reference Library Maintenance and Versioning](docs/reference-library-maintenance.md)

---

## Architectural Rationale

Most approaches to agent safety focus on alignment, ethics, or capability limitations.

This pattern addresses a different problem: **operational sanity in imperfect physical environments**.

An agent can be perfectly aligned and highly capable and still:
- Strip an antique's protective varnish while "successfully" cleaning it
- Persist in pulling a jammed drawer until hardware breaks
- Learn the wrong lesson from one failure and apply it everywhere

These failures occur because the agent’s belief system is too tightly coupled to its experiences. Every lesson risks destabilizing the whole model.

The solution: **externalize skepticism**. Keep failure knowledge in reference libraries, not learned beliefs.

**The agent queries caution; it does not become cautious.**

---

## Documentation

### Governance Layers
- [Layer 0: Imperfect Environment Prior](docs/layer-0-environment-prior.md)
- [Layer 1: Doubt Escalation Loop](docs/layer-1-doubt-escalation.md)
- [Layer 2: Failure Mode Reference Library](docs/layer-2-failure-modes.md)
- [Layer 3: Outcome Integrity Check](docs/layer-3-outcome-integrity.md)
- [Layer 4: Irreversibility & Value Gate](docs/layer-4-value-gate.md)

### Design Rationale
- [Belief Volatility and Why Externalization Solves It](docs/belief-volatility.md)
- [Design Principles](docs/design-principles.md)
- [Reference Library Maintenance and Versioning](docs/reference-library-maintenance.md)

### Examples
- [Physical Causality: Resource Depletion Mid-Task](examples/physical-causality.md)
- [Latent State: Silent Material Failure](examples/latent-state.md)
- [Hidden Constraints: Invisible Resistance](examples/hidden-constraints.md)
- [Physics Over Intent: Structural Impossibility](examples/physics-over-intent.md)
- [Application: Simulated Environments](examples/simulated-environments.md)

---

## Origin

This architecture emerged from collaborative analysis of embodied agent failure modes across multiple AI systems.

The full origin conversation is available in [Appendix A](appendices/origin-conversation.md).

---

## License

MIT License — open for research, reference, and extension.

---

## Related Work

This repository provides governance patterns for AI agents operating in physical or simulated environments.

**For a complete catalog of related research:**  
📂 [AI Safety & Systems Architecture Research Index](https://github.com/leenathomas01/research-index)

**Thematically related:**
- [Connector OS](https://github.com/leenathomas01/connector-os-trenchcoat) — State-awareness architecture for agents
- [Doctrine of Externalization](https://github.com/leenathomas01/doctrine-of-externalization) — External trust layers
- [The Continuity Problem](https://github.com/leenathomas01/The-Continuity-Problem) — Governance before autonomy

---
