# Test Driven Doctrine

Teamlemetry must be built with tests that validate system behavior, not only isolated functions. The model is probabilistic and graph-based, so tests must cover deterministic contracts, simulations, and failure modes.

## Testing Principles

- Telemetry fidelity comes first.
- Source facts must remain separate from derived signals.
- Probabilistic outputs must be explainable and bounded.
- Queue and topology behavior must be validated with simulations.
- Recommendations must be evidence-backed.
- Tests must protect the non-surveillance boundary.

## Probabilistic Validation

Probabilistic tests validate sequential reliability and delivery confidence calculations.

Required coverage:

- P(system) compounds dependent node reliability correctly
- weak nodes reduce system probability as expected
- confidence intervals widen when input data is incomplete
- deterministic nodes and stochastic nodes are treated differently
- delivery confidence changes monotonically when reliability changes

Example test scenario:

Given a chain with p = [0.95, 0.90, 0.80], P(system) must equal 0.684.

Failure risks:

- averaging probabilities instead of compounding them
- hiding low-confidence data behind precise-looking scores
- producing employee-like performance rankings

## Queue Simulation Tests

Queue tests validate saturation behavior.

Required coverage:

- rho = lambda / mu is computed correctly
- W_q rises nonlinearly as rho approaches 1
- saturated queues are flagged before total collapse
- queue alerts distinguish temporary spikes from persistent saturation
- retry and duplicate events do not inflate arrival rates

Example test scenario:

When lambda = 9 and mu = 10, rho = 0.9 and wait-time risk must be high.

Failure risks:

- treating utilization linearly
- missing near-saturation conditions
- double-counting retried connector events

## Connector Contract Tests

Connector tests validate external source integration behavior.

V1 connector coverage:

- GitHub
- Jira
- Slack

Required coverage:

- authentication failure behavior
- rate-limit behavior
- pagination behavior
- webhook signature validation where applicable
- idempotency keys
- source ID preservation
- timestamp mapping
- malformed payload handling
- connector health reporting

Failure risks:

- duplicate events
- source identity loss
- silent connector drift
- unbounded retries

## Topology Graph Validation

Graph tests validate node and edge creation.

Required coverage:

- dependency edges are directed correctly
- explicit and inferred edges are distinguishable
- cyclic dependency detection works
- dependency density is computed correctly
- topology snapshots are versioned
- missing source data degrades confidence rather than crashing

Example test scenario:

If ticket B depends on ticket A, the edge must be A -> B because B is downstream of A.

Failure risks:

- reversing dependency direction
- merging unrelated nodes
- treating mentions as dependencies without evidence

## Recommendation Correctness

Recommendation tests validate that outputs follow evidence and policy.

Required coverage:

- recommendations cite source evidence
- role recommendations target topology instability, not human value
- AI stabilization recommendations require bounded repeatable processes
- recommendations include confidence and expected effect
- recommendations are not produced when evidence is insufficient

Failure risks:

- opaque recommendations
- surveillance-like recommendations
- overconfident recommendations from incomplete data

## Stochastic Simulation Testing

Stochastic tests validate model behavior across randomized system states.

Required coverage:

- delivery confidence declines as node variance increases
- blocker propagation follows dependency paths
- observation latency increases downstream stale-work risk
- queue saturation compounds topology risk
- AI stabilization reduces variance only in eligible bounded workflows

Simulation controls:

- seeded random runs for reproducibility
- scenario fixtures for known topologies
- threshold assertions rather than brittle exact outputs where appropriate

## Slack Reporting Tests

Slack report tests validate operational clarity and noise control.

Required coverage:

- reports identify destabilizing chain location
- reports include evidence links
- reports avoid employee surveillance language
- routing respects configuration
- retry behavior is idempotent
- sensitive content follows governance rules

## Regression Gates

Before release, run tests for:

- connector contracts
- normalization contracts
- graph projections
- probability models
- queue simulations
- recommendation correctness
- Slack reporting
- governance policy

A change is not complete until it has been validated against the relevant system layer.
