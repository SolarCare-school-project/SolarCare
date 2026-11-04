# Interfaces

**Status:** proposed

## Context

[Which parts of SolarCare talk to each other, and why the connections are drawn the way they are.]

## Decision

| Connection | Who starts it | Protocol | What is sent | What happens when it fails |
| --- | --- | --- | --- | --- |
| Gateway to broker | | MQTT over TLS | | |
| API to broker | | MQTT | | |
| Website to API | | HTTPS (REST) | | |
| API to database | | SQL | | |
| API to email | | | | |

[Which connections are not allowed, for example the website never talks to the broker.]

## Consequences

- [What this makes easy.]
- [What this makes harder or riskier.]

## Open questions

- How long is an old API version supported?
- Does the gateway keep readings when the house loses internet, and how does it send them later?
- What happens when the MQTT broker is down: do readings queue, or are they lost?