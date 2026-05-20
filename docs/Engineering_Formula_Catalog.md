# Engineering Formula Catalog

This catalog ports the mathematical formulas from the Engineering doctrine source into Teamlemetry as implementation-ready product formulas.

Use this file when building engines, database schemas, tests, dashboards, Slack reports, and CTO-facing explanations.

The formulas model team topology and delivery systems. They must not be used as employee-worth scores, surveillance scores, loyalty scores, or raw productivity leaderboards.

## Component Map

| Formula Area | Teamlemetry Component |
| --- | --- |
| Sequential probability | `ProbabilityEngine`, `TopologyGraphEngine`, Delivery Confidence panel |
| Incentive and node replacement | `RecommendationEngine`, AI Stabilization recommendations |
| Queueing and flow | `QueueingEngine`, Queue Heatmap, Sprint Trend |
| Work inventory and cost of delay | `SprintMetricsEngine`, Delivery Confidence, CFO report |
| Topology and dependency density | `TopologyGraphEngine`, Blocker Propagation, Topology Health |
| Cognitive load and context surface | `CognitiveLoadEngine`, Topology Graph, Recommendation Engine |
| Quality and defect amplification | `DeliveryFidelityEngine`, QA risk panel, test-risk reporting |
| Recovery and failure metrics | `IncidentEngine`, Recovery Velocity, Slack sprint report |
| Governance and fairness | `GovernanceEngine`, model-audit reports |
| Platform economics and ROI | `RecommendationEngine`, CTO/CFO intervention analysis |

## Sequential Probability

Formula:

```text
P_system = product(p_i for i = 1..n)
```

Meaning:

The probability of successful delivery is the compounded reliability of every dependent node in the chain.

Implementation inputs:

- node reliability
- dependency chain order
- topology graph edges
- sprint delivery outcomes
- incident and rollback penalties

Component:

- `ProbabilityEngine.compute_system_reliability(chain_id)`

Dashboard:

- Delivery Confidence
- Topology Graph
- Sprint Report

Boundary:

Do not average dependent reliability when work is sequential. Use multiplication for dependent delivery chains.

## Strict Complementarity

Formula:

```text
p_(k+2) - p_(k+1) > p_(k+1) - p_k
```

Meaning:

Improvements become more valuable when surrounding nodes are reliable. The order of intervention matters.

Implementation inputs:

- marginal reliability improvement
- upstream/downstream dependency position
- topology bottleneck location

Component:

- `RecommendationEngine.rank_interventions()`

Dashboard:

- Recommendations

Use:

Prioritize the intervention that improves system probability fastest, not the most visible activity metric.

## Monolith Chain Risk

Formula:

```text
as N -> infinity, P_system -> 0
```

Meaning:

Long tightly coupled dependency chains collapse delivery probability because every additional required node compounds failure risk.

Implementation inputs:

- dependency depth
- number of required deployment gates
- services touched
- review and approval chain length

Component:

- `TopologyGraphEngine.compute_chain_depth_risk()`

Dashboard:

- Blocker Propagation
- Topology Health

## Deterministic AI Variance

Formula:

```text
sigma_AI^2 -> 0
```

Meaning:

AI is valuable when it reduces variance in bounded repeatable workflows.

Implementation inputs:

- repeated task category
- validation coverage
- error rate before and after automation
- review or triage time before and after automation

Component:

- `AIStabilizationEngine.evaluate_candidate()`

Dashboard:

- AI Stabilization Opportunities

Boundary:

AI stabilization is measured by team-level reliability improvement, not individual output volume.

## Incentive Compatibility

Formula:

```text
expected_utility(effort) >= expected_utility(shirking)
```

Minimum wage equation:

```text
w_i^x = c / (p_n - zeta_i^x)
```

Where:

- `w_i^x` = required incentive for node `i` under policy `x`
- `c` = cost of effort or coordination
- `p_n` = probability of success if the chain works
- `zeta_i^x` = probability the system still succeeds if node `i` shirks

Meaning:

If downstream automation or heroics make failure unlikely, the incentive margin shrinks. Coordination cost and weak accountability can make delivery more expensive even when tooling improves.

Implementation inputs:

- automation coverage
- downstream safety net strength
- handoff latency
- review or QA rescue frequency
- repeated downstream rework

Component:

- `RecommendationEngine.evaluate_ai_insertion_risk()`
- `GovernanceEngine.evaluate_accountability_gap()`

Dashboard:

