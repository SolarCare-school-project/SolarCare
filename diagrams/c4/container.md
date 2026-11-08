# SolarCare container diagram

C4 level 2. This opens the SolarCare box into the website, the phone app, the API, the database, and the MQTT broker. A container here means a program or a data store that runs on its own. It does not mean a Docker container.

Arrows show who starts the communication.

```mermaid
C4Container
title SolarCare containers

Person(owner, "Solar System owner", "Sees only their own home")
Person(staff, "SolarCare staff", "Support engineer, support team lead, software engineer, management")
Person(tech, "Regional technician company", "Assigned incidents in its region")

Container_Boundary(home, "Home") {
    Container(gateway, "Gateway", "OpenDTU on ESP32", "Reads the inverter and publishes readings")
}

Container_Boundary(solarcare, "SolarCare") {
    Container(web, "Website", "React", "Screens for owners and staff")
    Container(app, "Phone app", "Not decided", "Registers systems and records visits")
    Container(api, "API", "Python, FastAPI", "Incidents, health checks, the part record, and the reading inbox")
    ContainerDb(db, "Database", "PostgreSQL", "Homes, parts, readings, and incidents")
    Container(broker, "MQTT broker", "Mosquitto", "Receives readings from the homes")
}

Rel(gateway, broker, "Publishes readings", "MQTT over TLS")
Rel(api, broker, "Subscribes", "MQTT over TLS")
Rel(owner, web, "Uses", "HTTPS")
Rel(staff, web, "Uses", "HTTPS")
Rel(tech, app, "Uses", "HTTPS")
Rel(web, api, "Calls", "HTTPS")
Rel(app, api, "Calls", "HTTPS")
Rel(api, db, "Reads and writes", "SQL over TLS")
```

The gateway stays in the home. The website and the phone app do not connect to the broker or the database. There is no line from the gateway straight to the database. The API subscribes to the broker, so the broker is not the side that opens that connection.

## Who can see what

| Role | Uses | Sees |
| --- | --- | --- |
| Solar System owner | Website | Own home only |
| Support engineer | Website | Homes with an open incident |
| Support team lead | Website | Incidents waiting for approval |
| Software engineer (CFG) | Website | One home, after an approved transfer |
| Regional technician company | Phone app | Assigned incidents in its region |
| Management | Website | Anonymised totals |

The full rules are in [Data security](../../adr/data-security.md). What is collected is in [Data collection](../../adr/data-collection.md). The terms are defined in the [Glossary](../../docs/glossary.md).
