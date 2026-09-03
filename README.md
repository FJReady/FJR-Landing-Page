# FJR Landing Page

A static rebuild of the **First Job Interview Coaching – Perth** landing page,
originally published on Manus at
<https://jobcoachpert-h9rmrnjb.manus.space/>.

The original is a React single-page app (Tailwind + shadcn/ui) served from a
compiled bundle. This repo reimplements it as plain HTML and CSS with no build
step, no framework, and no JavaScript, so it can be hosted anywhere that serves
static files.

## Where the page came from

**Structure from the original** — layout, spacing, type scale, responsive
breakpoints, card treatments, hover states and gradient dividers are all derived
from the original Manus markup.

Section order has since diverged. It now runs: hero, Why Me?, Coaching
Packages, What Happens After You Book, FAQ.

Tinted and plain backgrounds were alternating down the page, and folding "What
We Coach" into the pricing section broke that: "Why Me?" and "Coaching
Packages" are now both `.section--tinted`, back to back. Removing the class
from either one restores the rhythm — left as-is pending a decision.

"Why Me?" replaced "About Your Coach": same slot, first-person copy, and
three trust markers (`.trust`) sitting side by side under the prose instead of
being stated inside it. They wrap and stack on a phone; from 768 px they become
three equal grid columns rather than three intrinsic widths. That is deliberate:
their natural widths total within about eight pixels of the container, so on a
flex row whether they fit on one line depends on the metrics of whichever font
has loaded, and a single orphaned badge on a second row is the one outcome worth
ruling out.

What the sessions cover used to be its own section — three numbered cards under
a "What We Coach" heading. It is now three plain lines (`.coach-topics`) sitting
directly above the prices, so the value and the cost are read together instead
of a section apart. The `.topics` / `.topic` rules went with it, along with
`.topic` in the reduced-motion block, which would otherwise have been a selector
matching nothing.

### Hover on the pricing cards

`.package` lifts 4px on hover, with a darker border and a heavier shadow. Three
things about that:

- The border and shadow carry the state **on their own**, so the hover still
  reads when the transform does not run.
- The transform does not run under `prefers-reduced-motion: reduce`; that block
  sits near the bottom of `css/style.css`.
- The hover rules are wrapped in `@media (hover: hover)`, so a tap on a touch
  screen cannot strand a card in its hover state. Phones and tablets therefore
  show no hover at all, by design.

**Copy has since moved on.** The hero was rewritten, "Why Me?" and the FAQ are
new, and the coaching topics were condensed into the pricing section. Only the
pricing cards, the steps and the footer still carry original wording.

**Audience is early-career, not teens.** The word "teen" appears nowhere on the
site — the framing is anyone early in their career, including first jobs, career
changes and returns to work. Nerves stay in the copy as one reason people
struggle, alongside lack of interview practice.

`privacy.html` was reconciled with that framing on 17 August 2026: it described
the service as being "for teenagers" in two places, which contradicted both the
landing page and the terms. It now opens with the early-career framing, and its
"Young people and parents" section states the age floor as 14 and over, matching
`terms.html` §4 rather than asserting an audience of its own.

Age and consent content is otherwise confined to `privacy.html` §4 and
`terms.html` §4 (minimum age 14, parent or guardian approval under 18), and
deliberately kept off the landing page.

**Palette and type from the brand guide.** Applying the guide moved three
things off the original design:

1. The header banner and footer are now **Deep Forest** (the guide assigns that
   colour to both).
2. Page backgrounds are **Warm Cream**, not white ("keep backgrounds warm").
3. The "Most Popular" banner label is charcoal rather than white — white on
   terracotta measures 2.7:1, which is unreadable. Charcoal gives 5.3:1.

## Logo

The masthead carries the FJR lockup — monogram flanked by terracotta rules,
wordmark beneath — in its reversed colourway (cream on Deep Forest).

It is built as **live text, not an image**. The supplied artwork was a raster
with a watermark, so it was rebuilt in HTML and CSS. That means it stays sharp
at any size and on any display, adds no image request, is selectable and
readable by screen readers, and the flanking rules space themselves off the
monogram rather than sitting at fixed coordinates — so nothing collides or
drifts if a font is slow to load or falls back.

