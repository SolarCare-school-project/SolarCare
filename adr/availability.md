
# Availability

**Status:** proposed

## Context

SolarCare monitors solar systems and manages incidents and health checks. It does not run the solar system, so the panels keep producing power when SolarCare is down. SolarCare also does not control the home's internet connection.

Solar System owners, support, technicians, R&D CFG and management use the website and API to see data and handle incidents.
When they are down, none of them can do this until they are back.
Technicians use the website on their phone to record their visits and get readings from the SolarCare system.

Telemetry sent while the API or broker is unavailable can be lost, because MQTT QoS 0 (at-most-once delivery) does not store or retransmit messages.
The Solar System owner then sees a gap in the time series. Readings are persisted in the database, so a single server failure does not lose them. Customers, incidents, visits and part records are kept in the CRM.


## Decision

How available the parts we control must be, and what we do when they fail.

| Part | Target | What happens when it is down |
| --- | --- | --- |
| Website and API | 99.5% per month (about 3.6 hours of downtime) | Nobody can see readings, handle incidents or update info about onsite visits until it is back. The panels keep producing power. |

The target applies only to the parts SolarCare controls: website, API, MQTT broker and database.
It does not cover the inverter, its firmware, the gateway, the home internet, manufacturer services or the CRM.


### Redundancy

- The API runs on two servers behind a load balancer, so one server can fail without an outage.
    The early architecture sketch ([SolarCare architecture diagram](../diagrams/architecture/SolarCare.drawio.png)) shows one API, one subscriber and one database. It does not yet show the second server or the load balancer.

- The database has a second copy in another data centre of the same region, so one data centre failure does not lose readings, the links to customers and the login accounts.
- Does the MQTT broker run with a spare, or is it a single server?

### When the CRM is down

The CRM is not part of SolarCare, so we cannot fix an outage ourselves. The user sees an error and is asked to contact support. Readings keep arriving. See [Interfaces](interfaces.md).

### When SolarCare is down

Users never open the CRM. When the website or API is down, only the support team lead role may use the CRM directly, to update a case or to create a new one. The lead must fill in the customer ID and the inverter serial number, so that SolarCare can find the case later.

SolarCare keeps no list of cases. It asks the CRM, for example for all cases of one customer ID. A case created or changed directly in the CRM therefore shows up the next time it is read, and nothing needs to be synced.

### When readings stop

When a home stops sending readings, SolarCare shows the last value with the time it was measured. A gap in readings does not by itself mean a part has failed. How support is told about the gap, and what is logged, is in [Logging](logging.md).
**The Team Need to explore more about this.**

## Consequences

- Readings sent while the API or broker is down can be lost, so the time series can have gaps.
- Customers, incidents, visits and part records cannot be used while the CRM is down, and our own redundancy does not help.
- Changes made directly in the CRM while SolarCare is down are not in our audit log. The CRM's own history has to cover them.
- A failure of the whole cloud region is not covered by the two servers or the second database copy.
- Running two API servers and a second database copy costs more.

## Open questions

- Is 99.5% a promised SLA to customers or an internal SLO? The team still needs to decide
- Can Salesforce limit direct access to the support team lead role, and can it require the customer ID and the serial number on a new case?
- Which QoS level do we use? QoS 1 with a persistent session would let the broker hold readings while the API is away
- Does the gateway buffer readings when the broker or the home internet is down? Is this possible with OpenDTU?
- If the gateway buffers, how large is the buffer and how long can it hold readings?
