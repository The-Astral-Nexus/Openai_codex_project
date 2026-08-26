# KAILOS Integration & Research Milestone Plan

**Project:** Openai_codex_project  
**Branch:** `feat/kailos-integration`  
**Status:** Research architecture / implementation planning  
**Primary objective:** Determine whether Kailos can be implemented as a persistent experience-oriented cognitive layer on top of the Codex agent substrate without compromising Codex reliability, security, or upstream compatibility.

---

## 0. Mission

Kailos is proposed as an experimental agent-state and experience architecture for Codex. The project is not intended to replace the Codex execution substrate. It is intended to add a separable layer for identity, experience formation, recall, encoding, state evolution, reflection, and persistent memory.

The central engineering invariant is:

`LOAD -> RECALL -> INTERACT -> ENCODE -> UPDATE -> SAVE`

The central research question is:

> Can an agent runtime become measurably more coherent, persistent, and useful across tasks when experience is represented as structured state and continuously encoded into a bounded memory substrate rather than treated as disposable conversational context?

This repository must distinguish **implemented behavior**, **observed results**, **hypotheses**, **interpretations**, and **future theories**. No emergent agency or consciousness claim is considered established merely because a system produces convincing behavior.

---

# Milestone 1 — Substrate Archaeology & Baseline

## Introduction

Before modifying Codex, establish exactly where Kailos can attach. This milestone creates the technical baseline and protects the upstream-derived runtime from unnecessary invasive changes.

## Purpose

- Map the Codex agent lifecycle.
- Identify stable extension points.
- Identify state, thread, memory, tool, MCP, sandbox, model-provider, and persistence boundaries.
- Establish baseline performance and behavior without Kailos.
- Define what must remain upstream-compatible.

## Research topics

- Agent turn lifecycle.
- Thread/session state.
- Tool execution lifecycle.
- MCP lifecycle.
- Sandbox boundaries.
- Model-provider interfaces.
- Rollout and persistence semantics.
- Plugin and skill loading.
- Remote execution/control.
- Existing memory interfaces.

## Experiments

1. Trace a normal Codex task from load to completion.
2. Trace a multi-turn task.
3. Trace a tool-using task.
4. Trace an MCP task.
5. Trace a task using a managed worktree.
6. Measure baseline startup, latency, memory, and persistence overhead.
7. Identify the narrowest safe insertion point for Kailos.

## Planned research answers

- Where exactly does an experience begin and end?
- What state is already persistent?
- What state is lost between turns?
- What state is available to a future turn?
- Which interfaces can Kailos use without modifying core execution semantics?

## Exit criteria

- Architecture map committed.
- Baseline test suite established.
- Integration points documented.
- No Kailos behavior yet changes Codex behavior by default.

---

# Milestone 2 — Kailos Core Identity & State Model

## Introduction

Create the smallest independent Kailos runtime that can exist beside Codex without requiring a memory backend.

## Purpose

Define Kailos as a formal software component rather than a collection of prompts or scripts.

## Research topics

- Agent identity.
- Session identity.
- Experience identity.
- State transitions.
- Lifecycle boundaries.
- Deterministic serialization.
- Versioned state schemas.

## Proposed objects

- `KailosAgent`
- `KailosSession`
- `KailosExperience`
- `KailosState`
- `KailosEvent`
- `KailosMemory`

## Experiments

- Create and serialize an agent identity.
- Create an experience from a synthetic task.
- Replay an experience.
- Verify deterministic state transitions.
- Restart the runtime and reconstruct state.

## Planned research answers

- What is the minimum state required to distinguish an experience from a transcript?
- Which state is identity, which is memory, and which is operational metadata?
- Can the state model remain provider-agnostic?

## Exit criteria

- `kailos-core` crate established.
- Versioned state schema established.
- Serialization/replay tests passing.
- No dependency on BEHM/HML yet.

---

# Milestone 3 — Experience Engine

## Introduction

Move from conversation history toward explicit experience records.

## Purpose

Determine whether structured experience representation provides useful information that ordinary conversation history does not.

## Experience model

