# Epistemic Clean
*Preserving meaning over time*

---

## What “epistemic” means in software

In this context, *epistemic* refers to **what a system can be said to know** — and how reliably that knowledge can be interpreted by humans and machines over time.

A system is epistemically sound if:

* its concepts are clearly defined
* its distinctions are enforced
* its assumptions are visible
* its symbols retain stable meaning

Epistemic Clean (E-Clean) is the discipline of maintaining these properties throughout the lifetime of a system.

---

## The core problem: semantic drift

Semantic drift occurs when:

* names remain stable, but meaning changes
* rules still execute, but intent is forgotten
* abstractions survive beyond their validity
* documentation describes a system that no longer exists

This drift is gradual and often invisible.

The system continues to compile.  
Tests may still pass.  
Delivery continues.

But reasoning becomes unreliable.

E-Clean exists to **prevent this form of decay**.

---

## Epistemic primitives

E-Clean treats the following as first-class architectural elements:

### 1. Concepts must be explicit
If a concept matters, it must exist as a named construct in code.

Implicit concepts are not design shortcuts — they are latent defects.

### 2. Names must carry intent
A name is not a label.  
It is a semantic commitment.

Names are chosen for:
* clarity over brevity
* longevity over convenience
* meaning over familiarity

Renaming is a corrective act — not churn.

### 3. Types define the space of meaning
Types exist to express:
* what is allowed
* what is forbidden
* what is undefined

A type that allows illegal or incoherent states is epistemically unsound.

### 4. Structure communicates semantics
Namespaces, modules, and composition convey meaning.

Flat structures obscure intent.  
Over-layered structures dilute it.

---

## What Epistemic Clean rejects

E-Clean explicitly rejects:

* “generic” abstractions with no semantic identity
* anemic domain models
* repositories or services as semantic catch-alls
* conventions that are not mechanically enforced
* documentation that contradicts executable reality

These patterns create systems that *appear* structured while being epistemically fragile.

---

## Documentation as a derived artifact

In an E-Clean system:

* Code is authoritative
* Documentation is derived
* Diagrams are generated or validated
* Rules are inspectable

If documentation must be manually kept in sync, it will eventually diverge.

E-Clean therefore prefers:
* reflection-driven documentation
* analyzers and validators
* executable invariants
* semantic tests

The goal is alignment — not narration.

---

## Falsification as a habit

E-Clean systems are designed to be questioned.

Every important assumption should be:
* explicit
* challengeable
* falsifiable

Architectural decisions are not protected by tradition or seniority.
They survive only if they continue to hold under inspection.

This makes E-Clean uncomfortable — and resilient.

---

## What Epistemic Clean does not guarantee

Epistemic Clean does **not** guarantee:

* correctness
* optimal performance
* business success
* architectural elegance

What it guarantees is simpler and more valuable:

> When the system behaves unexpectedly, it is possible to understand *why*.

---

## Relationship to Semantic Architecture

Epistemic Clean removes ambiguity.

Semantic Architecture builds on that clarity to encode meaning deliberately and mechanically.

Without Epistemic Clean, Semantic Architecture collapses into convention.
Without Semantic Architecture, Epistemic Clean remains fragile.

They are complementary — but ordered.

---

## Summary

Epistemic Clean is the discipline of refusing ambiguity.

It insists that:
* meaning is explicit
* distinctions are enforced
* assumptions are visible
* knowledge survives change

It is not a stylistic preference.  
It is an architectural constraint.

And it is the first prerequisite for any system that claims to reason.

---

### Navigation

- Up: [E-Clean & Semantic Architecture (overview)](../e-clean-and-semantic-architecture.md)
- Prev: [Introduction](00-introduction.md)
- Next: [Semantic Architecture](02-semantic-architecture.md)