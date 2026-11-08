# SolarCare container diagram

C4 level 2. This opens the SolarCare box into the website, the API, the database, and the MQTT broker. A container here means a program or a data store that runs on its own. It does not mean a Docker container.

Arrows show who starts the communication. Technology names are candidates, not final choices.

```mermaid
C4Container
title SolarCare containers

Person(owner, "House owner", "Sees only their own house")
Person(team, "Monitor team, technician, team lead", "Uses the team screen")

Container_Boundary(home, "Home") {
    Container(gateway, "Gateway", "OpenDTU is a candidate", "Reads the inverter and publishes measurements")
}

Container_Boundary(solarcare, "SolarCare") {
    Container(web, "Website", "React is a candidate", "House owner screen and team screen")
    Container(api, "API", "FastAPI is a candidate", "Jobs, health checks, the part record, and the reading inbox")
    ContainerDb(db, "Database", "PostgreSQL is a candidate", "Homes, parts, readings, and jobs")
    Container(broker, "MQTT broker", "Mosquitto is a candidate", "Routes readings from the home")
}

Rel(gateway, broker, "Publishes readings", "MQTT")
Rel(api, broker, "Subscribes", "MQTT")
Rel(owner, web, "Opens the house owner screen", "HTTPS")
Rel(team, web, "Opens the team screen", "HTTPS")
Rel(web, api, "Calls", "HTTPS")
Rel(api, db, "Reads and writes", "SQL")
```

The gateway stays in the home. The house owner screen does not connect to the broker. There is no line from the gateway straight to the database. The API subscribes to the broker, so the broker is not the side that opens that connection.

## Who can see what

| Role | Uses | Sees |
| --- | --- | --- |
| Solar System owner | Website | Own home only |
| Support engineer | Website | Homes with an open incident |
| Support team lead | Website | Incidents waiting for approval |
| Software engineer (CFG) | Website | One home, after an approved transfer |
| Regional technician company | Phone app | Assigned incidents in its region |
| Management | Website | Anonymised totals |

The full rules are in [Data security](../../adr/data-security.md).

What is taken from the house: power, daily energy, temperature, whether the inverter is reachable, and the time of the measurement. A fuller log is stored only when a report is sent, and only the team can read it. Name, address, and which home it is are personal data.
