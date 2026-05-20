# Telemetry Connectors

Telemetry connectors are the first implementation priority after semantic stabilization.

## V1 Connector Scope

Build only:

- GitHub
- Jira
- Slack

These provide the minimum viable telemetry triangle:

- code flow
- work flow
- coordination flow

## Connector Doctrine

Connectors must preserve source truth. They do not create model conclusions directly.

A connector is responsible for:

- authenticating with the source system
- fetching or receiving source events
- validating payload shape
- preserving source IDs
- preserving source timestamps
- applying idempotency
- handling rate limits
- reporting health
- emitting telemetry envelopes

## GitHub Connector

Primary signals:

- repositories
- branches
- commits
- pull requests
- reviews
- review comments
- checks
- workflows
- deployments
- issues where used

Destabilization indicators:

- stale PRs
- review bottlenecks
- repeated CI failures
- high change coupling
- deployment failure patterns
- ownership concentration

Non-goals:

- ranking developers by commits
- ranking developers by lines changed
- inferring individual productivity from activity volume

## Jira Connector

Primary signals:

- projects
- issues
- epics
- statuses
- transitions
- blockers
- assignees as role participation
- links and dependencies
- comments where governed

Destabilization indicators:

- blocked issue chains
- long status dwell time
- unclear ownership
- dependency cycles
- high work-in-progress
- delivery scope instability

Non-goals:

- using tickets closed as a direct human-performance score
- treating assignee fields as employee-worth indicators

## Slack Connector

Primary signals:

- configured channels
- incident channels
- blocker mentions
- operational reports
- decision threads
- routing signals
- coordination fragmentation signals

Destabilization indicators:

- unresolved decisions
- high channel fragmentation
- repeated blocker escalation
- delayed operational response
- unclear report routing

Non-goals:

- private surveillance
- sentiment scoring for loyalty
- monitoring personal messages outside governed configured surfaces

## Event Envelope

Every connector should emit a common envelope:

- source
- source_event_id
- source_resource_id
- source_actor_id where governed
- source_timestamp
- ingested_at
- event_type
- raw_payload_reference
- normalized_payload
- connector_run_id
- idempotency_key

## Connector Health

Each connector must report:

- last successful run
- last failure
- failure reason
- rate-limit status
- events fetched
- events rejected
- checkpoint position
- webhook delivery status where applicable

## Contract Tests

Each connector requires contract tests for:

- auth failures
- pagination
- rate limits
- malformed payloads
- duplicate events
- timestamp mapping
- source ID preservation
- checkpoint recovery

## Implementation Order

1. GitHub connector
2. Jira connector
3. Slack connector
4. Unified event envelope
5. Graph projection from these three sources

Do not add additional connectors until V1 telemetry fidelity is validated.
