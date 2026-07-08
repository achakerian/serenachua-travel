# Serena Chua Travel — bespoke itinerary planning

A five-page marketing site for an independent **day-by-day itinerary planner**.
The client brings the city, hotel, dates and budget; Serena designs what they
actually *do* each day — tuned to wake-up time, interests and pace — delivered as
a polished PDF, either a strict quarter-hour plan or an options-per-slot one.
**Not a travel agency:** no flights, hotels or fixed packages are sold here.

- **Live:** https://achakerian.github.io/serenachua-travel/
- **Repo:** https://github.com/achakerian/serenachua-travel
- **Stack:** plain static HTML + one CSS file + tiny inline JS. No build step, no
  framework. Hosts on **GitHub Pages** as-is; any push to `main` redeploys.

> **Status — design prototype (2026-07-08).** Structure, visual system and
> interactions are built and deployed. Content is illustrative placeholder
> pending Serena's real details. See **[Handoff](#handoff--picking-this-back-up)**
> for what needs her input and open questions.

---

## Design system — Canva-inspired

Bright, playful and colourful (Serena loves Canva): vivid **gradients**,
gradient-filled headlines, floating colour blobs on the hero, **pill** buttons,
rounded/frosted surfaces, and expressive display type.

### Five themes

Pick one from the grid on the home page — it live-switches the whole site and
follows you across pages (saved to `localStorage`). Default is `lagoon`.

| Theme | Gradient | Display face |
|-------|----------|--------------|
| `sorbet` | Coral → pink → gold | Fraunces |
| `lagoon` | Teal → blue → violet (default) | Space Grotesk |
| `ultra` | Violet → fuchsia → blue | Bricolage Grotesque |
| `meadow` | Lime → emerald → aqua (dark text) | Fredoka |
| `punch` | Magenta → red → amber | Unbounded |

**How theming works:** every colour is a CSS variable. Each theme is one
`[data-theme="…"]` block at the top of `styles.css` defining the gradient
(`--grad` + stops `--c1/2/3`), accent, text colours, corner radius (`--radius`)
and display font (`--display`). A tiny inline script in each page's `<head>`
applies the saved / `?theme=` theme before first paint (no flash). Deep-link a
theme on any page, e.g. `…/about.html?theme=punch`.

To **add a theme:** copy a `[data-theme]` block in `styles.css`, add a `.tcard`
button in the home-page picker, and add its key to the `ok` array in the head
script (present in all five HTML files).

Body font is **Inter**; captions use **JetBrains Mono**. All faces load from
Google Fonts.

---

## Pages — one job each, one layout each

| File | Page | Layout | Job |
|------|------|--------|-----|
| `index.html` | Home | Gradient hero + platitude stickers + testimonials + theme picker | Sell the feeling ("all sorted") + social proof; choose a theme |
| `how-it-works.html` | How it works | Numbered flow + tuning "dials" | Brief → plan → itinerary, and what it's tuned to |
| `sample.html` | Sample | **Strict ⇆ Options** toggle over a PDF viewer | Show the two formats as the real PDF you'd receive |
| `about.html` | About | Passport-style spread | Who plans your days |
| `contact.html` | Start | Trip-brief form | Capture the brief (city, hotel, dates, wake time, interests, budget, format) |

The nav is a slim shared bar; the footer is a single line — no repeated content
blocks across pages. Shared nav/footer markup is duplicated in each file (no
templating in plain HTML — edit one, edit all five).

**Home page specifics:** the hero's right side is a playful cluster of
confidence **stickers** ("all sorted", "no worries", "easy, fun & exciting",
"just show up", "every detail handled"). Below the hero, a **testimonials** row
holds three example quotes (clearly marked as examples). Then the theme picker.

**Sample page specifics:** the Strict/Options toggle is **pure CSS** (hidden
radio inputs + `:checked`) — no JavaScript — and swaps which PDF the embedded
`<iframe>` viewer shows.

