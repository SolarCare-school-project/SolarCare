# Incident: FAQ and the customer's software

An incident is an unplanned problem at a private house. L0 is the FAQ. L1 is a support engineer.

This is a UML sequence for the first step. It does not show the website talking to the API. If the software looks fine and the data looks wrong, the case continues in [incident-software-sequence.md](incident-software-sequence.md). A case that is already known to be hardware starts in [incident-hardware-sequence.md](incident-hardware-sequence.md).

```mermaid
sequenceDiagram
  actor Owner as House owner
  participant FAQ as L0 FAQ
  actor L1 as L1 Support engineer

  Owner->>FAQ: Look up the incident
  alt The FAQ answers it
    FAQ-->>Owner: Answer
  else The FAQ does not answer it
    Owner->>L1: Ask for help
    Note over L1: Check the customer's software, such as an app crash
    alt The problem is on the customer's side
      L1-->>Owner: Help with that software
    else Software looks fine, data looks wrong or strange
      Note over L1: Continue in the software incident sequence
    end
  end
```
