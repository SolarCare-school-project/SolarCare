# SolarCare context diagram

C4 level 1. SolarCare is one box. Arrows show who starts the communication. The gateway stays outside SolarCare, at the home.

```mermaid
C4Context
title SolarCare context

Person(household, "Household", "Sees only their own home and can open a report")
Person(team, "SolarCare team", "Monitor team, technician, and team lead")
System(solarcare, "SolarCare", "Coordinates health checks and replacement of parts for one home")
System_Ext(gateway, "Gateway at the home", "Reads the inverter and publishes measurements. OpenDTU is a candidate.")

Rel(household, solarcare, "Uses the household screen", "HTTPS")
Rel(team, solarcare, "Uses the team screen", "HTTPS")
Rel(gateway, solarcare, "Publishes readings", "MQTT")
```

Email and a supplier system are not on this picture. They are not part of the current design.