---

## The sample itinerary PDFs (`pdf/`)

The Sample page embeds two **detailed, city-agnostic** itinerary PDFs in an
iframe styled as a document viewer. Each stop carries the practical detail a real
planner provides: **cost**, **location**, **getting there**, **watch-out
warnings**, and a **"tell the driver"** phrase card (noted as printed in the
local language). Strict day ends with a cost estimate; options list cost +
watch-out per choice.

Generated from self-contained HTML sources (their own inline styles — they use a
fixed navy document palette and do **not** follow the site themes):

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
Desktop browsers render inline; download links under the viewer are the fallback
for browsers that won't (some mobile ones).

---

## Enquiry form (needs a form service)

`contact.html` posts the trip brief to **Formspree**. Static hosting can't email
form data on its own:

1. Create a free form at <https://formspree.io>.
2. Replace `YOUR_FORM_ID` in the `<form action="…">` with your endpoint.

Interests submit as multiple `interests` values; wake time, budget and format are
their own fields. Equivalent services: Netlify Forms, Getform, Web3Forms.

---

## Run & deploy

```bash
python3 -m http.server 8000   # preview at http://localhost:8000
```

Already deployed via GitHub Pages (Settings → Pages → deploy from `main`, root).
Relative links mean it works under the project sub-path unchanged.

---

## Handoff — picking this back up

Everything below is **placeholder awaiting Serena's real input**. When her
requirements/adjustments arrive, work through these.

### Needs Serena's content
- [ ] **Business basics** — real city/base, contact email, phone/WhatsApp, socials
- [ ] **Portrait + bio** — real photo (the gradient block in `about.html`) and her story
- [ ] **Placeholder markers** — search the HTML for `[TBC]` and `[City]` and fill each in
- [ ] **Testimonials** — home-page quotes are examples; swap for real ones as trips wrap
- [ ] **Turnaround / tweak claims** — confirm the numbers on `about.html`
- [ ] **Copy pass** — tune the voice to Serena's own
- [ ] **Pricing** — currently "quote on brief" (no prices shown); confirm that's the model

### Needs a decision / account
- [ ] **Form endpoint** — create a Formspree form, drop in the ID (`contact.html`)
- [ ] **Default theme** — `lagoon` for now; Serena may prefer another (or a custom brand gradient)
- [ ] **Domain** — optional custom domain via GitHub Pages settings
- [ ] **Legal** — privacy note on the form, basic booking terms

### Open questions to raise with Serena
1. **Real sample city?** The sample PDFs are deliberately city-agnostic. A single
   signature city would make them far more convincing (real "tell the driver"
   phrases in local script, real spots). Worth doing once she picks one.
2. **Brand colours / logo?** If she has a Canva brand kit, we can make one theme
   match it exactly (or replace the five with her palette) and drop in a real logo.
3. **Themed PDFs?** The PDFs are navy regardless of theme. Match them to a chosen
   theme / add a gradient cover page if she wants full consistency.
4. **Scope of a trip** — single day sample shown; do full multi-day itineraries
   need their own page/preview, or is the PDF enough?
5. **Booking/deposit** — currently none (quote-first). If she later wants online
   deposits, that needs a Stripe Payment Link (works on static hosting).

### Where the moving parts live
- **Themes & all styling:** `styles.css` (theme blocks at the very top).
- **Theme switch logic:** inline `<script>` in each page's `<head>` (applies theme)
  + the `setTheme()` script at the bottom of `index.html` (the picker).
- **PDF content:** `pdf/itinerary-*.html` → regenerate PDFs (see above).
- **Form fields:** `contact.html`.

---

## Notes

- Responsive to mobile; nav collapses to a "Menu" toggle under 780px.
- `prefers-reduced-motion` and keyboard focus are respected.
- Commit history tells the design evolution (wireframe → Departures → itinerary
  pivot → theme system → Canva gradients → stickers → detailed PDFs).
