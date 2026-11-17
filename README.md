# f-abdul

A proposed LINE-based AI personal assistant for office workers and other individuals managing everyday obligations in Thailand, initially focused on Bangkok.

## Current direction

Recorded by the project owner on **15 November 2026**:

- **Current scope:** calendar-connected reminders for everyday obligations, delivered through LINE.
- **Proposed, awaiting team decision:** a spending-and-bills assistant (receipt and transaction-email capture, budgets, bill and subscription tracking).
- **Phase 2:** booking, which depends on partnerships and evidence of a user base.
- **AI:** paid model APIs, task-based routing between two model tiers, and three selectable assistant personalities (provisional).

Details: [Proposed features](docs/planning/proposed-features.md) and [AI implementation](docs/design/ai-implementation.md). Where an older document disagrees with these, these win.

## Project status

Planning and team discussion. The documents contain research, recommendations, and decisions to approve; they do not represent a completed implementation or an approved commercial launch.

- Team: **three members**, each contributing **3–4 hours per week**.
- Proposal submission: **22 November 2026**, with architecture and overall design finalized.
- Project completion: **30 April 2027**.
- Proposed delivery baseline: a **small evaluated prototype**. The broader product vision remains in the research brief.
- The governance charter was withdrawn on 17 November 2026 because it no longer matched the direction. Roles, gates and change control will be re-documented once scope is settled.

## Documentation

Start with the [documentation guide](docs/README.md).

| Document | Purpose |
|---|---|
| [Proposed features](docs/planning/proposed-features.md) | Latest scope: feature register, spending pilot, estimates, open decisions |
| [AI implementation](docs/design/ai-implementation.md) | Hosting, model routing, response generation, personalities |
| [Project Timeline and Development Phases](docs/planning/timeline-and-development-phases.md) | Capacity-based SDLC, gates, workload estimates, and Gantt chart |
| [Advisor Briefing — November 2026](docs/governance/advisor-briefing-2026-11.md) | One-page summary of the project for the advisor |
| [Database Design](docs/design/database-design.md) | Relational schema, row-level security, provider recommendation |
| [Business and Architecture Brief](docs/research/business-and-architecture-brief.md) | Research snapshot: competition, workflows, booking feasibility, privacy, architecture, monetization, risks |
| [Meeting and Discussion — 7 November](docs/meetings/2026-11-07-kickoff.md) | Kickoff agenda (historical; outcomes never recorded) |

## Repository structure

```text
README.md
docs/
  README.md
  research/
    business-and-architecture-brief.md
  meetings/
    2026-11-07-kickoff.md
  governance/
    advisor-briefing-2026-11.md
  planning/
    proposed-features.md
    timeline-and-development-phases.md
  design/
    ai-implementation.md
    database-design.md
```

Other team members' earlier work (a combined proposal, requirements specification and review material) lives on the `jui` branch, and an extended scope with competitor analysis lives on the `vj` branch. Neither is merged, and where they disagree with the 15 November direction they are superseded.

## Working with these documents

- Record actual decisions and named owners in a dated meeting file; the 7 November file is a prepared agenda, not completed minutes.
- Update the timeline and proposed features together when scope, responsibilities, or capacity change.
- Keep proposed decisions distinct from approved commitments.
- Preserve the research date and sources when updating the brief.
- Keep credentials and personal user records out of the repository.
