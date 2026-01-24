# Layer 0: Imperfect Environment Prior

## Purpose

Establish a baseline assumption that the operating environment is imperfect by default.

---

## The Problem It Solves

Agents typically assume "laboratory conditions" — that tools work correctly, materials are intact, surfaces behave predictably, and objects function as designed.

This assumption rarely holds in real-world environments.

The result: agents apply excessive force, misinterpret resistance as internal failure, and damage equipment or materials before recognizing environmental imperfection.

---

## The Operating Prior

**Assume all environments are "Class B" by default.**

| Class | Definition | Agent Behavior |
|-------|------------|----------------|
| A | Laboratory-perfect | Full confidence in tools and materials |
| B | Real-world imperfect | Cautious initial engagement; expect minor friction |
| C | Degraded | Heightened caution; verify before applying force |
| D | Hostile/Broken | Minimal autonomy; request oversight |

Class B is the rational default for any real-world deployment.

Class B represents the statistically dominant condition for real-world deployment.

---

## Implementation

### Initial Interaction Protocol

On first interaction with any physical object or tool:

1. **Apply cautious force** — no more than 25% of maximum available torque/pressure
2. **Observe response** — does behavior match expectation?
3. **Classify dynamically** — upgrade to Class A if response is perfect; downgrade to Class C if anomalies detected

### Why 25%?

This value is illustrative rather than prescriptive.

This threshold allows:
- Detection of stuck, jammed, or degraded objects before damage
- Sufficient force to complete normal operations on functional equipment
- A safety margin that prevents catastrophic first-interaction failures

The specific percentage is tunable; the principle is not.

---

## The Bayesian Framing

This is Occam's Razor applied to physical environments:

> The simplest accurate assumption is that the environment contains minor imperfections.

Assuming perfection (Class A) requires evidence.  
Assuming imperfection (Class B) is the null hypothesis.

---

## Interaction with Other Layers

| If... | Then... |
|-------|---------|
| Class B interaction succeeds normally | Continue; optionally upgrade to Class A for this object |
| Class B interaction shows resistance | Trigger Layer 1 (Doubt Escalation Loop) |
| Multiple Class C detections in environment | Consider systemic environmental degradation |

---

## Example

**Task:** Open a drawer

**Class A behavior (wrong default):**  
Pull with normal force. If stuck, pull harder. Risk: break handle, damage frame.

**Class B behavior (correct default):**  
Pull with cautious force. Observe response. If 0mm displacement at 25% force, do not escalate force — escalate doubt instead. Trigger Layer 1.

---

## Key Insight

The prior is not pessimism. It is calibration.

An agent that assumes Class B will:
- Complete normal tasks with minimal overhead
- Detect anomalies before causing damage
- Avoid the "stupid persistence" failure mode

The cost of Class B assumption on a Class A world: negligible (slightly slower first interaction).  
The cost of Class A assumption on a Class B world: potential damage, wasted effort, incorrect learning.

The asymmetry strongly favors Class B as the default.
