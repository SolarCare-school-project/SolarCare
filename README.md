# SolarCare

## What is SolarCare?

**SolarCare is a residential solar maintenance service focused on managing the replacement of solar-system components.**

The service is designed for residential solar installations in Germany. Our target customers are:

- **Homeowners** with private rooftop solar systems.
- **Apartment owners** with individual permitted solar installations.
- **Apartment-building owners or homeowners' associations (WEG)**, typically represented by a property manager.

SolarCare manages the replacement process for components such as:

- Solar panels
- Inverters or microinverters
- Power optimizers, where installed
- Batteries, where installed
- Energy meters and monitoring gateways
- Cables and connectors
- Relevant electrical protection components

A job can start in three ways:

- **Corrective replacement:** a component has failed or a fault has been identified.
- **Scheduled health check:** that part's check interval is due. A proposal, still to confirm: insurers often expect a yearly look and a fuller electrical check about every 4 years. A private home is not required by law to follow that. Use a shorter interval when the manual asks for one.
- **Planned replacement:** only when that model's manual gives a real limit, such as a battery cycle count, a capacity floor, or a part the manual says to swap, such as an inverter filter.

### Replacement triggers

| Trigger | Business rule |
| --- | --- |
| **Corrective replacement:** component failure or degradation | A fault, inspection result or technician assessment confirms that a component should be replaced. |
| **Scheduled health check:** the part's interval is due | A check is opened. The part is replaced only when the check shows that its condition, safety or performance requires it. |
| **Planned replacement:** a limit stated in the manual | A replacement task is created for that limit. This is for a battery that has reached its cycle count or capacity floor, or for a service part such as an inverter filter. Panels, cables, connectors, microinverters, optimizers, meters and most protection parts do not get a replace-by date. A string inverter is checked on a schedule. It gets a replace-by date only when that model's manual or warranty states one. |

SolarCare coordinates the information required for the replacement process, including the installed equipment, component history, compatible replacement parts, technician work and replacement records.

> **Customer value:** SolarCare provides one coordinated process for identifying, planning and documenting solar-component replacements, reducing avoidable downtime and keeping the installation's maintenance history up to date.

### Initial scope

SolarCare initially focuses on **residential component replacement**, including the scheduled health check that can lead to a replacement.

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

Stakeholders, market - Johannes R 

Technologies used, APIs, Cloud - Vlad K

## Technologies  Initial Draft

- **Device integration:** OpenDTU on ESP32, subject to equipment compatibility.
- **Communication:** MQTT over TLS with a Mosquitto broker.
- **Backend and APIs:** Python, FastAPI, and REST APIs.
- **Database:** PostgreSQL for measurements, equipment, and maintenance records.
- **Frontend:** React website with a household screen and a team screen.
- **Cloud:** AWS considered for hosting; specific services to be decided.
- **Notifications:** Email provider API for alerts and reminders.
- **Development:** GitHub, Docker, and GitHub Actions.

All technology choices are preliminary.
