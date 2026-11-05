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
| API | Subscribes to readings and answers requests from the website and phone app | Python with FastAPI | One language for the API and the rules, so one codebase and one skill set. Python suits the trend and gap checks. |
| Database | Stores readings, incidents, health checks and customer and part records | PostgreSQL | Open source, relational, fits records that link to each other. |
| Website | The screen for owners, support, CFG and management. Talks only to the API over HTTPS. | React | Builds screens from reusable parts, which suits a dashboard of readings and charts. Many chart libraries exist for it. |
| Phone app | Lets technicians register a system and record a visit on site, for example the part replaced and the date. Talks only to the API over HTTPS. | Not decided | See Open questions. |
| Hosting | Runs the broker, API, database and website | One cloud provider, AWS proposed | One provider keeps operations simple. Customer data stays in an EU region because of GDPR. Redundancy is described in [Availability](availability.md). |

## Consequences

- The proposed tools are open source, so there are no license costs. The main running cost is cloud hosting.
- We run our own broker, so we are responsible for its availability. See [Availability](availability.md).
- The phone app is a second client to build, release and keep working with the API.
- One cloud provider is simple to run, but changing it later takes work.
- Only inverters that OpenDTU can read are supported. Another brand later needs a new way to get readings in.

## Open questions

- Which inverter series do we support (HM, HMS, HMT)? Inverters with built-in Wi-Fi (a "W" in the name) cannot be read by OpenDTU.
- Who supplies, installs and configures the gateway (broker address, login, base topic, certificate), and who updates its firmware?
- Which certificate authority do we use for the broker, and who updates the certificate stored on a gateway when it expires or changes?
- Which MQTT broker do we use? Mosquitto is proposed. We still need to check persistent sessions and running with a spare.
- Does ingestion (broker to database) run separately from the API?
- How is the phone app built: a phone-friendly website, a cross-platform app, or native apps? Does it need to work without mobile coverage?
- Do we send email notifications, and with which email service?
- Which cloud provider and region do we use, and do we run the database and broker ourselves or use the provider's managed services?
- How often do we collect readings, and how long do we keep them? See [Data collection](data-collection.md).
