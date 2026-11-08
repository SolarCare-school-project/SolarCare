# GDPR

## Context

SolarCare processes personal data of Solar System owners: name, email, address, phone number, and the readings of their solar system. Readings are personal data too, because the serial number links them to a home.

## Decision

### Roles

| Party | Role | Agreement |
| --- | --- | --- |
| SolarCare | Controller | |
| External data protection officer | Data protection officer | Service contract |
| AWS | Processor | DPA with standard contractual clauses |
| Email provider | Processor | DPA |
| Regional technician company | Processor | DPA |

The data protection officer is external, because management and software engineers decide on the processing themselves.

### DPIA

SolarCare collects continuous data from private homes. A DPIA is done before launch and reviewed by the data protection officer. See [DPIA](../docs/dpia.md).

### Legal basis

| Processing | Legal basis |
| --- | --- |
| Readings, incidents, health checks, visits | Contract, Art. 6(1)(b) |
| Logs and audit log | Legitimate interest, Art. 6(1)(f) |
| Marketing emails | Consent, Art. 6(1)(a) |

### Hosting

AWS, region Frankfurt. AWS is a US company, so transfers are covered by the EU-US Data Privacy Framework and the standard contractual clauses. If the framework is no longer valid, SolarCare moves to an EU provider. The open-source stack makes this possible.

When SolarCare expands to larger institutions, a switch to Microsoft Azure can be considered, as many of them already use Microsoft. Azure is also a US company, so the same Data Privacy Framework applies.

### Retention

| Data | Kept for |
| --- | --- |
| Raw readings | 2 years, then daily totals |
| Incidents and visits | Contract + 3 years |
| Customer details | Contract + 30 days |
| Application logs | 30 days |
| Audit log | 1 year |
| Backups | 30 days |

### Owner rights

- Access and export of own data in the website (CSV).
- Correction of contact details in the website.
- Deletion on request within 30 days.

When a house is sold, the serial number is unlinked from the old owner first. The new owner does not see older readings.

### Data breach

Reported to management and the data protection officer at once. Management informs the authority within 72 hours if there is a risk to owners.

### Website

Only necessary cookies, no tracking.

## Consequences

- Deletion and export have to be built into the API and website.
- Every new processor needs a DPA first.
- Hosting depends on the Data Privacy Framework.
