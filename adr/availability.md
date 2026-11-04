
# Availability

**Status:** proposed

## Context

What SolarCare does and does not control. What keeps working when SolarCare is down, for example the panels still produce power.

## Decision

!!Update the Team decison!!

| Part | Target | What happens when it is down |
| --- | --- | --- |
| Website and API | | |
| Readings from a home | | |
| Support cases and visits | | |

[Whether the target is an internal SLO or a promised SLA.]

### Redundancy

- [Which parts run with a spare, and why.]
- [Which parts do not, and why that is acceptable.]

### When readings stop

When a home stops sending readings, SolarCare shows the last value with the time it was measured. A gap in readings does not by itself mean a part has failed. How support is told about the gap, and what is logged, is in [Logging](logging.md). 
**The Team Need to explore more about this.**

## Consequences

- [What the target protects.]
- [What redundancy does not cover.]
- [Cost impact.]

## Open questions

- [What is not decided yet.]