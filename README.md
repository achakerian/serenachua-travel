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
> Formspree endpoint. Sample content (Kyoto) is illustrative.

## Pages — one job each, one layout each

| File | Page | Layout | Job |
|------|------|--------|-----|
| `index.html` | Home | Hero + itinerary teaser card | "Your days, dialled in" — pitch + a sample day |
| `how-it-works.html` | How it works | Numbered flow + tuning "dials" | Brief → plan → itinerary, and what it's tuned to |
| `sample.html` | Sample | **Strict ⇆ Options toggle** | Show the two formats on one real day |
| `about.html` | About | Passport spread | Who plans your days |
| `contact.html` | Start | Trip-brief slip | Capture the brief (city, hotel, dates, wake time, interests, budget, format) |

The Strict/Options toggle and the "pick one" option cards are **pure CSS**
(hidden radio inputs + `:checked` / `:has()`), so they work with no JavaScript.
Type loads from Google Fonts. The nav is a slim shared bar and the footer is a
single line — no repeated content blocks across pages.

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
- [ ] **Sample day** — keep Kyoto as a demo or swap for a signature city
- [ ] **Turnaround / tweak claims** — confirm the numbers on `about.html`
- [ ] **Copy pass** — tune the voice to Serena's own
- [ ] **Legal** — privacy note on the form, basic terms

## Notes

- Responsive to mobile; nav collapses to a "Menu" toggle under 780px.
- Reduced-motion and keyboard focus are respected.
- No templating in plain HTML, so the shared nav/footer markup lives in each
  file — edit one, edit all five.
