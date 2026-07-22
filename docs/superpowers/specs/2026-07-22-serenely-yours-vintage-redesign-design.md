# Serenely Yours — vintage love-letter redesign

**Date:** 2026-07-22
**Status:** Approved design, pending implementation plan
**Supersedes:** the previous Canva-inspired five-theme gradient system

## Goal

Replace the site's bright five-theme gradient identity with a single, cohesive
**vintage love-letter** identity — moody oxblood ground, dusty-mauve nav band,
high-contrast Playfair headlines, a Pinyon-script "Serenely Yours," signature,
and typewriter (Special Elite) "letter" content. The look and per-page concepts
are specified by the client's 8-page mockup PDF (`Serenely Yours,.pdf`).

This is a **content + structure** redesign, not a recolour: every page's markup
changes.

## Decisions (locked)

1. **Single identity.** "Serenely Yours" becomes the whole site. The theme
   picker, the four gradient themes, and the no-flash theme `<script>` in every
   page `<head>` are **removed**. There is no theme switching.
2. **Full-concept fidelity.** Rebuild every page to match the mockup's concepts
   and adopt its copy.
3. **Decorations are CSS/SVG.** All ornaments (torn paper, wax seals, scroll
   frame, postmark, airmail border, "WANTED" stamp) are stylised
   approximations built in CSS/SVG — **no external image assets**, preserving
   the no-build/static-hosting ethos. Approximations, not photoreal raster.
4. **Uploads are placeholder-only.** The Start-page dropzones are styled,
   non-functional UI (static hosting can't receive files without a paid
   service). Clients email files after the brief; wiring a service is a future
   task.
5. **Sample = real client samples on one page.** Keep the two existing sample
   PDFs (`sample-strict.pdf`, `sample-options.pdf`) on the Sample page, shown
   inside the "Previously on…" scroll frame with a restyled **Strict / Options**
   toggle. The mockup's invented Corporate/Adventurous/Relaxation moods are
   dropped (no content for them, and these are the real samples). A small
   scroll-frame itinerary **teaser** is added on the homepage, linking to Sample.
6. **Nav mapping.** Every page shares: brand **"Serenely Yours,"** (Pinyon
   script) centred; left links **HOME · ABOUT · GIST · SAMPLE**; a right-aligned
   outlined rectangular **START** button → the brief form (`contact.html`).
   The mockup's redundant separate START/CONTACT is merged into one START CTA.

## Brand & motif

- Wordmark: **"Serenely Yours,"** in Pinyon Script.
- Serena remains the person behind the brand (About page, letter sign-offs).
- A light **cinema/letter motif** runs through the copy: *"Previously on…"*,
  *"The big reveal"*, "Love Letters", epistolary sign-offs. The stray "cue"
  clapperboard doodle from the mockup is **not** reproduced (reads as a stray
  placeholder); the motif lives in copy instead.

## Colour system (CSS variables, single palette)

Defined once on `:root`. No per-theme blocks.

| Token | Value | Use |
|---|---|---|
| `--wine` | `#5a1626` | Page ground (deep oxblood) |
| `--wine-deep` | `#4a1120` | Wax seals, deeper panels, insets |
| `--wine-line` | `#743349` | Hairlines/borders on wine |
| `--mauve` | `#a85a7a` | Nav band, buttons, accents, links hover |
| `--rose-faded` | `#b3768d` | Large translucent Playfair headlines |
| `--cream` | `#f2e8e4` | Paper surfaces, light text on wine |
| `--cream-dim` | `rgba(242,232,228,.72)` | Muted cream text |
| `--paper` | `#efe3dd` | The letter / kraft surfaces |
| `--ink` | `#20141a` | Typewriter text on paper |
| `--stamp-red` | `#a23a2e` | "WANTED" stamp, airmail accents |

Flat and printed — **no gradients**. Sharp corners (`--radius: 4px`); pill
buttons become clean rectangles (the outlined CONTACT/START button).

## Type system (four Google Fonts, replacing the previous six)

| Face | Role |
|---|---|
| **Pinyon Script** (`--script`) | *Only* the "Serenely Yours," signature & sign-offs |
| **Playfair Display** (`--display`) | Caps headlines **and** flowing italic lines (*waste mine instead.*, *You bring me the vision.*, *Previously on…*) |
| **Special Elite** (`--type`) | Every typewriter element: the letter, testimonials, step cards, wax-seal labels, form lines, dropzone labels |
| **EB Garamond** (`--body`) | Prose (About paragraph, hero sub-copy, general body) |

The whole Google-Fonts `<link>` is **replaced** with just these four families on
all five pages (dropping Space Grotesk, Fraunces, Bricolage Grotesque, Fredoka,
Unbounded, Inter, JetBrains Mono).

## Global chrome (shared, duplicated per page — no templating)

- **Nav:** solid `--mauve` band; cream links (uppercase, letter-spaced);
  centred Pinyon-script brand; right-aligned outlined rectangular START button.
  Collapses to the existing "Menu" toggle under 780px.
- **Footer:** single line on `--wine`; script "Serenely Yours,".
- Each page `<head>`: remove the theme `<script>`, swap the fonts `<link>`,
  update `<title>`/meta.

## Page specifications

### 1. Home — `index.html`
- Oxblood hero. Headline **"YOUR TIME IS PRECIOUS."** (Playfair caps,
  `--rose-faded`) + italic **"waste mine instead."** (Playfair italic, cream).
  A short EB-Garamond line restates the service beneath.
