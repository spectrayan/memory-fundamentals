# Memory Fundamentals

**A database returns what was written. A memory engine reconstructs what is reachable from a cue at this moment — under decay, association, and tier physics — without losing a live trace because the first index was the wrong one.**

This repository contains the Memory Fundamentals specification — a formal model, algebra, and conformance rules for memory engines, in the tradition of Codd's relational model for databases.

## What is this?

The industry classifies "agent memory" by storage topology: vector indexes, graph stores, document databases, orchestration layers. Those are storage patterns. None is a definition of memory.

This specification draws the distinction. It defines:

- **A data model** — traces, cues, signals, associations, tiers, and strength as first-class concepts
- **A recall algebra** — closed operations over memory state (remember, recall, rehearse, reconsolidate, forget, consolidate, simulate, dream, and more)
- **Normal forms** — integrity constraints a well-formed memory must satisfy (NF0–NF7)
- **Twelve conformance rules** — what an implementation must demonstrate to be called a memory engine (M0–M12)

The identity of a memory engine is its recall algebra, not its storage topology.

## Specification

| Document | Description |
|---|---|
| [MF-001](spec/MF-001-Memory-Model.md) | A Memory Model of Data for Persistent Agents — Model, Algebra, and Conformance Rules for Memory Engines |

## Key concepts

### The truncation trap

A plan that takes cosine top-*k* then re-ranks by importance will often miss the trace that matters most — an old, high-importance, emotionally vivid memory with low cosine similarity to the current cue. This is the truncation trap. Rule M2 forbids single-signal candidate generation as the sole generator.

### Strength is not existence

Databases have existence. Memory has two axes: retrieval strength *D* (current accessibility, decays with disuse) and storage strength *S* (encoding durability, non-decreasing). Forgetting is loss of accessibility, not loss of storage. High *S* slows future loss of *D* — the protective effect of strong encoding (Bjork & Bjork, 1992).

### Source honesty

Every trace carries a closed provenance tag: `experienced`, `distilled`, `simulated`, or `rehearsed`. Offline `simulate` and `dream` operations may generate hypotheses; they must not mint autobiographical episodes. Default recall hard-gates simulated traces.

## Reference implementation

[Spector](https://github.com/spectrayan/spector) is cited as an existence proof that the model can be implemented on a single substrate. It is the companion, not the definition.

## How to cite

```
Spectrayan. 2026. "A Memory Model of Data for Persistent Agents."
Memory Fundamentals Specification MF-001, v1.0.0. August 2026.
https://github.com/spectrayan/memory-fundamentals
```

## Roadmap

- [x] Model, algebra, and twelve rules (MF-001 v1.0.0)
- [ ] RCL (Recall Language) BNF grammar
- [ ] Conformance test suite
- [ ] M8 lineage record format
- [ ] Truncation-trap benchmark corpus

## Related work

This specification builds on and acknowledges:

- **ACT-R** (Anderson et al., 1993–) — formal cognitive architecture; the closest ancestor
- **Soar** (Laird, Newell, Rosenbloom, 1987–) — production-system cognitive architecture
- **Bjork & Bjork** (1992–) — new theory of disuse; two-factor strength model
- **CoALA** (Sumers et al., 2023–24) — cognitive architecture taxonomy for LLM agents
- **Animesis** (Li, 2026) — constitutional memory architecture; governance axioms

ACT-R and Soar formalize how cognition works; this specification formalizes what an engine must do. The distinction is between a theory of mind and a specification for a machine.

## License

This work is licensed under [CC BY 4.0](LICENSE).

Comments and counterexamples are invited. A specification that cannot be challenged in public is not a specification.
