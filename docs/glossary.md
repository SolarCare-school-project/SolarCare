# Glossary

The terms used across the SolarCare docs. Each role and concept has one name. The "Not used" column lists older names that should no longer appear.

## Roles

| Term | Meaning | Not used |
| --- | --- | --- |
| Solar System owner | Private house owner who uses SolarCare to watch their own system and raise incidents | House owner, Solar Panel owner, private customer |
| Support engineer (L1) | Handles incidents remotely. Sees only homes with an open incident | Monitor team, monitor |
| Support team lead | Approves a transfer to the software engineer or a technician visit | Team lead |
| Software engineer (L2, CFG) | Investigates after an approved transfer. Fixes software bugs. CFG is the Customer Focus Group, a group in R&D | Developer |
| Regional technician company (L3) | External company that installs systems and does visits in its region | Technician team, third-party replacement company |
| Management | Founders. See only anonymised totals | Administrator |
| External data protection officer | Outside service that checks data protection | |

## Process

| Term | Meaning | Not used |
| --- | --- | --- |
| Incident | Unplanned problem at a home. Opened by the owner or automatically by SolarCare | Job, ticket, report |
| Case | The record of an incident or health check in the CRM. Created from SolarCare | |
| Proactive incident | An incident SolarCare opens itself because readings look wrong | System trigger |
| Health check | Planned inspection on an agreed date. The visit decides whether a part is replaced | Replacement |
| Transfer | Handing an incident from the support engineer to the software engineer, after approval | |
| Support levels L0–L3 | L0 FAQ, L1 support engineer, L2 software engineer, L3 regional technician company | |

## System

| Term | Meaning | Not used |
| --- | --- | --- |
| Reading | One value from the solar system, such as power or daily energy, with the time it was measured | Measurement |
| Gateway | Small device in the home (OpenDTU on an ESP32) that reads the inverter and sends readings | DTU |
| Inverter | Turns the panels' DC power into AC power. SolarCare reads Hoymiles microinverters | |
| Serial number | Unique number of the inverter. Links readings to a home | |
| MQTT broker | Server that receives readings from all gateways and passes them to the API (Mosquitto) | |
| API | The only way into the data. The website talks to it, never to the database | Backend |
| Website | Screen for owners, support, technicians, software engineers and management. Also works on a phone and offline | Web dashboard |
| CRM | The company's CRM (Salesforce), outside SolarCare. Holds customers, cases, visits and part records | |
| Customer ID | The customer's ID in the CRM. SolarCare stores only this, plus its link to the serial number | |
| Audit log | Record of who opened which home or case, and each time customer details are read from the CRM. Nobody can change it | |

## Documentation and data protection

| Term | Meaning |
| --- | --- |
| ADR | Architecture decision record. A short file for one decision: context, decision, consequences |
| C4 | Diagrams that zoom in step by step: context, containers, components |
| GDPR | EU data protection law (DSGVO) |
| DPA | Data processing agreement (AVV) with a company that handles data for SolarCare |
| DPIA | Data protection impact assessment. Risks for owners and the measures against them |
| TLS | Encryption of a connection |
