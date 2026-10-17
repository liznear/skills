---
name: tickets-management
description: 'Track work as tickets (local files or GitHub Issues): create, retrieve, update, dependency graph. Use when the user starts non-trivial work in a repo (a feature, refactor, or fix spanning multiple steps or files) so the work is captured as a ticket, and when they mention tickets: "create a ticket", "list tickets", "update ticket status", "what am I blocked on".'
---

Tickets are work items tracked per repo. Each ticket has a title, status,
dependencies on other tickets, and completion criteria. Storage location is
decided once per repo and persisted.

## Ticket storage gate (runs before EVERY operation)

1. If `.tickets/config.yaml` exists and has a `location`, proceed with it and
   never ask again in this repo.
2. Otherwise ask the user where tickets for this repo live:
   - `local`: ticket files under `.tickets/` (suggested default)
   - `github`: GitHub Issues in the repo's default remote (via `gh` CLI)
   - or explicitly decline ticket tracking for this repo
3. Persist an explicit choice by writing `.tickets/config.yaml`:
   ```yaml
   location: local  # or github
   ```
4. If the user declines: write nothing, create nothing, and continue the
   actual work without tickets. You will ask again next invocation — the gate
   exists precisely so no ticket or config is ever created unprompted.
5. If the user later asks to switch location, update the config on request;
   do not migrate existing tickets unless asked.

## Ticket shape

Every ticket, regardless of location, carries:

- `id`: `TICKET-NNN` (local, next free number) or the issue number (github).
- `title`: one line, imperative.
- `status`: `open` | `in-progress` | `done`.
- `depends_on`: ids of tickets that must be `done` before this one starts.
  GitHub tickets reference issue numbers; local tickets reference `TICKET-NNN`.
- `completion criteria`: a checklist. A ticket may only be marked `done` when
  every item is checked.

Local ticket file template (`.tickets/TICKET-NNN-slug.md`):

```markdown
# <title>

status: open
depends_on: []

## Completion Criteria

- [ ] <criterion>

## Notes

<optional context>
```

For GitHub tickets, put `status`/`depends_on` and the criteria checklist in
the issue body under the same headings (`## Status`, `## Depends on`,
`## Completion Criteria`), since Issues have no extra fields.

## Create

1. Run the storage gate.
2. From the user's request, draft the ticket: title, completion criteria, and
   dependencies (empty if none). Ask the user to confirm the criteria if they
   were not stated explicitly — criteria must be verifiable, not vague.
3. Write it:
   - local: create `.tickets/TICKET-NNN-slug.md` from the template.
   - github: `gh issue create --title "<title>" --body "<body>"`, then read
     back the issue number and record dependencies in the body if any.

## Non-trivial work (implicit ticket)

When this skill triggers because the user is starting non-trivial work
(multi-step feature, refactor, or fix — not an explicit ticket request):

1. Run the storage gate. If the user declines tracking, skip this section
   and just do the work.
2. Before starting, create one ticket for the work: title from the task,
   completion criteria from its definition of done, `depends_on` = open
   tickets this work builds on (empty if none).
3. While working, keep the criteria checklist current.
4. On completion, mark criteria checked and set status `done` per the Update
   rules. If the work is abandoned or descoped, say why in Notes instead.

## Retrieve

- local: list `.tickets/*.md` and read front sections; report id, title,
  status, deps, unchecked criteria.
- github: `gh issue list --state all --json number,title,state,body` and parse
  the body sections.
- Default listing view is a dependency graph, not a flat list:
  - Roots are tickets with no `depends_on`; each ticket's children are the
    tickets that depend on it (shared subtrees may repeat).
  - Tag each node `[status]` plus `READY` (not `done` and all deps `done`) or
    `blocked by <ids>`.
  - If a dependency cycle exists, report the cycle instead of rendering.
  ```text
  TICKET-001 [done] Set up repo
  └─ TICKET-002 [open · READY] Add CI job
     └─ TICKET-003 [open · blocked by 002] Publish build artifacts
  TICKET-004 [in-progress] Write docs
  ```

## Update

1. Locate the ticket by id (or unambiguous title prefix).
2. Allowed changes: edit title/notes, add/remove dependencies, check criteria,
   change status.
3. Enforce the rules:
   - `done` only when every completion criterion is checked AND every
     dependency is `done`. Otherwise explain what is missing and do not flip
     the status.
   - `in-progress` only when every dependency is `done` (warn if not).
4. Write back:
   - local: edit the ticket file.
   - github: `gh issue edit <n> --body "<new body>"` for body changes;
     `gh issue close <n>` / `gh issue reopen <n>` to mirror `done`/`open`.
