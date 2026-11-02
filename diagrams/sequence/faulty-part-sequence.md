# Faulty part: the monitor team starts the job

The monitor team sees a reading that a part looks faulty. They create a job for a technician, then tell the house owner.

This is a UML sequence for one corrective case. It does not show the website talking to the API.

```mermaid
sequenceDiagram
  actor Monitor as Monitor team
  participant Site as SolarCare
  actor Tech as Technician
  actor Home as House owner
  actor Lead as Team lead

  Monitor->>Site: Open this unit's readings
  Site-->>Monitor: A part looks faulty
  Monitor->>Site: Create a job for the technician
  Site-->>Tech: Assign the job
  Monitor->>Home: A visit is planned
  Note over Tech,Home: Technician visits and checks the part
  alt No fault, and the last visit also found no fault
    Tech->>Home: Visit fee applies
  else Fault found
    opt Part is not one that commonly breaks
      Tech->>Lead: Ask for approval
      Lead-->>Tech: Approve
    end
    alt Covered by warranty
      Note over Tech,Home: This visit is free
    else Not covered by warranty
      Tech->>Home: Agree to the cost of the new part
      Home-->>Tech: Agree
    end
    Note over Tech: Technician replaces the part
  end
```

Still to check with the team: if the part is one that commonly breaks, can the technician start the replacement with no team lead?

Proposed team policy, still to be agreed: the visit is free when the fault is under warranty, and a visit fee applies after two visits in a row with no fault.

In this drawing the team lead is asked before the house owner is asked to pay.