To use the primary colourway on a light background, set `.logo` to
`color: rgb(var(--forest))`.

`images/favicon.svg` uses the same monogram.

## Palette

Straight from the brand guide, defined once at the top of `css/style.css`:

| Role | Colour | |
| --- | --- | --- |
| Primary | Sage Green | `#5B8C6E` |
| Secondary | Deep Forest | `#2F4A3B` |
| Accent | Warm Terracotta | `#D98B5F` |
| Neutral | Warm Cream | `#FAF6F0` |
| Neutral | Charcoal Text | `#2B2B28` |

Terracotta appears twice, per the guide's "use sparingly" note: the rule under
the hero headline, and the "Most Popular" banner. There the banner sweeps
terracotta → champagne
(`#E5C185`) → terracotta so it reads as premium rather than as a status chip.
Champagne is a highlight stop for that gradient only, not a sixth brand
colour. The label stays charcoal, which clears 4.5:1 at every point along the
ramp (worst case 5.28:1 at the terracotta ends); white would drop to 1.71:1
over the champagne.

### Button colour

Buttons use `#538065`, one step darker than the brand sage — an 18% shift
toward Deep Forest. White label text on brand sage measures 3.87:1, under the
WCAG AA minimum of 4.5:1 for text at this size; the darker face clears it at
4.52:1. Sage itself is unchanged everywhere else on the page.

It is a single token at the top of `css/style.css`:

```css
--button-face: 83 128 101; /* #538065 */
```

Set it to `var(--primary)` to go back to the brand sage exactly.

## Typography

Per the brand guide's pairing rule — serif for headlines and section titles
only, sans-serif for body copy, buttons and UI text:

- **Lora** (400–700) — headings
- **Inter** (300–700) — body copy, buttons, prices, UI

Both load from Google Fonts.

## Package banners

Each pricing card carries a full-width banner across its head, rather than the
pill that used to float in the top-right corner: **Sale Price** on the
20-minute session, **Most Popular** on the 60-minute one.

The banner has no radius of its own. Negative margins pull it out to the card's
padding edge and the card's `overflow: hidden` mitres it to the rounded top
corners, so nothing needs adjusting if the radius or the border width changes —
and the featured card's 2px border is handled without a special case.

The two are coloured apart on purpose. The sale banner takes the terracotta →
champagne sweep with a charcoal label (5.28:1 at its darkest stops; white would
fall to 1.71:1 over the champagne, which is why the label is not cream there),
and "Most Popular" is Deep Forest with cream type (9.02:1). Two identical
banners would have read as one repeated component rather than as two different
kinds of claim.

## Booking

The two pricing CTAs are anchors pointing at Calendly, opening in a new tab so
the landing page stays put.

The hero's "Book Now" is **not** one of them: it is an in-page anchor to
`#pricing`, so the visitor picks a package before Calendly opens. It stays in the
same tab, and `scroll-behavior: smooth` on `html` glides down instead of jumping
— set inside `@media (prefers-reduced-motion: no-preference)`, so anyone who has
asked for less motion gets the instant jump. `.section` carries a
`scroll-margin-top` so the heading does not land flush against the top edge.

Unlike the card buttons it is sized to its label, via `.button--inline`. The
card buttons are `width: 100%` to fill their cards; that same rule in the hero
would run a button the full width of the page.

| CTA | Event |
| --- | --- |
| Practice Interview + Feedback | `calendly.com/firstjobready/20min` |
| Full Coaching Session | `calendly.com/firstjobready/60-minute-mock-interview-coaching-session` |

Both card durations match the copy on their cards.

The 20-minute session is on sale: `$39` struck through with `<s>`, `$29` beside
it. Because line-through is not announced by screen readers, the markup carries
`visually-hidden` "Was" and "now" labels, so it reads as *"Was $39, now $29"*.
No end date is set; `.package__sale-note` is styled and sits commented out in
the markup, so adding "Sale ends …" later is a text edit.

