# V1 System Specification

This document is the build contract for the first working version of Teamlemetry.

The goal of V1 is not AI magic.

The goal of V1 is to convert real sprint telemetry into mathematical topology intelligence.

## Core Execution Model

Teamlemetry continuously ingests engineering events.

Each event updates:

- node reliability
- queue pressure
- dependency latency
- coordination entropy
- delivery confidence

The system turns real sprint telemetry into a live engineering probability graph.

## Primary Product Question

The system must answer:

Where is the delivery chain destabilizing?

It must not answer:

Who wrote more code?

That distinction is existential to the product.

## V1 Architecture

```text
GitHub / GitLab / Bitbucket
        |
Jira / ClickUp / Monday
        |
Slack + CI/CD
        |
Telemetry Collectors
        |
Event Normalization Engine
        |
Topology Graph Engine
        |
Probability Engine
        |
Queueing Engine
        |
Recommendation Engine
        |
Slack Reporting + Dashboard
```

## Phase Priority

### Phase 1

- telemetry ingestion
- normalized events
- graph engine

### Phase 2

- sequential probability
- queueing metrics
- delivery confidence

### Phase 3

- recommendations
- topology intelligence
- Slack reporting

### Phase 4

- AI stabilization
- predictive forecasting
- simulations

Phase 4 must not start before V1 telemetry fidelity is working.

## Step 1: Raw Telemetry Ingestion

V1 collectors must prioritize real metadata and events from engineering systems.

### GitHub Connector

Collect:

- PR opened
- PR merged
- PR review latency
- review approvals
- review rejection
- commit timestamps
- deployment tags
- rollback commits
- branch churn
- file ownership overlap

Derived signals:

- review queue age
- review approval ratio
- rejection ratio
- branch churn rate
- ownership overlap density
- deployment stability

### GitLab And Bitbucket Later

GitLab and Bitbucket are part of the broader architecture, but V1 implementation should start with GitHub before adding parallel code-host connectors.

### Jira / ClickUp / Monday Connector

V1 implementation should start with Jira, then generalize to ClickUp and Monday after the normalization model is stable.

Collect:

- sprint start and end
- ticket status transitions
- blocked state duration
- reopen frequency
- assignee changes
- story point drift
- overdue tickets
- cycle time
- WIP count

Derived signals:

- blocker duration
- status dwell time
- rollover risk
- cycle-time trend
- scope instability
- work-in-progress pressure

### Slack Connector

Collect metadata and operational events only.

Collect:

- blocker notifications
- incident alerts
- interruption density
- escalation frequency
- coordination channels
- timezone coordination lag

Do not collect message content for V1 topology scoring.

Slack ingestion must be legally and culturally safe. The connector should use metadata, event types, timestamps, channels, routing, reactions where governed, and thread-level state where configured. It must not perform broad private-message surveillance.

### CI/CD Connector

CI/CD is part of the V1 architecture but should be attached after the core GitHub, Jira, and Slack telemetry loop is functional.

Collect:

- build failures
- test failures
- deployment events
- failed deploys
- rollback events
- MTTR where available

## Step 2: Event Normalization

External systems have different formats. Teamlemetry must use a unified event schema.

### Core Event Model

```python
class TelemetryEvent:
    event_id: str
    timestamp: datetime
    source: str
    team_id: str
    engineer_id: str | None
    sprint_id: str | None
    event_type: str
    metadata: dict
```

Field rules:

- `event_id` must be globally unique and idempotent.
- `timestamp` must represent the source event time when available.
- `source` must preserve the external system name.
- `team_id` must identify the modeled team or tenant team boundary.
- `engineer_id` is optional and must represent role participation, not human worth.
- `sprint_id` is optional because not all events belong to a sprint.
- `event_type` must use the canonical event enum.
- `metadata` must preserve source IDs, links, and metric inputs needed for audit.

### Canonical Event Types

- `PR_OPENED`
- `PR_REVIEWED`
- `PR_MERGED`
- `DEPLOYMENT`
- `ROLLBACK`
- `TICKET_BLOCKED`
- `TICKET_COMPLETED`
- `INCIDENT_OPENED`
- `INCIDENT_RESOLVED`
- `QUEUE_SATURATION`
- `REVIEW_DELAY`
- `HANDOFF_DELAY`
- `BUILD_FAILURE`
- `TEST_FAILURE`

### Normalization Requirements

- preserve source identifiers
- preserve source URLs where allowed
- preserve source timestamps
- mark missing fields explicitly
- separate source facts from derived model signals
- make retries idempotent
- reject malformed payloads with observable error reasons

## Step 3: Topology Graph Engine

The topology graph engine is the heart of V1.

Teamlemetry must model the engineering organization as a dependency graph.

### Graph Model

```python
class EngineeringNode:
    node_id: str
    node_type: str
    reliability_score: float
    cognitive_load: float
    queue_pressure: float
    upstream_nodes: list[str]
    downstream_nodes: list[str]
```

### Node Types

- `ARCHITECTURE`
- `BACKEND`
- `FRONTEND`
- `QA`
- `DEVOPS`
- `PLATFORM`
- `DATA`
- `MOBILE`
- `SECURITY`
- `INTEGRATION`
- `PRODUCT`

### Graph Requirements

- Directed edges must point from upstream dependency to downstream dependent node.
- Explicit dependencies must be marked separately from inferred dependencies.
- Graph snapshots must be versioned by time and sprint.
- Node scores must be recomputable from stored telemetry events.
- Missing telemetry must reduce confidence rather than crash computation.

## Step 4: Sequential Probability Engine

The sequential probability engine is the mathematical moat.

### Core Equation

```text
P(system) = product from i=1 to n of p_i
```

Each node gets a reliability score. Reliability is derived from sprint telemetry.

