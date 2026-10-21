---
name: public-writing
description: Write clear, human-readable public copy for everything that is not reference documentation — blog posts, landing pages, X and LinkedIn queues, Show HN and community drafts, cold emails, newsletters, creator pitches, and press kits. Use this whenever you draft, edit, or review public-facing marketing or narrative text for any product.
---

# Public writing (non-docs)

This skill applies to every public-facing text that is **not** reference
documentation (`/docs/*`, API specs, or legal terms):

- Blog posts (`/blog/*`)
- Landing page copy and comparison callouts
- Social posts and queues (`business_ops/growth_channels/social-queue.json`, X, LinkedIn)
- B2B cold email and LinkedIn outreach templates (`b2b-outreach-frameworks.json`)
- Creator pitches, press kits, newsletters, and community launch drafts (Show HN, Reddit)

Always read `business_ops/why_we_are_the_fucking_best_verified.md` (see the
`why-we-are-best` skill) before writing so you only make claims we have
verified. This skill governs **how** to write those claims so a human actually
wants to read them.

## 1. Write for a human engineer, not a compliance audit

These are the owner's standing preferences for all public writing.

- **Most important first.** Order sentences from the most important to the
  least important, and order the information inside each sentence the same
  way. Lead with what the product is or does.
  - *Bad:* "Mountable gives your AI agents a persistent filesystem."
  - *Good:* "Mountable is a persistent filesystem for your AI agents."
- **Short sentences.** Keep sentences rather short and direct. If a sentence
  has more than two commas or a semicolon, split it into two sentences.
- **Short paragraphs.** 1 to 3 sentences per paragraph. One idea per paragraph.
- **Everyday words.** Write the way a technical founder explains the product to
  another engineer over coffee.
  - *Bad:* "Mountable establishes a bounded credential-delegation perimeter by
    exchanging a single-use stdin admission artifact for an ephemeral mTLS
    session identity."
  - *Good:* "Your agents share one folder. Its files survive even when the
    sandbox is gone."
- **Benefits people care about, not mechanisms.** Say what the reader gets:
  files that survive, one shared folder, a spending cap per user. Leave out
  how it works inside, such as where the API key lives or what the sandbox
  receives to mount.

## 2. Open with what matters most

Social posts, intros and landing copy open with what the product is or does.
Blog posts, threads and emails may open with the reader's problem when that is
the most important thing to say.

Never open a blog post, social thread, or email with product history, category
definitions, or abstract throat-clearing. Open in the first 3 lines with the
exact moment something breaks for the reader:

- *Bad:* "In today's rapidly evolving AI agent ecosystem, managing persistent
  state across ephemeral compute environments presents a significant
  architectural challenge."
- *Good:* "Your coding agent runs for ten minutes inside a sandbox. It clones a
  repo, installs packages, and writes a fix. Then the sandbox stops—and the
  folder is gone."

## 3. Never include "What we can't do yet" or self-sabotaging disclaimer tables

Agents often copy the raw `Limits:` notes from
`why_we_are_the_fucking_best_verified.md` or `/docs/limits` straight into blog
posts and social copy, producing self-sabotaging sections like *"Where we lose"*
or *"What doesn't work yet."*

- **Never put limitation tables, edge-case matrices, or "what we can't do yet"
  sections in a blog post, social post, or outreach message.** Protocol edge
  cases belong in `/docs/limits`.
- **Never write that something is unverified, untested, desk research, not
  run, or doesn't work.** Not even as a short caveat. Say only what is true and
  works; leave the rest out until it works.
  - *Bad:* "E2B is our tested sandbox. The other recipes are marked as not yet
    tested."
  - *Good:* "Mount it in your E2B sandbox."
- **State verified claims accurately in the first place** so they do not need a
  four-line legal footnote:
  - Instead of claiming *"Works on Linux and Mac"* when only Linux mounts
    work, say *"Mount it in sandboxes or on your local machines."* Announce the
    Mac in its own post once it works.
  - Instead of claiming *"Cheaper than S3"* and then pasting a tariff table,
    say: *"Downloads cost $0.02/GB instead of S3 Standard's $0.09/GB."*

## 4. One working example beats a wall of flags

- Show the shortest working code or terminal snippet (5–15 lines) that proves
  how the product solves the problem.
- Do not dump every CLI flag, retry code, or configuration knob into narrative
  copy. Link to the exact `/docs` page for the full reference.

## 5. Banned words and internal jargon

Never use these in public copy:

- **AI filler:** *delve, tapestry, landscape, testament, beacon, symphony,
  unlock, unleash, elevate, revolutionize, game-changer, cutting-edge,
  first-class, world-class, seamless, robust, comprehensive, pivotal,
  moreover, furthermore, in conclusion, it's worth noting.*
- **Internal repository / spec jargon:** *stage-0 cell, archive ratio 1,
  admitted mutation window, non-goal, checklist item, board ticket, canonical
  inventory, desk research, load-bearing, gates.*

## 6. Blog-writing process

Whenever you write a blog post, follow this sequence before drafting the
article prose:

1. **Create a high-level outline file in a `.gitignored` directory** (so the
   scratch outline is never committed to Git). In that file, write down:
   - **What the blog is about and what we are trying to tell (inform about):**
     the core message and takeaway for the reader.
   - **What feeling the user should get:** the intended emotional/intellectual
     response (e.g., informed, excited, relieved).
   - **What each paragraph should be about (high level):** a paragraph-by-paragraph
     plan describing only the high-level purpose of each paragraph. Make sure
     the order of paragraphs makes sense and that the article has a natural
     flow from start to finish (for example, never start with super low-level
     information).
2. **Spin up a review agent** to go over the paragraph composition and order.
   Instruct the review agent to keep its review high-level as well—evaluating
   whether the sequence of paragraphs, the opening hook, the progression from
   high-level context/problem to concrete details, and the intended reader
   feeling hold together cleanly.
3. **Once the outline is approved**, write the actual article following the
   approved paragraph order and all paragraph, sentence, example, and
   banned-word rules in Sections 1–5 above.

## 7. Pre-PR read-aloud check

Before committing any blog post, landing page edit, social queue, or outreach
template:

1. Read every paragraph out loud. If you run out of breath or stumble over a
   clause, cut it in half.
2. Delete the first paragraph if the post makes more sense starting at
   paragraph two.
3. Verify every comparative number or claim appears in
   `business_ops/why_we_are_the_fucking_best_verified.md`.
4. Confirm there are zero *"what we cannot do yet"* sections or internal audit
   terms.
5. Record every published blog post, social post, launch thread, directory
   listing, notebook, showcase PR, cold-email batch, or video in
   `business_ops/growth_channels/publications.yaml` with a unique `id` and
   `utm_content` (tagging product links with
   `?utm_source=<channel-slug>&utm_medium=<kind>&utm_content=<utm_content>`,
   where `<channel-slug>` is `re.sub(r"[^a-z0-9]+", "-", channel.lower()).strip("-")`).
   Links in social posts stay plain, such as `https://mountable.io`: the
   platforms show the start of the address, and UTM tags look messy there.
   For human-published (`D14`) drafts awaiting human action in a `human_help`
   ticket, note the pending draft in `state.yaml` `current_focus` and append the
   entry to `publications.yaml` once published. After merging changes to
   `state.yaml` or `publications.yaml` into `main`, run
   `autoboard growth-ledger publish` (see the `business-ops` skill).

This skill ships from `new-project-bootstrap/skills/public-writing/` in
Autoboard. Reinstall it in a product to receive changes.
