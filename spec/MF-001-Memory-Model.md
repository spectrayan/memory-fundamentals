---
title: "A Memory Model of Data for Persistent Agents"
subtitle: "Model, Algebra, and Conformance Rules for Memory Engines"
author: "Spectrayan"
date: "28 August 2026"
id: MF-001
version: "1.0.0"
license: "CC BY 4.0"
---

**Memory Fundamentals Specification MF-001**  
v1.0.0 · 28 August 2026 · CC BY 4.0

# A Memory Model of Data for Persistent Agents

Model, Algebra, and Conformance Rules for Memory Engines

| | |
|---|---|
| **Document** | MF-001 |
| **Status** | Published |
| **Version** | v1.0.0 |
| **Companion** | A reference engine ([Spector](https://github.com/spectrayan/spector)) implements this model. Physical layout is not part of the model. |
| **Method** | Same shape as Codd ([1970](#references), [1985](#references)): a data model, a closed algebra, then rules that define conformance. |
| **How to cite** | Spectrayan. 2026. *A Memory Model of Data for Persistent Agents.* Memory Fundamentals Specification MF-001. |

## Abstract

This specification defines memory as a computational concept distinct from storage. It proposes a recall algebra closed over an explicit memory state, normal forms for well-formed traces, and twelve conformance rules for systems that claim to be memory engines. The model borrows tier physics, decay, and emotional modulation from cognitive science as *named failure modes*; it does not assert biological substrate identity. Every trace carries a source tag. Offline simulation and dreaming may replay and generate hypotheses; they must not write autobiography. Persistence is shared-nothing per rememberer.

The central engineering claim is signal-complete recall: if a trace is available and passes a cue's hard gates, no retrieval plan may discard it before all first-class signals have participated in ranking. Candidate generation by a single signal (typically cosine top-*k*) followed by post-filtering does not satisfy that claim. We call the resulting miss the *truncation trap*.

This specification proposes a Codd-style conformance layer for agent memory engines. It does not claim that memory has never been formalized. Cognitive architectures formalize how cognition works; surveys and taxonomies classify systems; governance frameworks ask who owns a memory. This specification asks what a machine must do in order to reconstruct a usable past.

## 1. The distinction

Industry practice often classifies "agent memory" by storage topology: a vector index plus an extraction prompt; a property graph coordinated with embeddings after ingest; editable blocks in an agent runtime backed by a foreign store; or a process that calls relational, document, graph, and vector systems and names the composition a memory layer.

Those are storage and orchestration patterns. Each is useful. None of them is a definition of memory.

**Founding distinction.** A database returns what was written. A memory engine reconstructs what is reachable from a cue at this moment — under decay, association, and tier physics — without losing a live trace because the first index was the wrong one.

A brain is not a relational database, a document store, a graph database, or a vector index. It is one tissue that admits content-addressable, associative, sequential, and schematic *views*. A memory engine is the same kind of object: one engram, many access paths — a unified system rather than separate stores coordinated by a client.

**The identity of a memory engine is its recall algebra, not its storage topology.**

## 2. What this model is not

**Not a fifth database model.** Relational, document, graph, and vector models answer how recorded data is structured and queried. Memory answers how a persistent agent reconstructs a usable past. Hybrid storage may appear in an implementation. It is not the identity of the system.

**Not a theory of mind.** Working, episodic, semantic, and procedural tiers; decay; reconsolidation; Hebbian co-activation; valence; arousal — these names come from cognitive science because they label real failure modes of "store everything and nearest-neighbor it." If every biological analogy were deleted, the model must still stand.

**Not extract-on-ingest as memory.** Committing content to a fixed schema before the future cue is known is a legitimate strategy for structured retrieval and a lawful *derived view*. It is distinct from memory: content discarded or flattened at ingest cannot be reconstructed if the later cue needs a different projection.

**Not generational lineage.** Session memory and personal long-term memory are in scope. Memory across decades and rememberers needs extra primitives (inheritance, social reinforcement, contested ownership, retelling). Those hooks are noted in [§10](#10-evaluation) and left unspecified.

## 3. A worked example: the truncation trap

A rememberer stated three years ago, with high importance and negative valence, "never book red-eyes." Last week the same rememberer joked about cheap overnight flights. A new cue arrives: "book my flight to London."

A plan that takes cosine top-100 over recent embeddings, then re-ranks those 100 by importance, will often keep the joke and drop the constraint. The constraint is old, lexically distant from "London," and not nearest in vector space. It is also the only correct constraint. No later filter can recover a trace that was never a candidate.

That miss is the truncation trap. It is stochastic: the same system will look correct on many conversation-QA items and fail on the items that matter. [§6](#6-normal-forms), rule [M2](#7-twelve-conformance-rules), forbids candidate generation by a single signal as the sole generator. [§10](#10-evaluation) gives a test that plants exactly this trace.

## 4. The model

### 4.1 Vocabulary

These terms are reserved. "Memory" is not a synonym for a row in a vector table.

| Term | Meaning |
|---|---|
| **Trace** | Atomic stored engram: identity, tier, payload, cognitive header, physical location. |
| **Cue** | Reconstruction request, not a SQL predicate. Carries embedding, tags, optional valence window, optional time window, optional minimum importance, tier mask, $k$, and context. |
| **Signal** | A first-class coordinate of recall (see [§4.5](#45-signals-and-completeness)). |
| **Association** | Hebbian co-activation, temporal succession, or multi-entity hyperedge over traces. Native structure, not a client-side join. |
| **Tier** | A regime of physics: working, episodic, semantic, procedural. Distinct capacity, durability, and decay. |
| **Availability** | The trace has not been erased (no tombstone). |
| **Accessibility** | The current cue can reach the trace inside budget $k$. Forgetting is usually loss of accessibility. |
| **Retrieval strength $D$** | Current accessibility. Decays with disuse; rises with use. |
| **Storage strength $S$** | Encoding durability. Non-decreasing except on tombstone (Bjork & Bjork, [1992](#references)). `forget` never lowers $S$. |
| **Importance $I$** | Encoding-time salience. May be revised later. Not a static label. |
| **Valence** | Signed emotional coloring. May hard-gate (window on the cue) and soft-score (alignment inside the window). |
| **Arousal** | Unsigned intensity. Modulates the *decay law* for $D$; it is not a cue filter. |
| **Rememberer** | Namespace that owns traces. The durable unit of $M$. A filter on a shared table or collection is not a rememberer. |
| **Source** | Closed tag: `experienced`, `distilled`, `simulated`, `rehearsed`. Episodic autobiography is only `experienced`. |
| **Lineage** | Derivation record required when content is rewritten, distilled, or committed from a simulation. |

**Pin versus unresolved.** Both can suspend decay. Pin is policy ("keep this"). Unresolved is task state (open loops stay fresh — the Zeigarnik effect). They are different flags.

### 4.2 Trace

$$
T = (\text{id},\; \text{tier},\; \text{payload},\; \text{header},\; \text{loc})
$$

- **payload** — content face (text or bytes, embedding, procedural rule).
- **header** — every field that can change recall: $I$, valence, arousal, $S$, $D$, tags, pin, tombstone, resolved, recall count, source.
- **loc** — physical address. Applications never see it. This is physical independence.

**Information rule.** A signal that can change recall lives on the trace or on an association the algebra is required to consult. Metadata the scorer does not read does not count.

### 4.3 Cue

$$
C = (\text{embedding},\; \text{tags},\; \text{valence-window}?,\; \text{time-window}?,\; I_{\min}?,\; \text{tier-mask},\; k,\; \text{allow-simulated},\; \text{context})
$$

Arousal is not a cue dimension. High-arousal traces surface later because they have decayed less, not because the cue requested intensity. `allow_simulated` defaults to false: ordinary recall does not return hypothetical traces.

Recall is $R(C, M) \rightarrow [(T_i, s_i)]$. Order and scores are part of the answer.

### 4.4 Memory state

$$
M = (\mathcal{T},\; \mathcal{A},\; \Pi,\; \delta,\; \rho)
$$

| Component | Role |
|---|---|
| $\mathcal{T}$ | Available traces across tiers |
| $\mathcal{A}$ | Hebbian, temporal, and hyperedge structure |
| $\Pi$ | Tier physics: capacity, durability, decay *law* |
| $\delta$ | Per-trace strength and flags: $(D, S, \text{unresolved}, \ldots)$ |
| $\rho$ | Rememberer (isolation boundary) |

How an engine quantizes $\delta$ for a scan (buckets, epochs, lookup tables) is physical design, not part of $M$.

Every algebra operation takes $M$ (and optionally a cue or trace) and returns $M'$ or a reconstruction from $M$.

### 4.5 Signals and completeness

Minimum closed set of signals:

- semantic similarity
- lexical or sparse overlap
- importance $I$
- retrieval strength $D$
- storage strength $S$
- valence alignment (inside a window, if any)
- arousal as a modifier of the decay law for $D$, not as a separate query-time multiplier
- tag containment
- Hebbian activation
- temporal-chain distance (experienced time)
- hyperedge / entity incidence
- habituation and suppression
- resolved status

**Signal completeness.** A conformant plan considers all signals before the final ranking. A plan that generates candidates from one signal and post-filters the rest risks the truncation trap.

**Hard gates** (binary; applied before distance): tombstone, tier mask, required tags, valence window, time window, optional $I_{\min}$.

**Soft signals** modulate rank. They must not eliminate a trace before fusion.

**Decay law (arousal lives here).** Retrieval strength declines with age and rises with use. High arousal slows decline of $D$. The model requires only that the decay-resistance term is monotonically non-decreasing in arousal. The mapping from arousal to that term is physical design.

**Storage strength.** `remember` initializes $S \ge 1$ and $D = D_0 \in (0, 1]$. $S$ is non-decreasing except on tombstone. Rehearsal when $D$ is low yields a larger gain: $\Delta S = s_{\text{gain}} \cdot (1 - D)$ (one-term approximation of Bjork & Bjork; the full theory also gives larger gain when $S$ itself is still low). An engine that decreases $S$ under `forget` should not invoke the two-factor theory by name.

An admissible query-time fusion (one member of a family; not a law of the model) is given in [Appendix A](#appendix-a-an-admissible-fusion). The critical question in [§11](#11-open-questions) is whether the model should fix that form.

### 4.6 Associations

| Kind | Physics | Distinct from |
|---|---|---|
| Hebbian | Weight grows with co-activation | A client-maintained property-graph edge |
| Temporal chain | Next / previous in experienced time | `ORDER BY` timestamp |
| Hyperedge | One episode binds many entities and roles | A join table of extracted triples |

Associations are indexes over traces. If the graph and the trace store can disagree about existence, the engine is not unified.

### 4.7 Tiers

| Tier | Capacity | Durability | Role |
|---|---|---|---|
| Working | Hard bound | Volatile | Workspace; eviction is expected |
| Episodic | Unbounded log | Durable | What happened |
| Semantic | Bounded, compacted | Durable | What is taken to be true |
| Procedural | Small | Durable | How to act |

Moving content across tiers is `consolidate`. "The last $N$ turns in the prompt" is a context window, not a working tier, unless the engine owns the bound and the eviction rule.

### 4.8 Strength is not existence

Availability is whether the trace is still there. Accessibility is whether *this* cue can reach it. $D$ moves accessibility. $S$ moves future durability and slows later loss of $D$.

A store that only inserts is a warehouse. Warehouses are valid. They are not this model.

## 5. Recall algebra

Operations are closed over $M$. If the caller must leave the engine to complete the graph part or the vector part, the algebra is not closed.

| Operation | Returns traces? | May rewrite payload? | Updates $D, S$? | Meaning |
|---|---|---|---|---|
| `remember` | No | n/a | Sets initial $D_0, S_0$ with $S_0 \ge 1$ | Encode payload and header; place in a tier; emit associations |
| `recall` | Yes | No | Yes: $D$ of returned traces ([M9](#7-twelve-conformance-rules) side effect) | Fused reconstruction from a cue |
| `rehearse` | No (ids only) | No | Yes: $\Delta S = s_{\text{gain}}(1-D)$, and $D$ | Explicit strength update without returning content |
| `reconsolidate` | Optional | Yes, with lineage | Yes | The only operation that may rewrite payload |
| `associate` | No | No | No | Create or strengthen Hebbian, temporal, or hyperedge structure |
| `forget` | No | No | $D$ only; never $S$ | Weaken, suppress, or tombstone |
| `consolidate` | No | n/a | n/a | Lossy lift episodic → semantic or procedural, with lineage |
| `inhibit` | No | No | No | Habituation or suppression for this rememberer |
| `reinforce` | No | No | No | Blend valence from outcome feedback (not last-write-wins) |
| `project_tier` | Yes | No | No | Restrict reconstruction to one physics regime |
| `introspect` | No | No | No | Catalog-level engine state |
| `simulate` | Yes (hypothetical) | No | No | Counterfactual run against $M$; returns `simulated` traces; does not write $M$ |
| `dream` | No | No | May `rehearse` / `associate` | Offline replay and hypothesis generation. Must not mint `experienced` episodes |
| `commit_simulation` | No | n/a | n/a | Promote selected simulations to `distilled` with lineage. Never to `experienced` |

**Closure.** Ids from `recall` are valid inputs to `associate`, `reconsolidate`, `rehearse`, `reinforce`, `forget`, and `inhibit` without a foreign system. `simulate` reads $M$ and does not write it. `dream` may write only `simulated` traces plus association and strength updates.

**Offline generation.** `consolidate` distills experienced episodes into traces taken to be true. `simulate` and `dream` produce hypotheses. Default `recall` hard-gates `source = simulated` unless the cue sets `allow_simulated`. That gate is the counterpart of the truncation trap: imagined plans must not rank as lived facts.

**Language.** The external surface may be an API, tools, or an embedded library. It must express these operations. A caller must not be required to compose results from multiple foreign query languages. Syntax is out of scope for this specification (see [§11](#11-open-questions)).

## 6. Normal forms

**NF0 — Single engram.** Vector body, document body, header, and graph incidence share one identity. Two stores that can disagree on existence do not satisfy NF0.

**NF1 — Header completeness.** Every factor used in ranking is present on $T$ or is a specified total function of $T$ and $C$.

**NF2 — Lineage of distillation.** Every semantic or procedural trace produced by `consolidate` points at the episodes it collapsed.

**NF3 — Rememberer isolation (shared-nothing persistence).** The durable unit of $M$ is $\rho$. Traces, associations, and durability logs of $\rho_A$ and $\rho_B$ must not share an authoritative collection, table, index, or file that can be scanned as one population. A `WHERE rememberer = A` predicate on a shared store does not satisfy NF3. Isolation must not depend on a query planner remembering a predicate. Separate directory trees, non-joinable keyspaces, separate processes, or separate cryptographic domains are lawful realizations. Shared embedders and shared code are allowed. Shared traces are not. Filesystem layout is physical design, not the definition of NF3.

**NF4 — Association well-formedness.** Every association endpoint is a live or tombstoned trace id in the same rememberer.

**NF5 — Payload honesty.** If `reconsolidate` rewrites payload, the prior payload is recoverable or versioned. Incremental $D$ updates from `recall` need not be versioned row-by-row; they must be monotonic per spec and visible to `introspect`.

**NF6 — Emotional completeness.** Every episodic, semantic, and procedural trace has defined valence, arousal, and importance at encoding. They may be derived. Working-tier traces are exempt.

**NF7 — Source honesty.** Every durable trace has a source in `{experienced, distilled, simulated, rehearsed}`. Only `remember` creates `experienced` traces. `dream` and `simulate` create at most `simulated` traces. `commit_simulation` may produce `distilled` traces with lineage. No operation promotes `simulated` into episodic `experienced`. Default `recall` omits `simulated` traces.

## 7. Twelve conformance rules

These rules define what it means, in this model, to be a memory engine. They grade mechanisms, not vendors.

**M0. Engine rule.** Cognitive operations run on a substrate the engine owns: allocation, layout, indexing, and scoring are not delegated to an external query planner. In-process foreign planners still fail M0. Rule 0 is binary.

*Example.* A single process inserts into a vector index, merges into a graph store, selects from a relational store, and merges hits in application code. The API may say `remember` and `recall`. Three planners still decide independently. The algebra is not closed on one substrate.

**M1. Engram rule.** Vector, document, header, and associations are faces of one trace. Independently authoritative stores that the application must keep in sync do not satisfy M1.

**M2. Guaranteed access.** If a trace is available and passes the cue's hard gates, no plan may discard it before fused scoring. Cosine top-$k$ as the sole candidate generator does not satisfy M2, including the variant "top-100 then re-rank." That variant is the truncation trap in [§3](#3-a-worked-example-the-truncation-trap).

**M3. Signal integrity.** $I$, $D$, $S$, valence, arousal, and tags are first-class and consulted on the recall path.

**M4. Physical independence.** Applications address traces by id, cue, entity, time, or tier — never by file, partition, or store-specific collection name.

**M5. Tier physics.** The four regimes have distinct capacity, durability, and decay. A type label on one embedding collection does not satisfy M5.

**M6. Association is native.** Co-activation, succession, and multi-entity episodes are engine operations, not joins the caller performs across stores.

**M7. Forgetting is specified.** Decay, interference, suppression, arousal-modulated resistance of $D$, and consolidation are part of the model. A durable store that never decays does not satisfy M7. Arousal must affect the decay of $D$. Unresolved traces must decay differently from resolved ones.

**M8. Consolidation carries lineage.** Distillation records what was collapsed.

**M9. Reconsolidation.** `recall` updates $D$. `reconsolidate` may update payload, with lineage. Blind overwrite of the only copy does not satisfy M9.

**M10. Isolation of rememberer.** Persistence is shared-nothing per $\rho$. Physical or cryptographically sealed namespaces satisfy M10. A logical filter on a shared table, collection, or index does not. A directory tree per rememberer is one realization; the rule does not require a particular filesystem.

**M11. Fusion is internal.** Dense, sparse, lexical, and graph activation are combined by the engine. Exposing four APIs and asking the agent to merge results does not satisfy M11.

**M12. Observability.** `introspect` reports tier populations, decay, strength, consolidation, and graph degree. A conformant engine can answer why an *available* trace was *inaccessible* to a given cue.

## 8. Related work

This specification sits next to four literatures.

**Cognitive architectures.** ACT-R ([Anderson et al., 1993–](#references)) gives a formal activation equation, spreading activation, partial matching, a retrieval threshold, and decay — the closest ancestor of a memory algebra. Soar ([Laird, Newell, and Rosenbloom, 1987–](#references)) gives working memory, long-term stores, and chunking as consolidation. Both are theories of how cognition works. This specification is a prescriptive standard for what an engine must do. ACT-R's activation *is* the retrieval plan; this model additionally constrains plans that would truncate before scoring.

**Human memory theory.** Bjork and Bjork's new theory of disuse ([1992](#references)) splits storage strength from retrieval strength and predicts larger storage gains when retrieval is hard. Tulving's episodic/semantic distinction ([1972](#references)) and the Atkinson–Shiffrin family of tiers ([1968](#references)) inform $\Pi$, not as a claim that the engine is a brain.

**Agent-memory taxonomies and surveys.** CoALA ([Sumers et al., 2023/24](#references)) decomposes language-agent memory into working, episodic, semantic, and procedural stores. Hu et al. ([2025](#references)) survey forms, functions, and dynamics of agent memory. Both organize the field. Neither states normal forms or disqualifying engine rules.

**Governance.** Li ([2026](#references)) treats memory as ontological ground and asks who may alter it. That question is complementary and out of scope here.

What this specification adds is a closed algebra over a defined $M$, normal forms, and conformance rules that an implementation can pass or fail.

## 9. Architectural patterns

The rules grade mechanisms. Four recurring patterns:

| Rule | Native engine | Orchestrated stores | Extract-and-index | Runtime blocks |
|---|---|---|---|---|
| M0 Engine | Y | N | N | P |
| M1 Engram | Y | N | N | P |
| M2 No trap | Y | N | N | P |
| M3 Signals | Y | P | N | P |
| M4 Physical independence | Y | N | Y | Y |
| M5 Tiers | Y | N | N | Y |
| M6 Association | Y | P | P | N |
| M7 Forgetting | Y | P | N | P |
| M8 Lineage | P | N | N | P |
| M9 Reconsolidation | Y | N | N | P |
| M10 Isolation | Y | P | P | P |
| M11 Fusion | Y | N | N | N |
| M12 Observability | P | N | N | P |

**Y** = satisfies by construction; **P** = possible if the pattern is extended; **N** = does not satisfy by construction.

"Native engine" is the pattern this specification defines. A reference implementation may still be **P** on M8 (lineage record format) and M12 (cue-level explanation). Running several foreign stores in one process does not change the orchestrated column: in-process planners still fail M0.

The matrix does not rank usefulness. An orchestration layer can score well on conversation-QA and miss M2 on the planted-trace test in [§3](#3-a-worked-example-the-truncation-trap).

## 10. Evaluation

Conversation-QA benchmarks measure a downstream reader. They do not measure conformance to this model. Minimum engine tests:

1. **Truncation trap.** Plant an old, high-$I$, low-cosine trace as the only correct answer. Single-signal top-$k$ misses it. Fused recall finds it.
2. **Emotional enhancement.** Equal age and $I$; high vs neutral arousal. After decay, the high-arousal trace ranks higher ($D$ has declined less).
3. **Valence window.** Traces outside the cue window are gated, not merely down-weighted. Inside the window, closer valence ranks higher at equal similarity.
4. **Desirable difficulty.** Rehearse at low $D$ versus high $D$. $\Delta S$ is larger at low $D$.
5. **Monotonic $S$.** `forget` lowers $D$ and never $S$.
6. **Zeigarnik.** Unresolved decays more slowly than resolved; pin is a separate flag.
7. **Availability vs accessibility.** Tombstone versus decay; `introspect` distinguishes gone from present-but-unreachable.
8. **Consolidation fidelity.** Lineage from semantic traces reaches source episodes.
9. **Operation partition.** `recall` does not rewrite payload. `rehearse` does not return payload. `reconsolidate` is the only payload rewrite and writes lineage.
10. **Isolation.** Rememberer A cannot recall B by cue collision. Omitting the rememberer predicate must not expose B's traces from A's persistence unit.
11. **Source gate.** After `simulate`, default `recall` returns no hypothetical traces. `allow_simulated = true` may. `commit_simulation` never yields `experienced`.
12. **Dream write budget.** After `dream`, no new `experienced` episodic traces exist.
13. **Signal ablation.** Disabling decay or the arousal term of the decay law changes rank after time has passed.

Without tests of this kind, "cognitive memory" is not assessable.

**Out of scope (hooks only).** Rememberer inheritance, social reinforcement of $D$, contested ownership, retelling DAGs, and generational valence overlay extend M8, M10, NF2, NF3, and NF6. They are not specified here.

## 11. Open questions

**Critical.** Should the model fix a multiplicative fusion, or only constrain candidate generation and publish a family of scorers? Multiplicative form makes "soft signals cannot eliminate" easy to state and is sensitive to floors. An additive family is more stable and must be constrained separately so a zero weight cannot drop a live trace. This question should be settled before a query language is specified.

Also open:

- Cheapest plans that satisfy M2 on $N$ traces without a full scan, with a proof that high-$I$, low-similarity traces cannot be systematically hidden.
- Minimum lineage record for M8 (pointer set, weighted attribution DAG, or stronger provenance).
- Whether extracted facts are only a derived view with lineage (intended) or a second engram (not intended).

## 12. Physical design is not the model

A reference engine may store a trace as one off-heap record with a compact header, a quantized embedding, time-partitioned episodic files, a write-ahead log for procedural traces, and compressed association structures, and may fuse signals in one gated scan. That is one lawful implementation of NF0, NF1, NF6, M1–M3, and M11.

Instruction sets, memory APIs, latency, quantization, and header widths are how one engine is fast. The model remains true if a correct engine is slow. Layout belongs in engine documentation, not in this specification.

## Document control

| | |
|---|---|
| **ID** | MF-001 |
| **Version** | v1.0.0 |
| **Date** | 28 August 2026 |
| **License** | CC BY 4.0 |
| **Internal companion** | MF-001 internal (not public) |
| **Not in this specification** | Wire protocol, language grammar, benchmark numbers, engine header layout, dream schedule, product comparison |
| **Breaking vs editorial** | Changing M0–M12 or NF0–NF7 is a breaking revision. Clarifying examples is editorial. |

Comments and counterexamples are invited. A specification that cannot fail in public is not a specification.

## References

Anderson, J. R., et al. ACT-R: a cognitive architecture. Ongoing; canonical treatments from the 1990s onward.  
↗ [act-r.psy.cmu.edu](http://act-r.psy.cmu.edu/)

Atkinson, R. C., and R. M. Shiffrin. 1968. "Human memory: a proposed system and its control processes." *Psychology of Learning and Motivation* 2:89–195.  
↗ [doi:10.1016/S0079-7421(08)60422-3](https://doi.org/10.1016/S0079-7421(08)60422-3)

Bjork, R. A., and E. L. Bjork. 1992. "A new theory of disuse and an old theory of stimulus fluctuation." In *From Learning Processes to Cognitive Processes: Essays in Honor of William K. Estes*, vol. 2, pp. 35–67.

Codd, E. F. 1970. "A relational model of data for large shared data banks." *Communications of the ACM* 13(6):377–387.  
↗ [doi:10.1145/362384.362685](https://doi.org/10.1145/362384.362685)

Codd, E. F. 1985. "Is your DBMS really relational?" / "Does your DBMS run by the rules?" *Computerworld*, October 14 and 21, 1985.

Hu, Y., S. Liu, Y. Yue, et al. 2025. "Memory in the age of AI agents." arXiv:2512.13564.  
↗ [arxiv.org/abs/2512.13564](https://arxiv.org/abs/2512.13564)

Laird, J. E., A. Newell, and P. S. Rosenbloom. 1987. "Soar: an architecture for general intelligence." *Artificial Intelligence* 33(1):1–64.  
↗ [doi:10.1016/0004-3702(87)90050-6](https://doi.org/10.1016/0004-3702(87)90050-6)

Li, Z. 2026. "Memory as ontology: a constitutional memory architecture for persistent digital citizens." arXiv:2603.04740.  
↗ [arxiv.org/abs/2603.04740](https://arxiv.org/abs/2603.04740)

Sumers, T. R., S. Yao, K. Narasimhan, and T. L. Griffiths. 2023/24. "Cognitive architectures for language agents." *Transactions on Machine Learning Research (TMLR)*.  
↗ [arxiv.org/abs/2309.02427](https://arxiv.org/abs/2309.02427)

Tulving, E. 1972. "Episodic and semantic memory." In *Organization of Memory*, pp. 381–403.

## Appendix A. An admissible fusion

This appendix is **not** part of the model. It is one monotone combination that satisfies signal completeness and the no-zero-soft-factor constraint. Implementations may substitute another member of the family.

$$
\mathrm{sim}(q,x)=\frac{1}{1+\|q-x\|_2}
$$

$$
s(T\mid C)=\mathrm{sim}(C,T)\cdot\bigl(1+\beta\,I_n(T)\,D(T)\,S(T)^{E_s}\bigr)\cdot V(T,C)\cdot\bigl(1+\tau\cdot\mathrm{tag}(C,T)\bigr)\cdot G(T,C)
$$

- $I_n=I/I_{\max}$, $I \in (0,I_{\max}]$ at encode.
- $D \in [\epsilon_D, 1]$, $\epsilon_D>0$.
- $S \ge 1$ at encode; $E_s>0$.
- $V=1$ if the cue has no valence window; otherwise $V=\max(\epsilon_V,\; 1-|v_T-v_C|/v_{\mathrm{range}})$.
- $G \ge 1$ (boost-only associations; inhibition deferred).
- Arousal does **not** appear as a query-time factor. It appears in $\Pi$ as a term that slows decline of $D$.

Example coefficients, also not part of the model: $I_{\max}=10$, $E_s=0.3$, $\epsilon_D=0.10$, $\epsilon_V=0.01$, $\beta=\tau=1$.

End of specification.