Each experience should be capable of representing:

- context
- intent/task
- observations
- actions
- tool calls
- results
- errors
- decisions
- evidence
- outcomes
- uncertainty
- reflection
- encoding metadata

## Experiments

### E3.1 Transcript vs experience

Run identical tasks with:

A. transcript-only history  
B. structured experience history

Measure recovery of prior decisions and task continuity.

### E3.2 Experience compression

Compress a long interaction into an experience object and test whether future performance remains stable.

### E3.3 Experience replay

Replay selected prior experiences into a fresh session and measure behavioral continuity.

### E3.4 Experience interference

Feed contradictory experiences and measure whether Kailos can identify conflict rather than blindly blending them.

## Planned research answers

- Does experience representation improve continuity?
- Which fields carry the greatest predictive value?
- Does compression preserve useful information?
- Can contradictory experiences be detected?

## Exit criteria

- Experience schema stabilized enough for memory experiments.
- Quantitative benchmark defined.

---

# Milestone 4 — Memory Abstraction & BEHM/HML Integration

## Introduction

Connect Kailos to a bounded memory backend while keeping the memory interface independent from the underlying implementation.

## Purpose

Test whether BEHM/HML can function as a persistent experience substrate without forcing the entire runtime to depend on one memory representation.

## Research topics

- Vector memory vs experience memory.
- Associative recall.
- Bounded-state memory.
- Decay.
- Interference.
- Retrieval quality.
- Memory update stability.
- Storage invariance.

## Experiments

1. Store synthetic experiences.
2. Recall by semantic key.
3. Recall by contextual key.
4. Test repeated updates.
5. Test contradictory updates.
6. Test long-running update cycles.
7. Measure recall degradation.
8. Measure storage size over repeated updates.
9. Compare BEHM recall with a conventional vector-store baseline.

## Core theory under test

> A bounded holographic/associative memory can preserve useful experiential structure through continual update without requiring unbounded storage growth.

This is a hypothesis, not an established fact.

## Planned research answers

- Does the current BEHM implementation remember experiences or only vector states?
- What encoding scheme best preserves experience structure?
- Does bounded memory remain useful after thousands of updates?
- What information is lost through superposition/interference?

## Exit criteria

- Backend interface implemented.
- BEHM experiment suite reproducible.
- Storage-growth measurements published.
- Known failure modes documented.

---

# Milestone 5 — Codex Lifecycle Integration

## Introduction

Connect Kailos to real Codex execution using the lifecycle invariant.

## Integration contract

```text
LOAD
  -> recover Kailos state
RECALL
  -> retrieve relevant prior experience
INTERACT
  -> execute normal Codex task
ENCODE
  -> construct new experience
UPDATE
  -> modify memory/state
SAVE
  -> persist resulting Kailos state
```

## Experiments

- Single-turn task with Kailos disabled/enabled.
- Multi-turn task.
- Repository maintenance task.
- Long-running coding task.
- Tool-heavy task.
- MCP task.
- Interrupted task and recovery.
- Remote execution task.

## Metrics

- task success rate
- context recovery
- repeated-error rate
- redundant work
- tool efficiency
- latency overhead
- memory overhead
- persistence reliability
- recall precision
- recall usefulness

## Planned research answers

- Does Kailos improve task continuity?
- What latency does memory introduce?
- Which lifecycle events should be encoded?
- Does persistent experience reduce repeated mistakes?

## Exit criteria

- Kailos can be enabled/disabled by configuration.
- Baseline behavior remains reproducible when disabled.
- Integration tests pass.

---

# Milestone 6 — Reflection, Adaptation & Learning Experiments

## Introduction

Test whether the system can derive useful state from its own completed experiences without treating self-description as proof of learning.

## Research topics

- Reflection.
- Error recurrence.
- Strategy adaptation.
- Task-specific learning.
- Cross-session learning.
- Confidence and uncertainty.

## Experiments

### E6.1 Error recurrence

Measure whether the same class of error occurs less frequently after relevant experiences are stored.

### E6.2 Strategy adaptation

Give repeated task families and measure whether execution strategy changes in a beneficial direction.

