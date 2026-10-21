# Product animations that explain the product

Use for landing heroes and animated feature illustrations. These are defaults
learned from the owner-approved Mountable work, not a requirement to copy its
palette, layout or filesystem metaphor. Honor the current product brief and
approved design system. The
[supporting analysis](https://github.com/writeitai/autoboard/blob/main/design/analysis/product-animation-lessons.md)
records the evidence and the retirement of mandatory vgpu.

## Start with the product's meaning

Write the sentence a visitor should understand after watching. Identify the
actors, their action, the visible consequence and what remains afterward.
Sketch that sequence before choosing a renderer. Put the content intent in
the page spec and the beats/timing in the frontend design or motion handoff.

Mountable provides three useful examples:

| Story | What the scene makes visible |
| --- | --- |
| Persistence | Agents and laptops connect at different times; reads move outward, writes inward. Saved files remain through a beat where every worker is offline. |
| Many entry points | API, filesystem, MCP, CLI and SDK paths converge on the same persistent files. |
| Budget control | An app uses storage; an illustrative usage meter fills and stops at its spending cap. |

Animate the cause and its result in a readable sequence. Keep the enduring
object visible if persistence is the promise; do not accidentally erase it at
the loop boundary. Distinguish concepts through labels and direction as well
as color. Let a viewer read the result before the next beat. Example amounts
are labeled as examples, and illustrated capabilities must actually ship.

For several feature stories, give each its own section, takeaway and useful
docs link. Alternating artwork left/right can give a desktop page rhythm;
on mobile, copy before artwork is a good default. Diagrams suit product
concepts; code examples still suit setup instructions and API details.

## Give the scene a deliberate visual identity

Reuse approved colors, typography roles, spacing, line weights and surface
treatments from the product's tracked pen.dev source. Design its static
composition first, including a mobile composition with fewer actors when
needed. Shrinking a large desktop diagram usually makes labels unreadable.

For a mature technical brand, try precise connectors, strong hierarchy,
restrained accents and clear status changes. Energy can come from contrast
and timing without constant bouncing, flashing or ornamental motion. Adapt
this direction when the product's brand calls for a different tone.

Use third-party logos as secondary context, with faithful local vector assets
and recorded official sources. A logo does not establish an endorsement or
prove an integration. Label the actual access mode and stay within verified
capabilities. Keep the product's own objects and outcome dominant.

## Choose the smallest renderer that fits

Start with **native SVG and CSS keyframes** for small labeled 2D diagrams,
connectors, highlights and meters. This can reuse the static artwork directly
without an additional animation player. Lottie is an option for an existing
authored animation and editing workflow. WebGPU/vgpu or another renderer is
an option when the scene needs capabilities that justify it. Record the choice
and its measured cost; vgpu is not required.

Keep the static illustration present in initial HTML. With Next.js static
export, render geometry in a build-time Server Component and pass the rendered
scene as children into a small client playback wrapper. Importing all the SVG
construction code into that wrapper puts it in the client dependency graph.
This follows Next.js's
[Server/Client composition guidance](https://nextjs.org/docs/app/getting-started/server-and-client-components).
Static SVG still costs HTML bytes and rendering work; measure those too.

For sibling illustrations, reuse a small local playback controller rather
than introducing a general animation framework. Start motion after critical
content has loaded and the browser is idle. Pause offscreen and in hidden
tabs, honor reduced motion before playback and when it changes, and preserve
the user's explicit pause across visibility changes. With CSS timelines,
use `animation-play-state` to pause/resume rather than remounting the scene.
For a heavy player, conditionally import it behind the same readiness gates;
disabling SSR alone does not make its download lazy.

## Make the static state and controls complete

The illustration must communicate its meaning with JavaScript disabled,
before loading, on initialization failure and with reduced motion. Keep
essential labels, actors and the result in that state. For SVG, supply a
meaningful accessible name/description; hide purely decorative detail from
assistive technology.

Provide a keyboard-accessible pause/play control for continuous autoplay.
Keep the user's pause in force while they interact with the rest of the
page. Reserve control width and caption height from the first render so
initialization does not reflow text or move surrounding content. Reduced motion
alone does not replace a pause mechanism: see
[WCAG 2.2.2](https://www.w3.org/WAI/WCAG22/Understanding/pause-stop-hide.html).

## Verify the story, not just the first screenshot

Inspect a complete loop on actual narrow and wide browser viewports. For a
hero, keep the promise, adoption prompt and primary action usable in the
initial screen. Prefer a different mobile composition over tiny text.

Exercise the narrative's invariants: saved files survive worker disconnects,
read-only clients never send writes, and a cap meter does not overshoot its
limit. Also check pause/resume, offscreen and hidden-tab suspension, reduced
motion, no JavaScript and renderer failure. Keep controlled illustration and
player states in Storybook, with browser behavior in the container layer.

Use a production build for network and performance checks. Record initial
HTML/SVG, JavaScript and player/asset costs, layout shift and loading metrics
against the project's budget or before/after baseline. Check that motion
dependencies stay off unrelated routes. Label local throttled measurements
as synthetic; they are not production RUM or proof of better conversion.

## Mountable implementation reference

At reviewed revision `4cbf94f`, the artwork is native SVG, CSS owns the loops
and a React hook owns playback. The hero includes a deliberate all-offline
interval with the saved file tree still visible. Use the pattern, adapting
its vocabulary and composition to the next product:

- [Hero geometry and semantic labels](https://github.com/writeitai/fsit/blob/4cbf94fb280ad1bf0bbf9f4701c37c99dbf6d3e0/web/src/components/views/landing/FilesystemIllustration.tsx)
- [Hero CSS timeline](https://github.com/writeitai/fsit/blob/4cbf94fb280ad1bf0bbf9f4701c37c99dbf6d3e0/web/src/components/views/landing/FilesystemHero.module.css)
- [Feature geometry](https://github.com/writeitai/fsit/blob/4cbf94fb280ad1bf0bbf9f4701c37c99dbf6d3e0/web/src/components/views/landing/FeatureIllustration.tsx)
- [Shared playback controller](https://github.com/writeitai/fsit/blob/4cbf94fb280ad1bf0bbf9f4701c37c99dbf6d3e0/web/src/components/containers/landing/useAnimationPlayback.ts)

Official technical references consulted 2026-10-19: the linked Next.js and
WCAG documentation, and
[CSS pause/resume behavior](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/animation-play-state).
