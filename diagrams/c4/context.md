# SolarCare context diagram

C4 level 1. SolarCare is one box. Arrows show who starts the communication. The gateway stays outside SolarCare, at the home.

```mermaid
C4Context
title SolarCare context

Person(owner, "Solar System owner", "Sees only their own home and can raise an incident")
Person(staff, "SolarCare staff", "Support engineer, support team lead, software engineer, management")
Person(tech, "Regional technician company", "Registers systems and records visits for assigned incidents")
System(solarcare, "SolarCare", "Monitors solar systems and handles incidents and health checks")
System_Ext(gateway, "Gateway at the home", "Reads the inverter and publishes readings. OpenDTU on an ESP32.")

Rel(owner, solarcare, "Uses the website", "HTTPS")
Rel(staff, solarcare, "Uses the website", "HTTPS")
Rel(tech, solarcare, "Uses the phone app", "HTTPS")
Rel(gateway, solarcare, "Publishes readings", "MQTT over TLS")
```

The terms are defined in the [Glossary](../../docs/glossary.md).
