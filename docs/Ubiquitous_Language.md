# Ubiquitous Language

This document defines the shared semantic contract for Teamlemetry. Product language, domain models, connector events, reports, tests, and AI-agent-readable documentation must use these terms consistently.

## Node

Exact meaning: A node is any observable unit that participates in engineering delivery. A node can be a person, team, repository, service, ticket, review queue, deployment pipeline, incident process, or governance step.

Example: A GitHub repository, a Jira epic, a Slack incident channel, and a deployment workflow can each be nodes.

Mathematical relation: A node has reliability p_i, load L_i, queue utilization rho_i, and dependency edges E_i.

Boundaries: A node is not a measure of human worth. A person node represents role participation and delivery dependency, not personal value.

## Sequential Node

Exact meaning: A sequential node is a node whose output must be completed or stabilized before a downstream node can reliably proceed.

Example: Code review is a sequential node before merge; CI is a sequential node before deployment.

Mathematical relation: P(system) = product(p_i) across dependent sequential nodes.

Boundaries: Parallel work is not sequential unless downstream delivery depends on its output.

## Topology

Exact meaning: Topology is the graph structure of nodes, dependencies, ownership boundaries, queues, communication paths, and delivery routes.

Example: A platform team owning CI/CD, three product squads, and a shared database team form an engineering topology.

Mathematical relation: Topology is represented as G = (V, E), where V are nodes and E are dependency or communication edges.

Boundaries: Topology is not an org chart alone. It includes technical, process, and communication dependencies.

## Topology Collapse

Exact meaning: Topology collapse occurs when the dependency graph can no longer support reliable delivery because critical paths are overloaded, ambiguous, cyclic, or missing ownership.

Example: Every product team requiring approval from one overloaded architect creates topology collapse around that role.

Mathematical relation: Collapse risk rises as dependency density, queue saturation, and coordination entropy increase while node reliability falls.

Boundaries: A delay is not collapse unless it creates systemic instability or repeated delivery failure.

## Sequential Probability

Exact meaning: Sequential probability is the compounded probability of successful delivery through an ordered chain of dependent nodes.

Example: A feature requiring design, implementation, review, CI, security approval, and deployment has sequential probability across those steps.

Mathematical relation: P(system) = product from i=1 to n of p_i.

Boundaries: It applies to dependent chains, not independent events without delivery coupling.

## Dependency Graph

Exact meaning: A dependency graph is the directed graph of upstream and downstream relationships that affect delivery flow.

Example: Service A depends on shared auth, database migrations, and platform deployment approval.

Mathematical relation: G_d = (V, E_d), where each edge u -> v means v depends on u.

Boundaries: A communication mention is not a dependency unless it affects execution, approval, sequencing, or delivery.

## Coordination Entropy

Exact meaning: Coordination entropy is disorder in communication and handoffs that increases uncertainty, rework, or latency.

Example: A ticket with unclear owner, conflicting Slack threads, and missing acceptance criteria has high coordination entropy.

Mathematical relation: Entropy can be approximated by fragmentation count, unresolved decision count, ownership ambiguity, and channel dispersion.

Boundaries: High communication volume alone is not entropy if decisions are clear and state is stable.

## Queue Saturation

Exact meaning: Queue saturation occurs when work arrival rate approaches or exceeds service capacity.

Example: Pull requests waiting for one reviewer while new PRs arrive faster than reviews complete.

Mathematical relation: rho = lambda / mu. As rho approaches 1, W_q grows nonlinearly.

Boundaries: A busy queue is not saturated if wait times remain stable and service capacity exceeds arrivals.

## Observation Latency

Exact meaning: Observation latency is the delay between a destabilizing event and system detection.

Example: A blocked deployment is not reported until the next weekly status meeting.

Mathematical relation: L_obs = t_detected - t_event.

Boundaries: Observation latency is about detection delay, not the total time to fix the issue.

## Delivery Confidence

Exact meaning: Delivery confidence is a probabilistic estimate that a delivery chain will complete successfully under current topology, queue, dependency, and reliability conditions.

