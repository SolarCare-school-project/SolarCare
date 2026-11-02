# Incident: hardware already clear

Support already knows this incident is hardware. The evidence comes from monitoring, or from the house owner, such as a photo of a broken cable. The support team lead approves, and L1 assigns the third-party technician for that region. The software engineer is not in this path.

This is a UML sequence for that step.

How to read it:

- Read from top to bottom. The names repeated at the bottom are the same people, not a second group.
- A solid arrow is a request. A dashed arrow is the reply.
- A dotted box marked **alt** is a choice. Only one of the bracketed cases happens.
- A yellow note is a reminder on that step. It is not a message to another person.

```mermaid
sequenceDiagram
  actor L1 as L1 Support engineer
  actor Lead as Support team lead
  actor L3 as L3 Regional technician

  Note over L1: Hardware is already clear
  L1->>Lead: Ask to assign the regional technician
  Lead-->>L1: Approve
  L1->>L3: Assign the technician team for that region
```

If the support team lead does not approve, the incident stays with L1.
