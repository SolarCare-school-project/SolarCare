# Technology stack

**Status:** proposed

## Context

SolarCare receives readings from solar systems in homes, stores them, and shows them to Solar System owners, support, technicians, CFG and management. Readings reach us from OpenDTU gateways over MQTT. The stack must support the availability target in [Availability](availability.md), run on one cloud provider, and keep running costs reasonable.

## Decision

Each part has a decided role. The technology for each role is proposed and can still change. See [Interfaces](interfaces.md) for how the parts connect.

| Part | Role | Proposed technology | Why |
| --- | --- | --- | --- |
| Gateway | Reads the inverter at the home and publishes readings. It runs at the home, so SolarCare does not control it after installation. | OpenDTU on ESP32 | Open source and free. Reads Hoymiles inverters (HM, HMS and HMT series) and a few rebrands such as TSUN and Solenso, but not models with built-in Wi-Fi. Sends readings over MQTT with TLS. |
| Message protocol | Carries readings from the home to SolarCare | MQTT over TLS | Light publish-subscribe protocol for devices with limited bandwidth. OpenDTU supports TLS, so readings are encrypted on the way. Each gateway signs in with its own username and password. |
| Broker | Receives readings and passes them to the API. We run it. | Eclipse Mosquitto | Open source and light. Supports MQTT over TLS, which OpenDTU also supports, and a login for each gateway. |
| API | Subscribes to readings and answers requests from the website | Python with FastAPI | One language for the API and the rules, so one codebase and one skill set. Python suits the trend and gap checks. |
| Database | Stores readings and daily totals, the link between an inverter serial number and a CRM customer ID, login accounts, and the audit and application logs | PostgreSQL | Open source, relational, fits time-stamped readings and records that link to each other. |
| CRM | Holds customers, incidents and cases, health checks, visits and part records. It tracks the SLA on each case. It is outside SolarCare. SolarCare stores only the customer ID and shows the data the user is allowed to see. | Salesforce | Customers and what they paid for already live there, so personal data has one copy. |
| Website | The screen for owners, support, technicians, CFG and management. Technicians use it on their phone to register a system and record a visit on site, for example the part replaced and the date. It works without mobile coverage and sends the data when the signal returns. Talks only to the API over HTTPS. | React as a progressive web app (PWA) | Builds screens from reusable parts, which suits a dashboard of readings and charts. Many chart libraries exist for it. One codebase serves desktop and phone, with no app store, and a PWA can work offline. |
| Hosting | Runs the broker, API, database and website | AWS, region Frankfurt | Managed services for the database copy (RDS), login (Cognito) and email (SES) save the small team building and running them. Free tier and start-up credits keep costs low at the start. AWS is not the cheapest for plain servers (EU providers such as Hetzner are cheaper), but cheaper in total working time. Data stays in Germany. GDPR and Plan B are in [GDPR](gdpr.md#hosting). |

## Consequences

- The proposed tools are open source, so there are no license costs. The main running cost is cloud hosting.
- We run our own broker, so we are responsible for its availability. See [Availability](availability.md).
- The website must work well on a phone and offline, which is more work than a desktop-only website. There is no separate app to build and release.
- SolarCare depends on the CRM for customers and incidents. The CRM is not part of SolarCare, so it is outside our availability target. See [Interfaces](interfaces.md).
- One cloud provider is simple to run, but changing it later takes work.
- Only inverters that OpenDTU can read are supported. Another brand later needs a new way to get readings in.

## Open questions

- Which inverter series do we support (HM, HMS, HMT)? Inverters with built-in Wi-Fi (a "W" in the name) cannot be read by OpenDTU.
- Who supplies, installs and configures the gateway (broker address, login, base topic, certificate), and who updates its firmware?
- Which certificate authority do we use for the broker, and who updates the certificate stored on a gateway when it expires or changes?
- Does ingestion (broker to database) run separately from the API?
- How does the website handle a visit recorded offline if the same incident was changed by someone else meanwhile?
- Which Salesforce fields and API limits apply to the SolarCare integration (case fields, serial number field, SLA fields)?
- Where are the files about a solar system stored: in the CRM or in SolarCare?
- Do we send email notifications, and with which email service?
- Do we use AWS managed services (RDS, Cognito, SES) or run some parts ourselves to save cost?
