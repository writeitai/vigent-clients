---
name: infra-inventory
description: Keep the repository's infra/ directory an accurate, non-secret record of current production state and verified live capabilities — infra/prod.yaml is the current-state inventory of real resources and functional verifications, infra/history.md holds one-time operational/migration events, and infra/README.md is the overview. Use this whenever you check whether an external service or resource is already verified, create, change, verify, or delete real infrastructure or its identifiers (a GCP project, region, service account, Secret Manager secret name, VM, DNS record, domain, email route, Brevo sender, OAuth client, Stripe webhook, PostHog telemetry, Cloudflare zone or Pages project, GitHub Actions secret or variable name, deploy target), finish a cutover or first deploy, or are asked "what is running in production".
---

# Infra inventory

**Keeping `infra/` accurate is a first-class duty**, not optional cleanup.
Changing the real world and leaving the record stale is an unfinished change.

## Layout

- `infra/prod.yaml`: the **current-state** inventory and verification record.
  It lists every real resource and external integration with its public
  identifiers, current as-built status, and current live verification status
  (`status`, `verified_at`, and a concise proof or receipt pointer). Only `prod`
  exists until another environment really does. Add `infra/<env>.yaml` beside it
  when one exists.
- `infra/history.md`: the log of **one-time historical operational events**
  (migrations, cutover rehearsals, database upgrades, VM/network/tunnel
  retirements, and dated rollout receipts) whose evidence is worth keeping
  human-scannable outside Git history. Add `infra/history.md` only when there
  is real historical event evidence to move out of `infra/prod.yaml`.
- `infra/README.md`: the overview, covering what the current pieces are and how
  they connect. It links to procedure documents and `infra/history.md`; it does
  not repeat them.
- Declared infrastructure code (OpenTofu, bootstrap scripts, rulesets) also
  lives under `infra/`. Reconcilers and scripts are the way to change live
  state. Do not hand-edit something that a reconciler owns.

Create `infra/prod.yaml` and `infra/README.md` as soon as the first real
resource exists (usually the GCP project). Do not wait for a "real" deploy.

Business operations—internal recurring costs, vendor free-tier limits,
accounting notes, signed DPAs/contracts, and growth-channel operations—belong
in `business_ops/` under the `business-ops` skill, not in `infra/prod.yaml`.

## Current-state rule for `infra/prod.yaml`

`infra/prod.yaml` describes **what exists in production right now** and **what
live capabilities have been verified**. It is not an append-only event log:

- When a deploy, cutover, credential rotation, or re-verification supersedes an
  earlier state, **update the entry in place**.
- Remove superseded pre-enrollment failure blocks, temporary rehearsal records,
  and step-by-step migration narratives from `infra/prod.yaml`; move notable
  one-time cutover or retirement evidence to `infra/history.md`.
- Preserve every key read by deployment, preflight, and CI scripts.
- A cold reader opening `infra/prod.yaml` must see one non-contradictory
  snapshot of current production.

## Trust rule for recorded verifications

Task agents (planners, implementers, classifiers, and reviewers) must read
`infra/prod.yaml` **before** running ad-hoc setup or audit probes against live
infrastructure, mail routes, OAuth flows, webhooks, telemetry pipelines, or
worker tool authentication:

- If `infra/prod.yaml` records that a resource or capability is `live` /
  `verified` (with `verified_at` and evidence) and the current task does not
  modify that component, its configuration, or its requirements, **trust the
  recorded verification and do not run redundant ad-hoc checks**.
- Task agents re-verify and update `infra/prod.yaml` only when:
  1. creating, changing, rotating, or retiring the resource or integration,
  2. a requirement or configuration affecting that capability changed,
  3. a live operation or monitor fails and contradicts the recorded status, or
  4. the owner explicitly requests a fresh verification run.
- **Preserved automated checks:** This rule prohibits redundant ad-hoc
  re-verification by task agents; it never disables scheduled operational
  monitoring or readiness timers (such as `worker-readiness.timer`) or CI/CD
  post-deploy smoke and release gates. Routine scheduled or CI pass runs do not
  rewrite `infra/prod.yaml` on every tick; `infra/prod.yaml` is updated only
  when initial verification completes or when the verified state or
  configuration changes.

## When to update (same PR, or an immediate follow-up)

Update `infra/prod.yaml` in place, and the related README or procedure document
when a procedure changes, whenever any of these change in the real world:

- GCP project ID or number, region, billing link, enabled APIs, service
  accounts, or Secret Manager **secret names** (created, mirrored, or moved
  from planned to present)
- GitHub Actions secret or variable **names** used for deploy or checks, and
  repository rulesets
- DNS: zone ID, nameservers, records, custom domains, Pages projects
- Hosts and VMs: provider, type, location, IDs, addresses
- Functional verifications: inbound email routing (`support@<domain>`),
  outbound transactional email (Brevo SPF/DKIM/DMARC and test delivery), Google
  OAuth sign-in round-trip, Stripe checkout/webhook delivery, PostHog
  log/error/pageview/alert reception, and worker VM CLI sign-ins
- As-built status: `planned`, `pending`, or `live`/`verified`, `verified_at`,
  and a concise pointer to the verification receipt

Record only what you verified. Never invent or guess live IDs. Mark anything
not yet created as `planned`.

Do **not** leave outcomes only in chat, CI logs or personal notes. A cold
reader must be able to open `infra/prod.yaml` and know what production is and
what has already been verified.

## Never commit

API tokens, private keys, PEMs, passwords, database URLs, or payment
secrets. Secret **values** stay in GCP Secret Manager and/or GitHub Actions
secrets. The repository holds **names** and public identifiers only. Do not
copy another product's secrets or account identifiers. Each project mints its
own.

## Example `infra/prod.yaml`

```yaml
env: prod
gcp:
  project_id: example-io
  project_number: "123456789012"
  organization: writeit.ai (247821698340)
  billing_account: 01A9D3-D4439B-C0D9F3
  status: live
  created: 2026-09-29
github:
  repository: writeitai/example
  actions_secrets: []   # names only
dns: {}
email:
  inbound_support:
    address: support@example.io
    status: verified
    verified_at: "2026-10-14T10:00:00Z"
  outbound_brevo:
    sender_domain: example.io
    status: verified
    verified_at: "2026-10-14T10:05:00Z"
hosts: []
```

Keep the file shaped by what exists. Add sections when real resources or
verifications appear, not in advance.
