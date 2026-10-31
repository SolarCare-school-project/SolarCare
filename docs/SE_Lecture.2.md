# Architecture, Networks and Access Rights

**CODE · Software Engineering · Class summary** (excluding team-specific topics) · 21 October 2026

> **Core idea:** Software engineering does not just mean writing code. It is about understanding systems, designing their structure sensibly, and justifying decisions in a traceable way.

---

## 1 Software architecture and diagrams

Software architecture describes the important components of a system and their relationships:

- Which components are there?
- How do they communicate?
- What information is exchanged?
- Which fundamental decisions shape the system?

A diagram should make something understandable for a specific target audience. More detail is not automatically better. System boundaries, components and the meaning of the connecting arrows should be easy to follow.

Adam recommends Simon Brown's **C4 model**: first show the system and its environment from a bird's-eye view, then zoom into individual parts as needed. A formal UML representation is not required for the current assignment. As a further reference, he mentions Martin Fowler's writings on architecture.

## 2 Authentication and authorization

| Concept | Question | Example |
| --- | --- | --- |
| **Authentication** | Who are you? | Logging in with username and password |
| **Authorization** | What are you allowed to do? | Viewing data, changing settings or managing users |

A successful login does not automatically permit every action. Using a case study of a central solar dashboard, the session also distinguished between *reading information* and *controlling a system*. Being allowed to view measurements should not automatically mean being allowed to shut down installations.

## 3 Data protection and information needs

The central question is: **What information do we need for which purpose – and who should be allowed to see it?**

- Aggregated statistics can be sufficient without making individual people or installations visible.
- Several data sources combined can enable additional, unexpected inferences.
- An organization can need data without all employees needing access to it.

Data protection is therefore also a question of architecture and permissions, not a topic that only comes up after development.

## 4 The internet as a network of networks

The internet connects many independent networks. The session's most important terms, simplified:

| Term | Meaning |
| --- | --- |
| **IP address** | Address for communication within an IP network |
| **Port** | Assigns communication to the appropriate service or endpoint |
| **Socket** | Communication endpoint; explained in the session in simplified terms via IP address and port |
| **DNS** | Resolves domain names, for example into their associated IP addresses |
| **BGP** | Used to exchange routing information between networks |

DNS and routing are not the same thing: DNS helps with name resolution; routing determines the paths along which data is forwarded. Communication depends on several systems and can fail.

## 5 Failures are part of architecture design

A system should not only be planned for normal operation. Key questions:

- What happens when a connection drops?
- What does a local device do if it cannot reach the cloud – and what does the cloud do if a device or data source is missing?
- Which functions can keep running?

**Behavior in the event of failure is a deliberate design decision.**

## 6 Software as a sociotechnical system

Technology is connected to people, organizations and societal rules. Software engineering therefore also includes questions of control, decision-making authority, responsibility for errors and societal consequences. Technically possible does not automatically mean sensible or appropriate.

## 7 AI in learning

Adam's position: AI can explain unfamiliar concepts in different ways, but should not replace your own engagement with the material. Anyone who delegates tasks completely may get a result without understanding how it was reached. Especially without prior knowledge, wrong answers are hard to recognize.

## Course organization and outlook

- **Focus:** understanding and explaining interfaces, and justifying architecture and technology decisions.
- According to Adam, a fully implemented product is not required for the exam; practical exercises remain part of the course.
- The exact assessment formats are to be clarified in a one-pager. By the next session, the architecture diagrams should be developed further.
- **Announced upcoming topics:** reliability, distributed systems, observability and cloud. Specific dates are not separately confirmed here.

---

*Basis: technically evaluated class notes from the online session; date taken from the source file name. Automatic transcript without audio verification. Team-specific content and personal contributions omitted. Specific incident anecdotes are not included as verified facts. Created on 22 October 2026 for private study purposes.*