### Backend Reliability Inputs

Derived from:

- rollback frequency
- reopen rate
- review rejection rate
- incident generation
- deployment stability

### QA Reliability Inputs

Derived from:

- escaped defects
- regression failures
- flaky test frequency
- validation latency

### Deployment Reliability Inputs

Derived from:

- failed deploys
- rollback rate
- incident coupling
- MTTR

### Probability Requirements

- Never average dependent reliability where sequential probability is required.
- Emit the node contributions that explain the system score.
- Include confidence when data is sparse.
- Avoid person-level scoring language.

## Step 5: Queueing Engine

Most engineering organizations collapse because utilization saturation creates unstable queues.

### Queue Formula

```text
W_q approx rho / (1 - rho)
```

Where:

```text
rho = utilization
```

### Utilization Calculation

```text
utilization = active_work_time / available_capacity
```

### Signals Of Saturation

- PR review delays
- blocked tickets
- escalating WIP
- sprint rollover
- incident spikes
- merge queue buildup

### Queue Requirements

- Detect near-saturation before total failure.
- Distinguish short spikes from sustained queue risk.
- Track queue pressure by node, team, sprint, and source.
- Prevent duplicate events from inflating utilization.

## Step 6: Cognitive Load Model

Cognitive load is a differentiator for Teamlemetry.

### Inputs

- concurrent repositories
- meeting density where available and governed
- PR review fanout
- service ownership count
- interruptions per hour
- cross-team dependencies
- Slack escalation frequency
- context switching

### Cognitive Saturation Score

```text
cognitive_load = repos_owned + dependency_count + active_interruptions + review_requests
```

This should be normalized later by team, role, capacity, sprint length, and available telemetry quality.

### Boundary

Cognitive load is a system signal. It must not be presented as intelligence, effort, loyalty, or employee worth.

## Step 7: Delivery Confidence

Delivery confidence is the CTO-facing metric.

### Formula

```text
delivery_confidence = system_reliability - queue_penalty - cognitive_penalty - coordination_entropy
```

### Requirements

Delivery confidence must include:

- system reliability
- queue penalty
- cognitive penalty
- coordination entropy
- trend
- explanation
- key destabilizing nodes
- source evidence

## Step 8: Topology Recommendation Engine

The topology recommendation engine is the main product differentiator.

It must infer missing or overloaded organizational nodes from telemetry patterns.

### Example Pattern

If:

- frontend is repeatedly blocked
- backend PR latency is rising
- integration incidents are increasing
- deployment instability is spreading

Then generate:

Recommended Role:

Senior Platform Integration Engineer

Reason:

Cross-service coordination entropy exceeded threshold.

Observed Signals:

- integration latency rising
- API drift increasing
- frontend blocker propagation detected
- deployment instability coupled to backend reviews

### Recommendation Requirements

Every recommendation must include:

- recommended topology change
- reason
- observed signals
- affected nodes
- expected impact
- confidence
- evidence links
- non-surveillance boundary language when relevant

## Step 9: Slack Reporting

Slack reporting must exist early because executives and engineering leaders live in Slack.

### Example Report

```text
TEAMLEMETRY - SPRINT REPORT

Sprint: 104

Delivery Confidence: 74%
Trend: down 8%

Sequential Reliability:
0.71

Queue Saturation:
QA exceeded 86% utilization

Coordination Entropy:
Rising in Integration topology

Observed Risks:
- PR review latency exceeded 11h
- Backend -> Frontend blocker propagation
- Deployment rollback probability increasing

Recommendation:
Add:
Senior QA Automation Engineer

Expected Impact:
+14% deployment stability
-22% blocker propagation
```

### Slack Requirements

- report by sprint
- include delivery confidence
- include trend
- include sequential reliability
- include queue saturation
- include coordination entropy
- include observed risks
- include recommendation
- avoid message-content surveillance
- include source links where governed

## Step 10: Storage Model

V1 should use PostgreSQL.

Required tables:

- `telemetry_events`
- `engineering_nodes`
- `sprint_metrics`
- `queue_metrics`
- `topology_snapshots`
- `recommendations`
- `incidents`
- `delivery_scores`

### Storage Requirements

- Store raw event envelopes or governed raw payload references.
- Store normalized event payloads.
- Store graph snapshots as reproducible model outputs.
- Store model version on computed records.
- Store evidence references for recommendations.
- Index by team, sprint, source, event type, and timestamp.

## Step 11: Initial API Endpoints

V1 API endpoints:

```text
GET /topology/current
GET /delivery/confidence
GET /queue/saturation
GET /recommendations
GET /sprint/report
POST /connectors/github
POST /connectors/jira
POST /connectors/slack
```

### API Requirements

- GET endpoints must return explainable model output, not opaque scores.
- POST connector endpoints must validate payloads and preserve idempotency.
- API responses must preserve the distinction between source facts and derived signals.
- Access control must prevent cross-tenant data exposure.

## Step 12: MVP Dashboard

Do not overbuild the dashboard.

Only include:

- topology graph
- delivery confidence
- queue heatmap
- blocker propagation
- recommendations
- sprint trend

Dashboard requirements:

- show system-level destabilization
- show node and queue risk
- show evidence-backed recommendations
- avoid employee leaderboard patterns
- avoid vanity activity metrics

## Existential Product Boundary

Teamlemetry is not:

- productivity surveillance
- employee scoring
- keystroke tracking
- activity policing

Teamlemetry is:

- organizational systems modeling
- dependency physics
- delivery stability analysis
- topology intelligence
- queueing collapse detection
- engineering governance

## Future State

The CTO should eventually ask:

What topology change increases delivery reliability fastest?

Teamlemetry should mathematically answer from real sprint telemetry.
