# Team Topology Method Data Model

This document extends the V1 build specification with telemetry-derived topology metrics from the Team Topology Method whitepaper.

The purpose is to turn engineering telemetry into measurable organization physics: reliability, queue pressure, coupling, blocking risk, context burden, review-flow integrity, and topology health.

## Source Principle

Use telemetry, not opinion.

Teamlemetry should infer topology from observable work signals:

- commit graph
- pull request review chains
- ticket handoffs
- sprint transitions
- deployment history
- rollback history
- incident traces
- code ownership overlap
- collaboration metadata
- configured Slack operational metadata

The system must not use manager ratings, self-assessments, private-message surveillance, keystroke tracking, or employee-worth scoring.

## Real Topology Versus Org Chart

The org chart is a hypothesis. The actual topology is recovered from work telemetry.

Teamlemetry must detect:

- gateway nodes
- phantom teams
- congested paths
- overloaded integrators
- reviewer choke points
- coupling hotspots
- hidden dependency bridges
- topology mismatch between designed teams and actual collaboration clusters

## Base Signals

Five base signals form the foundation of topology diagnostics.

## Throughput Signal: T

Throughput measures accepted movement through the delivery system, not raw activity.

```text
T = 0.6 * V_issue + 0.4 * O_code
```

Where:

- `V_issue` = normalized completed work from the ticket system
- `O_code` = normalized accepted code contribution signal

Implementation inputs:

- completed tickets
- accepted PRs
- merged work
- deployment-linked work items
- reopened work discount
- rejected review discount

Boundary:

Throughput is not lines of code. It is completed, accepted movement that has passed to the next system stage.

## Reliability Signal: R

Reliability measures execution consistency.

```text
R = 1 - CV(v)
CV(v) = sigma_v / mu_v
```

Where `v` is the sprint-to-sprint delivery output vector.

Implementation inputs:

- sprint output variance
- reopen rate
- rollback frequency
- review rejection rate
- escaped defect rate where available
- incident coupling
- delivery completion consistency

Small-sample rule:

Use regularization for sparse histories. New nodes must be pulled toward population priors rather than punished for limited data.

Boundary:

Reliability is a system-behavior estimate. It must not become a personal performance verdict.

## Context Surface Area Signal: C

Context surface area measures cognitive and coordination load exposure.

```text
C = repos * sqrt(collaborators) * (1 + cross_team_fraction)
```

Implementation inputs:

- repositories touched
- active collaborators
- cross-team work fraction
- service ownership count
- PR review fanout
- Slack escalation frequency where governed
- active dependencies

Interpretation:

Rising context surface area with stable reliability may indicate growing mastery or healthy bridge work. Rising context surface area with declining reliability indicates overload risk.

Boundary:

Context surface area is load exposure, not productivity.

## Systemic Value Signal: S

Systemic value measures stabilizing impact beyond isolated output.

```text
S = 0.4 * B + 0.3 * RR + 0.3 * BF
```

Where:

- `B` = betweenness centrality in collaboration or dependency graph
- `RR` = balanced review ratio
- `BF` = bus-factor exposure contribution

Implementation inputs:

- shortest-path centrality through collaboration graph
- review-given and review-received balance
- unique file or service ownership concentration
- cross-team bridge edges
- incident or deployment dependency centrality

Boundary:

High systemic value identifies graph-critical stabilizing roles. It is not a leaderboard.

## Complexity Signal: X

Complexity measures difficulty of carried work.

```text
X = 0.5 * P + 0.3 * CT + 0.2 * RJ
```

Where:

- `P` = normalized points or work-unit difficulty
- `CT` = pull request cycle time
- `RJ` = rejection or rework rate

Implementation inputs:

- story points where available
- ticket complexity labels
- PR cycle time
- review rejection count
- reopen count
- integration touch count
- services changed

Boundary:

Slow progress on hard work is not equivalent to slow progress on easy work.

## Derived Structural Metrics

## Coupling Coefficient: K_c

