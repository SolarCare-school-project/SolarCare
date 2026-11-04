# Logging

**Status:** proposed

## Context

SolarCare has several places that produce logs: the gateway at the home, the MQTT broker, the API, the website, and the cloud setup. Different people need different logs. Support needs enough to investigate an incident. A software engineer (CFG) needs more depth after a transfer is approved. The Solar System owner has no access to the backend and needs none.

A log can leak personal data. Name, address, and which home a reading belongs to are personal data, as described in [Data collection](data-collection.md). Who may read each log is in [Data security](data-security.md).

## Decision

### Log layers and who can read them

| Layer | What it holds | Who can read it |
| --- | --- | --- |
| Gateway | Its own logs stay at the home. Only what it publishes, such as `dtu/status` and `dtu/uptime`, reaches SolarCare. | Support engineer and software engineer, as readings |
| Broker | Connections, failed logins, and denied topics | Software engineer and platform team |
| Application (API) | Errors, slow calls, and request IDs. It does not hold the content of a request. | Software engineer, after an approved transfer for live data |
| Audit | Who opened which home or case, who approved a transfer, and who assigned a technician | Support team lead and security. Nobody can edit it. |
| Diagnostic report | A fuller log stored only when a report is sent | Support engineer. The regional technician, for the assigned incident only. The S cannot read it. |
| Infrastructure | Cloud, load balancer, database, and the deployment pipeline | Platform team |

The Solar System owner sees no log. They see their own readings with the time they were measured, and the status and history of their incident.

### What is never logged

- Name, email, phone number, and address.
- Passwords, tokens, and other secrets.
- The body of a request or a response.

### How an entry is written

- Every entry has the same fields: time, source, event type, and an ID for the incident or home.
- The entry holds IDs, not values. It names the incident, not the person.
- The inverter serial number is treated as an identifier, because it links to a house.
- Every entry carries a correlation ID, so one incident can be followed across gateway, broker, API, and database.

### How private data is kept out

- A filter in the logging code removes known personal fields before an entry is written.
- A test in the build fails if a log line looks like an email address or a phone number.
- Logs are kept for a short, fixed time, so a mistake expires.

## Consequences

- Support can investigate an incident without seeing customer details.
- Keeping IDs instead of values makes a log less readable. An engineer has to look the ID up through the API.
- This lowers the risk of personal data in logs but does not guarantee zero. The filter and the build test catch known patterns only.
- Each layer has its own readers, so access has to be set up and checked for each one.

## Open questions

- How long is each log kept?
- What does the fuller diagnostic log contain?
- Is an entry flat, such as one JSON object per event, or nested?
- When is support told that a home has stopped sending readings?
- What does the website collect from the browser, such as errors or usage analytics?
- Which third parties, such as the cloud and email providers, keep their own logs, and how are those covered?