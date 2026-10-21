---
name: business-ops
description: Manage the repository's business_ops/ directory — business_ops/ops/ for internal recurring costs, vendor tiers/limits, accounting, signed DPAs/agreements, and monthly margin checks, and business_ops/growth_channels/ as the strategic and operational brain for growth channels coordinated with the Autoboard board. Use this whenever you record or check internal costs, vendor free-tier budgets, legal/vendor agreements, or monthly margins (pricing/monthly-margins), or whenever you brainstorm, evaluate, plan, execute, update, or reconcile growth channels for a product. Use this also when mapping competitors under business_ops/competition/; verified competitive claims live in business_ops/why_we_are_the_fucking_best_verified.md (see the why-we-are-best skill).
---

# Business operations (`business_ops/`)

`business_ops/` is the repository's Git-reviewed home for ongoing business
operations (`ops/`) and the strategic and operational brain for customer
acquisition (`growth_channels/`), with the competition map under
`competition/` and the verified competitive claims in
`why_we_are_the_fucking_best_verified.md`. It complements:
- `design/` (`design-corpus`): software/product architecture, pre-launch unit
  economics, and high-level decisions
- `infra/` (`infra-inventory`): technical resources, deployment, and live
  functional verifications
- `autoboard checklists` (`product-checklists`): engineering quality gates in
  the Autoboard database

## Layout

```text
business_ops/
├── README.md                      # Navigable index (any answer in two hops)
├── ops/
│   ├── README.md                  # Operations overview and ownership scope
│   ├── costs.md                   # Recurring internal costs, vendor tiers/limits, and monthly margin checks
│   └── agreements.md              # Vendor DPAs, contracts, tax/compliance records (when applicable)
├── growth_channels/
│   ├── state.yaml                 # Current per-channel positioning, focus, and owned accounts (matched by channel name)
│   ├── publications.yaml          # Append-only ledger of published items, utm_content keys, and outcomes
│   ├── ideas.md                   # Candidate channel evaluations, prioritization, and rejected/deferred ideas
│   └── <channel-slug>.md          # Optional deeper narrative research or playbook notes for a channel
├── competition/
│   └── README.md                  # Market map, alternatives, sources, and review date
└── why_we_are_the_fucking_best_verified.md  # Measured/verified claims we publish (why-we-are-best skill)
```

Add files under `business_ops/ops/`, `business_ops/growth_channels/`, or
`business_ops/competition/` only when real operational records, channel work,
or competition research exist (YAGNI). The exception is
`why_we_are_the_fucking_best_verified.md`, which every product creates empty
at project start.

## 1. `business_ops/ops/`: costs, accounting, and agreements

### `ops/costs.md`
- **Recurring internal costs:** List actual recurring infrastructure and vendor
  costs (servers, backups, domains, email, analytics/telemetry, payment
  processing, model usage), their billing cadence, currency, and source
  (invoice, provider API quote, or contract).