Coupling coefficient measures whether output movement is tied across nodes.

```text
K_c(i, j) = corr(T_i, T_j)
```

Implementation requirements:

- compute correlation over sprint windows
- detrend org-wide seasonality where possible
- support lead/lag offsets of one to three sprints
- distinguish healthy collaboration from brittle coupled fate

Interpretation:

High positive coupling can indicate shared delay risk. Negative or low coupling can indicate resilience, substitution, or load balancing.

## Blocking Probability: P_b

Blocking probability models dependency delay risk.

```text
P_b = 1 - (1 - p_wait)^d
```

Where:

- `p_wait` = probability a dependency is not ready
- `d` = active dependency count

Implementation inputs:

- blocked tickets
- handoff delay
- unresolved dependencies
- stale PRs
- review delay
- external team wait time
- deployment gate wait time

Use this to show why dependency-heavy delivery chains need different topology than loosely coupled teams.

## Synchronization Penalty: S_p

Synchronization penalty measures timing latency from review windows, timezone spread, and context switching.

```text
S_p = sum(T_wait + T_context_switch)
```

Implementation inputs:

- reviewer timezone offset where governed
- time from request to first response
- handoff lag
- review idle time
- async thread resolution time
- deployment approval wait time

Boundary:

Timezone and async coordination should be modeled as system physics, not as personal availability judgment.

## Centrality Concentration: CC

Centrality concentration measures whether too much system flow depends on too few nodes.

```text
CC = sum(S_i for top k nodes) / sum(S_i for all nodes)
```

Implementation inputs:

- top-k systemic value share
- review centrality concentration
- service ownership concentration
- incident-response centrality
- deployment approval concentration

Interpretation:

High concentration means fragility. The system may be dependent on a few load-bearing roles.

## Review Flow Integrity: RFI

Review flow integrity measures whether code review is distributed or collapsing into a bottleneck.

```text
RFI = 1 - Gini(review_load)
```

Implementation inputs:

- reviews requested
- reviews completed
- review turnaround time
- approval distribution
- rejection distribution
- domain-specific reviewer concentration

Interpretation:

Low RFI indicates reviewer bottlenecks, approval choke points, burnout risk, and single points of architectural truth.

## Topology Health Score: THS

Topology Health Score is the executive topology metric.

```text
THS = w1 * R_avg
    + w2 * (1 - C_norm_avg)
    + w3 * S_distribution
    + w4 * (1 - K_c_positive_avg)
    + w5 * (1 - P_b_avg)
    + w6 * (1 - S_p_norm_avg)
```

Suggested initial weights:

- reliability: 25%
- load balance / context burden: 20%
- systemic value distribution: 15%
- coupling risk: 15%
- blocking risk: 15%
- synchronization penalty: 10%

Initial interpretation bands:

- `85-100`: resilient topology
- `70-84`: functioning but exposed
- `55-69`: structurally unstable
- below `55`: topology risk

Implementation rule:

Thresholds should become cohort-relative and self-calibrating as real data accumulates. Hardcoded bands are acceptable only for seed MVP reporting.

## Worker And Node Archetypes

Archetypes are structural roles inferred from telemetry. They must not be treated as personality types or employee labels.

## Reliability Anchor

Signal pattern:

- high reliability
- moderate throughput
- manageable context surface area

System meaning:

A stabilizing node that reduces variance.

Recommendation pattern:

Protect from overload and avoid forcing unnecessary cross-team responsibilities.

## Flow Stabilizer

Signal pattern:

- high reliability
- strong systemic value
- moderate to high throughput
- balanced review participation

System meaning:

A node that improves topology and flow, not just output.

Recommendation pattern:

Use as the model profile for stabilizing hires or technical leadership coverage.

## Graph Glue

Signal pattern:

- high systemic value
- high context surface area
- not necessarily top throughput

System meaning:

A bridge node holding multiple subsystems together.

Recommendation pattern:

Reduce context load and add redundant connectors before reliability drops.

## Entropy Injector

Signal pattern:

- high throughput
- low reliability
- spiky variance

System meaning:

A node that can produce output while increasing downstream instability.

Recommendation pattern:

Add guardrails, tests, review support, and AI-assisted quality checks.

## Overloaded Integrator

Signal pattern:

- high context surface area
- high systemic value
- declining reliability

System meaning:

The system depends on this integration path too heavily.

Recommendation pattern:

Redistribute load, add role coverage, automate knowledge transfer, and reduce peripheral responsibilities.

## Isolated Specialist

Signal pattern:

- high complexity
- lower systemic value
- narrow cluster participation

System meaning:

Hard focused work with possible knowledge concentration risk.

Recommendation pattern:

Pair with a secondary contributor and ensure documentation continuity.

## Choke-Point Reviewer

Signal pattern:

- high review centrality
- rising PR cycle time
- high blocking probability around one review lane

System meaning:

Review authority has collapsed into a bottleneck.

Recommendation pattern:

Redistribute review authority, add qualified reviewers, and automate low-risk review preparation.

## Hiring And Topology Recommendation Math

A hire should be evaluated as a topology intervention, not a role-label fill.

```text
NTV = Delta_THS + Delta_P_success - Delta_Cost_annualized
```

A new node helps only when:

```text
Benefit_node > CoordinationTax_node
```

Expanded:

```text
R * F * T * S_fit > E_new + D_new + Sync_new
```

Where:

- `R` = reliability
- `F` = flow fit
- `T` = throughput contribution
- `S_fit` = network fit
- `E_new` = new coordination edges
- `D_new` = dependency overhead
- `Sync_new` = synchronization burden

Recommendation requirement:

A role recommendation must show how it improves topology health, reduces blocking probability, lowers centrality concentration, or improves delivery probability.

## Effective Capacity

Capacity must be modeled as usable throughput under variance, not raw headcount.

```text
Cap_eff = sum(T_i * R_i * (1 - C_penalty_i) * (1 - Block_penalty_i))
```

Implementation use:

- sprint planning
- roadmap confidence
- staffing simulation
- overload detection
- topology intervention ROI

## Program Delivery Probability

```text
P_delivery = P_base * THS * A_architecture * G_governance
```

Where:

- `P_base` = baseline probability from delivery chain composition
- `THS` = topology health score
- `A_architecture` = architecture support factor
- `G_governance` = governance and operating discipline factor

This should be updated each sprint as telemetry changes.

## AI Insertion Value

AI should be inserted where it reduces friction without increasing opacity or hidden coupling.

```text
V_AI(x) = G_automation(x) - D_coordination(x) - R_opacity(x)
```

High-value insertion points:

- review preparation
- issue classification
- dependency monitoring
- test generation with human review
- contract verification
- operational summarization
- knowledge transfer for overloaded integrators

Wrong measurement:

Individual activity boost.

Correct measurement:

Improvement in team-level O-Ring score, topology health, blocking probability, review flow integrity, or delivery confidence.

## Team Topologies Classification

Teamlemetry should support these topology classes:

- `STREAM_ALIGNED_TEAM`
- `PLATFORM_TEAM`
- `COMPLICATED_SUBSYSTEM_TEAM`
- `ENABLING_TEAM`

### Stream-Aligned Team

Organized around end-to-end value delivery for a work stream.

Telemetry indicators:

- owns full path from ticket to deployment
- low external dependency count
- stable delivery confidence
- manageable context surface area

### Platform Team

Provides internal services consumed by stream-aligned teams.

Telemetry indicators:

- many downstream consumers
- service request queues
- API or infrastructure dependency edges
- high leverage when self-service reduces cognitive load

### Complicated-Subsystem Team

Owns specialized domains requiring deep expertise.

Telemetry indicators:

- high complexity signal
- narrow but critical dependency edges
- high knowledge concentration risk
- need for secondary contributor coverage

### Enabling Team

Helps other teams adopt new practices, tooling, or capabilities.

Telemetry indicators:

- cross-team interaction
- temporary collaboration spikes
- declining dependency over time if enablement succeeds
- improvement in downstream reliability or review flow

## Interaction Modes

Team interaction modes should evolve deliberately.

Supported modes:

- `COLLABORATION`
- `X_AS_A_SERVICE`
- `FACILITATING`

Telemetry rules:

- Discovery work may start in collaboration mode.
- Stable interfaces should move toward X-as-a-Service.
- Capability-building work should use facilitating mode.
- If coupling rises while review flow integrity falls, the interaction mode is likely wrong.

## Event-To-Metric Mapping

| Telemetry Event | Metric Updates |
| --- | --- |
| `PR_OPENED` | review queue age, WIP, context surface area |
| `PR_REVIEWED` | review flow integrity, review latency, systemic value |
| `PR_MERGED` | throughput, cycle time, delivery chain progression |
| `DEPLOYMENT` | deployment reliability, delivery confidence |
| `ROLLBACK` | reliability penalty, deployment risk, incident coupling |
| `TICKET_BLOCKED` | blocking probability, dependency latency, queue pressure |
| `TICKET_COMPLETED` | throughput, cycle time, sprint delivery movement |
| `INCIDENT_OPENED` | reliability penalty, interruption density, cognitive load |
| `INCIDENT_RESOLVED` | MTTR, recovery reliability, incident queue pressure |
| `QUEUE_SATURATION` | queue pressure, delivery confidence penalty |
| `REVIEW_DELAY` | review flow integrity, blocking probability, synchronization penalty |
| `HANDOFF_DELAY` | dependency latency, synchronization penalty, coordination entropy |
| `BUILD_FAILURE` | reliability penalty, delivery confidence penalty |
| `TEST_FAILURE` | QA reliability, rework risk, delivery fidelity |

## Storage Additions

The V1 storage model should be extended with metric-ready fields or tables for:

- base signal snapshots: `T`, `R`, `C`, `S`, `X`
- coupling metrics
- blocking probability metrics
- synchronization penalty metrics
- centrality concentration metrics
- review flow integrity metrics
- topology health score snapshots
- archetype classifications as explainable derived labels
- interaction mode classifications
- team topology classifications

Suggested additional tables:

- `base_signal_snapshots`
- `structural_metrics`
- `topology_health_scores`
- `node_archetypes`
- `team_topology_classifications`
- `interaction_mode_snapshots`

## Executive Dashboard Cadence

| Signal | Audience | Refresh Rate |
| --- | --- | --- |
| Topology Health Score | CTO / CEO / Board | Monthly |
| Delivery Probability | CTO / COO | Sprint |
| Effective Capacity | CTO / CFO | Monthly |
| Centrality Concentration | CTO | Sprint |
| Reviewer Load Distribution | Engineering Leadership | Weekly |
| Context Surface Area Hotspots | Engineering Leadership | Sprint |
| Timezone Synchronization Penalty | CTO for distributed teams | Monthly |
| Rework Cost Trend | CFO | Monthly |
| Attrition Fragility Index | CTO / CHRO | Quarterly |
| Expected ROI of Proposed Hire | CTO / CFO | Per decision |

## Governance Rules

- Fix systems before judging individuals.
- Compare like with like: role, complexity, tenure, team, and topology context matter.
- Do not confuse load with productivity.
- Do not tie topology diagnostics directly to compensation.
- Engineers should know what telemetry is collected and how it is used.
- Engineers should be able to question model outputs.
- Thresholds must recalibrate as the organization changes.
- AI should be measured by team-level stability gains, not individual output volume.

## Implementation Priority For V1

1. Capture GitHub, Jira, and Slack metadata events.
2. Normalize events into canonical telemetry records.
3. Compute base signals `T`, `R`, `C`, `S`, and `X` for nodes and teams.
4. Compute blocking probability, review flow integrity, and centrality concentration.
5. Compute seed Topology Health Score.
6. Generate evidence-backed topology recommendations.
7. Report sprint-level findings to Slack and the MVP dashboard.
