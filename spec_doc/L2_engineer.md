User stories - L2 software engineer (CFG)

User Story 1

As a CFG software engineer, I want to review a transferred incident and the relevant system readings, so that I can identify the cause and decide whether software work or a technician visit is needed.

EARS requirements for that story

How?

- The CFG engineer logs in to SolarCare using two-factor authentication and opens a transferred incident.
- The frontend sends a GET request to the backend through the REST API over HTTPS.
- The backend checks the engineer’s role and whether the transfer was approved.
- The backend retrieves the incident details and L1 findings from Salesforce, and the relevant system readings from the SolarCare database.
- The frontend displays the information for the engineer to investigate.
- Server-Sent Events (SSE) notify the frontend when relevant information changes. The frontend retrieves and displays the updated information.
- The engineer records their findings through SolarCare. The backend checks their permission and saves the findings in the Salesforce case.
- If the issue is a software bug, CFG works on the fix. If it is a hardware issue, CFG assigns the regional technician through SolarCare.
