---
name: why-we-are-best
description: Manage a product's two "why we are the best" files — design/why_we_are_the_fucking_best_intent.md, the owner-managed list of ways we intend to beat the competition, and business_ops/why_we_are_the_fucking_best_verified.md, the measured or verified claims we are willing to publish — and mirror them to the Autoboard console's "Why we are best" tab. Use this whenever you set up a new product, write or change a differentiator, measure or benchmark a claim against a competitor, promote a claim from intent to verified, re-check or retire a published claim, or write public copy (landing page, blog, social post, newsletter, outreach) that says we are better than someone at something.
---

# Why we are the best

Every product keeps two short Markdown files. They are the single source of
truth for how the product beats the competition:

| File | What it holds | Who changes it |
| --- | --- | --- |
| `design/why_we_are_the_fucking_best_intent.md` | Where we **intend** to be better than the competition, and how we will prove it. | The owner decides. Agents propose changes through a PR. |
| `business_ops/why_we_are_the_fucking_best_verified.md` | Claims we have **measured or verified** and will publish in blogs, social posts, newsletters, the landing page and outreach. | Agents add a claim only with evidence that meets the bar below. |

The intent file steers what we build and measure. The verified file is what
we say in public. A claim moves from intent to verified only when the
evidence exists. Nothing in the intent file may be published as fact.

Both files exist to **sell the product**. They are ammunition for the landing
page, posts and outreach, not legal documents. Write every claim as a punchy
line a founder would say to a prospect, and keep the proof short. The claim
must be true, but do not bury it in caveats.

The competition map stays in `business_ops/competition/README.md` (see the
`business-ops` skill). Read it before writing either file, so every claim
names a real alternative.

## Starting a product

Fill the intent file with the owner at project start, before designing the
product. Ask which dimensions should set the product apart: performance,
pricing model, number of integrations or connectors, permission controls,
spend limits, interfaces (CLI, MCP, SDKs), guides, setup time and so on.
Create the verified file empty. It stays empty until a claim is measured.
Commit both through the same PR.

## Intent file

One bullet per differentiator. Keep each bullet short and concrete:

```markdown
# Why we are the fucking best — intent

- **Mounting is faster than GCS FUSE and Mountpoint for S3.**
  Proof: time to first successful read and list on a cold mount, matched
  region and file set, median of 10 runs.
```

- Name the comparison target: a competitor, tool or common approach.
- Name the user benefit, unless it is obvious.
- Say what would prove it: the metric, test, price comparison or count.
- Optionally add a `Today:` line with the current gap, so the owner sees
  what is still missing. Keep it current or leave it out.
- Order by importance to the owner.
- A bullet that has been proven stays here and also appears in the verified
  file. Remove a bullet only when the owner drops that goal.

## Verified file

```markdown
# Why we are the fucking best — verified

- **Set up in 2 commands. Competitor X takes 9 steps.**
  Proof: both quick-starts followed in a clean VM on 2026-10-16
  (<run>, <X's docs>). Careful: Linux only so far.

## Do not claim

- "Works everywhere": we have tested Linux and E2B only.
```

Each claim has:

- **The headline**: one bold, plain sentence we would post as-is. Lead with
  the buyer's win and name who we beat. A useful point does not have to be
  unique; "everything in one product" is a fine claim when it is true.
- **Proof**: the date it was checked and one or two links (our test, code,
  benchmark or `infra/prod.yaml` record, and the competitor's own docs or
  price list).
- **Careful** (optional, one short line): only the limit that would change
  what we say in public. Leave everything else out.

The **Do not claim** section lists tempting claims that the evidence does not
support, so no one repeats them in public.

### The bar for a verified claim

- **Performance and cost claims are measured.** Use a reproducible method:
  matched workloads, regions and configurations, the competitor's recommended
  setup, recorded versions, several runs and raw output. A one-off
  observation from a different workload is not evidence.
- **Feature and price claims cite primary sources** with the date they were
  checked: the competitor's own docs, contract or price list, and our code,
  tests or verified production records in `infra/prod.yaml`.
- Missing documentation does not prove that a competitor lacks a capability.
- A person (the owner or an engineer) checks the method and results of a
  measured claim before it enters the file. Name who checked it and when.
- Do not stretch a claim to make it sound stronger. When the evidence is
  narrower than the headline, sharpen the headline to what we proved instead
  of piling on caveats, or leave the claim in intent only.
- Re-check a claim when we change the measured area, a competitor ships a
  relevant change, or the claim is more than six months old before a new
  campaign uses it. Fix or remove a claim that no longer holds, and add it to
  **Do not claim** when people are likely to repeat it.

## Using the claims

Public material (landing page, blog, social, newsletter, press and outreach)
may state only claims from the verified file, with the same scope. The
`business/competitive-claims` check needs five strong verified claims. Do not
invent claims to reach five.

## Mirror to the Autoboard console

The project's **Why we are best** tab in the Autoboard console shows the
verified file first, then the intent file. The board stores a read-only copy.
Never edit that copy; change the files in Git.

After a PR that changes either file merges, update your `main` checkout and
publish from the product root with `BOARD_PROJECT_ID` set:

```sh
autoboard why-best publish \
  --verified business_ops/why_we_are_the_fucking_best_verified.md \
  --intent design/why_we_are_the_fucking_best_intent.md \
  --repository <product repository URL from the Autoboard project> \
  --commit "$(git rev-parse HEAD)"
```

Never publish from an unmerged branch. `autoboard why-best get` shows what
the board has, with its source commit.

This skill ships from `new-project-bootstrap/skills/why-we-are-best/` in
Autoboard. Reinstall it in a product to receive changes.
