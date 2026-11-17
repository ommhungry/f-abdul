# Documentation guide

[Back to the repository overview](../README.md)

## Source of truth

The latest direction was recorded on **15 November 2026**. Read these first:

1. [Proposed features](planning/proposed-features.md): calendar-connected reminders are the current scope, financial tracking is proposed, and booking moves to phase 2.
2. [AI implementation, routing, and personalities](design/ai-implementation.md): paid APIs, two model tiers, and three selectable personalities (provisional).
3. [Timeline and development phases](planning/timeline-and-development-phases.md) (v0.4): schedule, gates, estimates and open decisions, reconciled to that direction.

Older documents carry a status note at the top. When a document disagrees with the three above, the three above win. Feature priorities, model choices, estimates, and the provisional personality concepts still need team validation.

## Document status and relationship

| Document | Date | Status |
|---|---|---|
| [Proposed features](planning/proposed-features.md) | 15 Nov | **Current.** Includes the mapping from earlier feature IDs |
| [AI implementation](design/ai-implementation.md) | 15 Nov | **Current.** Exact models and personalities still to validate |
| [Timeline](planning/timeline-and-development-phases.md) | 17 Nov | **Current draft (v0.4).** Not approved |
| [Advisor briefing](governance/advisor-briefing-2026-11.md) | 12 Nov, updated 17 Nov | Updated to the 15 Nov direction |
| [Database design](design/database-design.md) | 12 Nov, updated 17 Nov | Schema is scope-independent; booking and sharing tables are kept for phase 2 and for deferred family sharing; neither is needed for April |
| [Research brief](research/business-and-architecture-brief.md) | 6 Nov | Research snapshot and long-term vision. Its feature list is not a commitment |
| [Kickoff agenda](meetings/2026-11-07-kickoff.md) | 7 Nov | Historical agenda; outcomes were never recorded |

The governance charter was withdrawn on 17 November 2026. The earlier combined proposal, requirements specification (SRS v0.9), review material, extended scope and competitor analysis exist only on the unmerged `jui` and `vj` branches and are superseded by the 15 November direction where they differ.

The research brief describes the **long-term product vision**. The timeline proposes a **smaller evaluated prototype** based on three members working 3–4 hours each per week. Do not interpret the research feature list as a commitment to deliver every capability by April.

## Updating the documentation

- Add future meeting records as `meetings/YYYY-MM-DD-topic.md`.
- Record approval dates and versions when the team adopts a baseline.
- Update cross-references when moving or renaming a file.
- Keep source citations and clearly label estimates, hypotheses, and confirmed decisions.
- When scope changes, update [Proposed features](planning/proposed-features.md), the timeline, and the advisor briefing together.
- Update this guide when adding a major document.

The architecture and timeline diagrams use fenced Mermaid blocks for GitHub rendering; their accompanying text and tables carry the substantive details.