Example: A release with passing CI, low review backlog, clear owner, and stable dependencies has higher delivery confidence.

Mathematical relation: Delivery Confidence combines P(system), queue risk, topology risk, and cognitive load signals.

Boundaries: It is not a performance rating for individuals.

## AI Stabilizer

Exact meaning: An AI stabilizer is an AI-assisted mechanism that reduces variance in a delivery chain by standardizing, summarizing, validating, routing, or accelerating repeated work.

Example: An AI-generated release-risk summary that detects missing tests and unresolved blockers before deployment.

Mathematical relation: sigma_AI^2 -> 0 for bounded repeatable processes.

Boundaries: AI stabilizers do not replace governance for high-risk decisions without validation.

## Cognitive Saturation

Exact meaning: Cognitive saturation occurs when a person, role, or team carries too much concurrent context to operate reliably.

Example: One tech lead context-switching across five incidents, reviews, planning meetings, and architecture questions.

Mathematical relation: Saturation risk rises with active work-in-progress, interruption density, dependency centrality, and decision load.

Boundaries: Cognitive saturation measures system load, not intelligence or effort.

## Blocker Propagation

Exact meaning: Blocker propagation is the movement of delay or failure from an upstream blocked node to downstream dependent nodes.

Example: A delayed API contract blocks frontend work, QA planning, and deployment readiness.

Mathematical relation: Propagation follows directed dependency edges and compounds through P(system).

Boundaries: A local issue without downstream impact is not blocker propagation.

## Topology Recommendation

Exact meaning: A topology recommendation is an operational suggestion to reduce instability by changing ownership, queues, role coverage, dependency boundaries, or reporting flows.

Example: Add a secondary reviewer group for a critical repository to reduce review queue saturation.

Mathematical relation: Recommendations target lower dependency density, lower rho, lower entropy, or higher p_i.

Boundaries: Recommendations must not infer personal value or employment decisions from telemetry alone.

## Deterministic Node

Exact meaning: A deterministic node has predictable output under defined inputs and constraints.

Example: A repeatable CI check with stable infrastructure and clear pass/fail rules.

Mathematical relation: Variance approaches zero and p_i approaches 1 when the node is healthy.

Boundaries: Deterministic does not mean infallible; infrastructure drift can reintroduce variance.

## Stochastic Node

Exact meaning: A stochastic node has variable output due to ambiguity, human judgment, incomplete information, or external instability.

Example: Product prioritization with unclear requirements and many stakeholders.

Mathematical relation: Higher variance lowers reliable p_i estimates and widens confidence intervals.

Boundaries: Stochastic does not mean bad. Some high-value work is inherently uncertain.

## Variance Reduction

Exact meaning: Variance reduction is the decrease of unpredictable outcomes in a delivery chain.

Example: Templates, checklists, automated validation, and AI summaries reduce repeated process variance.

Mathematical relation: Lower sigma^2 increases expected delivery reliability and narrows confidence intervals.

Boundaries: Variance reduction must not erase necessary human judgment or creativity.

## Delivery Fidelity

Exact meaning: Delivery fidelity is the degree to which delivered output matches intended requirements, quality constraints, and operational expectations.

Example: A feature that satisfies acceptance criteria, passes tests, and deploys without incident has high delivery fidelity.

Mathematical relation: Fidelity modifies effective p_i because low-quality completion increases downstream failure probability.

Boundaries: Completion count is not fidelity.

## System Reliability

Exact meaning: System reliability is the probability that the full delivery network can produce expected outcomes under current conditions.

Example: A release train with healthy queues, clear dependencies, stable CI, and low incident pressure has higher system reliability.

Mathematical relation: System Reliability = P(system) adjusted by topology, queue, and observation latency risks.

Boundaries: Reliability is about the system, not moral judgment of participants.

## Dependency Density

Exact meaning: Dependency density is the concentration of required dependency edges per node or delivery unit.

Example: A feature requiring approvals from security, platform, data, legal, and architecture has high dependency density.

Mathematical relation: Density = |E| / |V| or local density for a subgraph.

Boundaries: Dependencies are counted when they affect delivery, not when they are merely informational.