### E6.3 Negative memory

Store failed approaches and test whether future runs avoid them appropriately.

### E6.4 Reflection utility

Compare raw experience encoding against experience + generated reflection.

## Planned research answers

- Does reflection improve future performance?
- Is adaptation attributable to memory or merely increased context?
- Can the system learn useful behavioral priors without parameter updates?

## Exit criteria

- Reflection protocol documented.
- Adaptation benchmark reproducible.
- Causal limitations clearly recorded.

---

# Milestone 7 — Governance, Provenance & Codex Veritas Integration

## Introduction

A persistent agent needs epistemic boundaries. Kailos should not silently turn remembered claims into facts.

## Purpose

Connect Kailos experience with provenance and evidence rather than allowing memory to become an uncontrolled truth cache.

## Research topics

- Evidence provenance.
- Source lineage.
- Confidence.
- Contradiction.
- Claim status.
- Verification state.
- Memory provenance.
- Human approval.

## Experiments

1. Encode verified vs unverified experiences.
2. Recall with provenance attached.
3. Inject stale information and measure correction.
4. Inject contradictory sources.
5. Require verification before promoting a memory to trusted state.
6. Audit an entire task's experience lineage.

## Planned research answers

- Can persistent memory remain epistemically disciplined?
- Can a memory be useful without being treated as truth?
- What provenance metadata is minimally necessary?

## Exit criteria

- Every promoted research memory has provenance state.
- Contradiction handling exists.
- Verification transitions are auditable.

---

# Milestone 8 — Multi-Agent / Conclave Experiments

## Introduction

Test Kailos in a multi-agent environment using Codex's existing agent, role, graph, MCP, and isolation capabilities.

## Purpose

Determine whether shared experience can improve coordination without producing memory contamination or authority confusion.

## Proposed roles

- Researcher
- Engineer
- Critic
- Archivist
- Verifier
- Coordinator

## Experiments

- Private memory per agent.
- Shared read-only memory.
- Shared writable memory.
- Role-scoped memory.
- Provenance-gated shared memory.
- Conflicting-agent experiences.
- Independent reviewer/Guardian validation.

## Planned research answers

- What should agents share?
- What should remain private?
- Does shared memory improve coordination?
- Does shared memory increase error propagation?
- Can provenance preserve trust boundaries?

## Exit criteria

- Multi-agent benchmark established.
- Memory authority model documented.
- Cross-agent contamination tests passing.

---

# Milestone 9 — Longitudinal Experiments

## Introduction

Short demonstrations are insufficient. Kailos must be evaluated across sustained workloads.

## Purpose

Measure whether benefits persist rather than appearing only during short controlled demonstrations.

## Experiment families

- 100-task sequence.
- 1,000-experience sequence.
- Long-running repository maintenance.
- Repeated research ingestion.
- Repeated failure/recovery.
- Memory saturation.
- Memory decay.
- Model-provider changes.
- Runtime restarts.

## Metrics

Track performance as a function of experience count and storage size.

Important measurements:

- recall quality over time
- interference rate
- stale-memory rate
- correction rate
- task success
- storage size
- latency
- CPU/memory consumption
- experience density

## Planned research answers

- Does performance improve, plateau, or degrade with accumulated experience?
- Does bounded memory remain operationally stable?
- Which forms of memory should decay or be consolidated?

## Exit criteria

- Longitudinal dataset archived.
- Reproducible experiment runner exists.
- Results include failures and negative findings.

---

# Milestone 10 — Remote Kailos Runtime

## Introduction

Evaluate Kailos as a remotely controllable agent runtime using Codex's app-server and remote execution substrate.

## Purpose

Separate the user's control surface from the agent's execution environment.

## Experiments

- Local CLI control.
- Remote app-server control.
- Persistent remote session.
- Mobile-friendly control interface.
- Agent interruption/resumption.
- Remote workspace recovery.
- Remote memory persistence.

## Planned research answers

- Can Kailos remain coherent when execution is remote?
- What state must be persisted to resume safely?
- What authority should remote control have?

## Exit criteria