Since both buttons read "Book Now", each carries an `aria-label` naming its
package so they are distinguishable out of context.

In the pricing cards the buttons are pinned to the bottom edge with
`margin-top: auto`, so they sit on the same line across both cards however
long the descriptions run.

To open bookings in the same tab instead, drop `target` and `rel` from the
anchors.

## Analytics

Two trackers, both loaded from the end of `<head>` in `index.html`, both async
so neither blocks the render. Between them they are the only JavaScript on the
site.

**Microsoft Clarity** (project `y2r8wl01qz`) records heatmaps and session
replays.

**Meta Pixel** (ID `1285489833562062`) fires `PageView` on load, and a `Lead`
event from each pricing CTA — `content_name` distinguishes
`20min_practice_session` from `60min_full_session`. Both handlers are guarded on
`window.fbq`, so a blocked or failed pixel script cannot throw and stop the
click reaching Calendly; blockers are common in this audience. The hero's "Book
Now" is deliberately untagged, being a same-page jump rather than a booking.

Two things worth knowing:

- Clarity's terms put the duty of disclosure on the site owner. `privacy.html`
  covers it, and is linked from the footer.
- Masking is set to **strict** in the Clarity dashboard, so page text is hidden
  in session replays. The privacy notice states this, so the two need to stay
  in step — if the masking level changes, update the notice.

To remove it, delete the `<script>` block at the end of `<head>`.

`privacy.html` describes what both collect — Clarity in section 1, the Pixel in
section 2. It is deliberately **not** tracked itself: neither snippet is in
`privacy.html`, so nobody is recorded while reading about being recorded.

> Adding or removing a tracker means editing the notice in the same commit. The
> Pixel's arrival also falsified two lines that had nothing to do with it: the
> intro said information was collected in "two ways", and section 4 said we do
> not use information for advertising. Both were corrected. Section numbering
> shifted by one from the old section 2 onward.

The notice is a starting draft written without access to primary sources, so
it should be reviewed by someone qualified before the site takes real traffic.

It carries **no contact details** by request. People who have booked can reach
us by replying to their Calendly confirmation, and the notice says so; visitors
who have not booked have no route to us except the OAIC. Adding an address
later means restoring a short "Contact us" section and pointing sections 5, 7
and 8 back at it — the numbering shifted when the Pixel section was added.

## Performance

A PageSpeed run on 21 August measured **FCP 2.7s, LCP 3.5s, Speed Index 4.3s**,
with **TBT 0ms and CLS 0** — no JavaScript cost, no layout shift. Its largest
single finding was *render-blocking requests, est. saving 1,930ms*.

Three of the four fixes are done:

1. **Fonts trimmed to what the CSS uses.** The page used to request nine files —
   Lora at 400/500/600/700 and Inter at 300/400/500/600/700. Only six were ever
   applied: Lora 600 (the monogram) and 700 (headings, topic numerals), Inter
   400–700. The URL now asks for ranges (`wght@600..700`, `wght@400..700`),
   which lets Google serve one variable file per family rather than one per
   weight.
2. **Every image is WebP**, no JPEG fallback. 390 KB → 185 KB, **53% less**, and
   the hero — the largest contentful paint — drops from 40 KB to 14 KB at the
   size a phone fetches. Measured against the JPEGs, the mean channel difference
   is 1.5–2.2 out of 255, and the hero sits under a scrim besides. WebP has been
   universal since 2020, so a `<picture>` fallback would have doubled the file
   count and created a trap: replace only the `.jpg` and every modern browser
   keeps serving the stale `.webp`.
3. **`fetchpriority="high"` on the hero.** Images sit at low priority until
   layout proves them visible; the attribute says so up front. This is the run's
   *LCP request discovery* item. Deliberately **no `<link rel=preload>`** — that
   would name the hero's filename in a second place, and a stale one silently
   downloads the photo twice.

### Still open

