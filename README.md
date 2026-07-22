# Serenely Yours — bespoke itinerary planning

A five-page marketing site for an independent **day-by-day itinerary planner**.
The client brings the city, hotel, dates and budget; Serena designs what they
actually *do* each day — tuned to wake-up time, interests and pace — delivered as
a polished PDF, either a strict quarter-hour plan or an options-per-slot one.
**Not a travel agency:** no flights, hotels or fixed packages are sold here.

- **Live:** https://achakerian.github.io/serenachua-travel/
- **Repo:** https://github.com/achakerian/serenachua-travel
- **Stack:** plain static HTML + one CSS file + tiny inline JS per page. No
  build step, no framework. Hosts on **GitHub Pages** as-is; any push to `main`
  redeploys.

> **Status — design prototype (2026-07-22).** The site carries one identity —
> "Serenely Yours," a vintage love-letter — across all five pages. Structure,
> visual system and interactions are built and deployed. Content is
> illustrative placeholder pending Serena's real details. See
> **[Handoff](#handoff--picking-this-back-up)** for what needs her input.

---

## Design system — Serenely Yours, a vintage love letter

One identity, no theme switcher: an oxblood ground, a mauve nav band, a torn
paper letter, a WANTED poster, wax seals, an airmail form. Everything reads
like a keepsake correspondence from Serena to her travellers.

### Palette

All colour is CSS custom properties defined once at the top of `styles.css`:

| Token | Use |
|---|---|
| `--wine` | Page background (oxblood) |
| `--wine-deep` | Darker recesses — stamp circle, scroll rod |
| `--wine-line` | Hairlines, dashed borders |
| `--mauve` | Nav band, solid buttons, active/checked states |
| `--rose-faded` | Muted headline colour (hero title, section leads) |
| `--cream` / `--cream-dim` | Primary text / secondary text on oxblood |
| `--paper` | The letter and PDF-viewer paper tone |
| `--ink` | Text on paper/cream surfaces (WANTED caption, etc.) |
| `--stamp-red` | WANTED stamp ink, airmail diagonal rule |

### Fonts (Google Fonts, loaded per page)

| Face | Role | Helper class |
|---|---|---|
| **Pinyon Script** | The "Serenely Yours," signature — nav brand, footer mark, letter sign-offs. Never used for anything else. | `.script` |
| **Playfair Display** | Headlines. Upright caps for section heads, italic for flowing display lines. | `.display` (caps) / `.italic-display` (flowing italic) |
| **Special Elite** | Typewriter face — buttons, labels, captions, form fields, PDF-viewer chrome. The site's "voice." | `.type` |
| **EB Garamond** | Prose — the letter body, About bio, base body copy. | Default `body` font (no utility class needed) |

### Decorations are CSS/SVG, not images

Every decorative element — the torn letter edge (`clip-path`), the WANTED
poster photo (radial-gradient placeholder), the wax seals (radial-gradient +
`border-radius` blob + inset shadows), the airmail diagonal stripe
(`repeating-linear-gradient`), the scroll rod ends, the dashed step cards — is
pure CSS. **No image assets ship in this repo.** The one real photo the site
needs (Serena's portrait) is still a placeholder gradient; see Handoff.

---

## Pages — one job each, one layout each

| File | Page | Concept | Job |
|------|------|---------|-----|
| `index.html` | Home | Hero + "Dear Passenger" torn-paper letter · Love Letters carousel · itinerary teaser | Sell the feeling + social proof; point to the sample |
| `about.html` | About | WANTED poster (portrait + stamp) · prose bio · sign-off · wax-seal stats | Who plans your days, and what you get |
| `how-it-works.html` | Gist | Italic lead + three dashed typewriter step cards | The brief → plan → itinerary flow, in three beats |
| `sample.html` | Sample | "Previously on…" scroll frame around a Strict/Options PDF viewer | Show the two formats as the real PDF you'd receive |
| `contact.html` | Start | Airmail brief form: wake-up spinner, interest pills, placeholder dropzones | Capture the trip brief |

The nav is a slim mauve bar (script brand centred, links left, Start CTA
right); the footer is a single line. Shared nav/footer markup is duplicated in
each file (no templating in plain HTML — edit one, edit all five). Under
780px the nav collapses to a "Menu" toggle with the script brand stacked full
width above it, and the footer stacks to a centred column.

**Home page specifics:** the hero's right side is the "Dear Passenger" letter
— a rotated, torn-edge notepaper aside with a handwritten-feel typewriter
note, signed in script. Below it, **Love Letters** is a horizontal,
scroll-snap carousel of testimonial quotes (prev/next buttons + dot
indicators, driven by a small inline script), clearly marked as examples.
Then a **teaser** card that mimics a stacked document and links to the Sample
page ("Previously on…").

**Sample page specifics:** the Strict/Options toggle is **pure CSS** (hidden
radio inputs + `:checked` sibling selectors) — no JavaScript — and swaps
which embedded PDF the `<iframe>` viewer shows, all inside a decorative
"scroll" frame (rounded rod ends top and bottom).

**Start page specifics:** the form styles itself as an airmail letter — a
diagonal red/mauve striped rule and a circular "AIR MAIL" stamp badge — with
a three-part wake-up time "spinner" (`<select>`s for hour/minute/AM-PM),
checkbox "pills" for interests, and three upload **dropzones that are visual
placeholders only** (styled as dashed drop targets, not wired to any real
upload — see caveat below).

---

## Placeholder-only dropzones (Start page)

The Flights / Accommodations / Others dropzones on `contact.html` are
**decoration, not functionality**. There is no file input, no drag-and-drop
handler, and no upload endpoint behind them — the boxes exist to set
expectations for a future feature. The page tells users as much: *"Uploads
coming soon — email your files after your brief."* Don't remove that note
without wiring up real uploads first (see Handoff).

---

## The sample itinerary PDFs (`pdf/`)

The Sample page embeds two **detailed, city-agnostic** itinerary PDFs in an
iframe styled as a document viewer. Each stop carries the practical detail a
real planner provides: **cost**, **location**, **getting there**,
**watch-out** warnings, and a **"tell the driver"** phrase card (noted as
printed in the local language). Strict day ends with a cost estimate; options
list cost + watch-out per choice.

Generated from self-contained HTML sources (their own fixed navy document
palette — deliberately distinct from the site's oxblood identity, since these
are meant to read as an independent deliverable):

| Source (edit these) | Output (embedded) |
|---|---|
| `pdf/itinerary-strict.html` | `pdf/sample-strict.pdf` |
| `pdf/itinerary-options.html` | `pdf/sample-options.pdf` |

To regenerate after editing a source (needs Chrome installed):

```bash
CHROME="/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
"$CHROME" --headless --no-pdf-header-footer --virtual-time-budget=6000 \
  --print-to-pdf=pdf/sample-strict.pdf  pdf/itinerary-strict.html
"$CHROME" --headless --no-pdf-header-footer --virtual-time-budget=6000 \
  --print-to-pdf=pdf/sample-options.pdf pdf/itinerary-options.html
```

The embed uses the browser's native PDF viewer via `#toolbar=0&view=FitH`.
Desktop browsers render inline; download links under the viewer are the
fallback for browsers that won't (some mobile ones).

---

## Enquiry form (needs a form service)

`contact.html` posts the trip brief to **Formspree**. Static hosting can't
email form data on its own:

1. Create a free form at <https://formspree.io>.
2. Replace `YOUR_FORM_ID` in the `<form action="…">` with your endpoint.

Interests submit as multiple `interests` values; wake time, budget and format
are their own fields. Equivalent services: Netlify Forms, Getform, Web3Forms.

---

## Run & deploy

```bash
python3 -m http.server 8000   # preview at http://localhost:8000
```

Already deployed via GitHub Pages (Settings → Pages → deploy from `main`,
root). Relative links mean it works under the project sub-path unchanged.

---

## Handoff — picking this back up

Everything below is **placeholder awaiting Serena's real input**. When her
requirements/adjustments arrive, work through these.

### Needs Serena's content
- [ ] **Real portrait** for the WANTED poster (`about.html`'s `.wanted__photo`
      is currently a grayscale gradient placeholder)
- [ ] **Real Love Letters** — the home-page carousel quotes are examples;
      swap for real client notes as trips wrap
- [ ] **Business basics** — real contact email (footer currently
      `hello@serenachua.travel` marked `[TBC]`), phone/WhatsApp, socials
- [ ] **Placeholder markers** — search the HTML for `[TBC]` and fill each in
- [ ] **Bio + turnaround claims** — confirm the About prose and the wax-seal
      numbers (48H draft turnaround, 2× tweak rounds, etc.)
- [ ] **Copy pass** — tune the voice to Serena's own
- [ ] **Pricing** — currently "quote on brief" (no prices shown); confirm
      that's the model

### Needs a decision / account
- [ ] **Formspree ID** — create a form, drop the ID into `contact.html`
      (replace `YOUR_FORM_ID`)
- [ ] **Real uploads** — the Start-page dropzones are placeholder-only; wire
      up an actual upload path if she wants brief attachments handled inline
      instead of by follow-up email
- [ ] **Domain** — optional custom domain via GitHub Pages settings
- [ ] **Legal** — privacy note on the form, basic booking terms

### Open questions to raise with Serena
1. **Real sample city?** The sample PDFs are deliberately city-agnostic. A
   single signature city would make them far more convincing (real "tell the
   driver" phrases in local script, real spots). Worth doing once she picks
   one.
2. **Themed PDFs?** The PDFs use their own fixed navy palette regardless of
   the site's oxblood identity. Match them if she wants full visual
   consistency, or keep them distinct on purpose (reads as a separate
   deliverable).
3. **Scope of a trip** — single day sample shown; do full multi-day
   itineraries need their own page/preview, or is the PDF enough?
4. **Booking/deposit** — currently none (quote-first). If she later wants
   online deposits, that needs a Stripe Payment Link (works on static
   hosting).

### Where the moving parts live
- **All styling/tokens:** `styles.css` (palette + font variables at the top).
- **Carousel logic:** inline `<script>` at the bottom of `index.html`.
- **Strict/Options toggle:** pure CSS in `styles.css` (`.sample` block) — no
  JS.
- **PDF content:** `pdf/itinerary-*.html` → regenerate PDFs (see above).
- **Form fields:** `contact.html`.

---

## Notes

- Responsive to mobile; nav collapses to a "Menu" toggle under 780px, footer
  stacks to a centred column.
- `prefers-reduced-motion` and keyboard focus (`:focus-visible`) are
  respected.
- Commit history tells the design evolution (wireframe → Departures →
  itinerary pivot → Canva-style theme system → Serenely Yours vintage
  redesign).