- Hero right: torn lined-paper **"Dear Passenger"** letter — cream paper card,
  ruled lines (`repeating-linear-gradient`), rough torn edge (`clip-path`),
  slight tilt, Special Elite text, signed "Serenely Yours, / Serena".
  Copy: *"Dear Passenger, Do nothing. Well… except pack your bags and prepare
  for the adventure of a lifetime. The rest? Leave it to me."*
- **"LOVE LETTERS"** section: testimonials as a typewriter **carousel**
  (CSS scroll-snap track + prev/next chevrons + dot indicators), each signed
  like a letter ("SERENELY, ADAM & MAYA"). Marked as example `[TBC]`.
- **Itinerary teaser**: a small scroll-frame thumbnail → Sample page.

### 2. About — `about.html`
- Headline: **'I TURN "WHAT SHOULD WE DO?" INTO "THAT WAS THE BEST DAY EVER."'**
- **"WANTED" poster** portrait: kraft card, placeholder B&W portrait block, red
  `--stamp-red` "WANTED" stamp (rotated, distressed look via CSS), typewriter
  caption "ONE UNSUSPECTING TRAVELER WITH EXCELLENT TASTE."
- Prose (EB Garamond): *"I simply refuse to let a perfectly good vacation be
  wasted on mediocre planning. I adore a hidden gem, a well-timed coffee stop,
  and an itinerary with just enough room for a happy little detour."*
- Sign-off: **~~Yours sincerely,~~ Serena** (strikethrough) + Pinyon-script
  "Serenely Yours,".
- Four **wax-seal** stats as CSS radial-gradient discs, Special Elite labels:
  **48H** Draft turnaround · **$0** Share the vision · **2×** Rounds of tweaks ·
  **1:1** My personal touch.

### 3. Gist — `how-it-works.html`
- Small caps "HOW IT WORKS" + italic **"You bring me the vision. I orchestrate
  the experience."** (Playfair italic, `--rose-faded`).
- Three **dashed-border typewriter cards**:
  1. **01. SET THE SCENE** — The city. The hotel. The dates. The budget.
  2. **02. I MAKE IT HAPPEN** — I take your wish list and turn it into days worth.
  3. **03. THE BIG REVEAL** — A perfectly planned itinerary.

### 4. Sample — `sample.html`
- Heading: **"Previously on…"** (Playfair italic, `--rose-faded`).
- Ornate **scroll frame** (SVG line-art with rods/handles) wrapping the PDF
  viewer; restyled **Strict / Options** toggle (pure-CSS radios, unchanged
  mechanism) swapping the embedded `<iframe>`.
- Download-link fallback retained beneath.

### 5. Start — `contact.html`
- **Airmail letter** form: postmark stamp (SVG), airmail barber-pole border
  (CSS repeating-linear-gradient), typed underline fields.
- Fields: Name · Email · City · No. of Travellers · Dates · **Typical wake-up**
  (styled select "spinner" — three styled `<select>`s for hour / minute / AM-PM
  evoking a rolling picker; functional, static) · **Interests** pills
  (City / Country / Gourmet / Arts) · Daily budget · Plan format · Others.
- Three **dropzones** (Flights / Accommodations / Others "eg., attractions"):
  dashed rounded cards, image-placeholder icon, "drop pdf/png" label,
  drag-over hover state — **non-functional placeholder UI**. Note added that
  files are emailed after the brief `[TBC]`.
- Form still POSTs to Formspree (`YOUR_FORM_ID` placeholder unchanged).

## Decorative-element implementation notes

All pure CSS/SVG, inline, no assets:
- **Torn paper**: cream card + `repeating-linear-gradient` rules +
  `clip-path: polygon(...)` jagged edge + soft box-shadow + rotate.
- **Wax seal**: `radial-gradient` disc, `--wine-deep`, inner ring via inset
  shadow, subtle irregular border-radius, Special Elite label.
- **Scroll frame**: inline SVG line-art (rods, finials, hanging cord) framing a
  content slot; content is the PDF viewer.
- **Postmark / airmail border**: SVG circular stamp + CSS repeating diagonal
  red/blue border stripe.
- **WANTED stamp**: rotated text with `--stamp-red`, low opacity + slight
  texture (letter-spacing, mix-blend), on the kraft card.

## Files touched

- `styles.css` — substantial rewrite: remove the five theme blocks and all
  gradient-specific component CSS; build the single vintage system + new
  components (nav band, letter, carousel, wanted poster, wax seals, gist cards,
  scroll frame, airmail form, dropzones).
- `index.html`, `about.html`, `how-it-works.html`, `sample.html`,
  `contact.html` — rewrite `<head>` (drop theme script, swap fonts, titles) and
  `<body>` (new nav/footer + page content).
- `README.md` — update to describe the single "Serenely Yours" identity;
  remove the theme-system documentation; refresh the handoff checklist.
- `pdf/*` — **unchanged** (out of scope).

## Out of scope

- Real photography/content, real testimonials, real business details.
- The two sample PDFs stay navy (not re-themed).
- A working Formspree endpoint; real file uploads.
- The "cue" clapperboard doodle.

## Handoff / follow-ups (post-implementation)

- Drop in Serena's real portrait (WANTED poster), real testimonials (Love
  Letters), business details, and Formspree ID.
- Optionally: wire real file uploads (paid form service); author three real
  mood itineraries if the moods concept is revived; theme the sample PDFs.
