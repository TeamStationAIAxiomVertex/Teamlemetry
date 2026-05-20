# Queueing Theory

Queueing theory explains why small capacity mismatches can create large delivery delays.

## Core Model

rho = lambda / mu

Where:

- lambda is work arrival rate
- mu is service rate
- rho is utilization

Expected queue wait time grows approximately as:

W_q approx rho / (1 - rho)

As rho approaches 1, wait time increases nonlinearly.

## Engineering Queues

Teamlemetry models queues across:

- pull request review
- Jira ticket refinement
- QA validation
- CI execution
- deployment approval
- incident response
- architecture review
- security review

## Saturation Signals

A queue may be saturated when:

- arrivals exceed completions for a sustained period
- wait time increases faster than work volume
- one role becomes a central bottleneck
- blockers repeatedly accumulate at the same stage
- downstream teams wait on upstream decisions

## Queue Saturation Is Not Laziness

Queue saturation is a system condition. It does not imply that a person or team is not working hard enough.

High utilization often reduces reliability because there is no slack for interruptions, review quality, incidents, or rework.

## V1 Queue Metrics

Initial queue metrics should include:

- arrival count
- completion count
- open work count
- age of oldest item
- median wait time
- p90 wait time
- service rate estimate
- utilization estimate
- repeated blocker count

## Recommendations

Queue-based recommendations may include:

- add reviewer coverage
- reduce batch size
- split overloaded ownership
- clarify intake policy
- automate validation
- introduce AI summarization for repeated review preparation

## Testing Requirements

Queue simulations must verify nonlinear wait-time behavior, duplicate-event protection, and sustained saturation detection.
