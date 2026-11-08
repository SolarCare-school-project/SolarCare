# SolarCare component diagram

C4 level 3. This opens the API only. The website, the phone app, the broker, and the database stay as neighbors. The modules below are parts of the API. They are not separate programs.

Arrows show who starts the communication.

```mermaid
C4Component
title Components inside the SolarCare API

Container(web, "Website", "React", "Screens for owners and staff")
Container(app, "Phone app", "Not decided", "Used by the regional technician company")
Container(broker, "MQTT broker", "Mosquitto", "Receives readings from the homes")
ContainerDb(db, "Database", "PostgreSQL", "Homes, parts, readings, and incidents")

Container_Boundary(api, "API") {
    Component(access, "Access check", "Allows a user to see only the homes and incidents they are permitted")
    Component(incidents, "Incidents", "Opens an incident or a health check")
    Component(parts, "Part record", "Updates the installed part when a replacement is recorded")
    Component(inbox, "Reading inbox", "Stores readings with the time they were measured")
}

Rel(web, access, "Calls", "HTTPS")
Rel(app, access, "Calls", "HTTPS")
Rel(access, incidents, "Allows the action")
Rel(incidents, parts, "Records a replacement")
Rel(inbox, broker, "Subscribes", "MQTT")
Rel(inbox, db, "Writes readings")
Rel(incidents, db, "Reads and writes incidents")
Rel(parts, db, "Updates the installed part")
Rel(access, db, "Reads who may see a home")
```

An incident or a health check is handled in the incidents module. The part record changes only when a visit says a part was replaced. The reading inbox does not decide that a part has failed.
