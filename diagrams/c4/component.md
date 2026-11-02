# SolarCare component diagram

C4 level 3. This opens the API only. The website, the broker, and the database stay as neighbors. The modules below are parts of the API. They are not separate programs.

Arrows show who starts the communication.

```mermaid
C4Component
title Components inside the SolarCare API

Container(web, "Website", "React, candidate", "House owner screen and team screen")
Container(broker, "MQTT broker", "Mosquitto, candidate", "Routes readings from the home")
ContainerDb(db, "Database", "PostgreSQL, candidate", "Homes, parts, readings, and jobs")

Container_Boundary(api, "API") {
    Component(access, "Access check", "Allows a user to see only the homes and jobs they are permitted")
    Component(jobs, "Jobs", "Opens a support ticket or a health check")
    Component(parts, "Part record", "Updates the installed part when a replacement is recorded")
    Component(inbox, "Reading inbox", "Stores readings with the time they were measured")
}

Rel(web, access, "Calls", "HTTPS")
Rel(access, jobs, "Allows the action")
Rel(jobs, parts, "Records a replacement")
Rel(inbox, broker, "Subscribes", "MQTT")
Rel(inbox, db, "Writes readings")
Rel(jobs, db, "Reads and writes jobs")
Rel(parts, db, "Updates the installed part")
Rel(access, db, "Reads who may see a home")
```

A support ticket or a health check becomes a job. The part record changes only when that job says a part was replaced. The reading inbox does not decide that a part has failed.
