# Serena Chua Travel — bespoke itinerary planning

A five-page site for an independent **day-by-day itinerary planner**. The client
brings the city, hotel, dates and budget; Serena designs what they actually do
each day — tuned to wake-up time, interests and pace, as a strict quarter-hour
plan or an options-per-slot one. Not a travel agency: no flights, hotels or
fixed packages sold here.

Design direction is **"Departures"**: navy / bone / amber, signage-style display
type (Saira Condensed), Inter body, JetBrains Mono flight-codes, and a recurring
**itinerary agenda** motif. Plain static HTML + one CSS file — no build step —
hosts on **GitHub Pages** as-is.

> Still a design pass: a few facts are `[TBC]` and the enquiry form needs a
> Formspree endpoint. Sample content is illustrative.

## Themes

Five **Canva-inspired gradient themes** — bright colour, gradient-filled
headlines, floating colour blobs on the hero, pill buttons and rounded surfaces.
Pick one from the grid on the home page; it live-switches the whole page and
follows you across the site (saved to `localStorage`).

| Theme | Gradient | Display face |
|-------|----------|--------------|
| `sorbet` | Coral → pink → gold | Fraunces |
| `lagoon` | Teal → blue → violet (default) | Space Grotesk |
| `ultra` | Violet → fuchsia → blue | Bricolage Grotesque |
| `meadow` | Lime → emerald → aqua (dark text) | Fredoka |
| `punch` | Magenta → red → amber | Unbounded |

**How it works:** every colour is a CSS variable; each theme is one
`[data-theme="…"]` block in `styles.css` that defines a gradient (`--grad` + its
stops `--c1/2/3`), an accent, text colours, corner radius and a display font. The
home hero uses `--grad` as its background with floating blurred blobs; light
pages use gradient-filled headlines (`background-clip: text`) and gradient pills.
A tiny inline script in each page's `<head>` applies the saved/`?theme=` theme
before first paint (no flash).

To add a theme: copy a `[data-theme]` block, add a `.tcard` button on the home
page, and add its key to the `ok` array in the head script.

Deep-link a specific theme with `?theme=punch` on any page. Note: the sample
**PDFs** are fixed artifacts and stay in a navy document palette regardless of
theme.

## Pages — one job each, one layout each

| File | Page | Layout | Job |
|------|------|--------|-----|
| `index.html` | Home | Hero + itinerary teaser card | "Your days, dialled in" — pitch + a sample day |
| `how-it-works.html` | How it works | Numbered flow + tuning "dials" | Brief → plan → itinerary, and what it's tuned to |
| `sample.html` | Sample | **Strict ⇆ Options toggle** over a PDF viewer | Show the two formats as the real PDF you'd receive |
| `about.html` | About | Passport spread | Who plans your days |
| `contact.html` | Start | Trip-brief slip | Capture the brief (city, hotel, dates, wake time, interests, budget, format) |

The Strict/Options toggle is **pure CSS** (hidden radio inputs + `:checked`), so
it works with no JavaScript — it swaps which PDF the embedded viewer shows.
Type loads from Google Fonts. The nav is a slim shared bar and the footer is a
single line — no repeated content blocks across pages.

### The sample PDFs (`pdf/`)

The Sample page embeds two real, city-agnostic itinerary PDFs in an iframe styled
as a document viewer. They're generated from self-contained HTML sources:

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

The embed uses the browser's native PDF viewer via `#toolbar=0&view=FitH`. Most
desktop browsers render it inline; the page also offers download links as a
fallback for browsers that won't (some mobile ones).

## Preview locally

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Deploy

Live at **https://achakerian.github.io/serenachua-travel/**. Any push to `main`
redeploys.

## Enquiry form (needs a form service)

`contact.html` posts the trip brief to **Formspree**:

1. Create a free form at <https://formspree.io>.
2. Replace `YOUR_FORM_ID` in the `<form action="…">` with your endpoint.

The interests submit as multiple `interests` values; wake time, budget and format
come through as their own fields. Alternatives that work the same way: Netlify
Forms, Getform, Web3Forms.

## Checklist before launch

- [ ] **Portrait** — real photo in `about.html` (the striped block)
- [ ] **`[TBC]` facts** — city, contact email
- [ ] **Form endpoint** — Formspree ID (see above)
- [ ] **Sample PDFs** — city-agnostic demo; edit `pdf/itinerary-*.html` and regenerate
- [ ] **Turnaround / tweak claims** — confirm the numbers on `about.html`
- [ ] **Copy pass** — tune the voice to Serena's own
- [ ] **Legal** — privacy note on the form, basic terms

## Notes

- Responsive to mobile; nav collapses to a "Menu" toggle under 780px.
- Reduced-motion and keyboard focus are respected.
- No templating in plain HTML, so the shared nav/footer markup lives in each
  file — edit one, edit all five.
