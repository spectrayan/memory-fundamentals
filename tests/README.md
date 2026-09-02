# Conformance Tests

This directory contains the conformance test suite defined in [MF-001 §10](https://github.com/spectrayan/memory-fundamentals/blob/main/spec/MF-001-Memory-Model.md).

These measure the **mathematical guarantees of recall algebra**, evaluated deterministically via closed-form assertion predicates (no LLM judge required).

## Conformance Fixtures

Fixtures are located under [`fixtures/`](fixtures/):

| Fixture ID | Specification | Key Verification |
|---|---|---|
| **MF-T01** | §10.1 Truncation Trap | M2 guaranteed access: high-$I$ standing constraints remain in candidate set and outrank recent lexical jokes; knowledge update ordering. |
| **MF-T03** | §10.3 Valence Window | §4.5 valence hard-gating: traces outside query valence window are hard-gated out at any rank. |
| **MF-T10** | §10.10 Isolation | M10 / NF3 multi-tenant isolation: distinct rememberer partitions never cross-contaminate despite identical prompt sentences. |

For full details on assertion syntax and report format, see [`fixtures/ASSERTIONS.md`](fixtures/ASSERTIONS.md).

## Planned tests

- Truncation-trap suite (MF-T01)
- Emotional enhancement test
- Arousal accessibility test
- Valence congruency test (MF-T03)
- Desirable difficulty test
- Storage strength monotonicity
- Encoding defaults test
- Zeigarnik test
- Availability vs accessibility
- Consolidation fidelity
- Reconsolidation stability
- Isolation (MF-T10)
- Source gate
- Dream write budget
- Association closure
- Signal ablation
- Operation partition test

These tests make conformance claims verifiable. Without them, claims of "cognitive memory" cannot be objectively assessed.

