# SolarCare

What is SolarCare? 

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

A replacement can be triggered in two ways:

- **Corrective replacement** — a component has failed or a fault has been identified.
- **Preventive replacement** — a component reaches a defined maintenance or replacement point based on manufacturer guidance or an agreed maintenance policy.

### Replacement triggers

| Trigger | Business rule |
| --- | --- |
| **Corrective replacement** — component failure or degradation | A fault, inspection result or technician assessment confirms that a component should be replaced. |
| **Planned replacement** — lifecycle or manufacturer-defined threshold | A replacement task is created when a component reaches a manufacturer-defined lifecycle, capacity, usage or service threshold. This applies only to components for which such a threshold exists. |
| **Preventive inspection** — scheduled condition check | Components without a fixed replacement interval, such as panels, cables and connectors, are inspected periodically and replaced only when their condition, safety or performance requires it. |

SolarCare coordinates the information required for the replacement process, including the installed equipment, component history, compatible replacement parts, technician work and replacement records.

> **Customer value:** SolarCare provides one coordinated process for identifying, planning and documenting solar-component replacements, reducing avoidable downtime and keeping the installation's maintenance history up to date.

### Initial scope

SolarCare initially focuses only on **residential component replacement management**.

Automated diagnosis, broader preventive-maintenance services, parts marketplaces and recycling services are possible future extensions, but are outside the initial scope.

Architecture diagram - Boris B

Stakeholders, market - Johannes R 

Technologies used, APIs, Cloud - Vlad K

## Technologies  Initial Draft

- **Device integration:** OpenDTU on ESP32, subject to equipment compatibility.
- **Communication:** MQTT over TLS with a Mosquitto broker.
- **Backend and APIs:** Python, FastAPI, and REST APIs.
- **Database:** PostgreSQL for measurements, equipment, and maintenance records.
- **Frontend:** React dashboard.
- **Cloud:** AWS considered for hosting; specific services to be decided.
- **Notifications:** Email provider API for alerts and reminders.
- **Development:** GitHub, Docker, and GitHub Actions.

All technology choices are preliminary.