- AI Stabilization
- Topology Recommendations

Boundary:

Teamlemetry should use this to reason about system incentives and topology, not compensation decisions.

## Shirking Safety

Formula:

```text
zeta_i^x = P(project succeeds | e_i = 0, policy = x)
```

Meaning:

`zeta` is the probability the project succeeds even when a node does not contribute effort. High `zeta` can hide topology problems because downstream nodes absorb the failure.

Implementation inputs:

- downstream rework
- rescue commits
- QA catch rate
- rollback recovery
- repeated handoff fixes

Component:

- `TopologyGraphEngine.compute_hidden_rescue_load()`

Dashboard:

- Topology Health
- Graph Glue and Overloaded Integrator detection

## Replacement Kinetics

Formula:

```text
partial C / partial x_i = DirectSavings - IncentiveDistortion
```

End-node condition:

```text
zeta_n^x = p_(n-1)
```

Meaning:

Replacing or automating end-of-chain work can create clean savings. Replacing middle integration nodes can distort upstream and downstream incentives.

Implementation inputs:

- node position in chain
- downstream dependency count
- upstream dependency count
- validation coverage
- observed rework after automation

Component:

- `AIStabilizationEngine.rank_safe_automation_points()`

Dashboard:

- AI Stabilization Opportunities

## Node Reduction

Formula:

```text
reduce n -> increase P_system when removed handoffs do not reduce p_i
```

Meaning:

The most reliable way to improve sequential delivery is often to reduce required handoffs and dependency length.

Implementation inputs:

- handoff count
- review gates
- approval queues
- dependency chain depth
- duplicate coordination channels

Component:

- `RecommendationEngine.recommend_node_reduction()`

Dashboard:

- Topology Recommendations

## Queue Utilization

Formula:

```text
rho = lambda / mu
```

Where:

- `lambda` = arrival rate
- `mu` = service rate
- `rho` = utilization

Meaning:

Utilization measures how close a queue is to capacity.

Implementation inputs:

- PRs opened per interval
- reviews completed per interval
- tickets entering a status
- tickets leaving a status
- incidents opened and resolved

Component:

- `QueueingEngine.compute_utilization(queue_id)`

Dashboard:

- Queue Heatmap

## Kingman Queue Delay

Formula:

```text
E[W_q] approx (rho / (1 - rho)) * ((C_a^2 + C_s^2) / 2) * tau
```

Where:

- `C_a` = arrival variability
- `C_s` = service-time variability
- `tau` = mean service time

Meaning:

Queue delay grows nonlinearly as utilization approaches 1 and grows further when arrivals or service times are variable.

Implementation inputs:

- arrival timestamps
- service start and end timestamps
- cycle time variance
- review latency variance
- incident resolution variance

Component:

- `QueueingEngine.estimate_wait_time(queue_id)`

Dashboard:

- Queue Heatmap
- Sprint Trend

## Simplified Queue Wait

Formula:

```text
W_q approx rho / (1 - rho)
```

Meaning:

A seed MVP approximation for queue pressure when full Kingman inputs are not yet available.

Component:

- `QueueingEngine.compute_seed_wait_multiplier(queue_id)`

Use:

Use this early, then upgrade to Kingman when arrival and service variance are available.

## Little's Law

Formula:

```text
L = lambda * W
```

Where:

- `L` = work in progress
- `lambda` = throughput
- `W` = lead time

Meaning:

Lead time falls when WIP falls or throughput rises. WIP is a policy lever.

Implementation inputs:

- active tickets
- active PRs
- throughput per sprint
- cycle time

Component:

- `QueueingEngine.compute_wip_law_metrics()`

Dashboard:

- Queue Heatmap
- Sprint Trend

## WIP Rule Of Two

Formula:

```text
WIP_person <= 2
```

Meaning:

Keep active concurrent work bounded to prevent context switching and queue collapse.

Implementation inputs:

- active assigned tickets
- active PRs
- active review requests
- active incidents

Component:

- `CognitiveLoadEngine.compute_wip_pressure()`

Dashboard:

- Cognitive Load
- Queue Heatmap

Boundary:

Show role or topology pressure, not employee policing.

## Work Inventory Carrying Cost

Formula:

```text
C_h = merge_conflict_risk + context_decay + capital_lockup
```

Meaning:

Unshipped code and unfinished tickets are inventory. Inventory carries cost until it reaches production.

Implementation inputs:

- PR age
- branch age
- ticket age
- merge conflicts
- stale reviews
- unreleased completed work

Component:

- `SprintMetricsEngine.compute_inventory_cost()`

Dashboard:

- Sprint Trend
- Delivery Confidence

## Cost Of Delay

Formula:

```text
CoD = partial(Value) / partial(Time)
```

Meaning:

Delay has an economic cost that often exceeds the cost of production.

Implementation inputs:

- planned value where supplied
- target release date
- actual release date
- delay duration
- priority or revenue metadata where governed

Component:

- `RecommendationEngine.compute_delay_impact()`

Dashboard:

- CTO/CFO report
- Recommendations

## Refactor NPV

Formula:

```text
NPV(reduced_future_wait_time) > C_refactor
```

Meaning:

Refactoring is justified when the future reduction in queue delay and variance exceeds the cost of doing it.

Implementation inputs:

- current service-time variance
- projected variance reduction
- frequency of affected work
- engineering cost estimate

Component:

- `RecommendationEngine.evaluate_refactor_candidate()`

Dashboard:

- Recommendations

## Communication Complexity

Formula:

```text
communication_edges = N * (N - 1) / 2
```

Meaning:

Coordination surfaces grow quadratically with participant count.

Implementation inputs:

- active collaborators
- teams involved
- reviewers involved
- channels involved
- services involved

Component:

- `TopologyGraphEngine.compute_coordination_surface()`

Dashboard:

- Coordination Entropy
- Topology Graph

## Dependency Density

Formula:

```text
dependency_density = |E| / |V|
```

Meaning:

The number of dependency edges per node in the graph.

Implementation inputs:

- graph nodes
- dependency edges
- service dependencies
- ticket dependencies
- review dependencies

Component:

- `TopologyGraphEngine.compute_dependency_density()`

Dashboard:

- Topology Health
- Blocker Propagation

## Blocking Probability

Formula:

```text
P_blocked = 1 - (1 - p_wait)^d
```

Where:

- `p_wait` = probability a dependency is late or unavailable
- `d` = active dependency count

Meaning:

Blocking risk rises nonlinearly as active dependencies increase.

Implementation inputs:

- blocked ticket count
- unresolved dependency count
- late dependency rate
- handoff delay
- stale review count

Component:

- `TopologyGraphEngine.compute_blocking_probability(node_id)`

Dashboard:

- Blocker Propagation

## Asynchronous Amplifier

Formula:

```text
sync_delay = missed_sync_windows * sync_window_size
```

Seed distributed-team approximation:

```text
missed_sync_window -> +24h latency
```

Meaning:

Missing an async handoff window can quantize work into day-sized delays.

Implementation inputs:

- review request timestamp
- first response timestamp
- timezone overlap window
- Slack escalation metadata
- handoff timestamps

Component:

- `CoordinationEntropyEngine.compute_async_delay()`

Dashboard:

- Coordination Entropy
- Sprint Trend

## Synchronization Penalty

Formula:

```text
S_p = sum(T_wait + T_context_switch)
```

Meaning:

Distributed work accumulates delay from waiting and context switching.

Component:

- `CoordinationEntropyEngine.compute_synchronization_penalty()`

Dashboard:

- Topology Health
- Sprint Trend

## Review Flow Integrity

Formula:

```text
RFI = 1 - Gini(review_load)
```

Meaning:

Healthy review systems distribute review load. Low RFI means review authority is collapsing into choke points.

Implementation inputs:

- reviews requested by reviewer
- reviews completed by reviewer
- review latency
- approval distribution
- rejection distribution

Component:

- `QueueingEngine.compute_review_flow_integrity()`

Dashboard:

- Queue Heatmap
- Choke-Point Reviewer detection

## Coupling Coefficient

Formula:

```text
K_c(i, j) = corr(T_i, T_j)
```

Meaning:

Measures whether output movement between nodes is tied together.

Implementation inputs:

- throughput time series per node
- sprint output vectors
- detrended seasonal effects
- optional lead/lag offsets

Component:

- `TopologyGraphEngine.compute_coupling()`

Dashboard:

- Topology Graph
- Topology Health

## Centrality Concentration

Formula:

```text
CC = sum(S_i for top k nodes) / sum(S_i for all nodes)
```

Meaning:

Measures whether too much system value flows through too few nodes.

Implementation inputs:

- systemic value score
- betweenness centrality
- review centrality
- service ownership concentration

Component:

- `TopologyGraphEngine.compute_centrality_concentration()`

