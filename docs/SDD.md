# System Design Doctrine

Teamlemetry is a telemetry and topology intelligence system. The first implementation phase must prioritize telemetry fidelity, semantic stability, and explainable computation before dashboards, ML, agent swarms, or LLM orchestration.

## V1 Scope

Build only these connectors first:

- GitHub
- Jira
- Slack

These sources are enough to model code flow, work flow, and coordination flow.

## Primary System Question

Where is the delivery chain destabilizing?

Every component must support that question.

## Non-Primary Questions

Teamlemetry must not be optimized around:

- who wrote more code
- who sent more messages
- who closed more tickets
- who appears more active

Activity volume is supporting evidence only when it helps explain topology, queues, or destabilization.

## Ingestion Pipeline

The ingestion pipeline captures external events and converts them into source-faithful telemetry envelopes.

Stages:

1. Connector authentication
2. Event fetch or webhook receive
3. Source payload validation
4. Idempotency check
5. Event envelope creation
6. Raw payload preservation where policy allows
7. Normalized event projection
8. Connector checkpoint update
9. Connector health recording

Design requirements:

- source IDs must be preserved
- ingestion must be idempotent
- retries must not duplicate events
- rate limits must be explicit
- connector failures must be observable
- timestamps must distinguish source time, ingestion time, and processing time

## Event Normalization

Normalized events must preserve source provenance while creating a common telemetry vocabulary.

Required event categories:

- CodeEvent
- ReviewEvent
- IssueEvent
- WorkItemEvent
- CommentEvent
- DeploymentEvent
- IncidentEvent
- CoordinationEvent
- BlockerEvent

Normalization rules:

- never discard source identifiers
- never infer intent from activity alone
- separate raw source facts from model-derived signals
- mark incomplete or low-confidence mappings
- keep connector-specific fields available for audit

## Graph Engine

The graph engine turns normalized telemetry into topology snapshots.

Graph entities:

- nodes
- edges
- edge types
- dependency paths
- ownership boundaries
- queues
- communication routes

Graph outputs:

- dependency graph
- coordination graph
- queue graph
- ownership graph
- delivery-chain graph

Graph constraints:

- graphs must be versioned by snapshot time
- graph edges must explain their evidence
- inferred edges must be distinguishable from explicit edges
- graph computation must tolerate missing connector data

## Probabilistic Engines

Probabilistic engines compute reliability and delivery confidence from topology and telemetry signals.

Inputs:

- node reliability estimates
- queue saturation signals
- coordination entropy
- blocker propagation
- observation latency
- delivery fidelity
- topology risk

Outputs:

- Delivery Confidence
- Sequential Reliability
- Topology Risk
- Confidence Interval
- Destabilizing Node Candidates

Design constraints:

- every score must be explainable
- confidence must degrade when data is missing
- deterministic and stochastic nodes must be handled differently
- no score may be framed as human worth

## Slack Push Architecture

Slack is both a telemetry source and a reporting target.

As a source, Slack provides coordination signals:

- incident channels
- blocker mentions
- unresolved decisions
- high-fragmentation conversations
- operational handoffs

As a reporting target, Slack receives operational summaries:

- delivery chain destabilization reports
- queue saturation alerts
- topology risk summaries
- role recommendation summaries
- AI stabilization candidates

Design constraints:

- Slack reports must be concise
- reports must include evidence links
- alerting must avoid noise
- sensitive data must follow governance policy
- report routes must be tenant-configurable

## Topology Computation

Topology computation combines code, work, and communication telemetry.

Required computations:

- dependency density
- centrality of overloaded nodes
- cyclic dependency detection
- queue bottleneck detection
- ownership ambiguity detection
- blocker propagation paths
- topology collapse risk

Design constraints:

- topology is not an org chart alone
- team and system boundaries must be modeled separately
- recommendations must cite topology evidence

## Recommendation Engine

The recommendation engine produces explainable operational recommendations.

Recommendation categories:

- reduce dependency density
- add role coverage
- clarify ownership
- split overloaded queues
- reduce observation latency
- introduce AI stabilizer
- adjust Slack reporting route

Design constraints:

- no recommendation should imply employee surveillance
- recommendations require evidence
- recommendations must include risk, expected impact, and confidence
- recommendations should be reversible or reviewable where possible

## Deployment Doctrine

Before production deployment, the system must have:

- connector contract tests
- idempotency tests
- queue simulation tests
- graph validation tests
- probabilistic validation tests
- Slack reporting tests
- governance and access-control tests
- health checks for ingestion and processing

## V1 Deferrals

Do not build in V1:

- generalized ML platforms
- LLM orchestration layers
- agent swarms
- fancy dashboards
- employee scoring
- invasive monitoring
- code-volume leaderboards

Telemetry fidelity comes first.
