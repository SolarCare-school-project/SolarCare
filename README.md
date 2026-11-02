# SolarCare

## What is SolarCare?

**SolarCare is a solar maintenance service for private house owners, focused on managing the replacement of solar-system components.**

The service is designed for private houses in Germany. Our target customer is:

- **House owners** with their own rooftop solar system.

SolarCare manages the replacement process for components such as:

- Solar panels
- Inverters or microinverters
- Power optimizers, where installed
- Batteries, where installed
- Energy meters and monitoring gateways
- Cables and connectors
- Relevant electrical protection components

A job can start in two ways:

- **Support ticket:** the house owner reports an incident, or support already has a case. A hardware case can end in a technician visit. The part is replaced only when that visit finds it faulty.
- **Scheduled health check:** that part's check interval is due. A proposal, still to confirm: insurers often expect a yearly look and a fuller electrical check about every 4 years. A private home is not required by law to follow that. Use a shorter interval when the manual asks for one. A limit in the manual, such as a battery cycle count, opens this check. It does not open a replacement on its own.

### Replacement triggers

| Trigger | Business rule |
| --- | --- |
| **Support ticket:** an incident reaches a technician | The technician visits. The part is replaced only when the visit finds it faulty. |
| **Scheduled health check:** the part's interval is due | A check is opened. The part is replaced only when the check shows that its condition, safety or performance requires it. |

SolarCare coordinates the information required for the replacement process, including the installed equipment, component history, compatible replacement parts, technician work and replacement records.

> **Customer value:** SolarCare provides one coordinated process for identifying, planning and documenting solar-component replacements, reducing avoidable downtime and keeping the installation's maintenance history up to date.

### Initial scope

SolarCare initially focuses on **component replacement for private houses**, including the scheduled health check that can lead to a replacement.

Automated diagnosis, cleaning contracts, parts marketplaces and recycling services are possible future extensions, but are outside the initial scope.

The current software picture is the C4 set:

- [Context](diagrams/c4/context.md)
- [Containers](diagrams/c4/container.md)
- [API components](diagrams/c4/component.md)

An incident at a private house is three sequences. A scheduled health check is separate:

- [Incident: FAQ and the customer's software](diagrams/sequence/incident-faq-sequence.md)
- [Incident: software engineer](diagrams/sequence/incident-software-sequence.md)
- [Incident: hardware already clear](diagrams/sequence/incident-hardware-sequence.md)
- [Scheduled health check](diagrams/sequence/health-check-sequence.md)

Architecture diagram - Boris B

Stakeholders & Market
## Stakeholder Analysis

### External Stakeholders

| Stakeholder | How they are affected / affect us | Interest | Power | Attention | Data access |
|---|---|---|---|---|---|
| **Private customers** | Use and pay for the software; protect & monitor their PVs. Creating a ticket for replacement & connecting them to the right replacement company. | High | High | Manage closely | Own data only; can export, delete and share (GDPR-compliant). |
| **Inverter manufacturers** | Our data source via their API; can block access; can compete with their own apps. | Medium | High | Manage closely | Receive no data |
| **Third-party replacement company** | Carry out repairs. We connect them with our platform through third-party partnerships. | High | Medium | Manage closely | System data + customer data, per ticket only. |
| **Government / regulator / BfDI & Federal Network Agency** | Sets rules (subsidies, grid, data law). We might provide them aggregated data for governmental reporting. | Low | High | Keep satisfied | Aggregated data only. |
| **Cloud / software providers** | Host and process our data – using their service only. | Medium | Medium | Keep informed | Technical processing under a data processing agreement. |
| **Competitors** | Influence pricing and positioning in the market. | Medium | Medium | Monitor | None |

### Internal Stakeholders

| Role | Responsibility | Interest | Power | Attention | Data access |
|---|---|---|---|---|---|
| **Founders / management** | Strategy, finance, partnerships with replacement companies & data protection. | High | High | Manage closely | All data in aggregated form; raw data only if needed. |
| **Software engineers** | Build the platform, connect inverter APIs, forecasting & designing models. | High | Medium | Manage closely | Anonymised or test data; live data only for incidents, logged. |
| **Support engineer** | Handles tickets, coordinates installers.<br><br>**Support levels:**<br>• **L0 – Self-service:** FAQ and help center.<br>• **L1 – Basic support:** General questions and account issues.<br>• **L2 – Technical support (software engineer):** Technical issues connected to our software.<br>• **L3 – Physical support:** Assign to a third-party replacement company. | High | Medium | Keep informed | Only customers with an open ticket |
| **Marketing / GTM & sales** | Customer acquisition/growth, installer partnerships. | Medium | Low | Keep informed | Opt-in contact data + anonymised aggregated analytics; no system data. |

# SolarCare – Market Analysis

SolarCare monitors PV systems of private homeowners, predicts which component needs maintenance or replacement, and connects owners with a technician before failures cost them money.

| Topic | Summary |
|---|---|
| [Target Group](Target%20Group.md) | Private homeowners in the DACH region with a 5–15 kWp rooftop system, 3–12 years old, connected via Wi-Fi |
| [Jobs to Be Done](Jobs%20to%20Be%20Done.md) | Customers want full yield and no unexpected failures, without needing technical knowledge |
| [Expansion Path & Scalability](Expansion%20Path%20%26%20Scalability.md) | Start with private homeowners, then multi-family buildings and farms, then businesses and institutions |
| [Competitor Analysis](Competitor%20Analysis.md) | Manufacturers and installers service only their own systems; independent tools offer data but no repair. SolarCare combines independent fault prediction with repair coordination |

Technologies used, APIs, Cloud - Vlad K

## Technologies  Initial Draft

- **Device integration:** OpenDTU on ESP32, subject to equipment compatibility.
- **Communication:** MQTT over TLS with a Mosquitto broker.
- **Backend and APIs:** Python, FastAPI, and REST APIs.
- **Database:** PostgreSQL for measurements, equipment, and maintenance records.
- **Frontend:** React website with a house owner screen and a team screen.
- **Cloud:** AWS considered for hosting; specific services to be decided.
- **Notifications:** Email provider API for alerts and reminders.
- **Development:** GitHub, Docker, and GitHub Actions.

All technology choices are preliminary.