Dashboard:

- Topology Health
- Graph Glue detection

## Throughput Signal

Formula:

```text
T = 0.6 * V_issue + 0.4 * O_code
```

Meaning:

Throughput is accepted movement, not raw activity.

Implementation inputs:

- completed tickets
- accepted PRs
- merged work
- deployment-linked work
- reopen discount
- rejection discount

Component:

- `SprintMetricsEngine.compute_throughput_signal()`

Dashboard:

- Sprint Trend

## Reliability Signal

Formula:

```text
R = 1 - CV(v)
CV(v) = sigma_v / mu_v
```

Meaning:

Reliability is consistency across delivery intervals.

Implementation inputs:

- sprint output vector
- delivery variance
- reopen rate
- rollback rate
- review rejection rate
- incident coupling

Component:

- `ReliabilityEngine.compute_reliability_signal()`

Dashboard:

- Delivery Confidence

## Context Surface Area

Formula:

```text
C = repos * sqrt(collaborators) * (1 + cross_team_fraction)
```

Meaning:

Context surface area measures load exposure and coordination burden.

Implementation inputs:

- repositories touched
- collaborators
- cross-team work fraction
- service ownership count
- PR review fanout
- active dependencies

Component:

- `CognitiveLoadEngine.compute_context_surface_area()`

Dashboard:

- Cognitive Load
- Topology Health

## Systemic Value

Formula:

```text
S = 0.4 * B + 0.3 * RR + 0.3 * BF
```

Where:

- `B` = betweenness centrality
- `RR` = balanced review ratio
- `BF` = bus-factor exposure contribution

Meaning:

Systemic value identifies graph-critical stabilizers and bridge roles.

Component:

- `TopologyGraphEngine.compute_systemic_value()`

Dashboard:

- Topology Graph
- Graph Glue detection

Boundary:

This is not a leaderboard. It is a fragility and continuity signal.

## Complexity Signal

Formula:

```text
X = 0.5 * P + 0.3 * CT + 0.2 * RJ
```

Where:

- `P` = normalized points or work-unit difficulty
- `CT` = cycle time
- `RJ` = rejection or rework rate

Meaning:

Complexity prevents false conclusions from raw throughput comparisons.

Component:

- `SprintMetricsEngine.compute_complexity_signal()`

Dashboard:

- Delivery Fidelity
- Sprint Trend

## Topology Health Score

Formula:

```text
THS = w1 * R_avg
    + w2 * (1 - C_norm_avg)
    + w3 * S_distribution
    + w4 * (1 - K_c_positive_avg)
    + w5 * (1 - P_blocked_avg)
    + w6 * (1 - S_p_norm_avg)
```

Seed weights:

```text
w1 = 0.25  reliability
w2 = 0.20  load balance / context burden
w3 = 0.15  systemic value distribution
w4 = 0.15  coupling risk
w5 = 0.15  blocking risk
w6 = 0.10  synchronization penalty
```

Meaning:

Executive topology health signal.

Component:

- `TopologyHealthEngine.compute_score()`

Dashboard:

- Topology Health
- CTO summary

## Delivery Confidence

Formula:

```text
delivery_confidence =
  system_reliability
  - queue_penalty
  - cognitive_penalty
  - coordination_entropy
  - delivery_fidelity_penalty
```

Meaning:

CTO-facing probability-informed delivery score.

Component:

- `DeliveryConfidenceEngine.compute_score()`

Dashboard:

- Delivery Confidence
- Slack Sprint Report

## Effective Capacity

Formula:

```text
Cap_eff = sum(T_i * R_i * (1 - C_penalty_i) * (1 - Block_penalty_i))
```

Meaning:

Usable capacity under variance, load, and blocking pressure.

Component:

- `CapacityEngine.compute_effective_capacity()`

Dashboard:

- Sprint Trend
- CTO/CFO report

## Program Delivery Probability

Formula:

```text
P_delivery = P_base * THS * A_architecture * G_governance
```

Meaning:

Executive delivery probability adjusted by topology, architecture, and governance.

Component:

- `DeliveryConfidenceEngine.compute_program_probability()`

Dashboard:

- Delivery Confidence

## Hiring / Topology Intervention Value

Formula:

```text
NTV = Delta_THS + Delta_P_success - Delta_Cost_annualized
```

Node benefit condition:

```text
Benefit_node > CoordinationTax_node
```

Expanded:

```text
R * F * T * S_fit > E_new + D_new + Sync_new
```