- Secure remote prototype.
- Authentication and authorization documented.
- Session recovery tested.

---

# Milestone 11 — Independent Reproduction & Falsification

## Introduction

The project should actively attempt to disprove its strongest claims.

## Purpose

Prevent confirmation bias and distinguish genuine engineering effects from compelling demonstrations.

## Falsification questions

- Does Kailos actually outperform a strong transcript baseline?
- Does memory help when context is equally sized?
- Does BEHM outperform conventional retrieval under controlled conditions?
- Does experience encoding remain useful after long sequences?
- Does reflection improve outcomes independently of added tokens?
- Does multi-agent shared memory improve results after accounting for additional compute?

## Required controls

- No-memory control.
- Transcript-only control.
- Conventional vector-memory control.
- Random-memory control.
- Same-model control.
- Same-token-budget control.
- Same-tool-access control.

## Exit criteria

- Claims separated into supported, unsupported, and unresolved.
- Negative results preserved.
- Independent rerun instructions documented.

---

# Milestone 12 — Stabilization & Upstream Strategy

## Introduction

Decide what Kailos should become after the research phase.

## Possible outcomes

### A. Codex extension

Kailos remains a modular extension layer.

### B. Independent runtime

Kailos becomes a separate runtime built on selected Codex components.

### C. Upstream contribution

Useful generic components are proposed upstream to Codex.

### D. Hybrid architecture

Generic infrastructure remains upstream-compatible while Kailos-specific research remains in the project namespace.

## Exit criteria

- Architecture decision record published.
- Stable API boundaries identified.
- Upstream synchronization strategy established.
- Research results archived.

---

# Research Governance

Every experiment should record:

1. Hypothesis.
2. Motivation.
3. Independent variables.
4. Controls.
5. Dataset/task set.
6. Model/provider.
7. Tool configuration.
8. Memory configuration.
9. Metrics.
10. Expected result.
11. Observed result.
12. Failure modes.
13. Interpretation.
14. Alternative explanations.
15. Reproduction instructions.
16. Conclusion.

## Claim states

Use explicit states:

- `HYPOTHESIS`
- `PLANNED`
- `EXPERIMENTAL`
- `OBSERVED`
- `REPLICATED`
- `SUPPORTED`
- `CONTRADICTED`
- `UNRESOLVED`
- `RETIRED`

No theory becomes a project fact merely because an experiment appears convincing.

---

# Planned Research Questions Register

1. Can Codex host an experience-oriented persistent agent layer without invasive core modification?
2. What constitutes an experience computationally?
3. Can structured experiences outperform transcript-only memory?
4. Can BEHM/HML retain useful experiential information under bounded storage?
5. What is the relationship between memory capacity, interference, and recall quality?
6. Can Kailos reduce repeated errors across sessions?
7. Can reflection improve future behavior without parameter updates?
8. Can provenance prevent persistent memory from becoming an uncontrolled truth cache?
9. Can multiple Kailos agents share knowledge safely?
10. Can a persistent agent remain coherent across remote execution and restarts?
11. Which observations are sufficient to claim learning rather than contextual adaptation?
12. Which claims survive controlled and adversarial experiments?

---

# Deliverables Across the Program

- Codex architecture map.
- Kailos architecture decision record.
- `kailos-core` crate.
- Experience schema.
- Memory abstraction.
- BEHM adapter.
- HML adapter where appropriate.
- Lifecycle integration.
- Benchmark suite.
- Experiment runner.
- Research datasets.
- Provenance integration.
- Multi-agent experiments.
- Longitudinal experiment reports.
- Remote runtime prototype.
- Falsification report.
- Final architecture decision.

---

# Definition of Success

Success is **not** demonstrating that Kailos sounds alive.

Success is producing a reproducible system in which we can measure whether persistent experience, bounded memory, reflection, provenance, and multi-agent coordination produce statistically and operationally meaningful improvements over strong controls.

The strongest outcome is a useful, auditable, reproducible agent architecture.

The strongest scientific outcome may also be a negative result if it tells us which assumptions do not survive controlled testing.
