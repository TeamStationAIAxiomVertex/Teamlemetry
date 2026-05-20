# Slack Reporting

Slack reporting turns Teamlemetry model output into concise operational awareness.

Slack is not a vanity metric surface. It is an observation-latency reduction surface.

## Purpose

Slack reporting should answer:

Where is the delivery chain destabilizing right now, and what evidence supports that conclusion?

## Report Types

V1 report types:

- Delivery Chain Risk Report
- Queue Saturation Report
- Topology Risk Report
- Blocker Propagation Report
- Role Coverage Recommendation
- AI Stabilization Candidate Report

## Report Requirements

Every report should include:

- short title
- destabilizing location
- severity or confidence
- evidence links
- affected delivery chain
- recommended action
- model timestamp
- source systems used

## Report Boundaries

Reports must not include:

- employee-worth scores
- loyalty language
- keystroke or invasive monitoring data
- private-channel content outside governed configuration
- unsupported conclusions about intent

## Routing

Reports should route based on configured ownership and governance policy.

Routing inputs:

- team ownership
- repository ownership
- Jira project ownership
- Slack channel mapping
- incident channel mapping
- governance visibility rules

## Noise Control

Slack reports must avoid alert fatigue.

Controls:

- deduplication
- severity thresholds
- cooldown windows
- grouped findings
- evidence-based suppression
- escalation only when risk persists or worsens

## Example Report Shape

Title: Delivery Chain Destabilization Detected

Finding: Review queue saturation in repository payments-api is delaying release chain REL-142.

Evidence:

- 8 open pull requests waiting for review
- p90 review wait time increased from 9 hours to 31 hours
- 4 downstream Jira items blocked by the same review queue

Recommendation:

Add secondary reviewer coverage or split review ownership for payments-api.

Boundary:

This is a queue and topology finding, not an individual performance judgment.

## Slack As Telemetry Source

When configured, Slack also provides coordination signals:

- blockers
- decisions
- incident coordination
- unresolved operational questions
- channel fragmentation

Slack source ingestion must follow governance rules and should prefer configured operational channels over broad workspace scraping.

## Testing Requirements

Slack reporting tests must verify:

- correct routing
- evidence links
- deduplication
- retry idempotency
- policy-safe language
- no surveillance framing
- report rendering under missing data
