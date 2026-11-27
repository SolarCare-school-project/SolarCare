<h1>User stories - L1 support engineer</h1>

<h3>User Story 1</h3>
      
      As a L1 support engineer, I want to be able to check users solar panel in the system. 

    <h4>EARS requirements for that story</h4>

      Event driven: When the user or their solar panel is searched up in SolarCare, SolarCare shall display all available information.

<h4>How?</h4>
      
- The L1 support engineer searches for a customer or their solar panel using a unique identifier or relevant search parameters.
- The frontend sends a GET request to the backend through a REST API.
- The backend retrieves the relevant information from the database, including solar panel details, component status, maintenance history and             reported faults.
- The frontend displays the retrieved information.
- Server-Sent Events (SSE) are used to notify the frontend whenever relevant data changes in the database.
- Upon receiving an event, the frontend retrieves the updated information and automatically refreshes the relevant UI components without                 requiring a full page reload.
- Role-Based Access Control (RBAC) ensures that the L1 support engineer can only access authorized information.

<hr>
