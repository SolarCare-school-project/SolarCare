# Data security

## Context

SolarCare stores the readings of Solar System owners and a link to each customer in the CRM. The contact details stay in the CRM. Readings can show when people are at home, so they need the same protection as contact details. Each role needs different data, and no role needs all of it. What is collected is in [Data collection](data-collection.md). Retention and GDPR roles are in [GDPR](gdpr.md).

## Decision

### Who may see what

| Role | Data they can see | What they can do |
| --- | --- | --- |
| Solar System owner | Own readings, own contact details, own incidents | Raise an incident, edit contact details, export and delete own data |
| Support engineer | Readings and contact details only for homes with an open incident. Incidents are opened by the owner or automatically by SolarCare when readings look wrong | Handle incidents, ask the team lead for approval |
| Support team lead | Incidents waiting for a decision, with name of the owner | Approve or refuse a transfer or a technician visit |
| Software engineer (CFG) | Anonymised or test data. Live data of one home only after an approved transfer | Investigate and fix software bugs, assign a technician |
| Regional technician company | Assigned incidents in its region: address, phone, name, readings | Record the visit and the replaced part, register a new system |
| Management | Anonymised totals over at least 10 homes | Reports and planning |

Every role sees only what it needs for its task. Access to live data of a home is recorded in the audit log, see [Logging](logging.md).

### How access is protected

- **Authentication:** login is handled by AWS Cognito (region Frankfurt). Owners log in with email and password. Staff and technicians must also use two-factor authentication. After login, the user gets a token that the API checks on every request. SolarCare never stores passwords itself.
- **Authorization:** the API checks every request against the role and scope of the user. The website never reaches the database directly.
- **CRM:** the API connects to the CRM with its own integration login. It reads only the fields the role may see, and never writes CRM details to the database or the logs.
- **Gateways:** each gateway has its own login at the broker and may only publish to its own base topic. It cannot read other homes.
- **Data in transit:** TLS on every connection: gateway to broker, broker to API, website to API, API to database, API to CRM. See [Interfaces](interfaces.md).
- **Data at rest:** database and backups are encrypted. Passwords and keys are kept in a secrets store, not in the code.
- **Monitoring of access:** the audit log records who opened which home or incident. Failed logins and denied topics at the broker are logged.
- **Access reviews:** accounts of own staff are removed when someone leaves. Technician companies must report when an employee leaves, as part of their DPA. Accounts not used for 90 days are disabled automatically. Technician company accounts are also checked every 6 months.

## Consequences

- An owner, technician or employee can only see what their task needs.
- No one looks at a home without an incident.
- A stolen gateway login only exposes the readings of one home.
- Two-factor authentication adds a step for staff and technicians.
- Software engineers often work without live data, which can make some bugs harder to find.
- Personal data lives in the CRM, which we do not run. Its access rules matter as much as ours.
- Login depends on AWS Cognito. It uses standard tokens, so it can be replaced if SolarCare leaves AWS.
