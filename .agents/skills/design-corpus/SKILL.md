---
name: design-corpus
description: The design-directory discipline for design and research programs — design/analysis/ holds the bulk of the thinking, design/designs/ the binding accepted design, design/proposals/ the live unchosen alternatives, and design/decisions.md the high-level decision log. Use this whenever you create, edit, review, or accept a design document, establish or revise a product's design system or brand, record an architecture or product decision, plan research for a design area, specify frontend pages for pen.dev, weigh alternatives ("should we use X or Y"), set up a design/ directory, or write the product's why_we_are_the_fucking_best_intent.md — even if the task only says "write a design", "add analysis", "document this decision", or "compare options".
---

# Design corpus

A design program's output is not the conversation that produced it — it is a
corpus of documents that a stranger (human or agent, months later, zero
context) can read cold and trust. The structure below exists to force one
habit: **think long and hard, in writing, before deciding.** A decision made
without visible analysis is indistinguishable from a guess, and gets
relitigated forever.

## The four places

- **`design/analysis/`** — the bulk of the thinking lives here. Research,
  audits, cost models, comparisons, drill-downs, working notes. Analysis is
  non-binding: documents may disagree with each other, reach dead ends, or be
  superseded — that is healthy and stays visible. Most of the corpus by volume
  should be analysis.
- **`design/designs/`** — the accepted, binding **current state**, not an
  additive record of every design ever accepted. A future implementer must
  be able to read it cold and understand the current scope, contracts,
  flows, costs, failure/recovery behavior, and rationale. When a new design
  changes old logic, reconcile every affected binding document: update or
  remove superseded rules, examples, implementation contracts, and links.
  Remove a wholly obsolete design from the binding set or reduce its old
  path to a short pointer to the successor when inbound links need
  continuity. Keep history in analysis, the decision log, and git history.
  A supersession banner does not make a contradictory body an acceptable
  current design.
- **`design/proposals/`** — alternatives that are relevant but not chosen:
  viable paths, future options, competing approaches. Each states its
  adoption trigger — what would have to become true for it to win. Proposals
  keep disagreement honest instead of burying it inside binding documents.
- **`design/decisions.md`** — the numbered log of **high-level** decisions:
  the ones that shape the product, architecture, or economics, or would be
  expensive to reverse. Routine choices don't go here; they live in the
  design docs they belong to. Each entry records context, the choice,
  alternatives considered and why they lost, consequences, status, and links
  to the analysis and design files behind it. The log states conclusions; the
  linked design makes them self-contained. When a later decision supersedes
  an earlier one, mark that **inside the earlier entry**, near its heading
  or status, with a link to the successor. For a partial supersession, name
  the clauses replaced and the parts that remain binding. Keep the original
  decision text as history; do not rely only on a note in the new entry or
  the index to warn a reader who lands on the old one.

