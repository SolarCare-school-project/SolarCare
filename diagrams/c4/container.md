# SolarCare container diagram

C4 level 2. This opens the SolarCare box into the website, the API, the database, and the MQTT broker. A container here means a program or a data store that runs on its own. It does not mean a Docker container.

Arrows show who starts the communication. Technology names are candidates, not final choices.

```mermaid
C4Container
title SolarCare containers

Person(household, "Household", "Sees only their own home")
Person(team, "Monitor team, technician, team lead", "Uses the team screen")

Container_Boundary(home, "Home") {
    Container(gateway, "Gateway", "OpenDTU is a candidate", "Reads the inverter and publishes measurements")
}

Container_Boundary(solarcare, "SolarCare") {
    Container(web, "Website", "React is a candidate", "Household screen and team screen")
    Container(api, "API", "FastAPI is a candidate", "Jobs, health checks, the part record, and the reading inbox")
    ContainerDb(db, "Database", "PostgreSQL is a candidate", "Homes, parts, readings, and jobs")
    Container(broker, "MQTT broker", "Mosquitto is a candidate", "Routes readings from the home")
}

Rel(gateway, broker, "Publishes readings", "MQTT")
Rel(api, broker, "Subscribes", "MQTT")
Rel(household, web, "Opens the household screen", "HTTPS")
Rel(team, web, "Opens the team screen", "HTTPS")
Rel(web, api, "Calls", "HTTPS")
Rel(api, db, "Reads and writes", "SQL")
```

The gateway stays in the home. The household screen does not connect to the broker. There is no line from the gateway straight to the database. The API subscribes to the broker, so the broker is not the side that opens that connection.

## Who can see what

| Role | Data they can see | What they can do |
| --- | --- | --- |
| Household | Their own numbers, the measurement time, and their job status | Open a report. They cannot open the fuller log. |
| Monitor team | Readings and jobs for the homes in their scope | Open a job |
| Technician | The assigned job and the log attached to it | Record the visit and the new part |
| Team lead | The job that needs an unusual approval | Approve or refuse |

What is taken from the house: power, daily energy, temperature, whether the inverter is reachable, and the time of the measurement. A fuller log is stored only when a report is sent, and only the team can read it. Name, address, and which home it is are personal data.