Meaning:

Evaluate a new role or topology change by expected network impact, not title.

Component:

- `RecommendationEngine.evaluate_topology_intervention()`

Dashboard:

- Recommendations

## AI Insertion Value

Formula:

```text
V_AI(x) = G_automation(x) - D_coordination(x) - R_opacity(x)
```

Meaning:

AI should reduce friction without creating opacity or hidden coupling.

Component:

- `AIStabilizationEngine.compute_insertion_value()`

Dashboard:

- AI Stabilization Opportunities

## Cognitive Fidelity

Formula:

```text
quality = P(M_engineer is isomorphic to S_system)
```

Meaning:

Quality depends on whether the mental model matches actual system state.

Implementation inputs:

- review accuracy
- defect escape
- root-cause correctness
- architectural dependency mapping
- test coverage against real failure modes

Component:

- `DeliveryFidelityEngine.compute_cognitive_fidelity_proxy()`

Dashboard:

- Delivery Fidelity

Boundary:

In Teamlemetry this must remain a team/system proxy from delivery evidence, not psychometric scoring of employees.

## L2-Aware Semantic Stability

Formula:

```text
abs(c_q - c_q_prime) < tau_trans
```

Where:

- `c_q` = original content score
- `c_q_prime` = translated or normalized counterfactual score
- `tau_trans` = allowed translation drift threshold

Meaning:

Model outputs should remain stable when language form changes but semantic content remains equivalent.

Component:

- `GovernanceEngine.validate_model_language_bias()`

Dashboard:

- Model Audit

## Adversarial Indistinguishability

Formula:

```text
AUC_adversary approx 0.5
```

Meaning:

Sensitive demographic or linguistic background should not be predictable from normalized scores.

Component:

- `GovernanceEngine.validate_fairness()`

Dashboard:

- Trust / Governance

Boundary:

Use only where governed, necessary, and privacy-safe.

## Defect Amplification

Formula:

```text
defect_cost = base_fix_cost * detection_stage_multiplier
```

Seed stage multipliers:

```text
design = 1x
development = 10x
integration = 100x
production = 1000x
```

Meaning:

Defect cost grows as detection moves later in the delivery chain.

Implementation inputs:

- defect discovery stage
- escaped defects
- rework duration
- incident severity

Component:

- `DeliveryFidelityEngine.compute_defect_amplification()`

Dashboard:

- Quality Risk
- Sprint Report

## Availability

Formula:

```text
A = MTBF / (MTBF + MTTR)
```

Equivalent long-run form:

```text
A = E[Uptime] / (E[Uptime] + E[Downtime])
```

Meaning:

Availability is improved by making failures rarer or recovery faster.

Component:

- `IncidentEngine.compute_availability()`

Dashboard:

- Recovery Velocity

## MTTR Limit

Formula:

```text
lim(MTTR -> 0) MTBF / (MTBF + MTTR) = 1
```

Meaning:

Fast recovery can drive availability toward 1 even when failure is not eliminated.

Component:

- `IncidentEngine.compute_recovery_leverage()`

Dashboard:

- Recovery Velocity
- Recommendations

## Restoration Loop

Formula:

```text
MTTR = TTD + TTDiag + TTM
```

Where:

- `TTD` = time to detection
- `TTDiag` = time to diagnosis
- `TTM` = time to mitigation

Meaning:

Recovery delay decomposes into detection, diagnosis, and mitigation.

Implementation inputs:

- incident opened timestamp
- alert timestamp
- root cause identified timestamp
- mitigation timestamp
- incident resolved timestamp

Component:

- `IncidentEngine.compute_restoration_loop()`

Dashboard:

- Recovery Velocity

## Mean Time To Innocence

Formula:

```text
MTTI = time_spent_proving_non-ownership_before_resolution
```

Meaning:

MTTI is negative work spent proving fault boundaries instead of restoring service.

Implementation inputs:

- incident handoffs
- team reassignment count
- time before accountable owner
- Slack escalation metadata
- root-cause ownership churn

Component:

- `IncidentEngine.compute_mtti_proxy()`

Dashboard:

- Failure / Governance

Boundary:

Measure silo friction, not blame.

## Permission Gap

Formula:

```text
permission_gap = organizational_MTTR - technical_MTTR
```

Meaning:

The difference between how long mitigation technically takes and how long the organization takes because authority is missing.

Implementation inputs:

- first identified mitigation timestamp
- actual mitigation timestamp
- permission escalation events
- rollback authority metadata

Component:

- `GovernanceEngine.compute_permission_gap()`

Dashboard:

- Recovery Velocity
- Recommendations

## Batch Size Variance

Formula:

```text
sigma_outcome^2 proportional_to B^2
```

Meaning:

Large deployment batches create nonlinear risk; small batches reduce variance and improve reversibility.

Implementation inputs:

- commits per deploy
- files changed per deploy
- services changed per deploy
- deployment failure rate
- rollback rate

Component:

- `DeliveryFidelityEngine.compute_batch_risk()`

Dashboard:

- Delivery Confidence
- Recovery Velocity

## Cognitive Load Under Incident Pressure

Formula:

```text
B_L -> infinity
```

Meaning:

Emergency hotfixes under pressure increase cognitive load and secondary-defect risk.

Implementation inputs:

- incident severity
- hotfix commits
- time since incident opened
- failed hotfix attempts
- missing rollback path

Component:

- `IncidentEngine.compute_hotfix_risk()`

Dashboard:

- Recovery Velocity
- Recommendations

## Platform Scaling

Formula:

```text
service_model_growth = O(headcount)
platform_model_growth = O(network_effects)
```

Meaning:

Service models scale linearly with people; platform models can scale through reusable capabilities and data network effects.

Implementation inputs:

- repeated manual workflows
- self-service adoption
- platform request queues
- automation reuse
- manual support reduction

Component:

- `RecommendationEngine.evaluate_platform_leverage()`

Dashboard:

- Recommendations

## Architecture Complexity Tax

Formula:

```text
C_arch_tax = alpha * N_s + beta * E_d + gamma * H_c
```

Where:

- `N_s` = number of independently deployed services
- `E_d` = dependency edges
- `H_c` = heterogeneity coefficient across stacks

Meaning:

Architecture cost rises with service count, dependency edges, and stack heterogeneity.

Implementation inputs:

- service inventory
- dependency graph
- language/framework distribution
- deployment units

Component:

- `TopologyGraphEngine.compute_architecture_tax()`

Dashboard:

- Topology Health
- Recommendations

## Rework Cost

Formula:

```text
C_rework = DefectRate * AvgFixCost * SeverityMultiplier
```

Meaning:

Rework cost is a function of defects, fix effort, and severity.

Implementation inputs:

- reopen rate
- defects
- fix cycle time
- severity
- incident linkage

Component:

- `DeliveryFidelityEngine.compute_rework_cost()`

Dashboard:

- Quality Risk
- CFO report

## Attrition Fragility

Formula:

```text
C_attrition = P_attrition * (C_backfill + C_ramp_loss + C_knowledge_loss)
```

Meaning:

The cost of losing graph-critical roles includes backfill, ramp loss, and knowledge fragmentation.

Implementation inputs:

- centrality concentration
- bus-factor exposure
- documentation coverage
- unique service ownership
- secondary owner presence

Component:

- `TopologyGraphEngine.compute_attrition_fragility_proxy()`

Dashboard:

- Topology Health

Boundary:

Use as topology continuity risk, not employee retention prediction unless explicitly governed.

## ROI

Formula:

```text
ROI = (V_gained + C_avoided - C_program) / C_program
```

Value gained:

```text
V_gained = CoD * Delta_t_saved
```

Cost avoided:

```text
C_avoided = C_mis_hire + C_rework_reduction + C_attrition_prevention + C_incident_reduction
```

Meaning:

Teamlemetry recommendations should explain expected economic value where data is available.

Component:

- `RecommendationEngine.compute_intervention_roi()`

Dashboard:

- Recommendations
- CTO/CFO report

## Formula Implementation Order

1. Implement telemetry-safe formulas first: `P_system`, `rho`, `W_q`, `L`, `T`, `R`, `C`, `P_blocked`, `RFI`.
2. Add topology formulas next: dependency density, communication complexity, coupling, centrality concentration, THS.
3. Add recovery formulas: availability, MTTR decomposition, permission gap, batch-size variance, rework cost.
4. Add recommendation economics: effective capacity, intervention value, CoD, ROI, platform leverage.
5. Add governance formulas only after policy, consent, and privacy constraints are explicit.

## Test Requirement

Every formula must have:

- deterministic unit tests
- example input fixtures
- expected output
- bounds checks
- missing-data behavior
- explanation text for the dashboard
- a non-surveillance boundary test where people-related signals are involved

