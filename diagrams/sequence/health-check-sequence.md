# Agreed date: the support engineer starts the health check

This health check exists only for a part that has its own agreed date on the record. The date comes from that model's maker guidance or from the policy agreed with the Solar System owner. There is no one number of years for every solar part.

The years and warranty lengths below are proposals to check. They are not rules we have confirmed.

IEC 62446-2 (in Germany, DIN EN 62446-2) says to follow the manufacturer. It does not set a replacement year. For a private home in Germany there is also no general law that says parts must be swapped on a calendar. Insurers often ask for a yearly look and an electrical check about every 4 years (DIN EN 62446-1). Those are inspections. They are not the same as replacing a part because a date arrived.

| Part | Health check on a date? |
| --- | --- |
| String inverter | Yes, when that model has a service life or warranty end. Often talked about as 10 to 15 years. Warranties are often 5 to 10 years. Store the check date for that unit. The visit decides whether to replace it. |
| Battery | Yes, when the maker gives a year, a cycle count, or a capacity limit. A 10-year warranty is common. Whichever limit is reached first opens a health check. The visit decides whether to replace it. |
| Inverter filter, and other parts the manual mentions on a schedule | Yes. The manual's date opens a health check. The visit decides whether to swap the part. |
| Surge protector | Only if that device's manual gives a maximum time. Otherwise check when its indicator says end of life, or after a strike. |
| Panels | No. A performance warranty of about 25 to 30 years is a promise about output, not a check date. Replace when a health check or a technician visit finds it damaged or below the promised output. |
| Cables, connectors, mounting | No. Inspect them during a health check or a technician visit. Replace when that visit says so. |
| Microinverters and optimizers | No short check date. Warranties are often 20 to 25 years. Use the manual for that model if it names a date. |

This is a UML sequence for the preventive case: a date opens a health check, not a replacement.

How to read it:

- Read from top to bottom. The names repeated at the bottom are the same people, not a second group.
- A solid arrow is a request. A dashed arrow is the reply.
- A dotted box marked **alt** is a choice. Only one of the bracketed cases happens.
- A yellow note is a reminder on that step. It is not a message to another person.

![UML health ceck](diagrams/sequence/diagrams/UML-health_check_sequence.drawio.png)


A replacement happens only when the check says so. An incident at the home follows the incident sequences. This scheduled check does not.

The terms are defined in the [Glossary](../../docs/glossary.md).
