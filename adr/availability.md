
# Availability

**Status:** proposed

## Context

SolarCare monitors solar systems and manages incidents and health checks. It does not run the solar system, so the panels keep producing power when SolarCare is down. SolarCare also does not control the home's internet connection.

Solar System owners, support, CFG and management use the website and API to see data and handle incidents.
When they are down, none of them can do this until they are back.
Technicians use a phone app to record their visits and get reading from SolareCare system.

Telemetry sent while the API or broker is unavailable can be lost, because MQTT QoS 0 (at-most-once delivery) does not store or retransmit messages.
The Solar System owner then sees a gap in the time series. Incident and customer records are persisted in the database, so a single server failure does not lose them.


## Decision

How available the parts we control must be, and what we do when they fail.

| Part | Target | What happens when it is down |
| --- | --- | --- |
| Website, Mobile App and API | 99.5% per month (about 3.6 hours of downtime) | Nobody can see readings, handle incidents or update info about onsite visits until it is back. The panels keep producing power. |

The target applies only to the parts SolarCare controls: website, API, MQTT broker and database.
It does not cover the inverter, its firmware, the gateway, the home internet or manufacturer services.


### Redundancy

- The API runs on two servers behind a load balancer, so one server can fail without an outage.
    The early architecture sketch ([SolarCare architecture diagram](../diagrams/architecture/SolarCare.drawio.png)) shows one API, one subscriber and one database. It does not yet show the second server or the load balancer.

- The database has a second copy in another data centre of the same region, so one data centre failure does not lose incident and customer records.
- Does the MQTT broker run with a spare, or is it a single server?

### When readings stop

When a home stops sending readings, SolarCare shows the last value with the time it was measured. A gap in readings does not by itself mean a part has failed. How support is told about the gap, and what is logged, is in [Logging](logging.md).
**The Team Need to explore more about this.**

## Consequences

- Readings sent while the API or broker is down can be lost, so the time series can have gaps.
- A failure of the whole cloud region is not covered by the two servers or the second database copy.
- Running two API servers and a second database copy costs more.

## Open questions

- Is 99.5% a promised SLA to customers or an internal SLO? The team still needs to decide
- Which QoS level do we use? QoS 1 with a persistent session would let the broker hold readings while the API is away
- Does the gateway buffer readings when the broker or the home internet is down? Is this possible with OpenDTU?
- If the gateway buffers, how large is the buffer and how long can it hold readings?
