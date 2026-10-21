---
name: product-checklists
description: Use Autoboard's product quality checklists when planning, implementing or reviewing project standards, especially before the MVP, and record the project's checklist status in the Autoboard board with the autoboard CLI.
---

# Product checklists

Writeit AI's [Autoboard checklists](https://github.com/writeitai/autoboard/blob/main/checklists/README.md)
define shared product quality checks. They are particularly useful before the
MVP when developing the product and establishing standards. Their definitions
stay in Autoboard. The **Autoboard board is the record** of this project's
status: every checklist is included automatically, and each check is done,
still to do, or will not be done. The repository keeps no status file. Shared
bootstrap skills are maintained in Autoboard: change their sources there and
reinstall them instead of editing the product's installed copies.

## Access

Use the native `autoboard` CLI with `BOARD_URL`, `BOARD_API_KEY` and
`BOARD_PROJECT_ID` (this project's board UUID) in the environment, as the
ticket-board skill describes. Never print the key or write it to a file or a
note. Without board access, report that limitation and do not claim to know
the project's status.

## IDs

Every checklist has an ID (its file name, such as `backend`), and every check
has an ID shown at its start (such as `openapi-export`). Cite a check as
`<checklist>/<item>`, for example `backend/openapi-export`. A check whose
requirement changes gets a new ID, so a new ID always needs a fresh decision.

## Read before planning

```sh
autoboard checklists status
```

The result lists every Autoboard checklist and every current check with its
`status`, `note`, `decided_at`
and `actor`. Read the requirement text of each check in Autoboard's
`checklists/<checklist>.md` at the result's `source_revision`, which is the
revision the board's catalog came from. Also read the project's `AGENTS.md`.

Consult the [new project cookbook](https://github.com/writeitai/autoboard/blob/main/new_project_cookbook.md)
when helpful for product setup. Project-specific instructions remain applicable.

A `done` check whose note's evidence still applies means the capability has
been verified. Do not propose the same setup or standards work again. Review
only checks affected by the task, changed requirements, or evidence that no
longer applies. Checklist discovery does not authorize unrelated
implementation or changes to external services.

## Statuses

- `done`: the required verification was performed. The note records the
  date, product revision and evidence links (PR, CI run, check output).
- `todo`: not done, failed, or not yet verified. The note may say what
  failed or what blocks it. A check without a decision is `todo`.
- `will-not-do`: deliberately not done for this product. The note gives the
  reason or links the accepted product decision.

`done` and `will-not-do` need a non-blank note. Missing required
capabilities are `todo`, not `will-not-do`. Existing code, design documents
and checked boxes alone do not prove that a capability works; read the
evidence behind an existing `done` before relying on it.

## All checklists are included

Every project automatically receives every checklist and new item when the
catalog is deployed. No adoption step or checklist hiding exists. If a check
does not apply (for example, a frontend requirement for a service without a
frontend), record `will-not-do` for that item with the reason. Missing or
unverified capabilities remain `todo`. Existing decisions and history persist
when definitions change.

## Record a decision

```sh
autoboard checklists set backend/openapi-export --status done \
  --note-file evidence.md --actor "<agent or person name>"
autoboard checklists set auth/magic-link --status will-not-do \
  --note "Private service: Google only (AGENTS.md)." --actor "<name>"
```

- Record verified results, not anticipated outcomes: set `done` after the
  change that delivers the check has merged and was verified.
- When a changed requirement, implementation or environment invalidates a
  `done`, set it back to `todo` and say why in the note; keep useful prior
  evidence there.
- `--actor` names who decided. It is self-reported: the board cannot verify
  it, and records separately whether the team key or the owner session wrote
  the decision.
- Setting the current status and note again records nothing. If you are
  unsure whether a write landed, read `status` before retrying, because a late
  retry overwrites a newer decision.
- Never put credentials or secrets in notes.

Per-change records in PRs and ticket results keep the four results (passed,
failed, unverified, not applicable) and cite `<checklist>/<item>`; the
board's notes link to that evidence.

## History

```sh
autoboard checklists history --item backend/openapi-export
autoboard checklists history --all
```

Every decision is kept with its previous status, note, actor and time. Checks
that leave the definitions disappear from `status` but stay in history.

The binding contract is Autoboard's
[checklist status design](https://github.com/writeitai/autoboard/blob/main/design/designs/checklist-status.md).
