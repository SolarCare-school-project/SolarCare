# Interface specification

**Status:** proposed

## Context

SolarCare exchanges data between its own parts and with the gateway at the home. 
The gateway publishes readings to the broker.
The API subscribes to the broker, reads and writes the database, and answers the website.
The API also talks to the CRM, which holds the customers, incidents, health checks, visits and part records.


## Decision

| Connection | Who starts it | Protocol | What is sent | What happens when it fails |
| --- | --- | --- | --- | --- |
| Gateway to broker| gateway | MQTT over TLS (port 8883), with a username and password per gateway | Readings and status under the gateway's base topic, as listed in `data-collection.md` | Readings are lost while the broker or the home internet is down, unless the gateway keeps them (see `availability.md`)|
| API to broker | The API | MQTT over TLS, with a username and password for the API | Readings and status from all gateways, received by subscribing to the base topics | Readings sent while the API is down are lost, unless the subscription is persistent (see `availability.md`).The API reconnects by itself when the broker is back |
| Website to API | The website, in the user's browser or installed on the technician's phone | HTTPS (REST, JSON), with the user logged in | Requests from owners, support, technicians, CFG and management, such as viewing readings, handling incidents, viewing summaries, registering a new solar system (serial numbers, address) and recording visits with the replaced part and date. The API answers with the data that user is allowed to see | The user sees an error and can try again later. Nothing is changed, and the readings keep arriving in the background. A visit recorded without coverage stays on the phone and is sent when the API is reachable again |
| API to database |The API | SQL to PostgreSQL, over TLS, with its own database login | Readings and status to store, and the data the website asks for: readings, the links to customers, users | The API can't store readings or answer requests, and the website shows an error. The second database copy takes over (see `availability.md`). Readings that arrive meanwhile are lost unless they are kept (open question) |
| API to CRM | The API | HTTPS (REST, JSON), with its own integration login | Customer lookups by customer ID, limited to the fields the user is allowed to see. A new case, created from a template, with title, description, category, priority, date, the user who raised it, the customer ID and the inverter serial number. Every later change a user makes in SolarCare (replies, status, transfers, visits, part records), sent after SolarCare has checked the user's role. SolarCare reads cases back from the CRM, with their status, owner and SLA, and keeps no copy | The user sees an error and is asked to contact support. Nothing is saved in SolarCare, and the readings keep arriving in the background |
|🟡 **Optional, under investigation:** Website to load balancer | Same as Website to API | HTTPS (REST, JSON), ending at the load balancer, which passes requests on to one of the two API servers | The same requests as in the Website to API row | If the load balancer fails, nobody can reach the API, even if both servers are running. If one API server fails, the load balancer sends requests to the other one |
|🟡 **Optional, under investigation:** Database to its second copy | The main database | PostgreSQL replication, over TLS, with its own login for the copy | Every change made in the main database, such as readings, the links to customers and users | If the link breaks, the copy falls behind. If the main database fails then, the takeover loses the changes the copy hasn't received yet |


Legend;
🟡 = not decided yet, still under investigation.

## Consequences

**The Benfits**
- Access is checked in one place, the API. The website never reaches the data directly.
- Owners, support, technicians, CFG and management all use the same website and API, so a new feature is built once.
- The home needs no open ports, because the gateway starts its own connection.

**Risks**

- Every client depends on the API. If it is down, the website is unusable.
- Customers, incidents, visits and part records depend on the CRM, which we do not run. If it is down, those parts show an error, while the readings keep arriving.
- An old version of the website may still be open or saved on a technician's phone after the API changes, so the API has to support it for some time.
- Encryption on every connection means certificates have to be issued and renewed for the broker, the API and the database.
- The readings can be lost at two points: between the gateway and the broker, and between the API and the database.

## Open questions

- How long is an old API version supported?
- Which fields does the CRM require for a case, and what happens if one is missing when the incident is opened?
- Can Salesforce be searched by customer ID, technician, region, status and "changed since"? Can it send a message when a case changes?
- Where are logs stored, and what happens to an entry when the log storage is down? Audit entries must not be lost.