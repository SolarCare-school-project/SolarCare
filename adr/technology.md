| Area | Proposed technology | Purpose |
|---|---|---|
| Device integration | OpenDTU on ESP32 | Collect readings from compatible solar microinverters. Compatibility with the selected equipment needs verification. |
| Data communication | MQTT over TLS | Transfer readings from the home installation to our backend securely. |
| MQTT broker | Eclipse Mosquitto | Receive device messages and make them available to the data-processing service. |
| Backend | Python with FastAPI | Process readings and provide application APIs. |
| Database | PostgreSQL | Store users, equipment, measurements, maintenance history, and tasks. |
| Frontend | React | Display system status, alerts, equipment details, and maintenance tasks. |
| Maintenance logic | Rules implemented in Python | Flag missing readings or unusual production patterns for investigation. |
| Notifications | Email service | Send maintenance reminders and fault notifications. |
| Cloud hosting | One cloud provider, such as AWS | Host the backend, database, and MQTT broker. Provider and services remain to be selected. |
| Packaging and delivery | Docker and GitHub Actions | Package services and automate testing and deployment. |


# SolarCare — Initial Technology Draft

This document outlines the technologies and interfaces we may need.
All choices are preliminary and will be evaluated against our requirements.

## Device Integration

- **OpenDTU on ESP32:** Collect readings from compatible solar microinverters.
- **MQTT:** Transfer measurements and device status to our backend.
- **MQTT broker:** Consider Eclipse Mosquitto to receive device messages.

Compatibility with the selected solar equipment needs verification.

## Backend

- **Python with FastAPI:** Provide application APIs and process incoming data.
- **Maintenance rules:** Flag unusual production patterns for investigation. A missing reading is marked out of date, as described under Availability.
- **Notification service:** Send fault notifications and maintenance reminders.

## Database

Consider **PostgreSQL** to store:

- Users and solar installations.
- Equipment details and installation dates.
- Measurements and device status.
- Maintenance tasks and history.
- Spare-part information.

Measurement frequency and data retention will influence storage requirements.

## Frontend

Consider **React** for a web dashboard where users can:

- View system status and measurements.
- Register and manage equipment.
- Review alerts.
- Create and track maintenance tasks.

## APIs and Interfaces

| Interface | Purpose |
|---|---|
| MQTT: installation → backend | Receive measurements and device status |
| REST API: frontend → backend | Access monitoring data and manage equipment and tasks |
| Email provider API | Deliver alerts and reminders |
| Optional weather API | Provide context for changes in solar production |

## Cloud Hosting

Use one cloud provider, potentially **AWS**, to host:

- Backend application.
- MQTT broker.
- Database.
- Frontend.
- Logs and monitoring.

Specific services will be selected based on cost, reliability, and maintenance effort.

## Security and Operations

- Authentication and authorization for user access.
- Encrypted communication through HTTPS and MQTT over TLS.
- Separate device credentials and restricted MQTT topic access.
- Database backups and secure storage of credentials.
- Logging and monitoring to detect failures.
- A home that goes offline is handled as described under Availability.

## Availability

A private house uses the ordinary internet connection of that house. That line is outside this target. The panels keep producing power while our website or the house connection is down.

- **Website and API:** an internal target of 99% in a month, about 7 hours. This is not a promise in a customer contract. It fits one cloud setup and no night-time on-call team.
- **Readings from a home:** no uptime percentage. Show the last value and the time it was measured. A reading sent later can arrive now and still be old. After a few hours with nothing new, mark the data out of date.
- **A gap in readings:** does not by itself mean a part has failed. A health check or a fault report still decides that.
- **Jobs and visits:** business days, not a minutes-long emergency response.

The useful check is how long it has been since each home last sent a reading.

## Development Tools

- **GitHub:** Version control, documentation, issues, and pull requests.
- **Docker:** Package services consistently.
- **GitHub Actions:** Automate testing and deployment.

## Open Questions

- Which solar equipment will we support?
- Which measurements are available, and how frequently should we collect them?
- How will installation data securely reach our cloud services?
- How long with no new reading before we mark the data out of date?
- Which maintenance rules can the available data support?
- How long should measurements be retained?
- Which cloud provider and services best fit our budget?
- Is spare-part tracking sufficient initially, or do we need supplier integration?