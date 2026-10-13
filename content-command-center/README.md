# Yellow Bag | Content Command Center

Stage 1 of the Content Command Center: the branded application shell and working interface. It uses the same design system as the Growth Command Center but is a separate application.

Open `index.html` in a browser. It is a single file with no build step. It needs an internet connection for Google Fonts and the graph library (Cytoscape.js 3.30.2 from cdnjs). Editor's Note loads from fonts installed on the machine; Cormorant Garamond is the fallback.

## What works now

| Section | Stage 1 behavior |
|---|---|
| Content Status | Opens by default. Shows the current date, Monday–Sunday week, ISO week and plan week in America/Indiana/Indianapolis time. Six completion tiles, a progress bar, channel breakdown, Core Concept Coverage, production pipeline, current week schedule, up to three observations and Immediate Attention. Period switch: week, month, quarter, all. |
| Content Calendar | Week, month and quarter views with six filters. Generates rule-based proposals for next week, month or quarter. You can edit titles and dates, remove items, approve one or all, or reject with a revision request. |
| Content Library | Search, filters and sorting. Each record opens with full metadata, relationships, versions, reviews, a status control and "add related content". |
| Sticky Web | Interactive graph with pan, zoom, filters, a focused content-family view, concepts you can collapse, a detail panel, the network's gaps and suggested connections you can approve. |
| Content Studio | Review against standards. Separates mandatory violations, substantive weaknesses and optional improvements, and cites the section of the standard. Includes the publishing checklists from the Content Standard. |
| Content Intelligence | Commercial outcomes kept separate from visibility metrics, three attribution levels, rule-based recommendations with the rules stated, and a weekly status review. |
| Standards & Settings | Standards library with upload, draft, approve and supersede. Plan dates and capacity, core concepts, integration status, the ClickUp status map, the ClickUp write queue, the audit trail and data mode. |
| Ask the Content Engine | Answers operational questions from the records: plan week, published counts, what is overdue, what awaits approval, concept coverage and Sticky Web gaps. Anything else says it needs Stage 3 or 5 rather than guessing. |

## Data integrity rules built in

- **Demonstration mode is labeled on every screen.** The records are examples, not Yellow Bag's content or results. Live mode shows empty states until ClickUp is connected.
- **One piece counts once.** A deliverable is one record. Reposts and early subscriber access are distribution placements on that record and do not add to the totals.
- **No double counting across concepts.** Each piece counts once, under its primary concept, so concept totals equal the plan total. Secondary concepts are listed separately.
- **Plan totals include approved items only.** Proposed items show in Awaiting Approval until someone approves them.
- **The publishing status rules are fixed.** On Track means nothing due before today is unpublished. At Risk means one late piece (15% or less of what was due), or a piece due within three days that still needs approval. Behind is anything worse.
- **Approved versions are locked.** Edits after approval start a new draft revision.
- **Writes to ClickUp are queued, not sent.** Approvals and status changes go into a write queue keyed by asset ID, so a retry cannot create a duplicate task. Nothing is written to any external system in Stage 1.
- **State is kept locally.** Proposals, approvals, the audit trail and uploaded standards are stored in this browser's local storage. This is the minimal technical cache, not a second editorial database.

## Governing configuration

Concepts, categories, cadence and measurement come from the **Yellow Bag Content Standard (Revised October 2026)**:

- Success Assurance (foundational message)
- One Customer at a Time (October–December 2026)
- Foundations, Controls, and Governance (FCG) (January–March 2027)
- Managing Project Risk (April–June 2027)

The active plan defaults to Q4 2026 (October 1 to December 31), so October 13, 2026 falls in week 3 of 14.

## ClickUp findings (read-only, October 13, 2026)

Content work sits in three lists under YB Team Management › Marketing, and each list uses its own statuses:

| List | Statuses | Maps to |
|---|---|---|
| Content Calendar | briefing, writing, proofreading, publication, complete | Researching → Published |
| YB Social Media | Open, writing, ready, publish, scheduled, posted, plan, complete, Closed | Planned → Published |
| 2025 Blogs | Open, outline & prep, ready, writing, semrush, review & approval, approved, rejected, publish, scheduled, Closed | Planned → Published |

The full map is in Standards & Settings. Some existing fields can be reused: Date Posted, ORI or RP (Original / Repost), Platform, Initiative, Series, CTA and task links.

These fields are missing and are needed before Stage 2: YB Asset ID, Core Concept, Secondary Concepts, Content Type, Blog Category, Commercial Situation, Target Audience, Published URL, Approval Status and Next Review Date. Adding them changes the workspace, so it waits for approval.

## Integration boundary for Stage 2

The page never holds credentials. ClickUp reads and writes go through a server-side connector that holds the token. That connector:

1. reads the three lists, applies the status map and returns content records in the shape the page already uses (`S.assets`, `S.links`);
2. drains the write queue: it creates tasks for approved items and updates statuses, using YB Asset ID as the key that prevents duplicates;
3. reports the result of each operation back, so the queue can show what succeeded and what failed.

There are two low-cost ways to run that connector, and the choice should be made at Stage 2. One is a single serverless function. The other is Claude's existing ClickUp connection, used through an artifact capability that reads connected data. Neither one requires a new paid platform.

## Known limits of Stage 1

- Observations, plans and reviews are rule-based checks against the standard. Claude drafting, judgment of the six editorial elements and conflict detection between standards come in Stage 3.
- No performance source is connected. The figures in Intelligence are demonstration values.
- Uploaded standards are stored only in the browser that uploaded them.
- The Marketing Communication Practice is referenced by the Content Standard but has not been uploaded.
