# Cognitive Load

Cognitive load is the active context burden carried by a person, role, or team inside the delivery system.

## Core Thesis

Reliability declines when critical nodes carry more concurrent context than they can process safely.

Cognitive saturation is a system risk, not a personal defect.

## Load Sources

Teamlemetry may model cognitive load from:

- active work-in-progress
- review obligations
- incident interruptions
- cross-team dependencies
- unresolved decisions
- Slack context fragmentation
- ownership ambiguity
- role centrality
- deployment pressure

## Cognitive Saturation

Cognitive saturation occurs when active context exceeds reliable processing capacity.

Signals:

- repeated delayed decisions
- frequent rework from missed context
- unresolved Slack decisions
- high interruption density
- many parallel review requests
- repeated blocker propagation through one role

## Mathematical Relation

Cognitive load modifies effective node reliability.

p_i_effective = f(p_i_base, active_context, interruption_density, decision_load, queue_pressure)

As load rises beyond capacity, p_i_effective should decline and variance should increase.

## Boundary

Teamlemetry does not measure intelligence, effort, loyalty, or human worth.

Cognitive load is used to identify overloaded system topology and stabilize delivery chains.

## Stabilization Actions

Possible actions:

- reduce work-in-progress
- clarify ownership
- add secondary reviewers
- move repeated decisions into policy
- automate summaries
- improve reporting routes
- split overloaded queues
- reduce meeting and channel fragmentation

## AI Stabilization

AI can reduce cognitive load when it summarizes repeated context, standardizes handoffs, prepares review packets, detects missing evidence, or routes operational reports.

AI must not be used to infer private intent or replace accountable governance.
