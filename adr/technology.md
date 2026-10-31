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
- **Maintenance rules:** Identify missing readings or unusual production patterns for investigation.
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
- Handling of home internet outages and missing measurements.

## Development Tools

- **GitHub:** Version control, documentation, issues, and pull requests.
- **Docker:** Package services consistently.
- **GitHub Actions:** Automate testing and deployment.

## Open Questions

- Which solar equipment will we support?
- Which measurements are available, and how frequently should we collect them?
- How will installation data securely reach our cloud services?
- How should the system behave when an installation goes offline?
- Which maintenance rules can the available data support?
- How long should measurements be retained?
- Which cloud provider and services best fit our budget?
- Is spare-part tracking sufficient initially, or do we need supplier integration?