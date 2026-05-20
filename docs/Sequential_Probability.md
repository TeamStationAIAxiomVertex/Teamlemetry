# Sequential Probability

Sequential probability is the core operating model for Teamlemetry.

## Thesis

Engineering delivery succeeds through dependent chains. Each dependent node contributes uncertainty, and the system-level probability is the product of local reliabilities.

P(system) = product(p_i)

## Delivery Chain Model

A delivery chain is an ordered set of nodes where downstream progress depends on upstream output.

Example chain:

1. Requirement clarity
2. Implementation
3. Review
4. CI
5. Security approval
6. Deployment
7. Incident-free verification

If each node has reliability p_i, then the whole chain has reliability P(system).

## Why Averages Fail

Averaging reliability hides dependency risk.

If nodes have reliabilities [0.99, 0.99, 0.60], the average is 0.86, but the sequential probability is 0.588.

The delivery chain is fragile because the low-reliability node compounds through the whole system.

## Deterministic And Stochastic Nodes

Deterministic nodes have predictable outputs under defined inputs.

Examples:

- stable CI check
- schema validation
- required deployment gate

Stochastic nodes have variable outputs due to ambiguity or incomplete information.

Examples:

- ambiguous requirements
- unbounded architecture review
- cross-team prioritization

Teamlemetry must model both without pretending all work has the same uncertainty profile.

## Confidence Intervals

When telemetry is sparse, Teamlemetry must lower confidence in the estimate.

A precise-looking score from weak data is worse than an honest uncertain estimate.

## Destabilizing Node Detection

A destabilizing node is a node whose reliability, queue pressure, cognitive load, or dependency centrality materially reduces system delivery confidence.

Detection signals:

- low local reliability
- high queue saturation
- high dependency centrality
- repeated blocker propagation
- high observation latency
- high coordination entropy

## Design Implication

The platform should recommend fixes in the order most likely to improve system reliability.

It should not optimize for the loudest metric or most visible activity stream.
