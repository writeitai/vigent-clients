# Working principles

## Keep it simple — YAGNI

- Build the simplest solution that meets the current requirements.
- Do not build features, abstractions, configuration, or infrastructure for
  hypothetical future needs. Add them when a concrete requirement demands them.
- Complexity must justify itself against a simpler alternative.
- Prefer removing code and dependencies when doing so preserves required
  behavior. Delete obsolete code, unused paths, and superseded implementations
  instead of keeping speculative fallbacks.
- Keep changes focused and easy to understand, operate, and maintain.

## Stay profitable

We build businesses that make money. Prefer solutions whose economics get
better as customers join, and avoid anything that could turn the product into
a business that loses money.

- **Each new customer adds profit.** Price usage so that a customer brings in
  more than the cost they add: storage, compute, traffic, payment fees and
  support.
- **Fixed costs may be recovered over time.** A first server, or the first
  customer on a new server, may not pay for itself. That is fine when later
  customers share the same cost (for example, several customers packed onto
  one server) and the margin grows with each one.
- **In total, we do not lose money.** At customer numbers we can actually
  expect, total revenue covers total cost. If a design only pays off at a
  scale we are far from, say so.
- **Nothing is free by accident.** Do not give usage away through defaults,
  rounding, waivers, unbilled gaps or missing measurements. Free tiers, trials,
  credits and discounts are the owner's decisions, with their cost stated.
- **Show the numbers.** When a design adds cost per customer, state that cost
  and the price that covers it. Flag anything that could leave us below cost,
  and let the owner decide.

## Design documentation

Follow the installed `design-corpus` skill when working on
design documents. Keep brainstorming and research separate from accepted design.

## Product checklists

Writeit AI's [Autoboard repository](https://github.com/writeitai/autoboard/blob/main/checklists/README.md)
contains checklists for maintaining project quality, especially before the MVP
when developing the product and setting up standards. The installed
`product-checklists` skill explains how to use them and record each check's
status in the Autoboard board with the `autoboard checklists` CLI.

## Public repository boundary

This is the public client repository, not the hosted product. Never put private
production inventory, infrastructure identifiers, internal costs/accounting,
signed agreements, customer records, credentials or private corpus data here.
The private writeitai/LegalIt repository owns infra/, business_ops/ops/, hosted
resources, commercial operations and verified service capabilities. Make changes
there when an authorized task needs them. Public documentation may link the
product website and public API behavior without copying private operational data.

The installed infra-inventory and business-ops skills remain available to
maintainers for their work in the private product repository; they do not change
this public boundary. Competitive claims belong to the product's approved public
copy and must not be invented here.

## Why we are the best

Follow the installed `why-we-are-best` skill for
`design/why_we_are_the_fucking_best_intent.md` (how we intend to beat the
competition, set by the owner) and
`business_ops/why_we_are_the_fucking_best_verified.md` (measured or verified
claims we publish). Write both as selling points, not legal copy: punchy,
true and briefly proven. Public copy may state only verified claims. Those files
live in the private product repository, not in this client repo.
Product maintainers publish their mirrored state from that repository after merge.

## Public writing

Follow the installed `public-writing` skill whenever drafting, editing, or
reviewing public-facing copy outside reference documentation (`/docs/*`): blog
posts, landing pages, X and LinkedIn queues, community launch drafts, B2B cold
emails, newsletters, creator pitches, and press kits. Write short sentences in
plain human prose, open with a concrete user failure, never include
self-sabotaging *"what we cannot do yet"* disclaimer sections, and strip all AI
filler and internal spec jargon.

## Shared infrastructure integrations

Consult the [integration checklist](https://github.com/writeitai/autoboard/blob/main/checklists/integration.md) when designing,
implementing or reviewing infrastructure and external-service integrations.
Record applicable checks and supporting evidence in the ticket result or PR;
identify failures, unverified checks and justified non-applicability.

## CI

Follow the [CI checklist](https://github.com/writeitai/autoboard/blob/main/checklists/ci.md): jobs run only for the areas a
change touches. In a private repository every job, deploys included, runs on
`runs-on: [self-hosted, writeitai-pool]`; a public repository uses
GitHub-hosted runners only. The pool needs no per-repository registration
([D28](https://github.com/writeitai/autoboard/blob/main/design/decisions.md#d28--private-repositories-share-one-ephemeral-runner-pool)).
Deploy credentials are federation restricted to `main` or a protected
environment, never long-lived secrets, and jobs are shared-Docker safe.

## Client stack and checks

The agreed client scope is CLI, MCP and SDKs for Python and TypeScript. No
Next.js frontend or hosted backend belongs here. Implementations and package
scaffolding are future work. When Python code is added, use uv, ruff and Pyright;
TypeScript clients use the checked-in package manager/lockfile and documented
build/type checks. Add concrete validation commands with the implementations,
not placeholder commands for nonexistent projects.

## GCP cost constraint

Never enable Artifact Analysis or automatic container vulnerability scanning.
Keep `containeranalysis.googleapis.com` and `containerscanning.googleapis.com`
disabled. If Artifact Registry repositories are introduced, explicitly disable
vulnerability scanning rather than inheriting project defaults. Do not enable
these services as part of setup, CI, security tooling, or deployment; the owner
has explicitly prohibited them because of cost.

## Changes through pull requests only

Every change reaches `main` through a pull request. Never push directly to
`main`.
