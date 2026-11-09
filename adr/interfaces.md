# Interface specification

**Status:** proposed

## Context

SolarCare exchanges data between its own parts and with the gateway at the home. 
The gateway publishes readings to the broker.
The API subscribes to the broker, reads and writes the database, and answers the website and the phone app.


## Decision

| Connection | Who starts it | Protocol | What is sent | What happens when it fails |
| --- | --- | --- | --- | --- |
| Gateway to broker| gateway | MQTT over TLS (port 8883), with a username and password per gateway | Readings and status under the gateway's base topic, as listed in `data-collection.md` | Readings are lost while the broker or the home internet is down, unless the gateway keeps them (see `availability.md`)|
| API to broker | The API | MQTT over TLS, with a username and password for the API | Readings and status from all gateways, received by subscribing to the base topics | Readings sent while the API is down are lost, unless the subscription is persistent (see `availability.md`).The API reconnects by itself when the broker is back |
| Website to API | The website, in the user's browser | HTTPS (REST, JSON), with the user logged in | Requests from owners, support, CFG and management, such as viewing readings, handling incidents and viewing summaries. The API answers with the data that user is allowed to see | The user sees an error and can try again later. Nothing is changed, and the readings keep arriving in the background |
| Phone app to API | The mobile app, on the technician's phone | HTTPS (REST, JSON), with the technician logged in | Registration of a new solar system (serial numbers, address), and visits with the replaced part and date. The API answers with the technician's systems and incidents | The technician sees no data from the API until it is reachable. The app keeps the data on the phone and sends it when the API is reachable again |
| API to database |The API | SQL to PostgreSQL, over TLS, with its own database login | Readings and status to store, and the data the website and the phone app ask for: systems, incidents, visits, users | The API can't store readings or answer requests, and the website and phone app show an error. The second database copy takes over (see `availability.md`). Readings that arrive meanwhile are lost unless they are kept (open question) |
|🟡 **Optional, under investigation:** Website and phone app to load balancer | Same as Phone/web app to API | HTTPS (REST, JSON), ending at the load balancer, which passes requests on to one of the two API servers | The same requests as in the two rows above | If the load balancer fails, nobody can reach the API, even if both servers are running. If one API server fails, the load balancer sends requests to the other one |
|🟡 **Optional, under investigation:** Database to its second copy | The main database | PostgreSQL replication, over TLS, with its own login for the copy | Every change made in the main database, such as readings, incidents and visits | If the link breaks, the copy falls behind. If the main database fails then, the takeover loses the changes the copy hasn't received yet |


Legend;
🟡 = not decided yet, still under investigation.

## Consequences

**The Benfits**
- Access is checked in one place, the API. The website and the phone app never reach the data directly.
- The website and the phone app use the same API, so a new feature is built once.
- The home needs no open ports, because the gateway starts its own connection.

**Risks**

- Every client depends on the API. If it is down, the website and the phone app are both unusable.
- Old versions of the phone app may still be in use after the API changes, so the API has to support them for some time.
- Encryption on every connection means certificates have to be issued and renewed for the broker, the API and the database.
- The readings can be lost at two points: between the gateway and the broker, and between the API and the database.

## Open questions

- How long is an old API version supported?
- Where are logs stored, and what happens to an entry when the log storage is down? Audit entries must not be lost.