- **Vendor tiers and operational limits:** Record free-tier or plan limits that
  constrain operations (such as Brevo's shared 300 emails/day free limit or
  PostHog's EU pay-as-you-go tiers) and the threshold at which an agent must
  open a `human_help` ticket for an owner-approved upgrade.
- **Monthly margin checks (`pricing/monthly-margins`):** After a product
  launches, record the monthly comparison of actual revenue against actual
  infrastructure, payment, and provider bills required by
  `checklists/pricing.md`. When a resource's margin shrinks or turns negative,
  flag it and propose a price or cost adjustment to the owner.

### `ops/agreements.md`
- Record non-secret metadata for signed vendor agreements, Data Processing
  Agreements (DPAs), and business verification statuses: contracting entity
  (WriteIt.ai s.r.o.), counterparty, status, date confirmed, and any scope
  notes or follow-up checks.

### Company-wide vs. product-specific split
- **Autoboard (`writeitai/autoboard`)** owns company-wide shared costs and
  vendor agreements in its `business_ops/ops/` (shared Hetzner database,
  runtime, CI, and observability hosts; shared GCP base; shared Brevo account;
  shared PostHog EU organization and DPA).
- **Each product repository** owns its product-specific costs, Stripe revenue,
  and monthly margin checks in its own `business_ops/ops/`, linking to
  Autoboard's `business_ops/ops/` for shared infrastructure rather than
  duplicating shared host bills.

## 2. `business_ops/growth_channels/`: the growth-channel brain

Growth channels are split across three layers with distinct responsibilities:

1. **Autoboard root `growth_channels/` (Shared Template Catalog):**
   Generalized, cross-product channel playbooks synced on deploy to
   `GET /v1/growth-channel-templates`. Contains no product-specific state.
2. **Autoboard Board `growth_channels` table and console tab (Technical Work Tracker):**
   Project-scoped channel records (`id`, `name`, `description`, `strategy`,
   `gate`, `version`, `archived_at`) so tickets can link to `growth_channel_id`
   and be filtered in the UI and API. Every active template from
   `growth_channels/*.md` is enabled by default on every project, either **Live**
   (`archived_at = null, gate = null`) or **Gated** (`archived_at = null,
   gate != null`); non-applicable or retired channels are **Archived/Disabled**
   (`archived_at != null`).
3. **Product `business_ops/growth_channels/` (Strategic & Operational Brain + Ledger):**
   - **`state.yaml` (Current channel state ledger, modeled on `infra/prod.yaml`):**
     Holds the current operational state for each active or evaluated channel under
     top-level `channels:`:
     - `channel`: Exact Board channel `name` (matched case-insensitively to the
       project's Board channels; no `board_channel_id` or `board_version` is
       stored in Git).
     - `positioning`: Optional product pitch/angle for this channel.
     - `current_focus`: Optional current tactical priority or next milestone.
     - `accounts`: Optional list of `{label, url}` (`https://` URLs of owned
       profiles, directories, or target repositories; never personal contact
       lists or PII).
   - **`publications.yaml` (Append-only ledger of everything published):**
     Every blog post, social post, launch thread, directory listing, notebook,
     showcase PR, approved cold-email batch, or video is recorded here under
     top-level `publications:` once it is actually live/published:
     - `id`: Unique slug (`[a-z0-9][a-z0-9-]{0,119}`, e.g. `2026-10-20-x-launch`).
     - `channel`: Exact Board channel `name` (case-insensitive match).
     - `kind`: One of `blog_post`, `social_post`, `launch_thread`,
       `directory_listing`, `notebook`, `showcase_pr`, `cold_email_batch`,
       `video`, `other`.
     - `published_at`: ISO date `YYYY-MM-DD`.
     - `title`: Headline or hook (`1..300` chars).
     - `url`: Optional public `https://` URL once live.
     - `source_ref`: Optional repository file path or PR reference.
     - `ticket_id`: Optional Autoboard ticket UUID (`null` or omitted when done
       outside the board).
     - `utm_content`: Attribution key (defaults to `id`). Outbound links to the
       product site should carry
       `?utm_source=<channel-slug>&utm_medium=<kind>&utm_content=<utm_content>`,
       where `<channel-slug>` is `re.sub(r"[^a-z0-9]+", "-", channel.lower()).strip("-")`.
       Links in social posts and launch threads stay plain (`D43`).
     - `body`: Optional exact post text/copy for short posts so future agents see
       what was already said.
     - `outcome`: Optional `{checked_at: YYYY-MM-DD, summary, metrics?: {views?, clicks?, likes?, replies?, signups?}}`.
     - **Human-published (`D14`) channels:** While a draft is prepared and
       awaiting human action in a `human_help` ticket, note the pending draft in
       `state.yaml` `current_focus` rather than adding a premature entry to
       `publications.yaml`. Append the entry to `publications.yaml` (with its
       actual `published_at` date and live `url`) once the human publishes it.
     - **YAML quoting rule:** Because the Board API validates string fields with
       strict types (`StrictStr`) after `StrictSafeLoader`, always quote strings
       that YAML 1.1 would otherwise coerce into booleans, numbers, or dates
       (for example `title: "2026"`, `channel: "yes"`, or a date used in a title).
   - **`ideas.md` (Brainstorming & evaluation):** Evaluates candidate channels
     (from Autoboard's template catalog or new ideas) against this product's
     audience, competitors, and unit economics. Records why an idea was chosen,
     deferred (with its adoption trigger), or rejected so later agents do not
     re-propose rejected channels.
   - **Optional `<channel-slug>.md`:** Deeper narrative research or playbook notes
     when a channel needs more prose than `state.yaml` holds.

### Board lifecycle and ledger synchronization

1. **Board owns lifecycle state:** The Board's `growth_channels` records own
   whether a channel is **Live** (`archived_at = null, gate = null`), **Gated**
   (`archived_at = null, gate != null`), or **Archived** (`archived_at != null`),
   plus its `description` and `strategy`. Never duplicate `board_channel_id` or
   `board_version` in Git.
2. **Read before acting:** Before planning growth tickets or drafting channel
   material, read `business_ops/growth_channels/state.yaml`,
   `publications.yaml`, `ideas.md`, and any `<channel-slug>.md`, and fetch
   `autoboard growth-channels list --archive include --all` and
   `autoboard growth-ledger get`.
3. **Match by channel name:** Every `channel` field in `state.yaml` and
   `publications.yaml` must match a project Board channel's `name`
   (case-insensitively). If a channel is renamed on the Board, update its
   `channel` name in `state.yaml` and `publications.yaml` and republish.
4. **Record and publish after merge:** Whenever a PR changes
   `business_ops/growth_channels/state.yaml` or `publications.yaml` (whether via
   a Board ticket or an ad-hoc session), after the PR merges into `main`, update
   the `main` checkout and run:
   ```sh
   autoboard growth-ledger publish \
     --state business_ops/growth_channels/state.yaml \
     --publications business_ops/growth_channels/publications.yaml \
     --repository <product repository URL> --commit "$(git rev-parse HEAD)"
   ```
   Check stderr for any `unmatched_channels` warning and fix any mistyped or
   renamed channel names.
5. **Upstream reusable playbooks:** When a product discovers a new channel or a
   general rule that helps other products, contribute a generalized template
   update to Autoboard's root `growth_channels/` through a pull request.

## 3. `business_ops/competition/`: the competition map

Map the competition before writing competitive claims. Read existing research
first and update it rather than duplicating it.

- **`README.md`:** State the product, target user, scope and research date.
  Cover direct competitors and practical substitutes. Record why each matters,
  where it is stronger or weaker, dated links to primary sources, and unknowns.
  Missing documentation is not proof that a competitor lacks a capability.
- **Claims:** the
  [why-we-are-best skill](../why-we-are-best/SKILL.md) owns them. Intended
  differentiators live in `design/why_we_are_the_fucking_best_intent.md`;
  measured or verified claims, and claims not to make, live in
  `business_ops/why_we_are_the_fucking_best_verified.md`. Do not keep a
  separate claims file here. Record the map against
  `business/competition-map` only after it has merged and been verified.
- Keep product-specific findings in that product's repository. Architecture
  decisions and accepted implementation designs still belong in `design/`.

## 4. Trust recorded state, and never commit secrets or PII

- **Trust recorded as-is state:** Trust verified entries in `business_ops/`
  (such as signed agreements, existing social/marketplace accounts, rejected
  channel ideas, and completed directory submissions or outreach batches). Do
  not repeat an already completed submission, re-propose a rejected channel
  without its trigger changing, or re-audit verified business state unless the
  task or owner explicitly requires it.
- **Never commit secrets or personal data (PII):**
  - Never commit API keys, passwords, bank/tax credentials, or private contract
    PDFs containing sensitive personal data.
  - Never commit personal contact lists, individual email addresses, or scraped
    social profiles of prospects or customers in `business_ops/growth_channels/`
    (per GDPR, D14, and D36). Record public company/community/repository
    targets, approved outreach frameworks, aggregate counts, and Autoboard
    ticket IDs instead.
