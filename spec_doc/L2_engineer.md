<h1>User stories - L2 software engineer (CFG) </h1>

<h3> User Story 1 </h3>

As a CFG software engineer, I want to review a transferred incident and the relevant system readings, so that I can identify whether a software bug needs fixing or a hardware issue which resuires a technician visit onsite.

<h4> EARS requirements for that story </h4>

- Event driven: When an authorised CFG engineer opens an incident transferred to CFG after team lead approval, SolarCare shall display the incident details, L1 investigation findings and latest available readings for the linked solar system.

- Event driven: When an authorised CFG engineer saves their investigation findings, SolarCare shall add them to the incident’s case in Salesforce.

- Event driven: When an authorised CFG engineer assigns a regional technician to the incident, SolarCare shall save the assignment in the incident’s case in Salesforce.

<h4> How? </h4>

- The CFG engineer logs in to SolarCare using two-factor authentication and opens an incident transferred to CFG after team lead approval.

- The frontend sends a GET request to the backend through the REST API over HTTPS to retrieve the incident information.

- The backend checks the engineer’s role and confirms that the incident was transferred to CFG with team lead approval.

- The backend retrieves the incident details and L1 investigation findings from Salesforce, and the relevant system readings from the SolarCare database.

- The backend sends the information to the frontend, which displays it for the CFG engineer to investigate.

- When the CFG engineer refreshes the incident view, the frontend requests the latest available system readings from the backend and updates the display.

- The CFG engineer records their investigation findings in SolarCare. The backend checks their permission and saves the findings in the incident’s Salesforce case.

- If the investigation identifies a software bug, CFG works on the Fix. If it identifies a hardware issue, CFG assigns the regional technician through SolarCare.



