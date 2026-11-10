# Data protection impact assessment (DPIA)

**Status:** draft [will be updated]
**Reviewed by:** [external data protection officer]
**Date:** [date]

## Why a DPIA

SolarCare collects data from private homes continuously and links it to a person. The readings can show when people are at home or away. This is systematic monitoring with new technology, so a DPIA is done before launch (Art. 35 GDPR).

## 1. Description of the processing

| What | Details |
| --- | --- |
| Purpose | Monitor the solar system, find faults early, handle incidents and health checks |
| Data subjects | Solar System owners, and people living in the home |
| Personal data | Name, email, address, phone number, inverter serial number, readings (see [Data collection](../adr/data-collection.md)) |
| Source | Gateway at the home (readings), regional technician on the website at registration, CRM (customer details) |
| Recipients | Support engineers, software engineers, regional technician company (assigned incidents only), processors (AWS, Salesforce, email provider) |
| Storage | Readings in AWS, region Frankfurt. Customer details, cases, visits and part records in Salesforce (CRM) |
| Retention | See [GDPR](../adr/gdpr.md#retention) |

Data flow: gateway → MQTT broker → API → database → website. The API reads customer details and cases from the CRM. See the [container diagram](../diagrams/c4/SolarCareContainerC4.png).

## 2. Necessity and proportionality

| Question | Answer |
| --- | --- |
| Is the data needed for the purpose? | Yes. Without readings, faults cannot be found. Without contact details, no technician visit. |
| Is less data possible? | Only the topics in [Data collection](../adr/data-collection.md) are stored. Grid details and inverter commands are never collected. |
| Legal basis | Contract for monitoring and incidents. Consent for marketing. See [GDPR](../adr/gdpr.md#legal-basis). |
| Is the retention limited? | Yes. Raw readings 2 years, logs 30 days. |
| Are owners informed? | Privacy notice in the website and at registration. |
| Can owners use their rights? | Access and export through the website. Correction and deletion are requested in the website, which creates a case, and support updates the CRM. |

## 3. Risks for the owners

| Risk | Example | Likelihood | Severity |
| --- | --- | --- | --- |
| Presence profile | Readings show the family is on holiday. Useful for burglars if leaked. | Medium | High |
| Data leak | Database or backup is accessed by an attacker. | Low | High |
| Access by the wrong person | A technician or employee looks at a home without an incident. | Medium | Medium |
| New owner sees old data | House is sold, serial number still linked to the old owner. | Medium | Medium |
| Personal data in logs | Name or address written into a log by mistake. | Medium | Low |
| Access by US authorities | AWS and Salesforce are US companies. | Low | Medium |
| Data kept too long | Old readings stay without a reason. | Low | Medium |
| Data used for another purpose | Readings used for marketing. | Low | Medium |

## 4. Measures

| Risk | Measure | Where |
| --- | --- | --- |
| Presence profile | Readings visible only to the owner and to roles with an open incident. Raw readings reduced to daily totals after 2 years. | [Data security](../adr/data-security.md), [GDPR](../adr/gdpr.md) |
| Data leak | TLS on every connection. Database encrypted at rest. Access only through the API. | [Interfaces](../adr/interfaces.md) |
| Access by the wrong person | Role-based access. Technicians see only assigned incidents. Every access is in the audit log. | [Logging](../adr/logging.md) |
| New owner sees old data | Serial number is unlinked before linking to a new owner. | [GDPR](../adr/gdpr.md) |
| Personal data in logs | Logs hold IDs only. Filter in the logging code and a test in the build. Logs kept 30 days. | [Logging](../adr/logging.md) |
| Access by US authorities | EU regions for AWS and Salesforce. EU-US Data Privacy Framework and standard contractual clauses. Plan B: EU provider. | [GDPR](../adr/gdpr.md#hosting) |
| Data kept too long | Fixed retention periods, deleted automatically. | [GDPR](../adr/gdpr.md#retention) |
| Data used for another purpose | Marketing has no access to readings. Marketing only with consent. | README, stakeholder table |

## 5. Remaining risk

| Risk | After measures |
| --- | --- |
| Presence profile | Low |
| Data leak | Low |
| Access by the wrong person | Low |
| New owner sees old data | Low |
| Personal data in logs | Low |
| Access by US authorities | Low to medium, depends on the Data Privacy Framework |
| Data kept too long | Low |
| Data used for another purpose | Low |

No high risk remains, so the data protection authority does not need to be consulted before launch (Art. 36).

## 6. Review

- The DPIA is reviewed by the external data protection officer before launch.
- It is updated when SolarCare collects new data, adds a processor, or changes hosting.