Keep `design/README.md` (or the directory's index) navigable — a reader
should find the binding answer to any question in two hops.

## Why we are the best: intent

Every product's `design/` also contains
**`design/why_we_are_the_fucking_best_intent.md`**: the owner-managed list of
ways we intend to beat the competition, each with its comparison target and
what would prove it. Fill it with the owner at project start, and link it
from `design/README.md`. Designs and the roadmap should serve these goals;
when a design trades one away, say so in its analysis. The intent file is not
a public claim. Follow the
[why-we-are-best skill](../why-we-are-best/SKILL.md) for its format, how a
claim becomes verified in `business_ops/`, and how both files reach the
Autoboard console.

PR review findings, reports and approval verdicts belong in PR discussions or
ticket results. Do not create or update Git-tracked PR review records, especially in
`design/`.
Update design documents for accepted design changes without copying review
records into the corpus.

## The discipline: analysis before design

A binding design is the tip of an iceberg. Before writing or materially
changing one:

1. **Frame the problem in analysis** — what question is being decided, what
   constraints are real, what would "wrong" cost.
2. **Do the research properly.** Survey genuine alternatives, not strawmen.
   For anything third-party — Stripe, queues, databases, auth, hosting —
   pull the official current documentation and cite it with URLs and
   retrieval dates; never design against a remembered API. Work the numbers
   where economics matter. Run independent parallel analyses when a decision
   is consequential or contested — convergence from different angles is
   strong evidence, and divergence is exactly what you need to see before
   committing.
3. **Let analyses argue.** Two analysis docs that disagree are more valuable
   than one that's sure. Resolve the disagreement in writing, not by
   picking silently.
4. **Only then decide**: write or update the binding design, move the
   losing-but-viable paths into proposals with adoption triggers, and add a
   decision-log entry if the choice is high-level.

The test for "enough analysis": could a skeptical reviewer reconstruct why
the decision is right from the corpus alone, without asking anyone? If a
design change has no supporting analysis to link, that is the signal to stop
and write the analysis first.

## Writing for the cold reader

- **Explain, don't just name.** Naming a technique or product is not
  explaining it. Lead with the plain-language meaning and why it was chosen;
  keep the precise technical term in parentheses as an anchor.
- **The reasoning lives in the document**, not in anyone's head or a chat
  log. If a reader must have been in the conversation to follow the doc, the
  doc is not finished.
- **Simplicity is the bar.** Prefer the simplest design that meets the
  requirement; complexity must justify itself. When a simpler alternative
  was rejected, it appears in the alternatives with the reason — "we chose
  the complex thing" without a recorded why is a red flag.
- **Sequencing lives in roadmap documents**, not as hedges inside designs.
  A design describes what is decided; when to build it is a separate,
  explicit document.

## Design and brand system

Follow the shared
[design and brand system checklist](https://github.com/writeitai/autoboard/blob/main/checklists/design-and-brand-system.md)
when establishing or changing a product's visual system. The pen.dev design
system and official logo are shared inputs for pages, emails and other product
surfaces, rather than independent artwork for each surface.

Define the design system in the repository-tracked `.pen` file before designing
pages: colors, typography, spacing, radii, shadows, icons and core components.
Prepare concrete frames and representative component states for owner review.
Record explicit owner approval tied to the source revision and frame identifiers
before treating the system as accepted. Agent review and owner silence do not
substitute for approval. When the system changes, obtain approval for the changed
version; an earlier approval covers only its recorded scope.

Have the owner select or approve the official logo. Supply horizontal (wide),
stacked (tall) and square layouts, reusing artwork where appropriate without
stretching or cropping it. Verify legibility at intended sizes and backgrounds.
Derive `favicon.ico` from the official square mark and verify it at small sizes.

Export the accepted system and logo to **`design/designs/brand/`** and commit
them through a PR. Include a design-system visual reference and token values,
and SVG and transparent PNG exports for each logo layout. Keep `README.md`
there as the asset index: source `.pen` file/revision and frames, links to owner
approval evidence, exported files, dimensions and intended uses. Link it from
`design/README.md`. Open the exports to verify they match the approved sources
and are not clipped or distorted. Source `.pen` files remain editable; exports
are the reusable assets, and a source alone does not complete export coverage.
Keep exports and their index in sync when approved sources change.

This section ships with the canonical skill under
`new-project-bootstrap/skills/design-corpus/` in Autoboard. Existing installed
project copies need a deliberate skill update or reinstall to receive changes;
do not assume they change when the canonical source changes.

## Frontend pages and the pen.dev handoff

For a product with a frontend, maintain **`design/designs/pages/`** as the
accepted inventory of all pages in the app's current agreed scope, including
public, sign-in and authenticated pages. These specs are the input for
designing every page in pen.dev. Apply the analysis-before-design discipline
above when deciding or changing the page scope.

Use the **pen.dev CLI** to create and edit the designs. Save the canonical
`.pen` file inside the product repository under **`design/designs/`** (for
example, `design/designs/mountable.pen`), and use that repository copy for
subsequent edits. Link it from `design/README.md` and reference its page/frame
identifiers in `pages/pen-dev-checklist.md`. An existing tracked workspace
should be reused rather than creating competing canonical files.

### Match the route tree

Mirror the Next.js App Router's folders relative to `app/` (or `src/app/`),
using **`page.md`** where the app has or will have a page file such as
`page.tsx` or `page.mdx`. Keep route segments, route groups and dynamic
parameter names aligned with the router.
For a new app, groups such as `(public)` and `(authenticated)` make the
access split easy to scan; follow an existing app's groups when present.
Record the URL pattern and access requirement in each spec regardless of
folder names. Route groups organize routes without adding URL segments;
their names do not enforce authentication. Sign-in pages are normally public.
Include user-visible error and not-found screens using matching spec names,
such as `error.md` and `not-found.md`, at their corresponding route level.

An illustrative inventory, not a required set of product pages:

```text
design/designs/pages/
├── pen-dev-checklist.md
├── (public)/
│   ├── page.md                        # /
│   ├── sign-in/page.md                # /sign-in
│   └── blog/
│       ├── page.md                    # /blog
│       └── [slug]/page.md             # /blog/[slug]
└── (authenticated)/
    └── dashboard/page.md              # /dashboard
```

Specify a dynamic page **once per page template**, not once per content
record. `blog/[slug]/page.md` describes the general article page and its
sections; 1,000 articles do not require 1,000 page specs. Apply the same rule
to other record-driven pages and catch-all routes. Dynamic parameters describe
the route shape, not a requirement to render the page at request time.
Content pages sharing one template, such as MDX docs articles, also share
one spec even when each article has its own route file. Put that spec at
their common route level (for example, `docs/article-template.md`), state
the URL family it covers, and list the template once in the checklist.

These conventions follow the official Next.js documentation for
[route groups](https://nextjs.org/docs/app/api-reference/file-conventions/route-groups)
and [dynamic segments](https://nextjs.org/docs/app/api-reference/file-conventions/dynamic-routes),
consulted on 2026-10-05.

### Keep each page spec high-level

Each page or screen spec, including shared content templates, states:

- **Route and access:** the URL pattern; public or authentication required,
  including any role restriction that matters for this page. For error and
  not-found screens, state the route subtree served and its applicable
  access requirements.
- **Purpose:** who the page is for and what it should help them understand
  or accomplish.
- **Sections:** an ordered outline of the sections and what each contains
  or is for. Mention a materially different state only if it changes that
  outline, such as an empty dashboard's first-use guidance.

Describe content intent, not finished headings, marketing copy, article text,
component implementations or visual styling. Include only sections that make
sense for the product's current stage and agreed capabilities. A launching
product without customer references must not get testimonials, customer-logo
strips or referral claims merely because established products have them.
Keep later-stage ideas in proposals or the roadmap until their adoption
conditions are met. Keep shared navigation or shell specifications in
`layout.md` at the corresponding route level; link them rather than
duplicating their content in every page.

### Landing page requirements

For a public product landing page, include these in its first screen (hero):

- A prominent, copyable prompt the visitor can paste into their coding agent
  to start using the product, following Mountable's Prompt entry point. The
  page spec describes its purpose; the implementation supplies a concise,
  actionable prompt pointing to the project's real agent-readable docs and
  shipped setup workflow.
- A visually compelling animation that explains the project's purpose or
  core workflow. Describe the product-specific visual metaphor in the page
  spec for pen.dev; keep renderer and timing details in the frontend design.
  Keep the promise, agent prompt and primary action readable and usable.

For heroes and animated feature illustrations, read the
[product animation playbook](references/product-animations.md). Start with
native SVG/CSS for small 2D diagrams and choose another renderer when the scene
justifies it; vgpu is not mandatory. Preserve a complete static composition,
reuse the approved brand and make the important product behavior visible.

The page must also show where the product is better than a named alternative
at something concrete and measurable. The page spec names the comparison
target, the dimension and the evidence the page will link.

Follow the shared [frontend checklist](https://github.com/writeitai/autoboard/blob/main/checklists/frontend.md)
for copy behavior, playback controls, lazy loading, reduced motion and static
fallback verification.

### Track every pen.dev page design

Create **`design/designs/pages/pen-dev-checklist.md`** at the inventory root.
It is both the page index and the design checklist; link it from the design
corpus index. Include one entry for every page spec, including each dynamic
template, with:

- A link to the spec, its URL pattern and its access requirement.
- An unchecked design item until the pen.dev design has been produced and
  reviewed against the spec.
- A reference to the actual design artifact and its page/frame identifier,
  plus the review result or remaining work. Do not invent artifact references
  or mark a page complete just because its Markdown spec exists.

Each page spec owns its route, access requirement and content outline. The
checklist's URL and access summaries are navigation aids copied from the
specs, not independent binding definitions; correct any mismatch there.

The checklist must cover every specified page, not just the landing page.
Include separately specified error and not-found screens as well.
Keep it and the specs in sync when routes or scope change. When a page or
layout spec changes, reopen every design item it affects until each design
has been checked against the new outline. Shared designs may serve several
pages, but each page still has an entry pointing to the relevant design.
Completing this checklist records design coverage; implementation and product
verification remain separate.

## Red flags to fix on sight

- A binding design (or design change) with no analysis behind it.
- A decision entry whose rationale exists only in conversation history.
- A superseded decision that still appears current when read on its own.
- A binding design that accumulates old and new logic instead of stating one
  current contract.
- A proposal that is actually decided (promote it) or dead (say so).
- Analysis conclusions being cited as if binding — only designs and the
  decision log bind.
- The same fact stated in multiple binding places (it will drift; pick one
  home and link).
