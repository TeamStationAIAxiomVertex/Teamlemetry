# Role Recommendation Engine

The Role Recommendation Engine proposes topology changes that stabilize delivery chains.

It does not score human worth.

## Purpose

The engine identifies missing, overloaded, ambiguous, or overly centralized role coverage in delivery topology.

It answers:

Which role, ownership, or queue change would reduce delivery-chain instability?

## Inputs

V1 inputs may include:

- GitHub review queues
- GitHub ownership and repository activity
- Jira blockers and status dwell time
- Jira dependency links
- Slack operational coordination signals
- topology graph centrality
- queue saturation metrics
- delivery confidence signals

## Recommendation Types

Role recommendations may include:

- add secondary reviewer coverage
- clarify repository ownership
- define incident response ownership
- split overloaded approval queue
- create platform support rotation
- assign explicit dependency owner
- reduce single-person approval dependency
- introduce AI stabilization for repeated handoffs

## Evidence Requirements

Each recommendation must include:

- observed destabilizing chain
- relevant nodes and edges
- queue or probability signal
- source evidence links
- expected reliability effect
- confidence level
- risk or tradeoff

## Mathematical Basis

Recommendations should target improvement in one or more of:

- higher p_i for unstable nodes
- lower rho for saturated queues
- lower dependency density
- lower coordination entropy
- lower observation latency
- higher delivery fidelity

## Example

Finding:

Pull request review for repository billing-api is a central bottleneck.

Evidence:

- review queue utilization is near saturation
- one reviewer is required on most critical PRs
- downstream Jira deployment tasks repeatedly wait on that review node

Recommendation:

Add secondary reviewer coverage for billing-api and update ownership mapping.

Expected effect:

Reduce queue saturation and increase delivery confidence for billing-api release chains.

Boundary:

This is a topology recommendation, not a judgment about the existing reviewer.

## Governance Boundary

Recommendations must not be used as automated employment decisions.

Any recommendation involving people, roles, or ownership must remain explainable, reviewable, and governed.

## Testing Requirements

Tests must verify:

- no recommendation without sufficient evidence
- recommendations cite source signals
- recommendations do not use surveillance language
- recommendations reduce at least one modeled risk dimension
- confidence decreases when source data is incomplete
