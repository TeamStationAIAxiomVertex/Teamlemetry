# CTO Interface Source Map

Teamlemetry may use the TeamStation Engineering doctrine site as an approved source of interface language, page taxonomy, visual patterns, and CTO-facing explanatory content.

Source repository:

```text
TeamStationAIAxiomVertex/Engineering
```

Use this source to help CTOs build and understand the Teamlemetry interface for free. Do not treat this as a code dependency until a deliberate implementation step imports or packages reusable assets.

## Source Truth Checked

The Engineering repo is a Vite/React doctrine site with structured data and reusable components.

Observed source areas:

- `data/index.ts`: aggregates doctrine content by category.
- `types.ts`: defines `DoctrineCategory`, `SubPageContent`, and `PillarContent`.
- `data/teams/*`: sequential probability, incentives, replacement kinetics, economics, regulation, agentic workflows, and math.
- `data/work/*`: stochastic queueing, inventory/liability, economics, regulation, and flow doctrine.
- `data/decisions/*`: vector-space decisions and latent signal doctrine.
- `data/quality/*`: cognitive fidelity, validation, quality economics, and testing doctrine.
- `data/integration/*`: interface invariant, dependency density, asynchronous amplifier, and topology.
- `data/change/*`: platform economics, transformation, architecture, service model, and future horizons.
- `data/failure/*`: warm body compromise, blameless retrospectives, recovery metrics, failure orientation, and mean time to innocence.
- `components/LandingPage.tsx`: executive mission brief, seven-pillar cards, and first-screen CTO language.
- `components/visualizations/DistributedOSMap.tsx`: layered distributed engineering operating system visual model.
- `components/visualizations/ThroughputEquation.tsx`: throughput-as-function visual model.

## Interface Principle

Use Engineering site data to explain Teamlemetry's math and recommendations to CTOs without making the interface feel like a surveillance tool.

The UI must frame metrics as:

- team topology health
- delivery reliability
- queue pressure
- cognitive load exposure
- coordination cost
- integration risk
- recovery velocity
- governance readiness

The UI must not frame metrics as:

- employee worth
- individual productivity policing
- keystroke activity
- loyalty scoring
- raw commit leaderboards
- message-content monitoring

## Seven Pillars To Interface Areas

| Engineering Pillar | Teamlemetry Interface Area | Primary Use |
| --- | --- | --- |
| Teams | Topology Graph and Sequential Reliability | Explain nodes, probability chains, gateway nodes, and topology fragility. |
| Work | Queue Heatmap and Flow Metrics | Explain utilization, WIP, queue saturation, code inventory, and flow collapse. |
| Decisions | Recommendation Evidence and Governance | Explain why recommendations are vector and evidence based, not keyword or opinion based. |
| Quality | Delivery Fidelity and Test Risk | Explain cognitive fidelity, rework risk, validation latency, defect amplification, and test confidence. |
| Integration | Dependency Graph and Blocker Propagation | Explain interface risk, dependency density, async delay, and cross-service topology. |
| Change | Platform Economics and Intervention ROI | Explain topology changes, operating leverage, AI insertion, and platform-vs-service economics. |
| Failure | Incident, MTTR, MTTI, and Recovery Metrics | Explain blameless recovery, root-cause latency, rollback readiness, and resilience. |

## Dashboard Component Mapping

## Topology Graph

Source inspiration:

- `data/teams/*`
- `data/integration/topology.ts`
- `components/visualizations/DistributedOSMap.tsx`

Teamlemetry data:

- engineering nodes
- upstream and downstream edges
- gateway nodes
- centrality concentration
- coupling coefficient
- topology health score

CTO-facing output:

- where the delivery network is fragile
- which node or interface is load-bearing
- whether team structure matches work topology

## Delivery Confidence

Source inspiration:

- `data/teams/sequential.ts`
- `data/teams/math.ts`
- `docs/Team_Topology_Method_Data_Model.md`

Teamlemetry data:

- sequential reliability
- node reliability
- delivery confidence
- queue penalty
- cognitive penalty
- coordination entropy

CTO-facing output:

- probability of delivery under current topology
- main reliability reducer
- trend since prior sprint

## Queue Heatmap

Source inspiration:

- `data/work/*`
- `components/visualizations/ThroughputEquation.tsx`

Teamlemetry data:

- utilization
- WIP
- PR review delays
- blocked tickets
- merge queue age
- sprint rollover

CTO-facing output:

- where utilization is approaching collapse
- which queue needs capacity or policy intervention
- whether WIP limits are being violated

## Blocker Propagation

Source inspiration:

- `data/integration/dependency.ts`
- `data/integration/async.ts`
- `data/failure/metrics.ts`

Teamlemetry data:

- dependency edges
- blocked tickets
- handoff delay
- review delay
- incident coupling
- async coordination lag

CTO-facing output:

- upstream blocker source
- downstream blast radius
- dependency latency by interface

## Recommendations

Source inspiration:

- `data/change/*`
- `data/decisions/*`
- `data/teams/regulation.ts`

Teamlemetry data:

- topology health delta
- blocking probability
- centrality concentration
- review flow integrity
- effective capacity
- role coverage gaps

CTO-facing output:

- recommended topology change
- evidence behind the recommendation
- expected effect on reliability, queue pressure, or delivery confidence
- implementation risk

## Sprint Trend

Source inspiration:

- `data/work/economics.ts`
- `data/failure/metrics.ts`
- `components/LandingPage.tsx` executive framing

Teamlemetry data:

- sprint-to-sprint reliability vector
- throughput trend
- delivery confidence trend
- queue saturation trend
- incident and rollback trend
- rework and reopen trend

CTO-facing output:

- whether topology is stabilizing or degrading
- which trend requires executive action
- whether intervention improved the system

## Interface Language Rules

Use language like:

- Delivery Confidence
- Topology Health
- Queue Saturation
- Dependency Drag
- Coordination Entropy
- Review Flow Integrity
- Recovery Velocity
- Effective Capacity
- Platform Leverage
- Blameless Recovery

Avoid language like:

- employee score
- low performer
- productivity rank
- surveillance
- loyalty
- message quality
- commit leaderboard

## Free CTO Interface Seed

The Engineering site content can seed:

- dashboard empty states
- metric explanations
- tooltip definitions
- executive report copy
- onboarding walkthroughs
- docs pages inside Teamlemetry
- public educational pages explaining the model
- demo content for first-time CTO users

## Implementation Notes

When Teamlemetry implementation starts:

1. Extract doctrine taxonomy from the Engineering repo into a normalized content registry.
2. Map each pillar to a Teamlemetry dashboard module.
3. Reuse the visual vocabulary only where it clarifies operational state.
4. Keep live product UI dense, quiet, and operational; do not turn the dashboard into a marketing page.
5. Keep doctrine content as explanatory layers, tooltips, reports, and documentation rather than the primary workflow surface.
6. Ensure every metric shown in the interface is backed by telemetry and evidence links.
7. Keep the non-surveillance boundary visible wherever people-related topology signals appear.

## Current Boundary

This document approves use of Engineering site data for Teamlemetry interface design and content. It does not copy the Engineering codebase into Teamlemetry and does not make Teamlemetry dependent on the Engineering repo at runtime.
