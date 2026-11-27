<h1>User stories - Solar system owner</h1>

<h3>User Story 1</h3>

      As a solar system owner, I want to see the current status and health of my own solar system, so that I know it is working properly and can raise an incident when something looks wrong.

    <h4>EARS requirements for that story</h4>

      Event driven: When the owner opens their system in SolarCare, SolarCare shall display the current status and health of only that owner's own solar system.

      Unwanted behaviour: If a reading is missing or outdated, then SolarCare shall mark the affected component as "no current data" instead of showing a stale value.

<h4>How?</h4>

- The owner logs in to the SolarCare website and opens their own system; access is limited to the systems linked to their home through the inverter serial number.
- The frontend sends a GET request to the backend through the REST API.
- The backend retrieves the system record and the latest readings (microinverter power, energy, temperature and status; panel inputs; gateway online status and signal) from the database, which is fed by the OpenDTU gateway through the MQTT broker.
- The frontend displays the current status and health of the system.
- Server-Sent Events (SSE) push live changes, so the frontend refreshes the affected components without a full page reload.
- If the owner finds an issue, they raise an incident, which SolarCare opens as a case in the CRM (Salesforce) from a template, storing only the customer ID.
- Role-Based Access Control (RBAC) ensures the owner can only see and act on their own system's data.

<hr>