**Self-hosting the fonts** is the rest of the 1,930ms: it removes a
render-blocking stylesheet on a third-party origin, along with two DNS lookups
and two TLS handshakes. It needs the `.woff2` files committed here, which has to
be done by hand — download the Lora and Inter latin subsets, drop them in
`fonts/`, and replace the Google `<link>` with local `@font-face` rules.

The cost is that changing typography stops being a one-line URL edit.

**Cache lifetimes** (the run's *85 KiB* finding) cannot be fixed on GitHub
Pages, which serves everything with a ten-minute lifetime and offers no way to
configure headers. A custom domain behind a CDN is the only route.

**Deliberately not done:** inlining the CSS. `css/style.css` is shared by all
three pages, so inlining means either three copies that drift apart or a
"critical" subset that drifts from the whole. It is the smaller half of the
render-blocking figure and the larger half of the maintenance cost.

## Local preview

No build tools required. From the project root:

```bash
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

## Publishing to GitHub Pages

1. **Settings → Pages**
2. Under "Build and deployment", set the source branch to `main` and the folder
   to `/ (root)`
3. Wait a minute for the first publish, then open the `https://….github.io/…`
   URL it gives you

## Project structure

```
index.html            Landing page
privacy.html          Privacy notice
terms.html            Terms & conditions
css/style.css         Brand tokens, reset, layout, components
images/favicon.svg    FJR monogram favicon
```

## Hero photograph

The hero is a photograph behind a dark scrim, with the copy over it: ranged
left, vertically centred, capped at `34rem` so it stays inside the heavy end of
the scrim.

The headline — *Practice an Interview Before the Real Thing* — is the one piece
of type on the page allowed to shout. It scales fluidly with
`clamp(2rem, 4.4vw, 3rem)` rather than stepping at the breakpoint, sets tighter
than the section headings, and closes with a short terracotta rule
(`.hero__title::after`). That rule and the "Most Popular" banner are the only
two places the accent colour appears.

| File | Size | Dimensions |
| --- | --- | --- |
| `images/hero-before-after.webp` | 31 KB | 1823 × 863 |
| `images/hero-before-after-960.webp` | 14 KB | 960 × 454 |

Served through `srcset` with `sizes="100vw"`, so phones fetch the 960 px file
and desktops the full one.

### Provenance

The photo arrived in the repo as `hero-before-after.jpg.png` — a 1.46 MB **PNG**
with a doubled extension, so the page could not find it. It was re-encoded here
to progressive JPEG at quality 82 (4:2:2 chroma), which is a **94% reduction**
with no visible loss at this size, especially under the scrim. The misnamed
original was removed.

If the photo is ever replaced, re-encode rather than dropping a PNG in: a
full-width photo as PNG costs well over a megabyte on the first paint.

### Why it does not break if the image goes missing

`.hero` paints the scrim colour (`--hero-scrim`, `#16221B`) as its own
`background-color`, with the photo as a separate `<img>` layered behind the
scrim. A missing, slow or undecodable image leaves a solid dark band carrying
the same cream type — a deliberate-looking hero rather than a broken one.

### What the scrim shows

The photograph is a **diptych**: the same person anxious on the left, shaking
hands on the right. The layout puts the copy over the left half, so the scrim is
weighted to match.

**At 768 px and up** it runs left to right — 0.94 behind the copy, lifting to
0.32 at the right edge so the handshake stays visible — and the copy is capped
at `34rem` to keep it inside the heavy end. Widening that cap, or lightening the
left stop, costs legibility.

The cost of this layout, accepted deliberately: the anxious half sits under the
heaviest part of the scrim, so it reads as atmosphere rather than as the "before"
of a before/after. A version that showed both halves is in the history at
`013144e` — it worked by running the scrim top to bottom and dropping the copy
below both faces, which needed a hero roughly 250 px taller.

**Below 768 px** the scrim is near-uniform (0.85 → 0.77). Narrow crops are
unpredictable, so legibility cannot depend on which part of the photo lands
behind the text. `object-position: 64% center` favours the right of the frame,
so the anxious half is what crops away first — the half worth losing here.

### Legibility

Cream on a photograph only works if the scrim guarantees it, so the scrim is
measured, not eyeballed: the hero is rendered with its text hidden and the real
composited pixels behind each line are sampled for the lightest one.

| Width | Behind the headline | Behind the body |
| --- | --- | --- |
| 1440 px | `rgb(31, 42, 35)` — **13.80:1** | `rgb(57, 66, 59)` — **7.91:1** |
| 1280 px | `rgb(31, 42, 35)` — **13.80:1** | `rgb(58, 67, 61)` — **7.83:1** |
| 390 px | `rgb(61, 66, 59)` — **9.56:1** | `rgb(61, 69, 63)` — **7.57:1** |

The headline is large text, where AAA is 4.5:1; the body copy is normal text,
where AAA is 7:1. Everything clears its own bar. The body is the tighter of the
two at every width, because it reaches further right than the headline, into the
lighter end of the scrim.

The mid-gradient stops were tuned against these numbers rather than by eye: with
the ramp at `0.58` at 62% the body measured 6.76:1 — AA, but short of AAA — so it
was moved to `0.62` at 66%.

## Coach portrait

`images/coach.jpg` sits beside the "Why Me?" copy. That section is the
only first-person writing on the page, so it is the one place a face answers a
question the reader is already asking — *who is "I"?* It stacks above the copy
at byline size on a phone, and runs as a second column from 768 px where it is
large enough to carry a face.

| File | Size | Dimensions |
| --- | --- | --- |
| `images/coach.webp` | 69 KB | 800 × 1000 |
| `images/coach-400.webp` | 20 KB | 400 × 500 |

Supplied as a 2.1 MB PNG at 1086 × 1448 — a 3:4 frame, where the slot is 4:5.
The 91 px difference comes off the top, since there is generous headroom above
the hair and none to spare below the collar. Re-encoded to progressive JPEG at
quality 82 in two sizes wired through `srcset`: **95% smaller**, and
lazy-loaded, since it sits below the fold.

Any replacement should be cropped to 4:5 the same way. The ratio is carried by
the file rather than by CSS, so a differently shaped photo will not letterbox —
it will change the height of the column.

> The `alt` text reads "Your coach at First Job Ready" because the page still
> names nobody. It should become the coach's actual name, in the alt text and in
> a visible byline — a face without a name does only half the work.

## Outcome photograph

`images/outcome.jpg` sits beside the steps in "What Happens After You Book" —
the thing the steps are for, next to the steps themselves, rather than floating
above or below where it would read as decoration between two sections.

| File | Size | Dimensions |
| --- | --- | --- |
| `images/outcome.webp` | 34 KB | 880 × 660 |
| `images/outcome-480.webp` | 15 KB | 480 × 360 |

Supplied as a 1.9 MB PNG at 1448 × 1086, already a clean 4:3, so it needed no
crop. Re-encoded to progressive JPEG at quality 82 in two sizes through
`srcset`: **96% smaller**, and lazy-loaded.

The two columns start at **960px**, not the usual 768px. With a 26rem image
beside it the steps column would be about 257px at 768px, which wraps every
step onto two lines; below 960px the photo stacks under the steps at full
width instead.

## Illustrations

There are none on the page, and no placeholders left either.

The clipboard placeholder went out with the "About Your Coach" section it sat
in — the copy that replaced it does not call for artwork, and a dashed
placeholder box is visible to every visitor. The `.art` styles were removed with
it. The two-chairs pencil sketch was never uploaded and lost its slot to the
photograph.

If a sketch does arrive, the style direction still stands: hand-drawn /
sketchbook, loose linework, accent colour used sparingly, faceless,
age-neutral.

## FAQ

Native `<details>` accordions — no JavaScript, keyboard-operable for free, and
the answers stay in the HTML so they remain findable while collapsed.

One flat list of five questions, no group headings. Ordered so the questions
about what the session actually is land before the logistics.

Two answers were removed from here on purpose: the refund question, which is
covered by `terms.html` §5, and the price question, which the pricing cards
already answer.
