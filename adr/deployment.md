# Deployment

**Status:** proposed

## Context

[How the system is changed after launch, and what must stay reliable while it changes.]

## Decision

| Step | What happens | Who does it | What checks it |
| --- | --- | --- | --- |
| Change is proposed | | | |
| Review | | | |
| Build and test | | | |
| Release | | | |
| Check after release | | | |
| Roll back | | | |

Each release writes a deployment log: which version went out, when, and who approved it. The pipeline log can be read by the people who run releases, and not by the Solar System owner or support. See [Logging](logging.md).
[Tools: GitHub, GitHub Actions, Docker.]

## Consequences

- [What this makes safer or faster.]
- [What a bad release still costs.]

## Open questions

- How long is the deployment log kept, and who can read it?
- How is a change to the API or the website reviewed, tested, built, deployed, checked, and rolled back?
- Where are secrets kept, and who may release a change?
- When is an old app version blocked and the user asked to update?